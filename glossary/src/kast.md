# Kast

## What is Kast.

Lets imagine Kast as a Librarian that checks for all the *Books* and its contents, Kast is a tool to manage Argocd Apps values, and that *Renders* new apps.

as an addition, Kast also offers standar resources and configurations similar to a package manager using common Helmcharts that are imported from the **Kaster**, and a set of standard resources to wrap the deploy any docker container, called **Summon**.

It allows you to standarize the whole deployment of apps by building metacharts that are automatically managed by Argo CD from the Kast sub-repo as an Apps of Apps. with both default and user defined Values from any helm chart and/or from Kaster or Summon.

This allows the User to define a whole platform with its standard set of resources and CRD in a simple, short, set of yaml file that follows a directory standard, setting the whole infrastructure as tidy, compact and manageable as posible, following a GitOps approach. 

All this works by the **Kast** main chart, added as sub repository, which has all the Helm code that renders the desired infrastructure using the charts, repos and values given by the User, Kaster and/or Summon.

To add Kast to your repository you have to add it as sub repository like this

```bash
git submodule add https://github.com/kast-spells/kast.git kast
```

and then apply the Apps of Apps definition just like this:

```bash
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-bookrack
  namespace: argocd
spec:
  project: default
  source:
    repoURL: git@github.com:me/my-bookrack.git
    targetRevision: main
    path: kast # dont change this!
    helm:
      parameters:
        - name: "name"
          value: my-first-book
  destination:
    server: https://kubernetes.default.svc
    namespace: argocd
  syncPolicy:
    retry:
      limit: 2
    syncOptions:
      - CreateNamespace=true
```
