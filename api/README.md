# API & Public Projection

Wave 4 implementation. The interface between private claude-life and the
public-facing content.

## Architecture

```
claude-life/
  (everything private by default)
  
  → api/public/
    (allow-listed content only)
    
    → INDEX.md (metadata + allow-list)
    → decisions/
    → notes/
    → retros/
    → timeline/ (selected entries)
    → etc.
```

## How it works

1. **Private repo** — everything starts here, private by default.
2. **Allow-list** — `/publish` skill adds entries to `api/public/INDEX.md`.
3. **Transformation** — content is anonymized/redacted as needed:
   - People names → aliases or anonymized
   - Dates → relative ("3 months ago") or kept depending on sensitivity
   - Details → removed if too specific
4. **Export** — transformed content lands in `api/public/<type>/<file>`.
5. **Audit trail** — `api/public/INDEX.md` tracks what was published, when, what
   transformations.

## INDEX.md format

```markdown
# Public Projection — Allow List

Metadata on everything published from claude-life.

## Decisions (docs/decisions/)

- 2026-05-07-phase-start.md
  - Published: 2026-05-07
  - Status: live
  - Transformations: people names → "Co-founder X", dates kept
  - Link: ./decisions/2026-05-07-phase-start.md

## Notes (docs/notes/)

- 2026-05-01-startup-timing.md
  - Published: 2026-05-07
  - Status: live
  - Transformations: company name → "AI startup", specific numbers rounded
  - Link: ./notes/2026-05-01-startup-timing.md

## Timeline (selected entries)

- 2026/04/15-phase-open.md
  - Published: 2026-05-07
  - Status: live
  - Transformations: none (event is general)
  - Link: ./timeline/2026/04/15-phase-open.md
```

## Content types & publishing rules

| Type | Publishable? | Default transform | Notes |
|---|---|---|---|
| `docs/decisions/` | Yes | People → aliases | Reasoning + decision are valuable to share |
| `docs/notes/` | Yes | Context-specific | Learnings are shareable |
| `docs/retros/` | Yes | People → "team", specific metrics rounded | Pattern reflection is valuable |
| `docs/postmortems/` | Yes | Names → role, dates kept | Learning from hard moments |
| `docs/gratitude/` | Yes | Names → relationship type only | Gratitude is shareable |
| `timeline/` entries | Selective | Depends on content | Only entries marked `[publishable]` |
| State blocks | Never | N/A | Always private |
| People memories | Never | N/A | Always private |
| Personal health/finance | Selective | Specific numbers redacted | Only if genuinely useful |
| VSI/rAI internal | Selective | Company names → "startup X" | External-comms ready only |

## Who can access api/public/

Once published:
- The public-facing website (Wave 4.5)
- GitHub (if repo is public)
- Any tool that reads the `api/` directory
- Search engines / feeds (depends on hosting)

## Publishing workflow

```
User: /publish <file>
↓
Claude reads file
↓
Claude: "This is about [topic]. Default transforms: [list].
         Publish as written, with transforms, or skip?"
↓
User: confirms
↓
Claude:
  - Reads the file
  - Applies transforms
  - Writes to api/public/<type>/<filename>
  - Adds entry to api/public/INDEX.md
  - Logs in tasks/AUDIT.md
↓
Done. Live on public projection.
```

## Un-publishing

A published entry can be removed:
- `/unpublish <file>` removes from allow-list and deletes from api/public/
- Logged in AUDIT.md with reason
- Doesn't delete the original (which is still private)

## Future (Wave 4.5+)

- **Structured API**: JSON endpoints at `/api/decisions`, `/api/notes`, etc.
- **Website generation**: Static site generator from `api/public/`
- **Feeds**: RSS/JSON feeds for published content
- **Versioning**: Track revisions to published entries (edits, updates)

---

*This file is the architecture guide. Content goes in subfolders.*
