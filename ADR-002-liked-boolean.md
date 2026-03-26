# ADR-002 — liked as nullable boolean, not enum or numeric score
> Date: 2026-03-26 | Status: Accepted

## Context
Food items need to capture whether the user liked them. Options considered:
1. Numeric score (1–5)
2. Enum: liked / neutral / disliked
3. Nullable boolean: true / false / null

## Decision
Use nullable boolean: `liked boolean` in PostgreSQL.

## Rationale
- This is explicitly **not** a rating app. A numeric score implies precision we don't want.
- Three states are sufficient: liked, didn't like, didn't form an opinion (or didn't want to log one).
- `null` is semantically correct for "no opinion recorded" — it's distinct from "neutral" in a way that matters for AI inference. If a user never liked or disliked a dish, the AI should treat that differently from a dish they actively said they were neutral on.
- Boolean is simpler to query and filter than an enum. `WHERE liked = true` is clearer than `WHERE liked = 'liked'`.

## Consequences
- UI must clearly communicate the three states without relying on database terminology.
- AI prompts (Phase 3) must handle the null case explicitly — don't conflate "no opinion" with "neutral" when building preference profiles.
