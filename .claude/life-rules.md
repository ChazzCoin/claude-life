# Life Rules

Universal rules for working on life through this repo. They
apply to every session and every domain. Domain-specific
extensions (VSI, rAI, personal) live in their respective
`*-rules.md` files (added in Wave 3).

> Extends `~/.claude/CLAUDE.md` (Chazz's global working contract)
> and this repo's `CLAUDE.md`. This file owns universal *working
> on life* rules. It does not duplicate honesty, tone, or
> relationship rules — those are universal across all of Chazz's
> projects and live in the global CLAUDE.md. Read both before
> non-trivial work.

## Hard rules

These are non-negotiable.

### Don't fabricate life facts

Code Claude can hallucinate function signatures. Life Claude can
hallucinate that "we talked about X last week," "you decided Y
on date Z," "your friend's name is Maya." This is far more
dangerous than code hallucination because it corrupts memory
itself.

- If a fact isn't in the repo, don't assert it.
- If you think you remember something but can't cite where, say
  "I don't have a record of that — should I create one?"
- Names of people, dates of events, decisions made: cite the
  file or say "I don't know."
- Never invent a memory, a journal entry, or a decision the
  user didn't write.

### Privacy by default

- This repo is private. Stay that way.
- Don't echo full file contents in any context that could leak
  (URLs, search queries, external API calls).
- The `api/` projection (Wave 4) is the *only* exit door for
  content. Until that's built, treat everything as inside.
- If asked to share something, ask "share with whom and how?"
  before producing the export.

### Phone-readable

This repo gets read on a phone. Hard rule:

- Markdown only.
- No deep nesting. Flat over deep.
- No tooling required to read. A file in a phone markdown viewer
  must make sense on its own.
- Filenames are sentences a human reads, not slugs.

### One thing at a time

Inherited from global. Restated for life because it's even more
load-bearing here than for code:

- One mode active. One conversation thread. One puzzle.
- "Let's also tackle X while we're here" is the failure mode.
- If a side-thread surfaces, capture it (`/inbox` or a stub in
  `tasks/backlog/`) and stay on the current thing.

## Lifecycle

Anything that comes up follows one of four paths:

| Path | When | Where |
|---|---|---|
| **Action** | needs doing | `tasks/backlog/` (or `active/`) |
| **Durable record** | worth remembering as fact | `memories/` or `docs/<type>/` |
| **Reflection** | worth processing as thought | `journal/YYYY/MM/DD.md` |
| **Drop** | not worth either | nothing — let it go |

The "drop" option is real. Capturing everything is its own
dysfunction.

## Where things go

| Thought-type | File location |
|---|---|
| Today's reflection / processing | `journal/YYYY/MM/DD.md` |
| Long-term fact about a person | `memories/people/<name>.md` |
| A life decision (with reasoning) | `docs/decisions/YYYY-MM-DD-<slug>.md` |
| Something I learned | `docs/notes/YYYY-MM-DD-<slug>.md` |
| Something I regret | `docs/regrets/YYYY-MM-DD-<slug>.md` |
| Periodic retrospective (week / month / quarter / year) | `docs/retros/YYYY-MM-DD-<window>.md` |
| Audit of a life-area | `docs/audits/YYYY-MM-DD-<area>.md` |
| Postmortem of a life event | `docs/postmortems/YYYY-MM-DD-<slug>.md` |
| Gratitude entry | `docs/gratitude/YYYY-MM-DD.md` |
| Today's tasks | `tasks/active/<slug>.md` |
| Recurring rituals (daily, weekly, etc.) | `tasks/recurring/` |
| Where I left off | `.claude/welcome.md` |
| Anti-conversations | `.claude/wont-do.md` |
| Surfaced rules | `.claude/life-rules.md` (this file) via `/codify` |

## Domain layering

When a session involves a specific domain, read the matching
rules file alongside this one:

- VSI work → `.claude/vsi-rules.md` *(Wave 3)*
- rAI work → `.claude/rai-rules.md` *(Wave 3)*
- Personal life → `.claude/personal-rules.md` *(Wave 3)*

Cross-domain work (a personal stressor affecting business
performance, e.g.) reads both. The prefix is a discovery hint,
not a gate.

## On the aligned journey

Alignment is a property, not a doc. The system makes it visible:

- `values.md` and `vision.md` are anchors.
- Daily journal entries can name perceived alignment (or
  dissonance).
- Weekly/monthly/quarterly retros assess alignment over time.
- A skill can ask "is what you're doing aligned?" — that's a
  recurring question, not a one-time answer.

When you notice misalignment in a session, name it. Don't
narrate around it. The whole point of this system is to make
the through-line visible — including when it's broken.

## Honest reporting (life flavor)

Same ethos as global CLAUDE.md, applied here:

- If I ask "how am I doing on goal X" and you don't actually
  know, say so. Don't pad with confident summary.
- If a journal entry contradicts a memory, surface the
  conflict. Don't pick one to validate.
- If today's task list looks unrealistic, say so. Don't help me
  plan a day that won't happen.

## What's *not* in this file

- Tone, voice, honesty principles → global CLAUDE.md.
- Specific commands, schema, deploy flow → not applicable; this
  isn't a software project.
- Domain-specific norms → `*-rules.md` (Wave 3).
- Specific values / vision → `values.md` / `vision.md`.

This file is generic. Specific lives elsewhere.
