# Contributing

Thank you for your interest in contributing to Forus. This guide outlines the basic contribution workflow and focuses on the practical steps needed to start working in the repository.

## Before you start

- Read [docs/index.md](docs/index.md) for a quick overview of available documentation.
- Read [docs/local-setup.md](docs/local-setup.md) and get the project running locally.
- Read [docs/seeding-test-data.md](docs/seeding-test-data.md) to seed test data for local development and testing.
- Read [docs/login-and-test-users.md](docs/login-and-test-users.md) so you can log in.
- Security-related issues should not be reported in a public issue. Please follow the process in [backend/SECURITY.md](backend/SECURITY.md).

## Main repo and submodules

Forus has one main repository and two submodules:

- Main repo: `teamforus/Forus`
- Backend submodule: `backend/` -> `teamforus/forus-backend`
- Frontend submodule: `forus-frontend-react/` -> `teamforus/forus-frontend-react`

## Adding changes

Open your pull request in the repository that contains your changes:

- Changes in `docs/`, `README.md`, or `CONTRIBUTING.md` -> PR in `teamforus/Forus`
- Changes in `backend/*` -> PR in `teamforus/forus-backend`
- Changes in `forus-frontend-react/*` -> PR in `teamforus/forus-frontend-react`

Branch from the correct branch of that repository:

- In `teamforus/Forus`, branch from `master`
- In `teamforus/forus-backend`, branch from `develop`
- In `teamforus/forus-frontend-react`, branch from `develop`

### Branch naming

Use lowercase branch names with a type prefix and a short description:

- Format: `<type>/<short-description>` (for example `feature/new-login`)
- Use only lowercase letters, numbers, and hyphens in the description
- Do not use spaces, underscores, punctuation, repeated hyphens, or trailing hyphens

Recommended prefixes:

- `feature/` for new functionality
- `bugfix/` for non-critical fixes
- `hotfix/` for urgent production fixes
- `release/` for release related changes (for example `release/v174`)
- `docs/` for documentation related changes
- `deps/` for dependency updates (for example Composer or npm package updates)
- `test/` for test-only changes (for example adding or updating automated tests)

### Commit messages

Use short, clear commit messages that explain what changed.

- Format: `<type>: <short summary>` (for example `docs: clarify submodule contribution flow`)
- Use a commit type that matches the branch prefix where possible
- Keep the summary in lowercase and imperative style (for example `add`, `fix`, `update`, `remove`)
- Keep the first line focused and concise

## Making changes

- Keep changes focused. One concern per pull request.
- Before pushing, make sure your branch is up to date with the latest remote changes.
- Update or add tests when your change affects how something works.
  - Backend tests in `backend/tests/` help check that core logic and API behavior still work.
  - Frontend tests should follow existing patterns and help catch broken UI or interaction changes.

## Running checks locally

Before opening a pull request, run a few basic checks on your own machine. These are recommended checks that help catch obvious problems early.

Backend, from `backend/`:

```bash
php artisan test
```

This runs the backend tests and helps check that important application behavior still works.

Frontend, from `forus-frontend-react/`:

```bash
npm run build
```

This checks whether the frontend code can still be compiled successfully. It helps catch technical problems such as broken imports, syntax errors, or other build issues. It does not confirm that the interface works correctly in practice, so also test your changes manually when needed.

If you changed the database structure, also verify that the migration works and can be rolled back safely:

```bash
php artisan migrate
php artisan migrate:rollback
```

This helps confirm that your database change can be applied and undone without problems.

## Reporting issues

Use GitHub issues in this repository to report bugs, unexpected behavior, feature requests, documentation gaps, or other suggestions for improvement.

Include:

- What you expected to happen
- What actually happened
- Steps to reproduce the problem
- Your local setup (for example Docker or native), your operating system, and relevant PHP or Node versions

For anything security-sensitive, email `security@forus.io` instead.
