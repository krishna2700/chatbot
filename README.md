<a href="https://chatbot.ai-sdk.dev/demo">
  <img alt="Chatbot" src="app/(chat)/opengraph-image.png">
  <h1 align="center">Chatbot</h1>
</a>

<p align="center">
  Chatbot (formerly AI Chatbot) is a free, open-source AI chatbot template built with Next.js and the <a href="https://ai-sdk.dev">AI SDK</a> by <a href="https://vercel.com">Vercel</a>. It provides a production-ready foundation for building powerful, multi-model chatbot applications with rich document editing, artifact generation, and more.
</p>

<p align="center">
  <a href="https://chatbot.ai-sdk.dev/docs"><strong>Documentation</strong></a> ·
  <a href="#features"><strong>Features</strong></a> ·
  <a href="#model-providers"><strong>Model Providers</strong></a> ·
  <a href="#architecture"><strong>Architecture</strong></a> ·
  <a href="#deploy-your-own"><strong>Deploy Your Own</strong></a> ·
  <a href="#running-locally"><strong>Running Locally</strong></a> ·
  <a href="#contributing"><strong>Contributing</strong></a>
</p>

<br/>

## Features

- **[Next.js](https://nextjs.org) App Router**
  - Advanced routing for seamless navigation and performance
  - React Server Components (RSCs) and Server Actions for server-side rendering and increased performance
  - React Compiler enabled for automatic optimizations
  - Turbopack for fast development builds

- **[AI SDK](https://ai-sdk.dev/docs/introduction)**
  - Unified API for generating text, structured objects, and tool calls with LLMs
  - Hooks for building dynamic chat and generative user interfaces
  - Supports multiple model providers via [Vercel AI Gateway](https://vercel.com/docs/ai-gateway)
  - Resumable streaming for reliable message delivery

- **Built-in AI Tools**
  - `createDocument` — Generate text, code, images, and spreadsheet artifacts
  - `editDocument` — Apply targeted edits to existing documents
  - `updateDocument` — Rewrite or update full documents
  - `requestSuggestions` — Get AI-powered suggestions for document improvements
  - `getWeather` — Retrieve weather information (example tool)

- **Rich Document Editor**
  - ProseMirror-based rich text editing
  - CodeMirror integration for code editing with syntax highlighting
  - Spreadsheet support via React Data Grid
  - Markdown rendering with math (KaTeX), Mermaid diagrams, and code highlighting (Shiki)

- **[shadcn/ui](https://ui.shadcn.com) Components**
  - Styling with [Tailwind CSS v4](https://tailwindcss.com)
  - Component primitives from [Radix UI](https://radix-ui.com) for accessibility and flexibility
  - Dark mode support via `next-themes`
  - Smooth animations with Framer Motion

- **Data Persistence**
  - [Neon Serverless Postgres](https://vercel.com/marketplace/neon) for saving chat history, user data, documents, and suggestions
  - [Vercel Blob](https://vercel.com/storage/blob) for efficient file storage
  - [Redis](https://vercel.com/docs/redis) for rate limiting

- **[Auth.js](https://authjs.dev) (NextAuth v5)**
  - Simple and secure authentication with email/password
  - Anonymous user support
  - Session management and protected routes

- **Observability**
  - OpenTelemetry instrumentation for tracing and monitoring
  - Vercel Analytics integration

## Model Providers

This template uses the [Vercel AI Gateway](https://vercel.com/docs/ai-gateway) to access multiple AI models through a unified interface. Models are configured in `lib/ai/models.ts` with per-model provider routing.

### Included Models

| Model | Provider | Description |
|-------|----------|-------------|
| DeepSeek V3.2 | DeepSeek | Fast and capable model with tool use |
| Codestral | Mistral | Code-focused model with tool use |
| Mistral Small | Mistral | Fast vision model with tool use |
| Kimi K2.5 | Moonshot AI | Moonshot AI flagship model (default) |
| GPT OSS 20B | OpenAI | Compact reasoning model |
| GPT OSS 120B | OpenAI | Open-source 120B parameter model |
| Grok 4.1 Fast | xAI | Fast non-reasoning model with tool use |

### AI Gateway Authentication

- **Vercel deployments**: Authentication is handled automatically via OIDC tokens.
- **Non-Vercel deployments**: Set the `AI_GATEWAY_API_KEY` environment variable in your `.env.local` file.

With the [AI SDK](https://ai-sdk.dev/docs/introduction), you can also switch to direct LLM providers like [OpenAI](https://openai.com), [Anthropic](https://anthropic.com), [Cohere](https://cohere.com/), and [many more](https://ai-sdk.dev/providers/ai-sdk-providers) with just a few lines of code.

## Architecture

```
├── app/                    # Next.js App Router
│   ├── (auth)/             # Authentication pages (login, register)
│   ├── (chat)/             # Chat interface pages
│   ├── layout.tsx          # Root layout
│   └── globals.css         # Global styles (Tailwind v4)
├── artifacts/              # Artifact rendering and management
├── components/
│   ├── ai-elements/        # AI-specific UI components
│   ├── chat/               # Chat interface components
│   └── ui/                 # Reusable shadcn/ui components
├── hooks/                  # Custom React hooks
├── lib/
│   ├── ai/
│   │   ├── models.ts       # Model definitions and configuration
│   │   ├── providers.ts    # AI provider setup
│   │   ├── prompts.ts      # System prompts
│   │   ├── entitlements.ts # User entitlements/permissions
│   │   └── tools/          # AI tool definitions
│   ├── db/
│   │   ├── schema.ts       # Drizzle ORM schema (PostgreSQL)
│   │   ├── queries.ts      # Database query functions
│   │   ├── migrate.ts      # Migration runner
│   │   └── migrations/     # SQL migration files
│   ├── ratelimit.ts        # Redis-based rate limiting
│   └── utils.ts            # Shared utilities
├── public/                 # Static assets
└── tests/                  # Playwright E2E tests
```

### Database Schema

The application uses PostgreSQL with [Drizzle ORM](https://orm.drizzle.team). Key tables:

- **User** — User accounts with email/password authentication and anonymous support
- **Chat** — Chat sessions with public/private visibility
- **Message_v2** — Chat messages with role, parts, and attachments
- **Vote_v2** — Message upvote/downvote tracking
- **Document** — Generated artifacts (text, code, image, sheet)
- **Suggestion** — AI-generated document suggestions
- **Stream** — Resumable stream tracking

## Deploy Your Own

You can deploy your own version of Chatbot to Vercel with one click:

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/templates/next.js/chatbot)

## Running Locally

### Prerequisites

- [Node.js](https://nodejs.org) 18+
- [pnpm](https://pnpm.io) 10+
- A PostgreSQL database (e.g., [Neon](https://neon.tech))
- A Redis instance (e.g., [Vercel KV](https://vercel.com/docs/redis))
- A [Vercel Blob](https://vercel.com/storage/blob) store (for file uploads)

### Environment Variables

Copy the example environment file and fill in your values:

```bash
cp .env.example .env.local
```

Required variables:

| Variable | Description |
|----------|-------------|
| `AUTH_SECRET` | Random secret for Auth.js session encryption ([generate one](https://generate-secret.vercel.app/32)) |
| `AI_GATEWAY_API_KEY` | Vercel AI Gateway API key (not needed on Vercel deployments) |
| `BLOB_READ_WRITE_TOKEN` | Vercel Blob storage token |
| `POSTGRES_URL` | PostgreSQL connection string |
| `REDIS_URL` | Redis connection string |

> **Note:** You should not commit your `.env.local` file or it will expose secrets that will allow others to control access to your various AI and authentication provider accounts.

### Setup with Vercel CLI (Recommended)

1. Install Vercel CLI: `npm i -g vercel`
2. Link local instance with Vercel and GitHub accounts: `vercel link`
3. Download your environment variables: `vercel env pull`

### Install & Run

```bash
# Install dependencies
pnpm install

# Run database migrations
pnpm db:migrate

# Start development server (with Turbopack)
pnpm dev
```

Your app should now be running on [localhost:3000](http://localhost:3000).

### Available Scripts

| Script | Description |
|--------|-------------|
| `pnpm dev` | Start development server with Turbopack |
| `pnpm build` | Run migrations and build for production |
| `pnpm start` | Start production server |
| `pnpm check` | Run linting and formatting checks |
| `pnpm fix` | Auto-fix linting and formatting issues |
| `pnpm db:generate` | Generate new Drizzle migration files |
| `pnpm db:migrate` | Apply database migrations |
| `pnpm db:studio` | Open Drizzle Studio (database GUI) |
| `pnpm db:push` | Push schema changes directly to database |
| `pnpm db:pull` | Pull schema from database |
| `pnpm db:check` | Check migration consistency |
| `pnpm test` | Run Playwright E2E tests |

## Tech Stack

| Category | Technology |
|----------|-----------|
| Framework | [Next.js 16](https://nextjs.org) |
| Language | [TypeScript](https://www.typescriptlang.org) |
| AI | [AI SDK](https://ai-sdk.dev), [Vercel AI Gateway](https://vercel.com/docs/ai-gateway) |
| Styling | [Tailwind CSS v4](https://tailwindcss.com) |
| Components | [shadcn/ui](https://ui.shadcn.com), [Radix UI](https://radix-ui.com) |
| Database | [PostgreSQL](https://www.postgresql.org) via [Drizzle ORM](https://orm.drizzle.team) |
| Auth | [Auth.js](https://authjs.dev) (NextAuth v5) |
| Storage | [Vercel Blob](https://vercel.com/storage/blob) |
| Cache | [Redis](https://redis.io) |
| Editor | [ProseMirror](https://prosemirror.net), [CodeMirror](https://codemirror.net) |
| Animation | [Framer Motion](https://www.framer.com/motion/) |
| Testing | [Playwright](https://playwright.dev) |
| Linting | [Biome](https://biomejs.dev) via [Ultracite](https://github.com/haydenbleasel/ultracite) |
| Observability | [OpenTelemetry](https://opentelemetry.io), [Vercel Analytics](https://vercel.com/analytics) |
| Deployment | [Vercel](https://vercel.com) |

## Contributing

Contributions are welcome! Please read the project's coding conventions and ensure your changes pass linting (`pnpm check`) before submitting a pull request.

## License

This project is licensed under the [Apache License 2.0](LICENSE).
