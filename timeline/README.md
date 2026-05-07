# Timeline

The spine of this repo. Every dated piece of life — events,
journal entries, state check-ins, retroactive memories from any
era — lives here. One unified tree.

> The "journal" concept dissolves into this. Today's daily file
> *is* a timeline entry; the only difference between today and
> 1995 is which folder the file lands in.

---

## Filename conventions

| When you know | Filename |
|---|---|
| Full date (today, dated past event) | `timeline/YYYY/MM/DD.md` |
| Multiple events on the same day | `timeline/YYYY/MM/DD-<slug>.md` |
| Month, not day | `timeline/YYYY/MM-<slug>.md` |
| Year, not month | `timeline/YYYY/<slug>.md` |
| No year — just an era | `timeline/era/<eraname>/<slug>.md` |

ISO prefix everywhere it's known. Sortable by `ls`.

## Inside the file: no template

Timeline entries are freeform. Format follows content:

- Today's brain-dump → free prose, bullets, whatever
- A childhood memory → paragraphs, fragments, sense-memories
- A factual event → bullet points or a list of facts
- A poem-shaped memory → a poem
- A transcript of a hard conversation → dialogue
- Any combination of the above

The file shape is yours. No required headings, no required
sections. The entry's *form* should match its *content*.

## The one structured exception: state blocks

When `/check-in` runs (Wave 2), it appends a structured block to
today's timeline entry. State blocks have a consistent shape so
trend skills can grep them across files later:

```
## STATE 14:32

Mood:    focused, less tired
Energy:  7/10
Focus:   clear
Body:    sore neck
Social:  isolated this week
Context: rAI fundraise call went well
```

The rest of the file ignores this shape — write whatever else
you want, however you want. State blocks are isolated by their
header convention; everything else is narrative.

## Cross-references

- A timeline entry that's about a person can link to the
  relevant `memories/people/<category>/<name>.md`.
- A people-file's "history" or "open threads" sections can
  link back to specific timeline entries.
- Cross-linking is **manual**. Don't auto-link.

## Privacy

Timeline is the most-private layer (along with `state` blocks
inside it). Default assumption: nobody reads this except
future-me. The `/publish` projection (Wave 4) should *never*
surface timeline contents without explicit per-entry
allow-listing.

## Today

Today's file: `timeline/YYYY/MM/DD.md`. If it doesn't exist,
the first entry creates it.

Skills that write here:
- `/check-in` — appends a state block *(Wave 2)*
- `/journal` — opens or appends to today's file *(Wave 2)*
- `/morning`, `/evening` — bookend rituals *(Wave 2)*
