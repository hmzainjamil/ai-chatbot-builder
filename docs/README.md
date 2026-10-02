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

This repository has no dedicated architecture, threat model, privacy/data handling, deployment, release, or operations guide. The README only summarizes source-visible facts and makes no assurance claim. Write separate documents when owners can maintain accurate scope, evidence, and update triggers.

## Validation boundary

The package scripts define checks, but this documentation update did not run them. A test or workflow file alone does not establish a passing result.