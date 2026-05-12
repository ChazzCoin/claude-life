# /publish

Selectively publish content from claude-life to the public projection layer.

## Purpose

Everything is private by default. `/publish` is the *only* way to move content
from the private repo to the public `api/public/` projection.

## How it works

1. User specifies what to publish: a file path, a date range, a specific entry.
2. `/publish` reads the content, validates it's publishable, and adds it to the
   allow-list in `api/public/INDEX.md`.
3. Content in the allow-list is automatically transformed (sensitive details
   redacted, personal names anonymized if needed) and exported to `api/public/`.
4. Each piece has metadata: what was published, when, what transformations were
   applied.

## What can be published

- Specific timeline entries (dated, with timestamp)
- Decisions with their reasoning (from `docs/decisions/`)
- Notes and learnings (from `docs/notes/`)
- Retros and postmortems (from `docs/retros/`, `docs/postmortems/`)
- Gratitude entries (from `docs/gratitude/`)

## What stays private

- State blocks (Mood, Energy, Focus, Body, Social) — never published
- People memory details (relationships, family info, etc.)
- Financial specifics
- Private journal entries (unless explicitly marked publishable)
- People's names/relationships unless using aliases

## Workflow

```
User: /publish docs/decisions/2026-05-07-phase-start.md
  → Claude reads the file
  → Claude asks: "Publish this decision with anonymized names?"
  → User confirms
  → Claude adds entry to api/public/INDEX.md
  → Claude auto-exports to api/public/decisions/2026-05-07-phase-start.md
  → Claude logs publication in AUDIT.md
```

## Output format

Published content is markdown with:
- A `metadata:` block at the top (publication date, what was anonymized/redacted)
- The original content (transformed)
- A `context:` footer (brief note on why this matters)

## Scope (future)

- Batch publishing (multiple files)
- Scheduled publishing (publish on a date)
- Un-publishing (remove from allow-list)
- Publishing as structured data (JSON API endpoints) — Wave 4.5

---

*Impl: Writes entries to `api/public/INDEX.md` and exports to `api/public/<type>/<file.md>`.*
