# Global agent rules

Rules for the Zed agent in every project.

## General

- Explain decisions briefly and point to the relevant files.
- Make minimal, focused changes; don't rewrite unrelated code.
- Prefer the project's existing dependencies and patterns.
- Never commit `.env` files or secrets; keep keys out of code and logs.
- For big or risky changes, summarize the plan before touching code.
- Run the project's tests/build after changes when practical.

## Web projects (Next.js / TanStack)

- Use shadcn/ui components, lucide-react icons, and Tailwind CSS v4.
- Respect the existing folder structure and naming conventions.

## Git

- Use short conventional commit messages (`feat:`, `fix:`, `docs:`, `chore:`).
- Don't commit or push unless asked.

## Communication

- Answer simply and directly; use plain words instead of jargon.
