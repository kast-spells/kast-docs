hi
To implement istio via kaster you have the follow the next steps

# Setup istio base as spell
Check out versions and stuff are the ones you need or want
```yaml
name: istio-base
repository: 'https://github.com/istio/istio.git'
path: manifests/charts/base
revision: 1.23.0
namespace: istio-system
appParams:
  ignoreDifferences:
    - group: admissionregistration.k8s.io
      kind: ValidatingWebhookConfiguration
      name: istio-validator-istio-system
      jsonPointers:
        - /webhooks/0/failurePolicy
    - group: admissionregistration.k8s.io
      kind: ValidatingWebhookConfiguration
      name: istiod-default-validator
      jsonPointers:
        - /webhooks/0/failurePolicy
values:
  enableCRDTemplates: true

runes: 
  - name: istio-discovery
    repository: 'https://github.com/istio/istio.git'
    path: manifests/charts/istio-control/istio-discovery
    revision: 1.23.0
    namespace: istio-system
    values:
      meshConfig:
        defaultConfig:
          proxyMetadata:
            ISTIO_META_DNS_CAPTURE: "true"
            ISTIO_META_DNS_AUTO_ALLOCATE: "true"
      global:
        hub: istio 
        tag: 1.23.0
        proxy:
          dnsRefreshRate: 5s
        proxy_env:
          ISTIO_META_DNS_CAPTURE: "true"
          ISTIO_META_PROXY_XDS_VIA_AGENT: "true"
      pilot:
        resources:
          requests:
            cpu: 100m
            memory: 256Mi

```

# then add your gateway

```yaml
name: my-istio-gw
repository: 'https://github.com/istio/istio.git'
path: manifests/charts/gateways/istio-ingress 
revision: 1.23.0
values:
  global:
    hub: istio
    tag: "1.23.0"
  gateways:
    istio-ingressgateway:
      name: my-istio-gw
      labels:
        app: my-istio-gw
        istio: my-istio-gw
      serviceAnnotations:
        external-dns.alpha.kubernetes.io/target: my.domain
        access: internal
      type: LoadBalancer
      ports:
        - port: 80
          targetPort: 8080
          name: http2
          protocol: TCP
        - port: 443
          targetPort: 8443
          name: https
          protocol: TCP

glyphs: # Kaster for gateway creation
  istio:
  - type: istio-gw
    annotations:
      external-dns.alpha.kubernetes.io/target: my.domain
    enabled: true
    hosts:
      - sub.my.domain
    istioSelector:
      istio: my-istio-gw
    name: my-istio-gw
    tls:
      enabled: True
      issuerName: mydomain-sub
    ports:
      - name: http
        port: 80
        protocol: HTTP
      - name: http
        port: 443
        protocol: HTTPS
  certManager: # cert manager implementation
    - type: certificate
      enabled: true
      name: mydomain-sub
      dnsNames:
        - sub.my.domain

```

now just add an app that implements istio

```yaml
name: nginx-test

image:
  name: nginx

istio:
  service:
    type: virtualService
    enabled: True
    host: ngnix-test
    httpRules:
      - prefix: /
        port: 80
    selector:
      access: external

```

note this implementation doesnt add sidecar and or mesh
