# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This repository provides centralized GitHub Actions resources for the organization:

1. **Reusable workflows** - Common CI/CD patterns that can be called from other workflows
2. **Starter workflow templates** - Templates available when creating new workflows in repositories
3. **Shared composite actions** - Reusable action definitions for repeated step sequences

## Repository Structure

Currently minimal:
- `LICENSE` - MIT License
- `README.md` - Basic repository description

Expected directory structure:
- `workflow-templates/` - Starter templates for new repositories
- `.github/workflows/` - Reusable workflows that can be called from other repos
- `.github/actions/` - Composite actions for shared step sequences

## Component Structure

### 1. Starter Workflow Templates (`workflow-templates/`)

Each workflow template consists of:

- **Workflow YAML file**: `workflow-templates/<template-name>.yml`
  - The actual GitHub Actions workflow definition
  - Can include template variables like `$default-branch`

- **Properties file**: `workflow-templates/<template-name>.properties.json`
  - Metadata for the template (name, description, icon, categories)
  - Example:
  ```json
  {
    "name": "Template Name",
    "description": "Template description",
    "iconName": "example-icon",
    "categories": ["Automation", "CI"]
  }
  ```

- **Icon (optional)**: `workflow-templates/<icon-name>.svg`
  - SVG icon displayed in the GitHub UI

### 2. Reusable Workflows (`.github/workflows/`)

Reusable workflows are called from other repositories:

- Must include `workflow_call` trigger
- Can define inputs and outputs
- Called using: `uses: org-name/.github/.github/workflows/<workflow-name>.yml@main`
- Example structure:
  ```yaml
  on:
    workflow_call:
      inputs:
        environment:
          required: true
          type: string
      secrets:
        token:
          required: true
  ```

### 3. Composite Actions (`.github/actions/`)

Each composite action is a directory containing:

- `action.yml` - Action definition with inputs, outputs, and steps
- Supporting scripts or files as needed
- Called using: `uses: org-name/.github/.github/actions/<action-name>@main`
- Example structure:
  ```yaml
  name: 'Action Name'
  description: 'Action description'
  inputs:
    input-name:
      description: 'Input description'
      required: true
  runs:
    using: 'composite'
    steps:
      - run: echo "Action steps here"
        shell: bash
  ```

## Common Tasks

### Creating a new workflow template

```bash
mkdir -p workflow-templates
# Create workflow-templates/<template-name>.yml
# Create workflow-templates/<template-name>.properties.json
git add workflow-templates/
git commit -m "Add <template-name> workflow template"
git push
```

**Testing:** Go to any org repository → Actions → New workflow → Look for template in "Workflows created by [organization]"

### Creating a reusable workflow

```bash
mkdir -p .github/workflows
# Create .github/workflows/<workflow-name>.yml with workflow_call trigger
git add .github/workflows/<workflow-name>.yml
git commit -m "Add <workflow-name> reusable workflow"
git push
```

**Using in other repositories:**
```yaml
jobs:
  call-workflow:
    uses: org-name/.github/.github/workflows/<workflow-name>.yml@main
    with:
      input-name: value
    secrets:
      secret-name: ${{ secrets.SECRET_NAME }}
```

### Creating a composite action

```bash
mkdir -p .github/actions/<action-name>
# Create .github/actions/<action-name>/action.yml
git add .github/actions/<action-name>/
git commit -m "Add <action-name> composite action"
git push
```

**Using in workflows:**
```yaml
steps:
  - uses: org-name/.github/.github/actions/<action-name>@main
    with:
      input-name: value
```

## Important Notes

**Workflow Templates:**
- Organization-wide, appear when creating new workflows in any org repository
- Changes only affect new workflows created after the change

**Reusable Workflows:**
- Must be in a public repo or the calling repo must have access
- Called workflows run in the context of the caller
- Can be pinned to specific refs (tags, commits, or branches)

**Composite Actions:**
- Run in the same job as the caller
- Lighter weight than reusable workflows
- Best for grouping common steps that don't need a separate job

**Versioning:**
- Use `@main` for latest version (auto-updates)
- Use `@v1` tags for stable versions
- Use `@<commit-sha>` for immutable versions

## Common Workflow Patterns

Based on organizational best practices, common workflows include:

**Security:**
- `gitleaks.yaml` - Secret scanning with Gitleaks
- `codeql.yaml` - Code vulnerability analysis
- `deps-review.yaml` - Dependency security review

**Code Quality:**
- `lint-pr.yaml` - PR title/commit message linting
- `pre-commit-ci.yaml` - Pre-commit hooks enforcement
- Linting workflows (YAML, Markdown, etc.)

**Automation:**
- `automerge.yml` - Auto-merge for dependency updates
- `stale.yaml` - Mark/close stale issues and PRs
- `cleanup-caches.yaml` - Clean up old workflow caches
- `pre-commit-auto-update.yaml` - Keep pre-commit hooks updated

**Release & Sync:**
- `release.yaml` - Semantic versioning and changelog generation
- `template-repo-sync.yaml` - Sync changes from template repo

**Maintenance:**
- `update-license.yml` - Keep license files current

## Best Practices

**Commit Convention:** Use conventional commits (feat, fix, docs, style, refactor, test, chore) for semantic versioning

**Security:** Include secret scanning and dependency review in reusable workflows

**Versioning:** Tag stable versions (v1, v2) and update tags on releases for easy consumption

**No build, lint, or test commands are currently configured for this repository**
