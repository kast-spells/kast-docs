# Spell

The central concept of **Kast** revolves around the idea of a "spell," which serves as the foundational definition that set all the resources required to build and deploy an applications using ArgocD. Each spell is intricately structured to encapsulate everything needed for an application, ensuring seamless integration within your platform.

What makes spells particularly powerful is their ability to support multiple resources simultaneously, enabling the creation of versatile and robust platform-driven applications on a single yaml file. Additionally, Spells can be defined from jost Dockers without providing a chart via `Summon`.

Spells are designed to work with optional complementary elements like **runes** and **glyphs**, which serve as  extensions to core chart. For more details, refer to them in this glossary.

An example of a spell looks like this, It also includes examples of Glyphs (resources that are pre-defined in the kaster) and Runes (complementary charts like add-ons and CRD):

```yaml
name: app-name
repository: https://github.com/chart/repo.git
revision: v1.x
path: helm-chart
appParams:
  disableAutoSync: false
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
