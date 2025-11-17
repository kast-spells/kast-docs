# Summon

`summon` is a way to define a microservice with a standarized deployment approach, by inherits most of its definitions from its chapter/book `index.yaml` via it's `summon` and/or `kaster`, this allows to easily deploy any kind of app with an standarized chart suitable for platform needs.

With the `summon` you can implement any container as part of your platform definitions following the same standard 

An example of `summon` implementation. 
```yaml
name: nginx-summon # name can be any
image:
  name: nginx # this looks for nginx on dockerhub
   
istio: # this adds the istio glyph to the summon, here goes the glyph set name
  my-external-virtualservice: # any name
    type: virtualService
    enabled: True
    # subdomain: nginx # if you need subdomains
    host: nginx-test
    httpRules:
      - prefix: /a
        port: 80
    selector:
      access: external
```

To how to setup a summon, see [link to index]
