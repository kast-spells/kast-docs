# kast-docs

Documentation repository for kast-system, automatically synced from [kast-spells/kast-system](https://github.com/kast-spells/kast-system).

**Live Documentation:** [docs.kast.ing](https://docs.kast.ing)

## Overview

This repository:
- Receives documentation from kast-system via automated sync workflow
- Deploys to GitHub Pages using MkDocs Material theme
- Updates automatically when `docs-*` tags are pushed to kast-system

## How It Works

1. Documentation is authored in `kast-system/docs/`
2. On tag push (`docs-*`), sync workflow copies `docs/` and `mkdocs.yml` to this repo
3. GitHub Actions builds and deploys MkDocs site to GitHub Pages
4. Site is available at https://docs.kast.ing

## Development

To contribute to documentation:
1. Edit files in [kast-system/docs/](https://github.com/kast-spells/kast-system/tree/master/docs)
2. Follow the [Contributing Docs](https://github.com/kast-spells/kast-system/blob/master/docs/CONTRIBUTING_DOCS.md) guide
3. Submit PR to kast-system (not this repo)

## Local Preview

```bash
# Clone kast-system repository
git clone https://github.com/kast-spells/kast-system.git
cd kast-system

# Install dependencies
pip install mkdocs-material mkdocs-git-revision-date-localized-plugin

# Serve locally
mkdocs serve

# Open http://127.0.0.1:8000
```

## Repository Structure

```
kast-docs/
├── docs/              # Documentation content (synced from kast-system)
├── mkdocs.yml         # MkDocs configuration (synced from kast-system)
├── .github/workflows/ # Deployment workflow
│   └── deploy-docs.yml
├── CNAME              # Custom domain configuration
└── README.md          # This file
```

## License

GNU GPL v3 - See [kast-system LICENSE](https://github.com/kast-spells/kast-system/blob/master/LICENSE)
