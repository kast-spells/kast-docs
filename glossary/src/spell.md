A spell is the definition of an app, it uses a Helm Chart to be defined, it belongs to an specific chapter, and can have multiple resources to work with it. it also can be defined as a **Summon** (reffer to summon in this glossary)

an example of a spell looks like this:

```yaml
name: app-name
repository: https://github.com/chart/repo.git
revision: v1.x
path: helm-chart
appParams:
  disableAutoSync: true
values: 
   some-vlue:
     to: override
   from:
    - default

runes:
  - name: complementaryt-spell
    repository: https://github.com/example/my-cool-crd.git
    revision: 0.0.9
    path: crds/my-cool-crd
    values:
      group: cool-crd
      otherValue:
       myValue: overrides
        default:
          - values-file
 
glyphs:
  glyph-name:
  - type: customResourseFromKaster
    specific: config

```

It also includes examples of Glyphs (resources that are pre-defined in the kaster) and Runes (complementary charts like add-ons and CRD).

Spells can be found inside chapters
