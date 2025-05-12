# wip
To setup a Vault instance on Kast, you will need the following resources, its declarations are listed bellow

- The Vault Operator from [Bank-vault](https://bank-vaults.dev/)
- A postgres database prom [Cloud native PG](https://cloudnative-pg.io/)
- Some other manifests that are added as noHelm spells 
   - The vault instance manifest
   - The vault rbac config

> This assumes you use [Istio](https://docs.kast.ing/kasting_manual/istio.html) for gateway configuration and you have cloudnative postgress operator already set up # TODO cloudnative operator docs

Then you need to set it up on your bookrack, here is an step-by-step guide

# 1) Add spells to your book 
> we will assume your book is called "my-book" and your chapter is called "vault", and is a new chapter.

add the following spells in the desired chapter:

### Vault operator:
> no need to modify!
this is the operator declaration for Bank-vault Vault
```yaml
name: vault-operator
repository: ghcr.io/bank-vaults/helm-charts
chart: vault-operator
revision: 1.22.5
appParams:
  annotations:
    argocd.argoproj.io/sync-wave: "1"
values:
```

### This is a postgress database, for Vaults's backend #TODO add cloudnative postgress spell
> no need to modify!, but check if you want to change something
```yaml
name: vault-pg
repository: ghcr.io/cloudnative-pg/charts
chart: cluster
revision: 0.2.1
namespace: vault
appParams:
  annotations:
    argocd.argoproj.io/sync-wave: "2"
values:
  type: postgresql
  mode: standalone
  cluster:
    instances: 1
    roles:
      - name: vault-authentication
        ensure: present
        connectionLimit: -1
        inherit: true
        comment: the vault authentication user
        login: true
        superuser: true
        createdb: true
        createrole: true
        passwordSecret:
          name: vault-authentication

    initdb:
      database: vault
      owner: vault
      postInitApplicationSQLRefs:
        configMapRefs:
          - name: vault-initdb
            key: configmap.sql

  backups:
    enabled: false
    provider: s3
    s3:
      region: "eu-west-1"
      bucket: "db-backups"
      path: "/v1"
      s3Credentials:
        accessKeyId:
          name: minio
          key: ACCESS_KEY_ID
        secretAccessKey:
          name: minio
          key: ACCESS_SECRET_KEY
    scheduledBackups:
      - name: daily-backup # Daily at midnight
        schedule: "0 0 0 * * *" # Daily at midnight
        backupOwnerReference: self
    retentionPolicy: "30d"

glyphs:
  freeForm:
    vaultinitdb:
      type: manifest
      definition:
        apiVersion: v1
        kind: ConfigMap
        metadata:
          name: vault-initdb # This creates the database
        data:
          configmap.sql: | 
            CREATE TABLE vault_kv_store (
              parent_path TEXT COLLATE "C" NOT NULL,
              path        TEXT COLLATE "C",
              key         TEXT COLLATE "C",
              value       BYTEA,
              CONSTRAINT pkey PRIMARY KEY (path, key)
            );

            CREATE INDEX parent_path_idx ON vault_kv_store (parent_path);
            CREATE TABLE vault_ha_locks (
              ha_key                                      TEXT COLLATE "C" NOT NULL,
              ha_identity                                 TEXT COLLATE "C" NOT NULL,
              ha_value                                    TEXT COLLATE "C",
              valid_until                                 TIMESTAMP WITH TIME ZONE NOT NULL,
              CONSTRAINT ha_key PRIMARY KEY (ha_key)
            );
            CREATE FUNCTION vault_kv_put(_parent_path TEXT, _path TEXT, _key TEXT, _value BYTEA) RETURNS VOID AS
            $$
            BEGIN
                LOOP
                    -- first try to update the key
                    UPDATE vault_kv_store
                      SET (parent_path, path, key, value) = (_parent_path, _path, _key, _value)
                      WHERE _path = path AND key = _key;
                    IF found THEN
                        RETURN;
                    END IF;
                    -- not there, so try to insert the key
                    -- if someone else inserts the same key concurrently,
                    -- we could get a unique-key failure
                    BEGIN
                        INSERT INTO vault_kv_store (parent_path, path, key, value)
                          VALUES (_parent_path, _path, _key, _value);
                        RETURN;
                    EXCEPTION WHEN unique_violation THEN
                        -- Do nothing, and loop to try the UPDATE again.
                    END;
                END LOOP;
            END;
            $$
            LANGUAGE plpgsql;
            ALTER TABLE vault_kv_store OWNER TO vault;
            ALTER TABLE vault_ha_locks OWNER TO vault;
            ALTER DATABASE vault OWNER TO vault;
            ALTER FUNCTION vault_kv_put(_parent_path TEXT, _path TEXT, _key TEXT, _value BYTEA) OWNER TO vault;
```

### Basic manifests, 
> this are loaded by kast from any folder in any repo, check out the values you have to set up!!.

```yaml
name: vault
path: manifests/vault # recommended path for the manifests, can be anything you like!
repository: #<your-git-repo>
revision: main # check you use the correct revision
appParams:
  annotations:
    argocd.argoproj.io/sync-wave: "4"
  noHelm: true
  disableAutoSync: true

glyphs: # if you want istio support for the console
  istio:
    - type: virtualService
      enabled: True
      subdomain: vault
      host: vault
      httpRules:
        - prefix: /
          port: 8200
```

# 2) Add manifests to your book 

> we are assuming you add your manifest in your repo in the /manifest/vault path, you can put them wherever you want, remember to refer them like that in the manifest above

### Rbac configuration
> no need to modify!
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: vault
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: vault
rules:
- apiGroups: [""]
  resources: ["secrets"]
  verbs: ["get", "watch", "list", "create", "update", "delete"]
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list", "patch"]
---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: vault-tokenreview
rules:
- apiGroups: ["authentication.k8s.io"]
  resources: ["tokenreviews"]
  verbs: ["create"]
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: vault-tokenreview
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: vault-tokenreview
subjects:
- kind: ServiceAccount
  name: vault
  namespace: vault
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: vault
subjects:
- kind: ServiceAccount
  name: vault
  namespace: vault
roleRef:
  kind: Role
  name: vault
  apiGroup: rbac.authorization.k8s.io
```

### Vault declaration itself
> Check if there are things you want to change, for references check [Bank-vaults docs](https://bank-vaults.dev/docs/operator/reference/)
```yaml
apiVersion: "vault.banzaicloud.com/v1alpha1"
kind: Vault
metadata:
  name: vault
spec:
  size: 2
  image: hashicorp/vault:1.19
  bankVaultsImage: ghcr.io/bank-vaults/bank-vaults:latest

  resources:
    vault:
      limits:
        cpu: "500m"
        memory: "2048Mi"
      requests:
        cpu: "250m"
        memory: "1024Mi"
  annotations:
    common/annotation: "true"

  # Vault Pods , Services and TLS Secret annotations
  vaultAnnotations:
    type/instance: "vault"

  # Vault Configurer Pods and Services annotations
  vaultConfigurerAnnotations:
    type/instance: "vaultconfigurer"

  # Vault Pods , Services and TLS Secret labels
  vaultLabels:
    example.com/log-format: "json"

  # Vault Configurer Pods and Services labels
  vaultConfigurerLabels:
    example.com/log-format: "string"

  serviceAccount: vault

  serviceType: ClusterIP

  unsealConfig:
    options:
      preFlightChecks: true
      storeRootToken: true
    kubernetes:
      secretName: vault-unsealing-key
      secretNamespace: vault
  config:
    storage:
      postgresql:
        ha_enabled: "true"
        max_idle_connections: 10
    listener:
      tcp:
        address: "0.0.0.0:8200"
        tls_disable: true
    ui: true
    cluster_addr: "http://${.Env.POD_NAME}:8201"
    api_addr: http://vault:8200
  externalConfig:
    policies:
      - name: allow_secrets
        rules: path "secret/*" { capabilities = ["create", "read", "update", "delete", "list"] }
      - name: allow_database
        rules: path "database/creds/*" { capabilities = ["read"] }
      - name: vault
        rules:
          path "sys/mounts/*" { capabilities = ["create", "read", "update", "delete", "list", "sudo"] }
          path "sys/*" { capabilities = ["create", "read", "update", "delete", "list", "sudo"] }
          path "auth/*" { capabilities = ["create", "read", "update", "delete", "list", "sudo"] }
          path "secret/*" { capabilities = [ "create", "read", "update", "delete", "list"] }

    auth:
      - type: kubernetes
        path:  control-plane
        roles:
          - name: vault
            bound_service_account_names: vault
            bound_service_account_namespaces: vault
            policies: vault
            ttl: 30m
    secrets:
      - path: secret
        type: kv
        description: General secrets.
        options:
          version: 2

  secretInitsConfig:
    - name: VAULT_PG_CONNECTION_URL
      valueFrom:
        secretKeyRef:
          name: vault-pg-cluster-app
          key: uri

  envsConfig:
    - name: VAULT_PG_CONNECTION_URL
      valueFrom:
        secretKeyRef:
          name: vault-pg-cluster-app
          key: uri
  # Istio configuration is set on Kast, so leave this in false
  istioEnabled: false
```

After adding this to `/manifest/vault` everything should be almost ready

# 3) Add chapter in your index

Now add the vault chapter in your `index.yaml` so it can be seen by kast

# 4) Refresh and Sync!

refresh and sync your argocd book Apps of Apps and wait and resync things untill is done, you can see now everything created

```yaml
```
