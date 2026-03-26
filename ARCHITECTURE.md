# Architecture
> Version: v0.1.0 | Created: 2026-03-26

---

## Stack

| Layer | Choice | Rationale |
|---|---|---|
| Framework | Next.js 14 (App Router) | Single codebase for web + PWA; API routes eliminate need for a separate backend; strong ecosystem |
| Styling | Tailwind CSS | Fast iteration; mobile-first utility classes; no CSS file sprawl |
| Database | Supabase (PostgreSQL) | Managed Postgres; Row Level Security (RLS) enforces user data isolation at the DB layer; scales from 1 to N users without re-architecting |
| Auth | Supabase Auth | Built into Supabase; supports email/password and OAuth; session management handled |
| Deployment | Vercel | Native Next.js support; zero-config PWA-compatible deployment; free tier sufficient for MVP |
| PWA | next-pwa | Adds service worker and manifest to Next.js with minimal config |
| AI (Phase 3) | Anthropic API (claude-sonnet) | Called from Next.js API routes; no additional backend needed |

---

## Data model

Designed to be AI-ready from Phase 1. Key decisions:
- `liked` is a **nullable boolean**, not a rating. `true` = liked, `false` = did not like, `null` = neutral / informational.
- `modifications` is a separate text field (not buried in notes) so it can be surfaced distinctly to the AI later.
- `cuisine_type` on restaurants is optional at entry time but valuable for preference inference.

### Tables

```sql
-- Users are managed by Supabase Auth (auth.users)
-- We extend with a profiles table for app-level user data

profiles
  id          uuid references auth.users primary key
  display_name text
  created_at  timestamptz default now()

dining_logs
  id              uuid primary key default gen_random_uuid()
  user_id         uuid references profiles(id) not null
  restaurant_name text not null
  restaurant_city text                          -- optional; useful for travel recs
  cuisine_type    text                          -- optional; e.g. "Japanese", "Italian"
  visit_date      date not null default current_date
  overall_notes   text                          -- free-form trip-level notes
  created_at      timestamptz default now()
  updated_at      timestamptz default now()

food_items
  id            uuid primary key default gen_random_uuid()
  dining_log_id uuid references dining_logs(id) on delete cascade not null
  food_name     text not null
  liked         boolean                         -- null = neutral, true = liked, false = disliked
  modifications text                            -- e.g. "asked for sauce on side, added avocado"
  notes         text                            -- anything else
  created_at    timestamptz default now()
```

### Row Level Security

All tables enforce RLS. Users can only read and write their own rows.

```sql
-- Example for dining_logs
create policy "Users can only access their own logs"
  on dining_logs for all
  using (auth.uid() = user_id);
```

---

## PWA configuration

- Manifest: app name, icons, `display: standalone`, `start_url: /`
- Service worker: cache-first for static assets; network-first for API routes
- Offline state: show a friendly "you're offline" message; do not silently fail

---

## Constraints and decisions

### Why not a native app?
PWA covers the mobile use case without App Store review cycles, separate codebases, or native SDKs. For a personal tool, this is the right tradeoff.

### Why Supabase over Firebase?
PostgreSQL's relational model is significantly better for the AI phase — structured queries over normalized data are much more useful for preference inference than document collections.

### Why not a separate Express/FastAPI backend?
Next.js API routes are sufficient for this app's complexity level through all three phases. Adding a separate backend would introduce deployment complexity with no benefit at this scale.

---

## Architecture decision records

`docs/decisions/` — see ADR index below.

| ADR | Decision | Date |
|---|---|---|
| ADR-001 | Use Next.js App Router over Pages Router | 2026-03-26 |
| ADR-002 | liked as nullable boolean, not enum or score | 2026-03-26 |
