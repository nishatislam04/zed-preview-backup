# Per-project AI setup for Zed

The Freebuff provider and global rules are set up **once** in your global
settings (`settings.json` + `AGENTS.md`). Per project you only need two small
files in a `.zed` folder inside the project. Zed reads them automatically.

```
your-project/
└── .zed/
    ├── settings.json   <- optional: per-project model choices
    └── AGENTS.md       <- stack-specific rules for the agent
```

---

## 1. minhaj-portfolio (Next.js 16 + Prisma + PostgreSQL + Vercel)

`.zed/AGENTS.md`:

```markdown
# minhaj-portfolio

Next.js 16 (App Router) + TypeScript + PostgreSQL + Prisma, deployed on Vercel.

## Conventions
- Server components by default; add "use client" only for interactive parts.
- UI: shadcn/ui components, lucide-react icons, Tailwind CSS v4.
- All data access through Prisma; no raw SQL in route handlers.
- After editing prisma/schema.prisma run `npx prisma migrate dev`
  and commit the generated migration.
- Never commit .env. Production secrets live in Vercel env vars.
```

## 2. team-docs (Next.js 16 + PostgreSQL + Redis + Vercel)

`.zed/AGENTS.md`:

```markdown
# team-docs

Next.js 16 (App Router) + TypeScript + PostgreSQL + Redis, deployed on Vercel.

## Conventions
- Server components by default; fetch data in the page/route that uses it.
- UI: shadcn/ui components, lucide-react icons, Tailwind CSS v4.
- PostgreSQL for durable data; Redis for caches, queues and sessions.
- Set explicit TTLs on Redis keys; don't cache user-specific data without
  a user-scoped key.
- Never commit .env. Production secrets live in Vercel env vars.
```

## 3. credets (TanStack monorepo + bun + Better Auth + Supabase + Render + Resend)

`.zed/AGENTS.md`:

```markdown
# credets

Monorepo: TanStack Router web app, bun backend, Better Auth, TanStack libs,
PostgreSQL on Supabase, deployed on Render, email via Resend.

## Conventions
- Respect the monorepo layout: apps/* for runnable apps, packages/* for
  shared code. Shared logic belongs in packages.
- Use `bun` for installs and scripts (bun workspaces).
- Auth lives in one place (packages/auth) via Better Auth; route all
  auth logic through it.
- Database is Supabase Postgres; apply schema changes through the project's
  migration tool, never by hand in prod.
- Emails go through Resend; templates live in their folder.
- Render deploys the backend. If a change affects deployment (env vars,
  start command, build step), say so.
- Never commit .env or secrets; use each platform's env vars.
```

---

## Optional: per-project model defaults

`.zed/settings.json` (only if you want a different default model than the
global one). These only override the agent settings; everything else still
comes from your global settings.

```json
{
  "agent": {
    "default_model": {
      "provider": "freebuff2api",
      "model": "deepseek/deepseek-v4-pro",
      "enable_thinking": true
    },
    "favorite_models": [
      { "provider": "freebuff2api", "model": "deepseek/deepseek-v4-flash", "enable_thinking": true },
      { "provider": "freebuff2api", "model": "deepseek/deepseek-v4-pro", "enable_thinking": true }
    ]
  }
}
```

---

## Project-local skills

Skills are global by default (`~/.agents/skills` — already working in all
projects). To add a skill only for one project, put it in the project's
`.zed/skills/` folder. Zed finds both automatically.

## Notes
- After changing `settings.json` or `AGENTS.md`, restart Zed or open a new
  agent thread so the agent picks up the new context.
- The provider API key is `zed-local` (from `FREEBUFF_API_KEY` in the
  freebuff2api `.env`). Zed stores it in the system keychain per provider.
