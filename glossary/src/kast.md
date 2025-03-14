# Kast

## What is Kast.

**Kast** is an open-source tool designed to streamline infrastructure management for platform development. It serves as a comprehensive set of standard directories and files to setup Cluster definitions.

Imagine **Kast** as a Librarian that checks for all the *Books* and its contents, **Kast** is a tool that uses Helm charts to deploy resources in ArgoCD following an **App of Apps** pattern. This allows you to orchestrate complex infrastructures by rendering them based on both chart and user defined values and configurations. **Kast** ensures that applications are deployed maintaining a clean and organized structure.

As an addition, **Kast** also offers standard resources and definitions via the `kaster`, which works similar to a package manager using common Helm charts that are imported from a repository to easily setup tools like **Istio**, **External DNS** or **Vault**, between others.

It also provides a set of standard resources to wrap the deployment of any docker container, that are taken from The `summon` repository, This feature enables users to wrap Docker containers with pre-standardized resources, simplifying deployment processes.

It allows you to standarize the whole whole platform by building Apps that are automatically managed by Argo CD from the **Kast** sub-repo with an **Apps of Apps** pattern. with both default and user defined Values, and resources from any Helm chart and/or from `kaster` and `summon` repositories.

This is managed by a library-like structure, with a `bookrack` full of `books` that contains `spells`, which are the definitions of an App that comes from a Helm chart and/or manifest.

**Kast** provides a structured environment where all infrastructure elements are organized, this allows the User to define a whole platform with its standard set of resources and CRDs in a simple, short, set of YAML files that follows a directory standard, setting the whole infrastructure as tidy, compact and manageable as posible, following a GitOps approach. **Kast** simplifies complex deployment processes with a simple and easily manageable YAML based structure. 

All this works by the **Kast** main chart, added as sub repository, which has all the Helm code that renders the desired infrastructure using the charts, repos and values given by the User, `kaster` and/or `summon`, in an Apps of Apps.

To how to setup a **Kast** managed repository reffer to [link to repo WIP]
