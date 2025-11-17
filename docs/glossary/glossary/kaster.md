# Kaster
The Kaster is one of the more useful parts of **Kast**,serving as a repository for preconfigured resource definitions, in the book and/or chapter `index.yaml` you can define a `kaster` repo, which contains preconfigured CRDs you can call via Glyphs. it works like a kind of Package manager for resource definitions, to learn more on how to use default `kaster` and understanding how its resources are setup, refer to Kaster Handbook(link to kaster handbook).

Kaster resource definition are managed by `glyphs` (which are invocations of Kaster's resource charts) and the `lexicon`, that defines shared configuration dependencies to configurate between glyphs, for more information refer to **Glyphs** and **Lexicon** in this Glossary.

You can make your own Kaster if you want or modify the one on the default Kaster repository to your own needs.

it is refered on book and/or chapters index like this:

```yaml
# ...
kaster:
  repository: git@github.com:kast-spells/arcane-praxis.git # default Kaster
  path: ./kaster
  revision: master
# ...
```
