# ADR-001 — Use Next.js App Router over Pages Router
> Date: 2026-03-26 | Status: Accepted

## Context
Next.js supports two routing paradigms: the legacy Pages Router and the newer App Router (introduced in Next.js 13, stable in 14). We need to choose one at project start.

## Decision
Use the App Router.

## Rationale
- App Router is the current standard; Pages Router is in maintenance mode.
- Server Components reduce client-side JavaScript, which benefits mobile performance.
- Layouts, loading states, and error boundaries are cleaner in the App Router model.
- Supabase's Next.js integration now targets App Router primarily.

## Consequences
- Slightly steeper learning curve if the developer is more familiar with Pages Router.
- Some community resources are still Pages Router-only; developer must verify examples apply.
