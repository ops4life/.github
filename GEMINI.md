# ops4life Centralized Configuration Hub (.github)

This repository serves as the central orchestration point for the **ops4life** GitHub organization. It hosts shared GitHub Actions workflows, starter templates, and organizational branding.

## 🌟 Project Overview

- **Purpose:** Standardize CI/CD, security, and automation across all ops4life repositories.
- **Technologies:** GitHub Actions (YAML), JSON (Template metadata), SVG (Branding).
- **Core Components:**
  - **Starter Templates:** Located in `workflow-templates/`, these allow repositories to quickly adopt standard pipelines (Security, Linting, Auto-merge).
  - **Reusable Workflows:** Shared logic defined once and called by multiple repositories to reduce duplication.
  - **Organization Profile:** Managed in `profile/README.md`, providing the public landing page for the organization.

## 🛠️ Building and Running

Since this is a configuration-only repository, there are no traditional build or run steps.

- **Validation:** Ensure all YAML files are valid and follow the GitHub Actions schema.
- **Testing Templates:**
  1. Commit and push changes to this repository.
  2. Go to a different repository within the **ops4life** organization.
  3. Navigate to **Actions** -> **New workflow**.
  4. Your new template should appear under the "Workflows created by ops4life" section.
- **TODO:** Implement automated CI for this repository to validate YAML syntax and template metadata consistency.

## 📐 Development Conventions

### Repository Structure
- `workflow-templates/`: Every template MUST have a matching `.yml` and `.properties.json` file.
- `.github/workflows/`: Place reusable workflows here (must use `workflow_call`).
- `.github/actions/`: Place composite actions here.

### Coding Standards
- **Naming:** Use kebab-case for all file names (e.g., `gitleaks-scan.yml`).
- **Template Variables:** Always use `$default-branch` in starter templates instead of hardcoding `main` or `master`.
- **Permissions:** Follow the principle of least privilege in workflow `permissions` blocks.
- **Documentation:** Every new template or action should be added to the categorization tables in the main `README.md`.

### Contribution Guidelines
- **Conventional Commits:** All commits must follow the [Conventional Commits](https://www.conventionalcommits.org/) specification (e.g., `feat: add trivy scanning template`).
- **Review:** Changes to organization-wide workflows should be tested in a personal or sandbox repository before being merged here.
