This is an example implementation of cert manager

```yaml
name: cert-manager # base 
repository: https://charts.jetstack.io
chart: cert-manager
revision: v1.15.3
appParams:
  disableAutoSync: true
values:
  installCRDs: true
  auth:
    useSecrets: true
  rbac:
    secretNamespaces: [cert-manager]
    secretNames:
      - he-credentials

runes: # you cert manager implemetation prefered
  - name: he-cert-manager
    repository: https://github.com/waldner/cert-manager-webhook-he.git
    revision: 0.0.5
    path: deploy/cert-manager-webhook-he
    values:
      groupName: my.domain
      auth:
        useSecrets: true
      rbac:
        # This controls which namespaces the webhook will be able to read
        # secrets from. BEWARE: AN EMPTY ARRAY MEANS THAT A ClusterRole WILL BE CREATED.
        secretNamespaces: [cert-manager]
        secretNames:
          - he-credentials
  - name: external-dns # external dns implementation
    repository: registry-1.docker.io/bitnamicharts
    chart: external-dns
    revision: 8.3.5
    values:
      provider: webhook
      interval: 60m
      triggerLoopOnEvent: true
      logLevel: trace
      policy: upsert-only   # or "sync" for full sync (ie also deletions)

      extraArgs:
        webhook-provider-url: http://localhost:3333

      sidecars:
        - name: he-webhook
          image: ghcr.io/waldner/external-dns-webhook-he:0.0.4 # or whatever
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 3333
              name: http
          livenessProbe:
            httpGet:
              path: /health
              port: http
            initialDelaySeconds: 10
            timeoutSeconds: 5
          readinessProbe:
            httpGet:
              path: /health
              port: http
            initialDelaySeconds: 10
            timeoutSeconds: 5
          env:
            - name: WEBHOOK_HE_LOG_LEVEL
              value: "debug"
            - name: WEBHOOK_HE_USERNAME
              valueFrom:
                secretKeyRef:
                  name: he-credentials
                  key: username
            - name: WEBHOOK_HE_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: he-credentials
                  key: password
            - name: WEBHOOK_HE_DOMAIN_FILTER
              value: "my.domain"
            - name: WEBHOOK_HE_DOMAIN_FILTER_EXCLUDE
              value: "int.my.domain"
              
      sources:
      - service
      - istio-gateway
      - istio-virtualservice

