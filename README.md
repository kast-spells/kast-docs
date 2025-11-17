# Kast Documentation

[![Deploy Documentation](https://github.com/kast-spells/kast-docs/actions/workflows/deploy-docs.yml/badge.svg)](https://github.com/kast-spells/kast-docs/actions/workflows/deploy-docs.yml)

This repository contains the official documentation for the Kast project - a Kubernetes automation system that makes deploying and managing cloud-native applications simple and repeatable.

## Documentation Site

Visit our documentation at: [https://kast-spells.github.io/kast-docs/](https://kast-spells.github.io/kast-docs/)

## Repository Structure

```
kast-docs/
├── .github/
│   └── workflows/
│       ├── deploy-docs.yml    # MkDocs deployment (triggered on tags)
│       └── mdbook.yml          # mdBook deployment
├── docs/                       # MkDocs documentation source
│   ├── index.md               # Documentation home page
│   ├── CODING_STANDARDS.md    # Development standards
│   ├── GOOD_PRACTICES.md      # Best practices
│   ├── kasting_manual/        # Kast component guides
│   └── glossary/              # Terminology and definitions
├── kasting_manual/            # mdBook source for manual
│   └── src/
├── glossary/                  # mdBook source for glossary
│   └── src/
├── mkdocs.yml                 # MkDocs configuration
├── CNAME                      # Custom domain configuration
└── README.md                  # This file
```

## Building Documentation

### MkDocs (Primary)

The documentation is built using [MkDocs](https://www.mkdocs.org/) with the [Material theme](https://squidfunk.github.io/mkdocs-material/).

#### Prerequisites

```bash
pip install mkdocs-material mkdocs-autorefs
```

#### Build Commands

```bash
# Build the documentation
mkdocs build --strict

# Serve locally for development (with live reload)
mkdocs serve

# Deploy to GitHub Pages (manual)
mkdocs gh-deploy
```

The documentation will be available at `http://localhost:8000` when serving locally.

### mdBook (Legacy)

The repository also contains mdBook sources for backward compatibility.

```bash
# Install mdBook
cargo install mdbook

# Build glossary
mdbook build glossary

# Serve glossary
mdbook serve glossary
```

## Deployment

Documentation is automatically deployed to GitHub Pages when tags are pushed:

- Tags matching `v*` (e.g., `v1.0.0`, `v2.1.0`)
- Tags matching `docs-v*` (e.g., `docs-v1.0.0`)

To trigger a deployment:

```bash
git tag -a docs-v1.0.0 -m "Release documentation v1.0.0"
git push origin docs-v1.0.0
```

You can also manually trigger deployment via the Actions tab using the `workflow_dispatch` trigger.

## Contributing

We welcome contributions to improve the documentation! Please read our contribution guidelines:

- [Coding Standards](docs/CODING_STANDARDS.md)
- [Good Practices](docs/GOOD_PRACTICES.md)

### Quick Contribution Guide

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improve-vault-docs`)
3. Make your changes
4. Test locally with `mkdocs serve`
5. Commit with conventional commit format
6. Push and create a pull request

### Commit Message Format

We use conventional commits:

```
<type>(<scope>): <subject>

<body>
```

Types: `docs`, `feat`, `fix`, `style`, `refactor`, `test`, `chore`

Example:
```
docs(vault): add troubleshooting section

Add common troubleshooting steps for Vault integration issues.
```

## Documentation Structure

### Kasting Manual

Comprehensive guides for each Kast component:

- **Cert Manager**: Automated certificate management with Let's Encrypt
- **Vault**: Secrets management and encryption
- **Istio**: Service mesh configuration and best practices
- **External DNS**: Automated DNS record management

### Glossary

Definitions and explanations of Kast-specific terminology:

- Spell, Rune, Lexicon, Chapter
- Kast, Kaster, Book, Bookrack
- Glyph, Summon

## Support

- **Issues**: [GitHub Issues](https://github.com/kast-spells/kast-docs/issues)
- **Discussions**: [GitHub Discussions](https://github.com/kast-spells/kast-docs/discussions)

## License

This documentation is licensed under [MIT License](LICENSE).
