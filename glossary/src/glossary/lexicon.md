> What is a namen

# Lexicon.

The lexicon is part of the Kaster, is a set of *configuration dependecies* from an specific **Glyph**, some **Glyph** depends on common configuration to work, so, they are defined in the **lexicon** and chosen via selectors, to understand more about the lexicon, you can see an implementation of it on Kaster the documentation [Link required], Kaster Handbook tells you when you need a lexicon for an specific resource.

Lexicon directory is defined inside a `book` with the name `_lexicon`, and its contents can be named however you want since are selected via selector.

```
my-bookrack
├── bookrack
│   └── my-first-book
│       ├── _lexicon
│       │    └── my-config.yaml
│       └── # ...
└── kast # ...
```


