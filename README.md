<a href="https://chatbot.ai-sdk.dev/demo">
  <img alt="Chatbot" src="app/(chat)/opengraph-image.png">
  <h1 align="center">Chatbot</h1>
</a>

<p align="center">
    Chatbot is a free, open-source template built with Next.js and the AI SDK that helps you quickly build powerful chatbot applications.
</p>

<p align="center">
  <a href="https://chatbot.ai-sdk.dev/docs"><strong>Read Docs</strong></a> ·
  <a href="#features"><strong>Features</strong></a> ·
  <a href="#tech-stack"><strong>Tech Stack</strong></a> ·
  <a href="#model-providers"><strong>Model Providers</strong></a> ·
  <a href="#environment-variables"><strong>Environment Variables</strong></a> ·
  <a href="#running-locally"><strong>Running Locally</strong></a> ·
  <a href="#deploy-your-own"><strong>Deploy Your Own</strong></a>
</p>

<br/>

## Features

- **[Next.js](https://nextjs.org) App Router**
  - Advanced routing for seamless navigation and performance
  - React Server Components (RSCs) and Server Actions for server-side rendering and increased performance
- **[AI SDK](https://ai-sdk.dev/docs/introduction)**
  - Unified API for generating text, structured objects, and tool calls with LLMs
  - Hooks for building dynamic chat and generative user interfaces
  - Supports OpenAI, Anthropic, Google, xAI, and other model providers via AI Gateway
- **[shadcn/ui](https://ui.shadcn.com)**
  - Styling with [Tailwind CSS](https://tailwindcss.com)
  - Component primitives from [Radix UI](https://radix-ui.com) for accessibility and flexibility
- **Data Persistence**
  - [Neon Serverless Postgres](https://vercel.com/marketplace/neon) for saving chat history and user data
  - [Vercel Blob](https://vercel.com/storage/blob) for efficient file storage
  - [Redis](https://vercel.com/marketplace/redis) for rate limiting and caching
- **[Auth.js](https://authjs.dev)**
  - Simple and secure authentication with session management
- **Rich Editor & Artifacts**
  - ProseMirror-based rich text editor
  - CodeMirror-powered code editor with Python support
  - Mermaid diagram rendering
  - Math (KaTeX) and CJK support via Streamdown
- **Observability**
  - OpenTelemetry instrumentation for tracing and monitoring
  - Vercel Analytics integration

---

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | [Next.js 16](https://nextjs.org) |
| Language | [TypeScript](https://www.typescriptlang.org/) |
| AI | [Vercel AI SDK](https://ai-sdk.dev) |
| Auth | [Auth.js (NextAuth v5)](https://authjs.dev) |
| Database | [PostgreSQL via Drizzle ORM](https://orm.drizzle.team/) |
| Storage | [Vercel Blob](https://vercel.com/storage/blob) |
| Cache / Rate Limiting | [Redis](https://vercel.com/marketplace/redis) |
| UI Components | [shadcn/ui](https://ui.shadcn.com) + [Radix UI](https://radix-ui.com) |
| Styling | [Tailwind CSS v4](https://tailwindcss.com) |
| Rich Text | [ProseMirror](https://prosemirror.net/) |
| Code Editor | [CodeMirror](https://codemirror.net/) |
| Testing | [Playwright](https://playwright.dev/) |
| Linting / Formatting | [Biome](https://biomejs.dev/) + [Ultracite](https://ultracite.dev) |
| Package Manager | [pnpm](https://pnpm.io/) |

---

## Model Providers

This template uses the [Vercel AI Gateway](https://vercel.com/docs/ai-gateway) to access multiple AI models through a unified interface. Models are configured in `lib/ai/models.ts` with per-model provider routing.

**Included models:**
- Mistral
- Moonshot
- DeepSeek
- OpenAI
- xAI

### AI Gateway Authentication

**For Vercel deployments:** Authentication is handled automatically via OIDC tokens — no extra configuration needed.

**For non-Vercel deployments:** Set the `AI_GATEWAY_API_KEY` environment variable in your `.env.local` file. You can obtain an API key from the [Vercel AI Gateway dashboard](https://vercel.com/ai-gateway).

With the [AI SDK](https://ai-sdk.dev/docs/introduction), you can also switch to direct LLM providers like [OpenAI](https://openai.com), [Anthropic](https://anthropic.com), [Cohere](https://cohere.com/), and [many more](https://ai-sdk.dev/providers/ai-sdk-providers) with just a few lines of code.

---

## Environment Variables

Copy `.env.example` to `.env.local` and fill in the required values:

```bash
cp .env.example .env.local
```

| Variable | Description | Required |
|---|---|---|
| `AUTH_SECRET` | Secret used to sign authentication tokens. Generate one at [generate-secret.vercel.app](https://generate-secret.vercel.app/32) or with `openssl rand -base64 32`. | ✅ |
| `AI_GATEWAY_API_KEY` | API key for the Vercel AI Gateway. Required for non-Vercel deployments. | ⚠️ Non-Vercel only |
| `BLOB_READ_WRITE_TOKEN` | Token for [Vercel Blob](https://vercel.com/docs/vercel-blob) file storage. | ✅ |
| `POSTGRES_URL` | Connection string for a [Postgres](https://vercel.com/docs/postgres) database. | ✅ |
| `REDIS_URL` | Connection string for a [Redis](https://vercel.com/docs/redis) instance used for rate limiting. | ✅ |

> ⚠️ **Never commit your `.env.local` file.** It is already included in `.gitignore`.

---

## Running Locally

### Prerequisites

- [Node.js 22+](https://nodejs.org/)
- [pnpm](https://pnpm.io/) (`npm install -g pnpm`)
- A Postgres database (e.g., [Neon](https://neon.tech/))
- A Redis instance (e.g., [Upstash](https://upstash.com/))
- A Vercel Blob store

### Setup with Vercel CLI (Recommended)

1. **Install the Vercel CLI:**
   ```bash
   npm i -g vercel
   ```

2. **Link your local project with Vercel:**
   ```bash
   vercel link
   ```

3. **Pull your environment variables:**
   ```bash
   vercel env pull
   ```

4. **Install dependencies:**
   ```bash
   pnpm install
   ```

5. **Run database migrations:**
   ```bash
   pnpm db:migrate
   ```

6. **Start the development server:**
   ```bash
   pnpm dev
   ```

Your app should now be running at [http://localhost:3000](http://localhost:3000).

### Manual Setup

1. Copy the example env file and fill in your values:
   ```bash
   cp .env.example .env.local
   ```

2. Install dependencies, migrate the database, and start the dev server:
   ```bash
   pnpm install
   pnpm db:migrate
   pnpm dev
   ```

---

## Project Structure

```
.
├── app/
│   ├── (auth)/         # Authentication routes (sign-in, sign-up)
│   └── (chat)/         # Main chat UI and API routes
├── components/
│   ├── ai-elements/    # AI-specific UI components
│   ├── chat/           # Chat interface components
│   └── ui/             # Shared UI primitives (shadcn/ui)
├── lib/
│   ├── ai/             # AI model configuration and utilities
│   ├── artifacts/      # Artifact rendering logic
│   ├── db/             # Drizzle ORM schema and migrations
│   └── editor/         # ProseMirror editor setup
├── hooks/              # Custom React hooks
├── public/             # Static assets
└── tests/              # Playwright end-to-end tests
```

---

## Available Scripts

| Command | Description |
|---|---|
| `pnpm dev` | Start the development server with Turbopack |
| `pnpm build` | Run DB migrations and build for production |
| `pnpm start` | Start the production server |
| `pnpm check` | Lint and type-check the codebase |
| `pnpm fix` | Auto-fix linting issues |
| `pnpm db:generate` | Generate Drizzle migration files |
| `pnpm db:migrate` | Run pending database migrations |
| `pnpm db:push` | Push schema changes directly to the DB |
| `pnpm db:studio` | Open Drizzle Studio (DB GUI) |
| `pnpm test` | Run Playwright end-to-end tests |

---

## Deploy Your Own

Deploy your own instance of Chatbot to Vercel with one click:

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/templates/next.js/chatbot)

---

## License

This project is licensed under the terms in the [LICENSE](./LICENSE) file.
