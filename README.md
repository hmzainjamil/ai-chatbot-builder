# AI Chatbot Builder

A Next.js chat application with authentication, streamed conversations, model selection, document and file flows, chat history, suggestions, and voting. The checked-in code is an application starter; this README does not establish production readiness.

> **Status:** Source structure and package scripts inspected. Runtime behavior, deployment, provider availability, security, and test results were not verified in this update.

## What is in the repository

| Area | Path | Source evidence |
|---|---|---|
| Next.js app and chat UI | `app/` | Pages and API route handlers |
| Chat, models, and tools | `lib/ai/` | Model list, provider setup, prompt code, and document/weather/suggestion tools |
| Database | `lib/db/` | Drizzle schema, query helpers, and migrations |
| Authentication | `app/(auth)/`, `proxy.ts` | Login, registration, guest flow, and auth route |
| Browser tests | `tests/e2e/` | Playwright test files; not run here |
| Environment example | `.env.example` | Variable names for auth, AI, blob, database, and cache configuration |

Package metadata declares Next.js 16.2.0, React 19.0.1, and pnpm 10.32.1. Provider/model availability depends on valid configuration and external services.

## Local development

Use Node.js compatible with the checked-in Next.js and pnpm versions. Configure the values required by your selected database, model, and storage features.

```sh
pnpm install
cp .env.example .env.local
# Set required values in .env.local
pnpm db:migrate
pnpm dev
```

The commands come from the package scripts and lockfile; they were not executed for this update. Use a disposable development database. The `build` script runs the migration command before `next build`, so it can change the configured database.

## Validation commands

```sh
pnpm check
pnpm test
pnpm build
```

These are available package scripts, not reported results. The test script invokes Playwright. No validation was run in this documentation update.

## Data and security

- Chat content and uploaded files may contain sensitive user data.
- Model requests may send prompts and related context to configured external providers. Review the provider, account, and retention settings before use.
- Keep `AUTH_SECRET`, AI gateway credentials, blob tokens, database URLs, and Redis URLs out of source control and issue reports.
- Review authentication, authorization, file upload handling, database access, rate limits, and provider data flows before public deployment.
- This README makes no security, privacy, compliance, or production-readiness certification.

## Documentation and source map

- [Documentation index](docs/README.md)
- [Chat API route](app/(chat)/api/chat/route.ts)
- [File upload route](app/(chat)/api/files/upload/route.ts)
- [Document route](app/(chat)/api/document/route.ts)
- [Model configuration](lib/ai/models.ts)
- [Database schema](lib/db/schema.ts)
- [Environment variable names](.env.example)
- [License](LICENSE)

## License

The root [LICENSE](LICENSE) is Apache License 2.0. See its terms for use and distribution.