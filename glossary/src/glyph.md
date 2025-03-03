# Glyph

A glyph is an specific resources that comes from a `Kaster`, a preset resource with its own configuration and usage, pretty simmilar to a package (since the **Kaster** is kind of package manager), that has both specific configurations (defined on the glyph) and general configurations (set up on **Lexicon**), we recommend to see the implementation of a glyph usage on Kaster Handbook [link required].

An example from the istio gateway glyph

```yaml
# ...
glyphs: # set of glyphs
  istio: # glyph name (can be anything)
  - type: virtualService # type defining it.
    enabled: True
    subdomain: app
    host: service-name
    httpRules:
      - prefix: /
        port: 80

# ...
```
