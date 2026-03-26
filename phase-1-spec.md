# Phase 1 Spec — Core MVP
> Version: v0.1.0 | Created: 2026-03-26
> Status: 🔲 Not started
> This file is a historical record. Do not edit once Phase 1 begins. Amendments go in a new versioned file.

---

## Goal

A single authenticated user can log dining experiences and browse their history. The app works on web and mobile (PWA).

---

## Out of scope for Phase 1

- Multiple users (Phase 2)
- AI recommendations (Phase 3)
- Social or sharing features (never)
- Pulling live data from restaurant APIs
- Search within the app (nice-to-have, defer if time-constrained)

---

## Milestone breakdown

### 1.1 — Project scaffolding

**Tasks:**
- Initialize Next.js 14 project with App Router and TypeScript
- Install and configure Tailwind CSS
- Install and configure next-pwa (manifest, service worker)
- Set up Supabase project; add environment variables
- Create Supabase client helper in `/lib/supabase.ts`
- Deploy skeleton to Vercel; confirm PWA manifest valid on mobile

**Acceptance criteria:**
- `npm run dev` starts without errors
- App is installable as PWA on iOS Safari and Android Chrome
- Vercel deployment URL is live

---

### 1.2 — Database schema + RLS

**Tasks:**
- Create `profiles`, `dining_logs`, `food_items` tables (see schema in `ARCHITECTURE.md`)
- Enable RLS on all tables
- Write and apply RLS policies (users access only their own rows)
- Create a database trigger: on new `auth.users` row, insert into `profiles`

**Acceptance criteria:**
- Schema matches `ARCHITECTURE.md` exactly
- RLS policies verified: a test query with a different `user_id` returns no rows
- Trigger confirmed working: new signup creates profile automatically

---

### 1.3 — Auth

**Tasks:**
- Build sign-up page (`/signup`) — email + password
- Build log-in page (`/login`) — email + password
- Implement log-out (button in nav)
- Redirect unauthenticated users to `/login`
- Redirect authenticated users away from `/login` and `/signup`

**Acceptance criteria:**
- User can create an account with email + password
- User can log in and see the app
- User can log out and is returned to `/login`
- Visiting `/` while logged out redirects to `/login`
- Visiting `/login` while logged in redirects to `/` (or `/logs`)

---

### 1.4 — Create a dining log entry

This is the core feature of the MVP.

**Entry structure:**
- Restaurant name (required, text)
- City / location (optional, text) — stored for future AI travel recommendations
- Cuisine type (optional, text free-form — e.g., "Japanese", "Italian")
- Visit date (required, defaults to today)
- Overall notes about the visit (optional, free-form text)
- One or more food items, each with:
  - Food name (required)
  - Liked? (optional — three states: Liked / Didn't like / No opinion)
  - Modifications (optional, text — "asked for sauce on the side")
  - Notes (optional, text)

**Tasks:**
- Build `/logs/new` page with the form above
- Allow adding multiple food items dynamically (add / remove rows)
- On submit: write to `dining_logs` and `food_items` in Supabase
- On success: redirect to the entry detail view

**Acceptance criteria:**
- Form validates that restaurant name and visit date are present
- At least one food item is required
- User can add up to ~20 food items without UI breaking
- Data appears in Supabase immediately after submit
- `liked` stored correctly: true / false / null

---

### 1.5 — View history (list)

**Tasks:**
- Build `/logs` page listing all of the user's dining log entries
- Sort by visit date descending (most recent first)
- Each list item shows: restaurant name, city (if set), visit date, number of food items
- Link each item to its detail view

**Acceptance criteria:**
- Empty state shows a friendly prompt to add a first entry
- List renders correctly with 50+ entries (no performance issues)
- Mobile layout is usable (cards or rows, not a table)

---

### 1.6 — View + edit a single entry

**Tasks:**
- Build `/logs/[id]` page showing all details of a single entry
- Display all food items with their liked status, modifications, and notes
- Allow editing the entry (inline or via an edit page — developer's choice, mobile UX is the priority)
- On save: update `dining_logs` and `food_items` rows; handle additions and deletions of food items

**Acceptance criteria:**
- All fields display correctly
- User can edit any field and save
- User can add new food items to an existing entry
- User can remove food items from an existing entry
- Liked status is clearly represented in the UI (not just a boolean — use a visual affordance)

---

### 1.7 — Delete an entry

**Tasks:**
- Add delete action on the entry detail page
- Require confirmation before deleting (modal or confirm dialog)
- On delete: remove `dining_logs` row (cascade deletes `food_items`)

**Acceptance criteria:**
- User is asked to confirm before deletion
- After deletion, user is returned to `/logs`
- Deleted entry no longer appears in the list

---

### 1.8 — PWA polish

**Tasks:**
- Confirm PWA manifest: app name, short name, icons (at minimum 192×192 and 512×512), theme color, background color, `display: standalone`
- Service worker: cache-first for static assets
- Offline state: if the user has no connection, show a clear offline message rather than a broken page
- Test install flow on iOS Safari ("Add to Home Screen") and Android Chrome

**Acceptance criteria:**
- App installs to home screen on both iOS and Android
- Offline state shows a message (not a blank screen or browser error)
- App icon and name are correct on the home screen

---

### 1.9 — Deployment

**Tasks:**
- Confirm all environment variables are set in Vercel
- Confirm Supabase project is not on local/dev mode
- Run a full end-to-end test on the Vercel URL (not localhost)

**Acceptance criteria:**
- All features work on the Vercel deployment URL
- PWA install works from the Vercel URL
- No secrets committed to the repo

---

## UI/UX notes

- **Mobile-first.** Design for a phone screen; desktop is secondary.
- **Simplicity.** This is a personal tool, not a consumer product. Clean and functional beats beautiful.
- **Liked status UX.** The three-state liked/disliked/neutral needs a clear visual treatment — consider emoji (👍 / 👎 / — ), color coding, or a segmented control. Avoid plain text dropdowns if possible.

---

## Definition of done for Phase 1

All 9 milestones marked complete, all acceptance criteria passing, app deployed and working as a PWA on mobile.
