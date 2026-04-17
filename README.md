# Data Institute Website

This is the website for OpenNews Data Institute, built with Jekyll and deployed via GitHub Actions to AWS S3.

## Table of Contents

- [Quick Start](#quick-start)
- [Local Development](#local-development)
  - [Prerequisites](#prerequisites)
  - [Core Commands](#core-commands)
  - [Testing & Validation](#testing--validation)
  - [Code Formatting](#code-formatting)
  - [Editor Setup (VSCode, Cursor, Antigravity, etc.)](#editor-setup-vscode-cursor-antigravity-etc)
- [Deployment](#deployment)
- [Repository Structure](#repository-structure)
- [Troubleshooting](#troubleshooting)

## Quick Start

1. **Install Dependencies:**
   ```bash
   bundle install
   ```

2. **Develop:** Run `jekyll serve` to preview locally at [http://localhost:4000](http://localhost:4000)

3. **Workflow:**
   - Make changes and test locally
   - Push to `staging` branch → auto-deploys to staging environment
   - Merge `staging` to `main` → auto-deploys to production

4. **Validate:** Run `bundle exec rake check` and `bundle exec rake test` before merging to `main`

## Local Development

### Prerequisites

This project requires Ruby and Bundler. Check if you have them installed:

```bash
ruby --version   # Should match .ruby-version
bundle --version # Should be 2.0 or higher
```

Install dependencies:

```bash
bundle install
```

### Core Commands

```bash
jekyll serve             # Serve locally with live reload at http://localhost:4000
bundle exec rake build   # Build the site to _site/
bundle exec rake clean   # Clean the build directory
bundle exec rake         # Run validate_yaml, check, build, and test
```

### Testing & Validation

```bash
bundle exec rake validate_yaml  # Validate YAML syntax and duplicate keys
bundle exec rake check          # Check _config.yml configuration
bundle exec rake test           # Test the built site
```

### Code Formatting

```bash
bundle exec rake lint  # Check Ruby code formatting with StandardRB
```

### Editor Setup (VSCode, Cursor, Antigravity, etc.)

If you're using VSCode or a VSCode-based editor, install these recommended extensions:

```bash
# StandardRB - Ruby linting and formatting
code --install-extension testdouble.vscode-standard-ruby
# OR use Ruby LSP (includes StandardRB plus autocomplete, go-to-definition)
code --install-extension shopify.ruby-lsp

# Red Hat YAML - validates YAML syntax and flags duplicate keys
code --install-extension redhat.vscode-yaml
```

Then reload your editor.

## Deployment

Deployment happens automatically via GitHub Actions:

- Push to `staging` → Staging environment (S3 only)
- Merge to `main` → Production (S3 + CloudFront invalidation)

AWS access is configured automatically via GitHub Actions using OpenID Connect (OIDC). No additional AWS credentials needed for automated deployments.

## Troubleshooting

### Common Issues

**Build won't start:**
```bash
bundle exec rake clean
bundle install
jekyll serve
```

**Changes not showing:**
- Hard refresh browser (Cmd/Ctrl+Shift+R)
- Clear Jekyll cache: `bundle exec rake clean`

**YAML errors:**
```bash
bundle exec rake validate_yaml
```

**Ruby version mismatch:**
Check `.ruby-version` and ensure your Ruby version matches.