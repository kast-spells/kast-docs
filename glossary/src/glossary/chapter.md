# Chapter

A chapter is a directory located within the book directory. Each chapter contains both a set of spells, which define the core functionality and resources required, as well as an optional `index.yaml` file that serves to customize default Summons and/or kaster settings, a common example of `chapter` usage is an "intro" chapter that defines all the CRDs, Storage Classes, and Network Stack.

The inclusion of this index file allows for flexibility in managing different deployment stages without altering the book's overall configuration. The `index.yaml` file is structured similarly to a chapter but excludes the listing of all chapters within it.
`
An example of a chapter overriding the book kaster and summon with the default ones:

```yaml
kaster:
  repository: git@github.com:kast-spells/arcane-praxis.git # Default Kaster
  path: ./kaster
  revision: master

summon:
  repository: git@github.com:kast-spells/arcane-praxis.git # Default Summon
  path: ./summon
  revision: master
```
