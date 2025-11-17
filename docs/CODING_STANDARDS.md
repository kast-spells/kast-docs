# Coding Standards

This document outlines the coding standards for the Kast project documentation and code examples.

## General Principles

- **Clarity**: Code should be self-documenting and easy to understand
- **Consistency**: Follow established patterns throughout the codebase
- **Maintainability**: Write code that is easy to modify and extend
- **Documentation**: Document complex logic and architectural decisions

## Documentation Standards

### Markdown Files

- Use ATX-style headers (`#` prefix)
- Include a table of contents for documents longer than 3 sections
- Use code blocks with language specifiers for syntax highlighting
- Keep line length reasonable (80-120 characters where practical)
- Use relative links for internal documentation references

### Code Examples

All code examples in documentation should:

- Be tested and verified to work
- Include necessary imports and dependencies
- Use meaningful variable and function names
- Include inline comments for complex operations
- Follow language-specific conventions

## YAML Standards

For Kubernetes manifests and configuration files:

```yaml
# Use 2 spaces for indentation
apiVersion: v1
kind: ConfigMap
metadata:
  name: example-config
  namespace: default
  labels:
    app: example
data:
  key: value
```

### Best Practices

- Always specify `apiVersion` and `kind`
- Include meaningful names and labels
- Use namespaces explicitly
- Add comments for non-obvious configurations
- Organize resources logically within files

## Git Commit Standards

Follow conventional commit format:

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types

- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Formatting changes
- `refactor`: Code refactoring
- `test`: Test additions or modifications
- `chore`: Build process or auxiliary tool changes

### Examples

```
docs(vault): add secret management guide

Add comprehensive guide for managing secrets with Vault including
setup instructions and common use cases.
```

```
fix(certmanager): correct issuer configuration

Update ClusterIssuer example to use correct solver configuration
for Let's Encrypt HTTP01 challenge.
```

## File Organization

- Keep related files grouped together
- Use clear, descriptive file names
- Follow consistent naming conventions:
  - Markdown files: `lowercase-with-hyphens.md`
  - YAML files: `lowercase-with-hyphens.yaml`
  - Directories: `lowercase_with_underscores`

## Review Guidelines

Before submitting changes:

1. Verify all code examples work as documented
2. Check spelling and grammar
3. Ensure links are valid and working
4. Test documentation builds locally
5. Review changes in rendered format
6. Ensure compliance with these standards
