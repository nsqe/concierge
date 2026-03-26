# Project Overview
> Version: v0.1.0 | Created: 2026-03-26

---

## What we're building

A personal dining memory app. The core idea: remember what you ate, where you ate it, what you loved, what you'd skip, and what tweaks made something better.

**This is not a rating app.** There are no stars, no public reviews, no social feed. It is a private, personal log.

---

## User story (Phase 1)

> As a solo user, I want to log a restaurant visit — the restaurant name, what I ordered, whether I liked each dish, and any notes or modifications — so that I can recall this information later and build up a picture of my dining preferences over time.

---

## Core design principles

1. **Memory over rating.** The app stores *experience*, not *scores*. Liked / didn't like / neutral are states, not ratings.
2. **Modifications matter.** If a user got a dish modified (e.g., "no sauce, add avocado") and it was great, that is valuable information — for the user and eventually for the AI.
3. **AI-ready from day one.** Even though AI recommendations are Phase 3, the data model will be structured to support inference from Phase 1 forward.
4. **Private by default.** All data is scoped to the authenticated user. There is no public-facing content in Phase 1 or 2.

---

## Phases at a glance

| Phase | Description | Status |
|---|---|---|
| 1 | Core MVP — single user, log entries, view history | 🔲 Not started |
| 2 | Multi-user — auth improvements, account management | 🔲 Not started |
| 3 | AI layer — Claude-powered preference learning and restaurant suggestions | 🔲 Not started |

Full specs: `docs/phases/`

---

## Out of scope (all phases)

- Public reviews or any social/sharing feature
- Star ratings or numeric scores
- Pulling live restaurant data from Yelp, Google Places, etc. (may revisit in Phase 3)
- Native iOS / Android app builds (PWA covers mobile)
