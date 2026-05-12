---
name: unpublish
trigger: /unpublish
description: |
  Remove a file from the public projection. Reverses /publish.
  
  The original private file stays untouched; only the public export is removed.
---

# /unpublish

Remove a published file from the public API projection.

## Trigger syntax

```
/unpublish <filename>          # Remove a file by public path
/unpublish --file=<path>       # Explicit original file reference
/unpublish --list              # Show currently-published files
```

## Behavior

1. **Find the entry** — look in api/public/INDEX.md
2. **Confirm deletion** — show what will be removed
3. **Delete from api/public/** — remove the exported file
4. **Update INDEX.md** — remove the entry (mark as archived, optionally)
5. **Audit log** — log removal in tasks/AUDIT.md

## Confirmation flow

```
User: /unpublish decisions/2026-05-01-hiring.md

Claude:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🗑️  Unpublish: decisions/2026-05-01-hiring.md

Original (private) file: docs/decisions/2026-05-01-hiring.md
Published (public) location: api/public/decisions/2026-05-01-hiring.md
Published: 2026-05-07

Will remove:
  - api/public/decisions/2026-05-01-hiring.md
  - Entry from api/public/INDEX.md

Original file remains untouched and private.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Confirm? (yes/no)
```

User confirms → Claude executes.

## What happens

1. **Deletes**: `api/public/decisions/2026-05-01-hiring.md`
2. **Updates**: `api/public/INDEX.md` (removes entry)
3. **Optionally archives**: Copies INDEX entry to archive log if useful for
   future reference
4. **Logs**: Records in `tasks/AUDIT.md` with reason

Example AUDIT.md entry:
```
- 🏗 **Unpublished decision: 2026-05-01-hiring.md.**
  Reason: too specific, decided to keep private.
```

## Reasons to unpublish

- Content became too specific / sensitive over time
- Information was wrong and publishing it is worse than silence
- Changed mind about what should be public
- Older content that's no longer relevant

## Notes

- **Reversible**: Can re-publish the original private file later with `/publish`
- **Audit trail**: INDEX.md and AUDIT.md record what was published and when
  removed
- **Original untouched**: The private file (e.g.,
  `docs/decisions/2026-05-01-hiring.md`) stays exactly as it was
- **Bulk**: Future: `--batch <pattern>` to unpublish multiple at once

## Implementation

- Read `api/public/INDEX.md` to find the entry
- Ask for confirmation with context
- Delete from `api/public/<type>/<file>`
- Update INDEX.md
- Log in AUDIT.md
- (Optional: create `api/archive/unpublished.md` with history of what was
  published then removed)

---

*Complements /publish. Keeps the projection in sync with user intent.*
