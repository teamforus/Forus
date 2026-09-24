# Forus docs

Docs for running and contributing to Forus.

## Getting started

To run Forus locally for development or testing, follow these steps in order:

1. [local-setup.md](local-setup.md) — prepare your machine (editor, Git, Docker or native stack), clone the repo with submodules, then start the backend and frontend (Docker or native).
2. [seeding-test-data.md](seeding-test-data.md) — load system data plus a sample municipality, funds, providers, and identities so the website and admin panels are usable; the seeder prints a dev access token.
3. [login-and-test-users.md](login-and-test-users.md) — sign in locally via email link (or log file when mail is disabled) or paste the seeded access token when you want to skip mail.


## Contributing

If you want to contribute to Forus, read [CONTRIBUTING.md](../CONTRIBUTING.md) for the workflow: which repository to branch from, branch naming, commits, and pull requests. Use [glossary.md](glossary.md) for domain terms when you write or review documentation.

## Feature docs

Documentation for specific features lives under `features/`. These guides will explain how features work.

- [features/digest-emails.md](features/digest-emails.md) — how digest emails collect recent activity and choose singular or plural text.
- [features/translations.md](features/translations.md) — how webshop translations work, and how to change strings safely.

## Testing docs

Testing documentation lives under `testing/`.

- [testing/email.md](testing/email.md) — how to test outgoing and digest emails locally.

## Security

Do not report security or privacy issues in public issues. See [SECURITY.md](../SECURITY.md) for how to report vulnerabilities privately (email `security@forus.io`) and how Forus handles disclosure and fixes.
