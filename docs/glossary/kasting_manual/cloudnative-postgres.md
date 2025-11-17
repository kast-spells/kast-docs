# wip

to add cloudnative postgres to your clusted add this manifest in desired chapter
```yaml
name: cloudnative-pg
repository: ghcr.io/cloudnative-pg/charts
chart: cloudnative-pg
revision: 0.23.2
appParams:
  annotations:
    argocd.argoproj.io/sync-wave: "1"
  syncPolicy:
    syncOptions:
    - ServerSideApply=true
    - CreateNamespace=true
    - PrunePropagationPolicy=foreground
    - PruneLast=true
```
