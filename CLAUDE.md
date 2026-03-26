# CLAUDE.md
> Instructions for Claude Code. Read this file before doing anything else.
> Last updated: 2026-03-26 | Framework version: v0.1.0

---

## What this project is

A personal dining memory app — **not a rating app**. Users log restaurants they've visited, food they ordered, whether they liked it, and notes (e.g., modifications that improved a dish). The goal is personal memory and, in a later phase, AI-powered dining recommendations.

See `docs/PROJECT.md` for full context and goals.

---

## Current phase

**Phase 1 — Core MVP (single user)**

See `docs/phases/phase-1-spec.md` for the full feature spec and acceptance criteria.

Do not build anything outside Phase 1 scope unless explicitly instructed.

---

## Tech stack

| Layer | Choice |
|---|---|
| Framework | Next.js 14 (App Router) |
| Styling | Tailwind CSS |
| Database + Auth | Supabase |
| Deployment | Vercel |
| PWA | next-pwa |

See `docs/ARCHITECTURE.md` for rationale and constraints.

---

## Project conventions

### File structure
```
/app                  ← Next.js App Router pages and layouts
/components           ← Reusable UI components
/lib                  ← Supabase client, utility functions, types
/docs                 ← Project management (do not modify without instruction)
CLAUDE.md             ← This file
```

### Code style
- TypeScript throughout. No `any` types without a comment explaining why.
- Components: functional, named exports.
- Supabase queries: always in `/lib` or server components, never in client components directly.
- Error handling: never silently swallow errors. Surface them to the user.
- Mobile-first CSS: write Tailwind for small screens first, add `md:` / `lg:` variants after.

### Git discipline
- Commit after each meaningful unit of work.
- Commit message format: `[phase-1] short description of what changed`
- Do not commit `.env.local` or any secrets.

---

## How to update the project management docs

When completing a phase or making a significant architectural decision:
1. Update `docs/CHANGELOG.md` with what changed and why.
2. If an architecture decision was made, add an ADR to `docs/decisions/`.
3. Do not modify `docs/phases/phase-X-spec.md` files retroactively — they are historical records.

---

## Environment variables required

```
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
```

For Phase 3 (not yet):
```
ANTHROPIC_API_KEY=
```
