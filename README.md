# Chatbot — Project README

An AI-powered chatbot application built with Next.js 16, the Vercel AI SDK, and a Postgres database. It supports multi-model conversations, document artifacts, file uploads, and real-time streaming.

---

## Done (What Is Already Implemented)

### Authentication
- User registration and login with hashed passwords (`bcrypt-ts`)
- Guest / anonymous sessions via a dedicated auth route
- Auth.js (NextAuth v5) integrated with Neon Postgres as the session store
- Per-user data isolation enforced at the database query layer

### Chat & Messaging
- Persistent chat sessions stored in Postgres (`Chat` and `Message_v2` tables)
- Real-time AI response streaming with resumable streams (`resumable-stream`)
- Multi-turn conversation history loaded per chat session
- Message voting (thumbs up / down) stored in the `Vote_v2` table
- Auto-generated chat titles via a dedicated title-generation prompt
- Public and private chat visibility toggles

### AI & Model Support
- Vercel AI Gateway integration for unified multi-provider access
- Supported models: OpenAI, Anthropic, xAI, Mistral, Moonshot, DeepSeek (configured in `lib/ai/models.ts`)
- Automatic OIDC token auth on Vercel deployments; `AI_GATEWAY_API_KEY` for non-Vercel
- Tool calling support with an approval/denial flow rendered in the UI
- Reasoning output display via a dedicated `MessageReasoning` component
- Slash-command interface for quick actions in the chat input

### Artifacts (Side Panel Documents)
- Three artifact types supported: `text` (ProseMirror rich text), `code` (CodeMirror editor), `sheet` (CSV/spreadsheet via react-data-grid)
- `createDocument`, `editDocument`, and `updateDocument` AI tools for generating and modifying artifacts
- `requestSuggestions` tool for inline document improvement suggestions
- Diff view for comparing document versions
- Version history footer on each artifact
- Image artifact editing via `image-editor` component

### File Uploads
- File upload API backed by Vercel Blob storage
- Attachment previews in the chat input before sending

### Observability & Infrastructure
- OpenTelemetry tracing via `@vercel/otel` and `instrumentation.ts`
- Rate limiting with Redis
- Bot detection via `botid`
- Vercel Analytics wired up in the root layout
- Drizzle ORM with Postgres for type-safe queries and migrations

### UI & Developer Experience
- shadcn/ui component library with Radix UI primitives
- Tailwind CSS v4 for styling
- Dark/light theme switching via `next-themes`
- Sidebar with paginated chat history
- Framer Motion and Motion animations throughout
- Biome (`ultracite`) for linting and formatting
- Playwright end-to-end test suite

---

## Todo (What Still Needs to Be Done)

### Environment Setup (Required Before Running)
- [ ] Copy `.env.example` to `.env.local` and fill in all values
- [ ] Provision a [Neon Postgres](https://neon.tech) database and set `POSTGRES_URL`
- [ ] Create a [Vercel Blob](https://vercel.com/storage/blob) store and set `BLOB_READ_WRITE_TOKEN`
- [ ] Set `AUTH_SECRET` to a strong random string (e.g. `openssl rand -base64 32`)
- [ ] For non-Vercel deployments, set `AI_GATEWAY_API_KEY` with a valid Vercel AI Gateway key
- [ ] Provision a Redis instance and set the Redis connection URL for rate limiting
- [ ] Run database migrations: `pnpm db:migrate`

### Deployment
- [ ] Link the project to Vercel (`vercel link`) to enable OIDC-based AI Gateway auth
- [ ] Add all environment variables in the Vercel dashboard or pull them with `vercel env pull`
- [ ] Verify the `NEXTAUTH_URL` / `AUTH_URL` is set correctly for the production domain

### Optional Enhancements
- [ ] Add additional AI models by extending `lib/ai/models.ts` with new provider configs
- [ ] Implement email verification (the `emailVerified` column exists in the schema but the flow is not wired up)
- [ ] Expand the Playwright test suite — currently minimal coverage of chat flows
- [ ] Add image generation artifact kind (`image` is referenced in the schema `kind` enum but the full pipeline is partial)
- [ ] Configure OpenTelemetry exporter endpoint for production tracing (currently uses default Vercel OTEL)
- [ ] Review and tighten rate-limit thresholds in `lib/ratelimit.ts` for production traffic

---

## Running Locally

```bash
# 1. Install dependencies
pnpm install

# 2. Set up environment variables
cp .env.example .env.local
# Edit .env.local with your credentials

# 3. Run database migrations
pnpm db:migrate

# 4. Start the development server
pnpm dev
```

The app will be available at [http://localhost:3000](http://localhost:3000).

---

## Useful Scripts

| Script | Description |
|---|---|
| `pnpm dev` | Start the dev server with Turbopack |
| `pnpm build` | Migrate the database then build for production |
| `pnpm start` | Start the production server |
| `pnpm db:migrate` | Apply pending database migrations |
| `pnpm db:studio` | Open Drizzle Studio (database GUI) |
| `pnpm db:generate` | Generate a new migration from schema changes |
| `pnpm check` | Run Biome linter checks |
| `pnpm fix` | Auto-fix Biome lint issues |
| `pnpm test` | Run the Playwright end-to-end test suite |

---

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | Next.js 16 (App Router) |
| AI | Vercel AI SDK v6, AI Gateway |
| Database | Neon Postgres via Drizzle ORM |
| Auth | Auth.js (NextAuth v5) |
| File Storage | Vercel Blob |
| Caching / Rate Limiting | Redis |
| UI | shadcn/ui, Radix UI, Tailwind CSS v4 |
| Rich Text Editor | ProseMirror |
| Code Editor | CodeMirror 6 |
| Spreadsheet | react-data-grid |
| Observability | OpenTelemetry, Vercel Analytics |
| Testing | Playwright |
| Linting | Biome (ultracite) |
