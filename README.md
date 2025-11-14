# .github

Organization-wide GitHub Actions resources for centralized CI/CD workflows and automation.

## Purpose

This repository provides:

1. **Workflow Templates** - Starter templates for new repositories
2. **Reusable Workflows** - Common CI/CD patterns callable from other repos
3. **Composite Actions** - Shared action definitions for repeated steps

## Available Workflow Templates

The following templates are available when creating new workflows in organization repositories:

### Security

- **Gitleaks** - Secret scanning to detect exposed credentials
- **CodeQL Analysis** - Automated vulnerability detection for JavaScript and Python
- **Dependency Review** - Security and license scanning for dependencies

### Code Quality

- **PR Title Linting** - Enforce conventional commit format for pull requests
- **Pre-commit CI** - Run pre-commit hooks and auto-commit fixes

### Automation

- **Auto-merge PRs** - Automatically merge pull requests from trusted bots (Dependabot, GitHub Actions)

## Usage

### Using Workflow Templates

1. Go to any repository in the organization
2. Navigate to **Actions** → **New workflow**
3. Find templates under "Workflows created by [organization]"
4. Click "Set up this workflow" to add it to your repository

### Calling Reusable Workflows

```yaml
jobs:
  call-workflow:
    uses: org-name/.github/.github/workflows/<workflow-name>.yml@main
    with:
      input-name: value
    secrets:
      secret-name: ${{ secrets.SECRET_NAME }}
```

### Using Composite Actions

```yaml
steps:
  - uses: org-name/.github/.github/actions/<action-name>@main
    with:
      input-name: value
```

## Contributing

When adding new templates or workflows:

1. Follow the directory structure outlined in `CLAUDE.md`
2. Include both `.yml` and `.properties.json` files for templates
3. Use `$default-branch` template variable for branch references
4. Test in a repository before committing
5. Use conventional commits for changes (feat, fix, docs, etc.)

## License

MIT License - See [LICENSE](LICENSE) for details