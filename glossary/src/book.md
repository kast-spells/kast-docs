The book is one of the standar structures that Kast offer as solution, A book is a set of Chapters, the idea of a book is to manage the defaults of all the chapters inside it, a book can be a cluster which you want to keep separated from other ones configuration, or a group of setting that you need to manage independintly to another group of settings, it counts with an index, Here you setup the chapters that going to be read by the book (a book doesn't adds a chapter by default, this is to make easy develop without deploying anything in case of autosync enabled),here is also where you can setup default **Kaster** and **Summon** (usually the Kast one), This is an example of Book `index.yaml`.

```yaml
name: my-book

chapters:
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
│   └── my--book
└── kast # ...

```
