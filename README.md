# ai-chatbot-builder

> **Open-source Next.js chatbot template — multi-model, tool-use, auth, and persistent history out of the box**

<p align="center">
  <img src="https://img.shields.io/github/stars/hmzainjamil/ai-chatbot-builder?style=for-the-badge&color=FFD700&labelColor=222" alt="Stars"/>
  <img src="https://img.shields.io/github/forks/hmzainjamil/ai-chatbot-builder?style=for-the-badge&color=00BFFF&labelColor=222" alt="Forks"/>
  <img src="https://img.shields.io/github/issues/hmzainjamil/ai-chatbot-builder?style=for-the-badge&color=FF4500&labelColor=222" alt="Issues"/>
  <img src="https://img.shields.io/github/issues-pr/hmzainjamil/ai-chatbot-builder?style=for-the-badge&color=9B59B6&labelColor=222" alt="PRs"/>
  <img src="https://img.shields.io/github/last-commit/hmzainjamil/ai-chatbot-builder?style=for-the-badge&color=2ECC71&labelColor=222" alt="Last Commit"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Next.js-000000?style=flat&labelColor=555&logo=nextdotjs&logoColor=white" alt="Next.js"/>
  <img src="https://img.shields.io/badge/Vercel_AI_SDK-000000?style=flat&labelColor=555" alt="Vercel AI SDK"/>
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=flat&labelColor=555&logo=typescript&logoColor=white" alt="TypeScript"/>
  <img src="https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=flat&labelColor=555&logo=tailwindcss&logoColor=white" alt="Tailwind"/>
  <img src="https://img.shields.io/badge/shadcn%2Fui-000000?style=flat&labelColor=555" alt="shadcn/ui"/>
  <img src="https://img.shields.io/badge/Neon_Postgres-00E5FF?style=flat&labelColor=555" alt="Neon"/>
  <img src="https://img.shields.io/badge/Auth.js-blueviolet?style=flat&labelColor=555" alt="Auth.js"/>
</p>

---

## Why This Exists

Building a production-grade AI chatbot from scratch takes weeks: streaming responses, multi-turn memory, auth, DB persistence, multi-model support, file attachments, error handling. This template ships all of that pre-wired. Fork it, add your system prompt, deploy to Vercel in minutes.

This is the same stack used at [chatbot.ai-sdk.dev](https://chatbot.ai-sdk.dev) — not a toy demo.

---

## At a Glance

| Feature | Status | Details |
|---------|--------|---------|
| Streaming responses | ✅ | Real-time token streaming via AI SDK |
| Multi-model support | ✅ | OpenAI, Anthropic, Google, Mistral, DeepSeek, xAI |
| Tool use / function calling | ✅ | Structured tool calls with schema validation |
| Persistent chat history | ✅ | Neon Serverless Postgres |
| File storage | ✅ | Vercel Blob |
| Authentication | ✅ | Auth.js (email, OAuth providers) |
| Vercel AI Gateway | ✅ | Unified API for all model providers |
| React Server Components | ✅ | Next.js App Router architecture |
| shadcn/ui design system | ✅ | Accessible, customizable components |
| One-click Vercel deploy | ✅ | Template registered on Vercel marketplace |
| TypeScript | ✅ | Full type safety end-to-end |

---

## 🧠 CONCEPTS

| Concept | Explanation |
|---------|-------------|
| **AI SDK** | Vercel's unified LLM library — `useChat`, `streamText`, tool schemas |
| **Vercel AI Gateway** | Proxy that routes to any model provider with one API key |
| **RSC (React Server Components)** | Server-rendered components — initial chat history loads server-side |
| **Server Actions** | Next.js async functions that run server-side — used for DB writes |
| **useChat hook** | AI SDK hook — manages messages, loading state, streaming in one line |
| **streamText** | AI SDK server function — returns a streaming response object |
| **Tool call** | Model returns a structured JSON object → app executes function → result fed back |
| **Neon Serverless** | Postgres that auto-scales to zero — no idle DB cost |
| **Vercel Blob** | File storage for attachments — S3-compatible, edge-fast |
| **Auth.js** | Authentication library — session management, OAuth, email magic links |

### 🔥 Hot

| Feature | Why Developers Fork This | Source |
|---------|--------------------------|--------|
| Multi-model per-conversation | Switch models mid-conversation — compare Sonnet vs GPT in same UI | [HMZ](https://github.com/hmzainjamil) |
| Tool use out of box | Calculator, web search, code execution — just add tool definitions | [HMZ](https://github.com/hmzainjamil) |
| Neon Postgres persistence | Full chat history with zero DB ops — scales to zero when idle | [HMZ](https://github.com/hmzainjamil) |

---

## ⚙️ HOW IT WORKS

```
User types message in React UI
  → useChat hook sends POST to /api/chat
       → Server Action calls streamText(model, messages, tools)
            → Vercel AI Gateway routes to selected model
                 → Streaming tokens return via ReadableStream
                      → useChat appends tokens to UI in real-time
                           → On completion: history saved to Neon Postgres
```

**Tool calls interrupt the stream:**

```
Model returns { tool_call: { name: "search", args: { query: "..." } } }
  → App executes search function
       → Result appended to messages
            → Model continues with search results
                 → Stream resumes
```

**Auth flow:**

```
User visits /chat
  → Auth.js checks session cookie
       → No session → redirect to /login
            → Email magic link or OAuth
                 → Session created → /chat accessible
```

---

## 🚀 INSTALL

### Option A: One-click Vercel deploy

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/templates/next.js/chatbot)

Vercel auto-provisions Neon Postgres + Blob storage and sets env vars.

### Option B: Local development

```bash
# Clone
git clone https://github.com/hmzainjamil/ai-chatbot-builder
cd ai-chatbot-builder

# Install deps
pnpm install

# Set up environment
cp .env.example .env.local
# Edit .env.local with your keys (see Configuration section)

# Link to Vercel (recommended for env var management)
npm i -g vercel
vercel link
vercel env pull

# Run DB migrations
pnpm db:migrate

# Start dev server
pnpm dev
```

App runs at [localhost:3000](http://localhost:3000).

---

## 📟 USAGE

### Basic chat

```bash
# Start the dev server
pnpm dev

# Open http://localhost:3000
# Sign in with email or OAuth
# Start chatting
```

### Switch models

```typescript
// lib/ai/models.ts — add or remove models
export const models = [
  { id: 'claude-sonnet-4-6', provider: 'anthropic', label: 'Claude Sonnet 4.6' },
  { id: 'gpt-4o', provider: 'openai', label: 'GPT-4o' },
  { id: 'gemini-2.0-flash', provider: 'google', label: 'Gemini 2.0 Flash' },
  // Add any model supported by Vercel AI Gateway
]
```

### Add a tool

```typescript
// app/api/chat/route.ts
const result = await streamText({
  model,
  messages,
  tools: {
    getWeather: {
      description: 'Get current weather for a city',
      parameters: z.object({ city: z.string() }),
      execute: async ({ city }) => fetchWeather(city),
    },
  },
})
```

### Customize system prompt

```typescript
// app/api/chat/route.ts
const result = await streamText({
  model,
  system: 'You are a helpful assistant for Acme Corp...',
  messages,
})
```

---

## ⚙️ CONFIGURATION

| Variable | Required | Description | Where to Get |
|----------|----------|-------------|-------------|
| `AI_GATEWAY_API_KEY` | ✅ | Vercel AI Gateway key | vercel.com/dashboard → AI |
| `AUTH_SECRET` | ✅ | NextAuth secret | `openssl rand -base64 32` |
| `DATABASE_URL` | ✅ | Neon Postgres connection string | neon.tech |
| `BLOB_READ_WRITE_TOKEN` | ✅ | Vercel Blob token | vercel.com → Storage |
| `OPENAI_API_KEY` | Optional | Direct OpenAI (bypass Gateway) | platform.openai.com |
| `ANTHROPIC_API_KEY` | Optional | Direct Anthropic (bypass Gateway) | console.anthropic.com |
| `GOOGLE_CLIENT_ID` | Optional | Google OAuth | console.cloud.google.com |
| `GOOGLE_CLIENT_SECRET` | Optional | Google OAuth | console.cloud.google.com |
| `AUTH_RESEND_KEY` | Optional | Email magic links via Resend | resend.com |
| `NEXT_PUBLIC_APP_URL` | Optional | Canonical URL for OAuth redirects | Your domain |

---

## 💡 TIPS AND TRICKS

### Performance
| Tip | Detail | Source |
|-----|--------|--------|
| Use RSC for history | Load conversation history server-side — no loading spinner | [HMZ](https://github.com/hmzainjamil) |
| Stream immediately | Don't wait for full response — UI feels 10x faster | [HMZ](https://github.com/hmzainjamil) |
| Cache model list | `models.ts` runs on every request — add `export const revalidate = 3600` | [HMZ](https://github.com/hmzainjamil) |

### Multi-model
| Tip | Detail | Source |
|-----|--------|--------|
| Route by task type | Code → Claude, reasoning → o1, quick → Flash — add model selector per message | [HMZ](https://github.com/hmzainjamil) |
| Per-user model default | Store preferred model in user table — fetch at session start | [HMZ](https://github.com/hmzainjamil) |
| Cost guard | Add middleware that blocks expensive models for free tier users | [HMZ](https://github.com/hmzainjamil) |

### Tools
| Tip | Detail | Source |
|-----|--------|--------|
| Always add confirmation step | Before destructive tool calls, ask user to confirm | [HMZ](https://github.com/hmzainjamil) |
| Validate tool results | Wrap tool execute functions in try/catch, return error string | [HMZ](https://github.com/hmzainjamil) |
| Limit tool count | >5 tools = model gets confused — use tool routing instead | [HMZ](https://github.com/hmzainjamil) |

### Deployment
| Tip | Detail | Source |
|-----|--------|--------|
| Use Vercel for zero-config | Edge runtime + Neon + Blob all connect automatically | [HMZ](https://github.com/hmzainjamil) |
| Set rate limits | Add Vercel Edge middleware to rate-limit by user ID | [HMZ](https://github.com/hmzainjamil) |
| Enable Vercel Analytics | Free — shows real user latency broken down by model | [HMZ](https://github.com/hmzainjamil) |

---

## 🔧 TROUBLESHOOTING

| Issue | Cause | Fix |
|-------|-------|-----|
| Streaming not working | Missing `edge` runtime config | Add `export const runtime = 'edge'` to route.ts |
| DB connection error | Wrong DATABASE_URL format | Neon requires `?sslmode=require` suffix |
| Auth redirect loop | Missing AUTH_SECRET | Set `AUTH_SECRET` in .env.local |
| Models not loading | AI Gateway key missing | Set `AI_GATEWAY_API_KEY` |
| File upload fails | Blob token missing | Set `BLOB_READ_WRITE_TOKEN` |
| OAuth callback error | Wrong redirect URI | Add `http://localhost:3000/api/auth/callback/google` to Google console |
| Tool call infinite loop | Missing `maxSteps` limit | Add `maxSteps: 5` to streamText config |
| History not persisting | Migration not run | `pnpm db:migrate` |

---

## 📊 ARCHITECTURE

```
ai-chatbot-builder/
├── app/
│   ├── (auth)/          ← login, register pages
│   ├── (chat)/          ← main chat UI (RSC)
│   │   ├── page.tsx     ← server component, loads history
│   │   └── chat.tsx     ← client component, useChat hook
│   └── api/
│       ├── chat/        ← POST handler, streamText
│       └── auth/        ← Auth.js handler
├── lib/
│   ├── ai/
│   │   ├── models.ts    ← model registry
│   │   └── tools.ts     ← tool definitions
│   ├── db/
│   │   ├── schema.ts    ← Drizzle ORM schema
│   │   └── queries.ts   ← typed DB queries
│   └── auth.ts          ← Auth.js config
├── components/
│   └── ui/              ← shadcn/ui components
└── .env.example
```

---

## 🗺️ ROADMAP

| Status | Feature | ETA |
|--------|---------|-----|
| ✅ Done | Multi-model via Vercel AI Gateway | Shipped |
| ✅ Done | Neon Postgres persistence | Shipped |
| ✅ Done | File attachments via Vercel Blob | Shipped |
| 🔄 In progress | RAG with vector search | Jun 2026 |
| 📋 Planned | Artifact rendering (code, HTML) | Jun 2026 |
| 📋 Planned | Voice input/output | Jul 2026 |
| 📋 Planned | Shareable conversation links | Jul 2026 |
| 📋 Planned | Admin dashboard with usage stats | Aug 2026 |
| 💡 Idea | Plugin marketplace for tools | Q4 2026 |
| 💡 Idea | White-label theming | Q4 2026 |

---

## ☠️ STARTUPS / BUSINESSES

What this template replaces:

| Option | Cost | What You Get Instead | Saving |
|--------|------|---------------------|--------|
| Custom dev (agency) | $15,000–30,000 | Fork + deploy in 2hrs | $15,000+ |
| Botpress (Pro) | $445/mo | Full control, no vendor lock | $5,340/yr |
| Intercom AI | $100+/mo | Custom models, your data | $1,200+/yr |
| Typebot | $89/mo | Multi-model + tool use | $1,068/yr |
| ManyChat | $49/mo | Persistent history + auth | $588/yr |
| **Total saved** | **~$23,000+** | | |

---

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=hmzainjamil/ai-chatbot-builder&type=Date)](https://star-history.com/#hmzainjamil/ai-chatbot-builder&Date)

---

Built by [HMZ](https://github.com/hmzainjamil)

---

## 📋 CUSTOMIZATION GUIDE

### Add a new model provider

```typescript
// lib/ai/models.ts
import { createOpenAI } from '@ai-sdk/openai'

const together = createOpenAI({
  baseURL: 'https://api.together.xyz/v1',
  apiKey: process.env.TOGETHER_API_KEY,
})

export const models = [
  ...existingModels,
  {
    id: 'meta-llama/Llama-3-70b-chat-hf',
    provider: 'together',
    apiModel: together('meta-llama/Llama-3-70b-chat-hf'),
    label: 'Llama 3 70B',
  },
]
```

### Add persistent memory

```typescript
// lib/db/schema.ts — add memories table
export const memories = pgTable('memories', {
  id: uuid('id').primaryKey().defaultRandom(),
  userId: uuid('user_id').notNull(),
  content: text('content').notNull(),
  embedding: vector('embedding', { dimensions: 1536 }),
  createdAt: timestamp('created_at').defaultNow(),
})
```

### Add a system prompt UI

```typescript
// app/(chat)/page.tsx
// Add system prompt input that persists per-conversation
const [systemPrompt, setSystemPrompt] = useState('')
// Pass to API: fetch('/api/chat', { body: JSON.stringify({ messages, systemPrompt }) })
```

---

## 🔒 SECURITY CHECKLIST

| Check | Status | Notes |
|-------|--------|-------|
| API keys in env vars | ✅ | Never in source code |
| Auth on all routes | ✅ | Auth.js middleware |
| Rate limiting | ⚠️ | Add Vercel Edge rate limit |
| Input sanitization | ✅ | AI SDK validates tool params |
| CSRF protection | ✅ | Next.js built-in |
| Blob access control | ✅ | Vercel Blob token required |
| SQL injection | ✅ | Drizzle ORM parameterized queries |
| XSS | ✅ | React escapes by default |
| Secrets scanning | 📋 | Add GitHub secret scanning |

---

## 🌐 DEPLOYMENT OPTIONS

| Platform | Cost | Scaling | Setup Time |
|----------|------|---------|-----------|
| Vercel (recommended) | Free tier available | Auto | 5 min |
| Railway | $5/mo | Auto | 15 min |
| Render | Free tier available | Manual | 20 min |
| Fly.io | $3/mo | Auto | 30 min |
| Self-hosted VPS | $5/mo | Manual | 2 hours |
| Docker + Coolify | $5/mo infra | Auto | 1 hour |


---

## 🗂️ REPO STRUCTURE

```
ai-chatbot-builder/
├── app/
│   ├── (auth)/
│   │   ├── login/page.tsx
│   │   └── register/page.tsx
│   ├── (chat)/
│   │   ├── page.tsx            ← RSC: loads history server-side
│   │   ├── [id]/page.tsx       ← specific conversation
│   │   └── layout.tsx
│   └── api/
│       ├── chat/route.ts       ← streaming POST handler
│       ├── auth/[...nextauth]/route.ts
│       └── files/upload/route.ts
├── components/
│   ├── chat.tsx                ← useChat, streaming UI
│   ├── message.tsx
│   ├── model-selector.tsx
│   └── ui/                     ← shadcn/ui components
├── lib/
│   ├── ai/
│   │   ├── models.ts
│   │   └── tools/
│   ├── db/
│   │   ├── schema.ts           ← Drizzle ORM
│   │   ├── migrations/
│   │   └── queries.ts
│   └── auth.ts
├── .env.example
├── drizzle.config.ts
└── package.json
```

---

## 📜 CHANGELOG

| Version | Change |
|---------|--------|
| Latest | Multi-model via Vercel AI Gateway, Neon Postgres, Vercel Blob |
| v2.0 | App Router migration, RSC history loading |
| v1.5 | File attachments |
| v1.0 | Initial release — single model, chat only |

---

## 🌐 LINKS

| Resource | URL |
|----------|-----|
| Live demo | [chatbot.ai-sdk.dev](https://chatbot.ai-sdk.dev) |
| Vercel template | [vercel.com/templates](https://vercel.com/templates/next.js/chatbot) |
| AI SDK docs | [ai-sdk.dev](https://ai-sdk.dev) |
| Neon Postgres | [neon.tech](https://neon.tech) |
| Auth.js | [authjs.dev](https://authjs.dev) |
| shadcn/ui | [ui.shadcn.com](https://ui.shadcn.com) |
