# Forus docs

Docs for running and contributing to Forus.

For branches, commits, and pull requests, see [CONTRIBUTING.md](../CONTRIBUTING.md).

## Getting started

1. [local-setup.md](local-setup.md) — run the backend and frontend.
2. [seeding-test-data.md](seeding-test-data.md) — seed organizations, funds, and test identities.
3. [login-and-test-users.md](login-and-test-users.md) — how login works locally, and the dev token shortcut.

## Documentation guidelines

Keep documentation focused on the work at hand. Start with a small, specific .md document when a feature or workflow needs explanation, then restructure later if several related pages grow into a larger topic. Avoid creating broad empty sections upfront.

### Folders
Use `features/` for product or platform behavior. Use `testing/` for developer workflows that explain how to verify behavior locally.

Use sentence case for headings: capitalize the first word and proper nouns only.

- Keep each page focused on one clear purpose.
- Prefer practical examples and commands over long theory.
- Use backticks for file paths, command names, and config keys.
- Update docs in the same PR when behavior changes.

## Feature docs

Documentation for specific features lives under `features/`. Add a new file here when a feature needs its own reference.

- [features/digest-emails.md](features/digest-emails.md) — how digest emails collect recent activity and choose singular or plural text.
- [features/translations.md](features/translations.md) — how webshop translations work, and how to change strings safely.

## Testing docs

Testing documentation lives under `testing/`. Add a new file here when a local verification workflow needs its own reference.

- [testing/email.md](testing/email.md) — how to test outgoing and digest emails locally.

## Related references

- [../CONTRIBUTING.md](../CONTRIBUTING.md) — minimal contribution flow.
- [../backend/readme-docker.md](../backend/readme-docker.md) — full backend Docker reference.
- [../forus-frontend-react/readme.md](../forus-frontend-react/readme.md) — frontend quick start.
- [../forus-frontend-react/readme-docker.md](../forus-frontend-react/readme-docker.md) — full frontend Docker reference.
- [../backend/SECURITY.md](../backend/SECURITY.md) — reporting vulnerabilities.
