# Rune

A rune is an independent spell that serves as a component within another spell. In some cases, creating a single app may require multiple charts, or certain charts might need additional resources like addons or CRD's for proper functionality. These specialized charts are where runes come into play they function independently but are specifically designed to complement the main chart without being an independent spell.

It's important to note that runes are not recommended for use in independent apps due to their specific requirements and limitations, such as not supporting glyphs or requiring additional resources beyond the main spell. Instead, they are primarily utilized within CRD addons, plugins, or supplementary charts that enhance the main application.

An example of a rune lookes like this.

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

runes: # Here is the rune delcaration
  - name: complementaryt-spell # just like a common spell
    repository: https://github.com/example/my-cool-crd.git
    revision: 0.0.9
    appParams:
      disableAutoSync: true
    path: crds/my-cool-crd
    values:
      group: cool-crd
      otherValue:
       myValue: overrides
        default:
          - values-file
```
