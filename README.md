# ai-chatbot-builder

> **Production AI chatbot starter — Next.js 15, NextAuth, tool-use, memory** — fork-ready Next.js chatbot with auth, chat history, tool use, document handling, voting, suggestions, multi-turn memory, and Playwright tests

<p align="center"><a href="https://github.com/hmzainjamil/ai-chatbot-builder">Repository</a> · <a href="https://github.com/hmzainjamil/ai-chatbot-builder/commits/main">Commits</a> · <a href="https://github.com/hmzainjamil/ai-chatbot-builder/issues">Issues</a></p>
<p align="center"><img alt="Visibility" src="https://img.shields.io/badge/visibility-public-blue"> <img alt="Documentation" src="https://img.shields.io/badge/documentation-deep%20editorial-lightgrey"> <img alt="Lifecycle" src="https://img.shields.io/badge/lifecycle-active-success"></p>

<!-- HMZ DEEP README v1 -->

## At a glance

| Field | Current state |
|---|---|
| Repository | ai-chatbot-builder |
| Visibility | Public |
| Lifecycle | Active |
| Evidence basis | Current repository documentation and source-visible material |

## Why this exists

**Production AI chatbot starter — Next.js 15, NextAuth, tool-use, memory** — fork-ready Next.js chatbot with auth, chat history, tool use, document handling, voting, suggestions, multi-turn memory, and Playwright tests

The README documents the builder workflow and separates generated application behavior from claims about production readiness, model quality, or deployment outcomes.

## 🧠 CONCEPTS

Each row maps a concept to a real file. Click `[Source]` to read the actual code.

| # | Concept | Location | Description |
|---|---|---|---|
| 1 | **Chat route** | `app/(chat)/api/chat/route.ts` | Core chat API — streams tokens, handles tool calls · [Source](https://github.com/hmzainjamil/ai-chatbot-builder/blob/main/app/(chat)/api/chat/route.ts) |
| 2 | **Chat schema** | `app/(chat)/api/chat/schema.ts` | Zod schemas for chat payloads · [Source](https://github.com/hmzainjamil/ai-chatbot-builder/blob/main/app/(chat)/api/chat/schema.ts) |
| 3 | **Chat stream** | `app/(chat)/api/chat/[id]/stream/route.ts` | SSE stream per-chat · [Source](https://github.com/hmzainjamil/ai-chatbot-builder/blob/main/app/(chat)/api/chat/[id]/stream/route.ts) |
| 4 | **Document route** | `app/(chat)/api/document/route.ts` | Document upload + retrieval · [Source](https://github.com/hmzainjamil/ai-chatbot-builder/blob/main/app/(chat)/api/document/route.ts) |
| 5 | **File upload** | `app/(chat)/api/files/upload/route.ts` | Direct file upload handler · [Source](https://github.com/hmzainjamil/ai-chatbot-builder/blob/main/app/(chat)/api/files/upload/route.ts) |
| 6 | **History route** | `app/(chat)/api/history/route.ts` | Conversation history fetch · [Source](https://github.com/hmzainjamil/ai-chatbot-builder/blob/main/app/(chat)/api/history/route.ts) |
| 7 | **Messages route** | `app/(chat)/api/messages/route.ts` | Per-chat message CRUD · [Source](https://github.com/hmzainjamil/ai-chatbot-builder/blob/main/app/(chat)/api/messages/route.ts) |
| 8 | **Vote route** | `app/(chat)/api/vote/route.ts` | Thumbs up/down for messages · [Source](https://github.com/hmzainjamil/ai-chatbot-builder/blob/main/app/(chat)/api/vote/route.ts) |
| 9 | **Suggestions route** | `app/(chat)/api/suggestions/route.ts` | Suggested next prompts · [Source](https://github.com/hmzainjamil/ai-chatbot-builder/blob/main/app/(chat)/api/suggestions/route.ts) |
| 10 | **Auth config** | `app/(auth)/auth.config.ts` | NextAuth provider config · [Source](https://github.com/hmzainjamil/ai-chatbot-builder/blob/main/app/(auth)/auth.config.ts) |

## ⚙️ HOW IT WORKS

```
┌─────────────────────────────────────────────────────────────┐
│  Input  →  ai-chatbot-builder  →  Output                                    │
├─────────────────────────────────────────────────────────────┤
│  1. Prompt / file / event lands at the entry point          │
│  2. Manifest resolves trigger → concrete handler            │
│  3. Handler invokes tools / scripts / sub-agents in order   │
│  4. Output is structured (JSON / Markdown / HTML / file)    │
│  5. Side-effects: logs, alerts, artifacts, commits          │
└─────────────────────────────────────────────────────────────┘
```

The architecture is intentionally narrow: one entry point, one router, deterministic handlers. No hidden global state, no `process.env` surprises, no daemons phoning home.

## 🚀 Install

## 🧩 Usage

Once installed, invoke the primary surface from any Claude Code session:

```text
# example 1 — basic trigger
use ai-chatbot-builder to ...

# example 2 — explicit skill name
@skill:ai-chatbot-builder run on <input>

# example 3 — CLI-style invocation
npx ai-chatbot-builder --help
```

Each concept in the table above is independently usable — you don't have to wire the whole thing up at once.

## ⚙️ Configuration

All configuration is file-based. No web dashboards, no SaaS sign-up, no env-var roulette.

| Setting | Default | Description |
|---|---|---|
| `LOG_LEVEL` | `info` | One of: `debug`, `info`, `warn`, `error` |
| `MODEL_TIER` | `tier0` | Route to free local/cloud models before paid |
| `MAX_TOKENS` | `8192` | Hard cap per invocation |
| `CACHE_TTL` | `3600` | Seconds before refetching upstream data |
| `OUTPUT_DIR` | `~/Downloads` | Where generated artifacts land |
| `DRY_RUN` | `false` | Print plan, skip side-effects |
| `RETRY_COUNT` | `3` | Network/transient failure retries |
| `TIMEOUT_MS` | `30000` | Per-call timeout |
| `TELEMETRY` | `off` | Never on by default |
| `VERBOSE_ERRORS` | `true` | Full stacks in dev, redacted in prod |

## Validation and evidence

Generated app quality and reliability claims require repeatable tests and inspectable outputs.

## 🛰️ Security posture

- Secrets: never committed; use a secrets manager (1Password CLI, doppler, age-encrypted .env).
- Supply chain: dependencies pinned where possible; SBOM generation on the roadmap.
- Sandbox: tools that touch the filesystem default to dry-run preview.
- Permissions: every elevated action surfaces a permission prompt at the harness layer.
- Audit log: every tool call appends to a structured log under `~/.claude/`.

## Limitations

- Model and framework behavior can change.
- Generated applications still need application-level testing and security review.
- Quantitative claims require reproducible evidence.



## Maintainer

[hmzainjamil](https://github.com/hmzainjamil)