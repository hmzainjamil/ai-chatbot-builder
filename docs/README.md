# Documentation index

The root README is the onboarding guide. The implementation and package files are the canonical references for runtime behavior and available commands.

## Product areas

- [Chat routes and pages](../app/)
- [AI models, providers, and tools](../lib/ai/)
- [Authentication routes](../app/%28auth%29/)
- [Database schema, migrations, and query helpers](../lib/db/)
- [Playwright tests](../tests/e2e/)
- [Example environment variable names](../.env.example)
- [Package scripts](../package.json)

## Documentation gaps

[Security and data handling](../SECURITY.md) documents the source-visible chat, upload, tool, provider, and storage boundaries. Separate architecture, threat model, deployment, release, and operations guides remain absent; the README and security guide make no assurance claim.

## Validation boundary

The package scripts define checks, but this documentation update did not run them. A test or workflow file alone does not establish a passing result.