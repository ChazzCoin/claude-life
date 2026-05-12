---
name: publish
trigger: /publish
description: |
  Publish a file from the private repo to the public API projection.
  
  Everything starts private. /publish is the only way to move content to
  api/public/. User confirms before any publication.
---

# /publish

Selectively export private content to the public projection.

## Trigger syntax

```
/publish <file-path>
/publish <file-path> --transform=<type>
/publish <file-path> --as-is
/publish --recent           # Show recently-publishable candidates
/publish --list             # Show current allow-list (api/public/INDEX.md)
```

## Behavior

1. **Read the file** — locate it in the repo by path
2. **Determine type** — decision? note? timeline entry?
3. **Show transformations** — what will be anonymized/redacted
4. **Confirm with user** — "publish as-is / with transforms / skip?"
5. **Apply transforms** — rewrite as needed for public audience
6. **Export** — write to api/public/<type>/<filename>
7. **Update INDEX.md** — add entry with metadata
8. **Audit log** — log publication in tasks/AUDIT.md

## What gets transformed

**People**
- If mentioned by name → `<role descriptor>` or anonymized
- Examples: "Mike (co-founder)" → "Co-founder A"
  - "My therapist Sarah" → "therapist"
  - "Dad" → "parent"

**Dates** (depends on sensitivity)
- General reflections: keep the date (e.g., "2026-05-07")
- Personal/sensitive: use relative ("3 weeks ago")
- Company-internal: use relative or drop

**Numbers**
- Revenue, funding, salary: round to nearest order of magnitude or drop
- Metrics/outcomes: round to 10% accuracy
- Timestamps: drop seconds, keep hour:minute

**Details**
- Specific company product names → "product X" or generic
- Internal terminology → explanation or drop
- Sensitive context → rewrite as general principle

**What stays untouched**
- Decisions + reasoning
- Learnings + patterns
- Emotions + reflections
- Timestamps if not sensitive

## Default transforms by type

| File type | Default transform | Example |
|---|---|---|
| `docs/decisions/*.md` | People → roles, dates kept | Keep full reasoning |
| `docs/notes/*.md` | Context-specific redaction | Keep learnings |
| `docs/retros/*.md` | People → team, numbers rounded | Keep reflection |
| `docs/postmortems/*.md` | Names → roles, keep event shape | Keep lessons |
| `docs/gratitude/*.md` | Names → relationship type | Keep sentiment |
| `timeline/YYYY/MM/DD.md` | Depends on content; ask | Keep shape, anonymize people |

## Confirmation flow

```
User: /publish docs/decisions/2026-05-01-hiring.md

Claude:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📄 File: docs/decisions/2026-05-01-hiring.md
Type: Decision
Size: 1.2k

Content preview:
  "I decided to hire [co-founder name] as VP Eng..."

Transformations:
  - Co-founder → "VP Eng candidate"
  - Company name kept (VSI is public)
  - Specific metrics rounded (3000 → "thousands")

Result will live at: api/public/decisions/2026-05-01-hiring.md
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Options:
  1. Publish with transforms (default)
  2. Publish as-is (no transforms)
  3. Skip

Your choice?
```

User selects → Claude executes.

## Implementation notes

- **Transform engine**: Simple regex + manual patterns for now; could be
  templated later
- **Fallback**: If something is hard to transform, ask the user
- **Metadata block** (added to exported file):
  ```
  ---
  published: 2026-05-07T14:30:00Z
  original: docs/decisions/2026-05-01-hiring.md
  transforms:
    - names: anonymized to roles
    - numbers: rounded
    - internal_context: dropped
  ---
  ```
- **INDEX.md** (updated with entry):
  ```
  - docs/decisions/2026-05-01-hiring.md
    Published: 2026-05-07
    Transforms: names → roles, numbers rounded
    Link: ./decisions/2026-05-01-hiring.md
  ```
- **AUDIT.md** (logged):
  ```
  - 🏗 **Published decision: 2026-05-01-hiring.md** to api/public/.
    Transforms: names, numbers. (api/public/INDEX.md)
  ```

## State & UX notes

- **Blocking question**: Always ask before publishing. User confirms.
- **Reversible**: Un-publish with `/unpublish` (removes from allow-list + deletes
  from api/public/)
- **Idempotent**: Re-publishing the same file overwrites (with updated metadata)
- **Batch**: Future: could add `--batch <pattern>` to publish multiple at once
- **Dryrun**: `--dry-run` shows what would be published without executing

## Off-limits (never publish)

- `.claude/state.md` (current state)
- `timeline/**/*/STATE` blocks (mood/energy/etc.)
- `memories/people/*` (all people files)
- Anything in `.claude/` except explicitly marked
- Private-marked entries (future: `[private]` tag)

## Future

- **Feeds**: RSS/JSON feeds from api/public/
- **Versioning**: Track edits to published files
- **Scheduling**: `--publish-on <date>` for future publication
- **Structured data**: Export as JSON alongside markdown
- **Website**: Auto-generate static site from api/public/

---

*Wave 4 skill. Foundational for making claude-life insights shareable.*
