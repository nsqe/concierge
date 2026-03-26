# Changelog
> All significant project management changes are recorded here.
> Code-level changes belong in git commit history.

Format: `[vX.Y.Z] YYYY-MM-DD — Description`

---

## [v0.1.0] 2026-03-26 — Initial project framework

**What was established:**
- Project concept defined: personal dining memory app, not a rating app
- Tech stack selected: Next.js 14, Supabase, Tailwind CSS, Vercel, next-pwa
- Data model designed with AI-readiness in mind (`liked` as nullable boolean, `modifications` as discrete field)
- Three-phase roadmap created (MVP → Multi-user → AI recommendations)
- CLAUDE.md, PROJECT.md, ARCHITECTURE.md, ROADMAP.md created
- Phase 1 spec written with acceptance criteria
- ADR-001 and ADR-002 recorded

**Key decisions:**
- PWA over native app (no App Store dependency, single codebase)
- Supabase over Firebase (relational model better for AI phase)
- Next.js API routes over separate backend (sufficient for all 3 phases)
- `liked` field is nullable boolean, not a rating or enum

---

*Future entries will follow this format as phases are completed and decisions are made.*
