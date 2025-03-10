# Bookrack

Bookrack forms the core structure of a Kast-managed repository. It features a directory named
`bookrack`, which acts as an identifier for where `books` are located within your infrastructure.

This directory contains multiple directories, each representing individual books—a collection of definitions that can be applied across your Kubernetes setup.

The primary purpose of Bookrack is to organize these books into distinct sections, such as
different platforms or deployment stages. This organization enhances flexibility and consistency
in managing diverse environments and workflows within your team's setup.

This is how the directory tree seem with the `bookrack`` 

```
my-bookrack # repo name
│
├── bookrack
│   └── ... # books!
└── kast # kast subrepo
```

Kast uses the name "bookrack" to identify the directory to work with,  so, it cannot be changed.
