# Memories

Durable long-term facts. Reference, not reflection.

```
memories/
├── people/<name>.md      # one file per person
├── events/<slug>.md      # significant events
└── places/<slug>.md      # places that matter
```

---

## What goes here vs. journal vs. docs

- **Journal** is processing in time — today's thinking. Fast,
  raw, chronological.
- **Docs** are reflective records — decisions, regrets,
  lessons, retros. Curated.
- **Memories** are durable facts — names, dates, relationships,
  contexts. The reference layer.

When Claude needs to know who someone is, what happened on a
date, or what a place means → memories.
When Claude needs to know what I learned, decided, or regret →
docs.
When I'm thinking *now* about *now* → journal.

## People

`memories/people/<lowercase-name>.md`. Suggested shape (adapt
freely):

- **Who** — relationship, role, how I know them
- **What I know** — context that wouldn't be obvious to a
  stranger
- **Recent state** — what's going on with them lately
- **Patterns** — how they communicate, what works/doesn't
- **Open threads** — last conversation, things still in motion

When Claude is about to write to or about a person, it should
read this file first.

## Events

`memories/events/YYYY-MM-DD-<slug>.md`. Things that happened
that need durable record beyond a journal entry. Births,
deaths, moves, milestones, watershed conversations.

## Places

`memories/places/<slug>.md`. Places with weight — homes,
hometowns, places where meaningful things happened.

## Privacy

Memories are private by default. Some people-files are
extremely sensitive. The `/publish` projection should require
an explicit allow-list before exposing any memory.
