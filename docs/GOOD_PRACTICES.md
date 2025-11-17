# Good Practices

This document outlines recommended practices for contributing to Kast documentation and working with the project.

## Documentation Writing

### Structure Your Content

- Start with an overview or introduction
- Break content into logical sections
- Use progressive disclosure (simple to complex)
- Include practical examples
- End with next steps or related resources

### Writing Style

- Write in clear, concise language
- Use active voice where possible
- Define technical terms on first use
- Be consistent with terminology
- Keep sentences and paragraphs focused

### Examples and Code Snippets

- Provide complete, working examples
- Show both the command and expected output
- Include error scenarios and troubleshooting
- Explain why, not just how
- Use realistic use cases

```bash
# Good: Complete example with context
# Deploy a sample application to test cert-manager
kubectl apply -f - <<EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
      - name: hello
        image: gcr.io/google-samples/hello-app:1.0
EOF
```

## Kubernetes Best Practices

### Resource Management

- Always specify resource requests and limits
- Use namespaces to organize resources
- Apply labels consistently for organization
- Use annotations for metadata and tooling
- Implement proper RBAC policies

### Configuration

- Store sensitive data in Secrets, not ConfigMaps
- Use external secret management (like Vault) for production
- Keep configuration separate from code
- Use ConfigMaps for non-sensitive configuration
- Version your configurations

### Deployment

- Use declarative configurations (YAML manifests)
- Implement health checks (liveness and readiness probes)
- Configure appropriate replica counts
- Use rolling updates for zero-downtime deployments
- Test in non-production environments first

## Git Workflow

### Branches

- Create feature branches from main
- Use descriptive branch names: `feature/add-vault-guide`, `fix/broken-link`
- Keep branches focused and short-lived
- Rebase on main before creating PRs
- Delete branches after merging

### Commits

- Make atomic commits (one logical change per commit)
- Write meaningful commit messages
- Reference issues when applicable
- Keep commits focused and reviewable
- Test before committing

### Pull Requests

- Provide clear description of changes
- Link related issues
- Request reviews from relevant team members
- Address review comments promptly
- Ensure CI checks pass before merging
- Squash commits if requested

## Testing Documentation

### Before Submitting

1. **Build locally**: Test the documentation build process
2. **Verify links**: Check all internal and external links work
3. **Test examples**: Run all code examples and commands
4. **Review rendering**: Check how content appears in rendered format
5. **Proofread**: Check spelling, grammar, and formatting

### Testing Commands

```bash
# Build documentation locally
mkdocs build --strict

# Serve documentation for preview
mkdocs serve

# Check for broken links (if tool available)
linkchecker http://localhost:8000
```

## Security Practices

### Sensitive Information

- Never commit secrets, passwords, or tokens
- Use placeholder values in examples
- Document proper secret management
- Review commits for accidental exposure
- Use `.gitignore` appropriately

### Examples

```yaml
# Bad: Hardcoded secret
apiVersion: v1
kind: Secret
metadata:
  name: database-credentials
stringData:
  password: "MyP@ssw0rd123"  # Never do this!

# Good: Reference to proper secret management
apiVersion: v1
kind: Secret
metadata:
  name: database-credentials
  annotations:
    vault.security.banzaicloud.io/vault-addr: "https://vault:8200"
    vault.security.banzaicloud.io/vault-role: "database"
    vault.security.banzaicloud.io/vault-path: "secret/data/database"
type: Opaque
```

## Collaboration

### Communication

- Be respectful and constructive
- Ask questions when unclear
- Share knowledge and context
- Document decisions and rationale
- Update documentation as code changes

### Code Review

- Review promptly
- Provide specific, actionable feedback
- Explain the "why" behind suggestions
- Acknowledge good work
- Be open to discussion

## Continuous Improvement

- Stay updated with Kubernetes ecosystem
- Learn from production issues
- Refactor and improve existing docs
- Gather feedback from users
- Keep dependencies updated
- Monitor documentation metrics

## Resources

- [Kubernetes Documentation Style Guide](https://kubernetes.io/docs/contribute/style/style-guide/)
- [Google Developer Documentation Style Guide](https://developers.google.com/style)
- [Write the Docs](https://www.writethedocs.org/)
- [The Documentation System](https://documentation.divio.com/)
