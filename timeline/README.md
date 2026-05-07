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

## Block headers — contribution markers

Skills that write to a daily timeline entry add a *contribution
marker* at the top of their block. The marker is just a header
that says "what added this and when" — it doesn't constrain the
content beneath it.

| Header | Source skill | Content shape |
|---|---|---|
| `## STATE HH:MM` | `/check-in` | Multi-axis structured (Mood, Energy, Focus, Body, Social, Context, Note) |
| `## NOTE HH:MM` | `/journal` | Freeform — whatever the user writes |
| `## MORNING HH:MM` | `/morning` | Today's intention (one line, usually) |
| `## EVENING HH:MM` | `/evening` | Stayed open / Surprises / Tomorrow |

State blocks are the only headers with a structured body —
trend skills grep `## STATE` and parse the axis labels. The
rest are just time-stamped markers; the content beneath them
is whatever the moment needs.

You can also write content with no header at all (just append
freeform). The headers are for skill-written contributions, not
a requirement.

Example of a daily file with multiple block types:

```
# 2026-05-07

## MORNING 08:30

Intention: get the rAI deck in shape for Thursday.

## STATE 08:35

Mood:    focused, slightly tired
Energy:  6/10
Focus:   clear
Context: Tuesday morning, deck day

## NOTE 11:42

The Slide 7 framing isn't landing. Need a different angle on
"why now." Maybe lead with the regulatory shift instead of the
tech. Trying something at lunch.

## STATE 14:32

Mood:    grinding
Energy:  5/10
Focus:   narrowing
Context: 3 hours into the deck rework

## EVENING 22:15

Stayed open: slide 7 still not solved
Surprises:   the architecture diagram took twice as long as expected
Tomorrow:    fresh eyes on slide 7 first thing
```

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
