A rune is an independent spell that is requiren inside another spell, sometimes you need many charts to define just one service, or some charts require also charts that provdes CRD's or addons for the main chart, thats when you use a rune. that behaves just like a common spell but the limitations that doesn't allow using glyphs on it, so, is not recomended for services. just for CRD's addons and similar behaving charts.

An example of a rune lookes like this

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
