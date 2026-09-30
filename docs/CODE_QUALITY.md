# Code Quality & Formatting Documentation Standard

> **Note:** Automated code quality checks are configured via `pre-commit` and run automatically on **`git commit`**. Manual execution commands are provided below as a fallback.

This project uses the Python ecosystem standard **Ruff** for unified linting, formatting, and import sorting, along with `conventional-pre-commit` for commit message validation.

## Environment Setup

To ensure linting and formatting are enforced locally:

```bash
# 1. Ensure dev dependencies are installed
pip install -e ".[dev]"

# 2. Install pre-commit hooks to your local git repository
pre-commit install

# 3. Install commit-msg hooks for conventional commit checks
pre-commit install --hook-type commit-msg
```

## Manual Execution Commands

### Run checks on ALL files repository-wide
```bash
pre-commit run --all-files
```

### Run checks ONLY on staged files
```bash
pre-commit run
```

### Isolated Tool Commands
If you want to run Ruff independently without invoking git hooks:

```bash
# Run Ruff Linter
ruff check .

# Run Ruff Linter and automatically fix issues
ruff check --fix .

# Run Ruff Formatter
ruff format .
```

## Maintenance & Cache

```bash
# Update hooks to the latest versions defined in .pre-commit-config.yaml
pre-commit autoupdate

# Clear pre-commit local cache if behaving unexpectedly
pre-commit clean
```

## Emergency Bypassing

If you absolutely must skip checks (e.g., in a critical hotfix), you can bypass git hooks using the `--no-verify` flag. **Use this responsibly.**

```bash
git commit -m "fix(core): emergency hotfix" --no-verify
```
