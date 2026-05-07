# Memories

Durable long-term facts. Reference, not reflection.

```
memories/
├── people/
│   ├── INDEX.md
│   ├── family/<name>.md
│   ├── business/<name>.md      # VSI, rAI co-founders, advisors, key reports
│   ├── core/<name>.md          # closest 3-5
│   ├── friends/<name>.md       # broader friends
│   └── lost/<name>.md          # deceased, faded, estranged
├── events/<slug>.md             # significant events
└── places/<slug>.md             # places that matter
```

---

## What goes here vs. timeline vs. docs

- **Timeline** is *time-keyed*. Today's thinking, today's
  events, retroactive memories from any era. One file per
  date or event-shaped piece.
- **Docs** are *reflective records* — decisions, regrets,
  lessons, retros. Curated.
- **Memories** are *durable facts* — names, dates,
  relationships, contexts. The reference layer.

When Claude needs to know who someone is → `memories/people/`.
When Claude needs the chronology of *a specific date or
event* → `timeline/`.
When Claude needs my thinking *now* about *now* → today's
timeline entry.

## People

`memories/people/<category>/<lowercase-name>.md`. Five
categories:

- **family** — relatives, blood and chosen
- **business** — co-founders (VSI, rAI), advisors, investors,
  key reports
- **core** — the closest 3-5; the ones who know me
- **friends** — broader friend circle
- **lost** — deceased, faded, estranged. Still in the record
  because the relationship was real and shaped me.

Suggested file shape (loose; adapt):

```markdown
# Name

**Relationship:** ...
**Status:** active / faded / estranged / deceased
**Where they live:** ...
**How we met:** [link to timeline entry if exists]

## YYYY-MM — Met
[freeform]

## YYYY-MM — [next milestone]
[freeform]

## YYYY-MM — Current
[most recent state]
```

**Append-only dated sections** — never erase, only add. The
shape of the file becomes a chronology of the relationship.

When Claude is about to write to or about someone, it should
read this file first.

## Events

`memories/events/YYYY-MM-DD-<slug>.md`. Things significant
enough for their own durable file beyond a timeline entry.
Births, deaths, moves, watershed conversations.

(Most events live in `timeline/` directly. The
`memories/events/` folder is for events you want extra-easy
to re-find by category, not date.)

## Places

`memories/places/<slug>.md`. Places with weight — homes,
hometowns, places where meaningful things happened.

## Privacy

Memories are private by default. Some people-files are
extremely sensitive. The `/publish` projection should require
an explicit allow-list before exposing any memory.
