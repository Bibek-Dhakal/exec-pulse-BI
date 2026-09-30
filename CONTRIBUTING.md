# Contributing to ExecPulse-BI

First off, thank you for considering contributing to this project!

## 1. Code of Conduct
By participating in this project, you agree to abide by basic professional standards of conduct.

## 2. Setting Up for Development
1. Fork and clone the repository.
2. Set up your environment and dependencies:
   ```bash
   python -m venv venv
   source venv/bin/activate
   pip install -e ".[dev,notebooks]"
   ```
3. Install pre-commit hooks (MANDATORY):
   ```bash
   pre-commit install
   pre-commit install --hook-type commit-msg
   ```

## 3. Commit Guidelines (Strictly Enforced)
This project uses automated versioning and `CHANGELOG.md` generation via Google's `release-please`. Therefore, **Conventional Commits** are strictly required. 

Your commit messages must be structured as follows:
`<type>(<optional scope>): <description>`

### Allowed Types:
- `feat:` A new feature (correlates to a MINOR version bump).
- `fix:` A bug fix (correlates to a PATCH version bump).
- `docs:` Documentation only changes.
- `style:` Changes that do not affect the meaning of the code (white-space, formatting, etc).
- `refactor:` A code change that neither fixes a bug nor adds a feature.
- `perf:` A code change that improves performance.
- `test:` Adding missing tests or correcting existing tests.
- `chore:` Changes to the build process or auxiliary tools and libraries.

*Note: For a MAJOR version bump, append an `!` after the type/scope, e.g., `feat!: rewrite core modeling engine`.*

## 4. Pull Requests
1. Create a feature branch (`git checkout -b feat/your-feature`).
2. Commit your changes strictly following the conventional commits standard.
3. Push to your branch and open a Pull Request against `main`.
4. Ensure all CI checks and tests pass.