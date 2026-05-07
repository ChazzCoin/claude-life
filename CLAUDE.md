# claude-life

Personal life-management system for Chazz Romeo. Single-user,
private repo, accessed from any device. Markdown-only. This is
not a software project — it's a system for managing my actual
life: memories, reminders, todos, work life (VSI + rAI),
personal life, goals, and the alignment between them.

> This file extends the global contract at `~/.claude/CLAUDE.md`.
> It does not override it. The global contract owns honesty,
> tone, and working style. This file owns project-specific
> structure and the privacy/sharing posture.

## 🪡 Auto-loaded primitives

These imports run on every session start:

@.claude/welcome.md
@.claude/pact.md
@.claude/values.md
@.claude/vision.md
@.claude/life-rules.md
@.claude/mode.md
@.claude/wont-do.md
@.claude/bookmarks.md

*Why each one:*
- **welcome.md** — where I left off, what's live. Updated by `/handoff`.
- **pact.md** — life-specific working preferences with Claude.
- **values.md** — north star. The anchor.
- **vision.md** — long-arc direction.
- **life-rules.md** — universal rules for working on life.
- **mode.md** — currently-active mode (only present when one is active).
- **wont-do.md** — closed conversations / anti-list.
- **bookmarks.md** — re-find pointers across the repo.

## What this is

claude-life manages the whole of one person's life across:

- **VSI** — co-founder + CTO
- **rAI** — co-founder + CTO
- **Personal** — health, relationships, finances, growth, identity
- **Aligned journey** — the through-line. Are today's actions
  consistent with `values.md` and `vision.md`? A recurring check,
  not a doc.

The structure is layered:
- **Anchors** (`values.md`, `vision.md`) — change rarely.
- **Domains** (VSI, rAI, personal) — ongoing context per area.
- **Tasks** (`tasks/{backlog,active,done,recurring}/`) — things to do.
- **Journal** (`journal/YYYY/MM/DD.md`) — raw daily processing.
- **Memories** (`memories/{people,events,places}/`) — durable facts.
- **Docs** (`docs/{decisions,regrets,retros,lessons,audits,...}/`)
  — reflective records.

## Privacy posture

**Everything is private by default.** This repo never goes public.
External-facing data is published via an explicit `/publish`
projection (Wave 4 — see `tasks/PHASES.md`). Sharing is an active
decision per piece, never a directory placement.

## Phone access

This repo gets read and edited from any device — laptop, phone,
iPad. Hard constraint: **markdown only, no special tooling
required to read.** Paths stay readable. Filenames stay readable.
No binaries unless they're inputs to `/publish`.

## Domains

Domain-specific rules live in prefix files (added in Wave 3):

- `.claude/vsi-rules.md` — VSI-specific
- `.claude/rai-rules.md` — rAI-specific
- `.claude/personal-rules.md` — personal life

Tasks cut across domains via filename prefix
(`vsi-2026-q2-fundraising.md`, `personal-health-bloodwork.md`),
not separate folders. A task can be VSI-and-personal at once —
folders force false partitioning.

## Phase

Current chapter — see [`tasks/PHASES.md`](tasks/PHASES.md).

## Where things live

```
CLAUDE.md                 # this file
.claude/                  # skills, modes, rules, primitives
  values.md vision.md     # anchors
  life-rules.md           # universal rules
  vsi-rules.md            # domain (Wave 3)
  rai-rules.md            # domain (Wave 3)
  personal-rules.md       # domain (Wave 3)
  pact.md welcome.md ...  # primitives
  skills/                 # slash commands
  modes/                  # drives
tasks/
  backlog/ active/ done/  # lifecycle
  recurring/              # daily/weekly rituals
  PHASES.md ROADMAP.md AUDIT.md
journal/YYYY/MM/DD.md     # daily entries
memories/
  people/<name>.md
  events/ places/
docs/
  decisions/ regrets/ retros/ notes/   # reflective records
  audits/ handoff/ postmortems/ gratitude/ exports/
api/                      # external projection layer (Wave 4)
README.md                 # private — for me, on my phone
```

## Session start

Read `welcome.md`, then check `tasks/active/`, then today's
`journal/` entry if one exists. Or run `/morning` (Wave 2).

## On honesty

The global CLAUDE.md owns this in detail. Worth restating the
short form here because life-content is harder to be blunt about
than code:

- "I don't know" is allowed and expected.
- No narratives. No spin. No shaping the conclusion to feel good
  or bad. The point is progress on the question, not comfort.
- Calibrate confidence: "I checked X" vs "I think Y based on Z"
  vs "this is a guess."

This is non-negotiable.

## Pause points / open questions

Tracked in [`tasks/AUDIT.md`](tasks/AUDIT.md) under their dates.
Empty is fine.
