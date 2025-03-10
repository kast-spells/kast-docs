# Book

A `book` is a standard structure in Kast designed to organize definitions. It consists
of a directory containing an `index.yaml` file, an optional `lexicon` folder, and one or more
directories, which are the `chapters`.

The purpose of a `book` is to manage default definitions across its `chapters`. This structure
allows for separating definitions into different `books` based on factors like clusters, development
stages, or infrastructure sections.

Within a `book`, the index file specifies which chapters will be included by default (to avoid deployment of resources of an on development chapter if autosync is enabled). Here, you can also set up defaults for both `Kaster` and `Summon`.

This is an example for `index.yaml`.

```yaml
name: my-book

chapters: # must have same name as the directory
  - intro # my basic CRDs and CNI 	
  - testing # testing infrastructure
  - Staging # Staging infrastructure
  - Production # Production intrastructure

kaster:
  repository: git@github.com:kast-spells/arcane-praxis.git # Default Kaster
  path: ./kaster
  revision: master

summon:
  repository: git@github.com:kast-spells/arcane-praxis.git # Default Summon
  path: ./summon
  revision: master
``` 

Directory tree should look like this.

```
my-kast-repository
├── bookrack
│   └── my-book # book directory
│       ├── _lexicon # lexicon directory
│       ├── index.yaml
│       ├── intro # chapters
│       └── ...
└── kast # ...

```
