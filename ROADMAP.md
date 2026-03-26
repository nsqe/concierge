# Roadmap
> Version: v0.1.0 | Created: 2026-03-26

---

## Phase 1 — Core MVP
**Goal:** A single user can log dining experiences and review their history.
**Status:** 🔲 Not started
**Spec:** `docs/phases/phase-1-spec.md`

### Milestones
- [ ] 1.1 Project scaffolding (Next.js, Supabase, Tailwind, next-pwa)
- [ ] 1.2 Database schema + RLS policies
- [ ] 1.3 Auth (sign up, log in, log out)
- [ ] 1.4 Create a dining log entry (restaurant + food items)
- [ ] 1.5 View history (list of past entries)
- [ ] 1.6 View / edit a single entry
- [ ] 1.7 Delete an entry
- [ ] 1.8 PWA manifest + basic offline state
- [ ] 1.9 Deployed to Vercel

---

## Phase 2 — Multi-user
**Goal:** The app supports multiple independent accounts. Each user's data is fully isolated.
**Status:** 🔲 Not started
**Spec:** `docs/phases/phase-2-spec.md` *(not yet written)*

### Anticipated milestones
- [ ] 2.1 OAuth login (Google)
- [ ] 2.2 Account settings page (display name, email, delete account)
- [ ] 2.3 Verify RLS holds correctly under multi-user load
- [ ] 2.4 Basic admin visibility (optional — count of users, not their data)

---

## Phase 3 — AI Recommendations
**Goal:** Claude learns the user's dining preferences and offers personalized suggestions.
**Status:** 🔲 Not started
**Spec:** `docs/phases/phase-3-spec.md` *(not yet written)*

### Anticipated milestones
- [ ] 3.1 Preference summary — Claude reads the user's log and produces a plain-language summary of their tastes
- [ ] 3.2 "What am I missing?" — given current city, Claude suggests restaurants the user likely hasn't tried
- [ ] 3.3 Travel mode — given a city the user is visiting, Claude provides personalized recommendations
- [ ] 3.4 Prompt versioning — maintain prompt versions so regressions can be identified

---

## Version history of this roadmap

| Version | Date | Change |
|---|---|---|
| v0.1.0 | 2026-03-26 | Initial roadmap created |
