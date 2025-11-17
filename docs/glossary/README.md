# Kast Docs

This are the kast system docs, 

## How to setup a Kast based repository

Since kast is for Gitops, you need a `git` repo first! 
```bash
mkdir -p kasting/book/intro #Setup for repo structure, feel free to change repo, book and chapter name
cd kasting 
git init # start a git repo
git submodule add https://github.com/kast-spells/librarian.git librarian # add the librarian repo for kast
```
Now you have to add the `index.yaml` into `kasting/book/index.yaml`
```bash
cat > book/index.yaml<< EOF
name: book
kaster:
  repository: https://github.com/kast-spells/kast-system.git
  path: ./charts/kaster
  revision: master
summon:
  repository: https://github.com/kast-spells/kast-system.git
  path: ./charts/summon
  revision: master
chapters:
   - intro
EOF 
```

This setup the repo for kast, then, you have to add the Apps of Apps on your ArgoCD

```yaml
---
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: book # name of the book
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://yourgit.com/user/kast.git
    targetRevision: main
    path: librarian
  destination:
    server: https://kubernetes.default.svc
    namespace: argocd
  syncPolicy:
    retry:
      limit: 2
    syncOptions:
      - CreateNamespace=true
```

