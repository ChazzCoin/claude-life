---
name: oncall
description: Read-only synthesis. Renders "what's live in life right now" — the chapter, current state, active tasks, last 3 days of timeline, recent decisions, and pending recurring rituals. Use when re-orienting after a context switch, after a long break, or any time the question is "where am I right now?". Distinct from `/morning` (which is daily kickoff with a write) and `/status` (which is broader but more historical). Triggered by "/oncall", "what's live", "where am I", "what am I in the middle of", "context switch", "I just got back".
---

# /oncall — What's live in life right now

Pure read. Synthesize the present moment from across the repo
into a 30-second re-orient. No writes, no prompts, no
questions. Just: here's what's live.

The skill name borrows from on-call rotations — when you come
back to the bleeper, you need to know what's hot *right now*,
not what happened last week.

## Behavior contract

- **Read-only.** Zero writes. Zero file modifications.
- **One screen.** The output should fit one phone screen
  (roughly 25 lines). Distill, don't dump.
- **Cite paths.** Every claim points at a file. Re-orientation
  needs to be drillable.
- **Surface staleness.** If state.md is from yesterday, say so.
  If welcome.md is from a week ago, say so.
- **No interpretation.** Don't analyze patterns ("looks like
  you've been overworked"). The user can read the same data.

## Process

### Step 1 — Read sources in parallel

- `.claude/state.md` — current state
- `tasks/PHASES.md` — find active chapter
- `tasks/ROADMAP.md` — find this-week / this-quarter goals
- `tasks/active/*.md` — active task list
- `tasks/recurring/*.md` — recurring rituals (check which are
  due based on filename or content cadence; this is fuzzy
  for now)
- Last 3 days of `timeline/` — synthesize one line per day
- `docs/decisions/` — last 3 by date
- `docs/regrets/` — last 1 if any in the past 30 days
- `.claude/mode.md` — current mode if active
- `.claude/welcome.md` — last-updated date (for staleness)

### Step 2 — Render the snapshot

```markdown
# What's live — <YYYY-MM-DD HH:MM>

## Chapter
**<chapter name>** — <North Star>
*(or: "*No active chapter. Run `/plan`.*")*

## State
<latest state, 1-2 lines, with timestamp>
*(if stale >12h: "*State is <Xh> old — `/check-in`?*")*

## Mode
<active mode if any, else "normal">

## Active tasks (<N>)
- [TASK-NNN](../tasks/active/<file>) — <title>
- [TASK-NNN](../tasks/active/<file>) — <title>
*(or: "*Nothing in `tasks/active/`. Pull from backlog?*")*

## This week's goals
<from ROADMAP.md "This week" section>
- ...

## Last 3 days
- **<YYYY-MM-DD>** — <one-line distillation>
- **<YYYY-MM-DD>** — <one-line distillation>
- **<YYYY-MM-DD>** — <one-line distillation>

## Recent decisions (last 3)
- [<date> — <title>](../docs/decisions/<file>)
- ...
*(or skip section if none)*

## Recurring due
- <ritual> — last done: <date>
*(or skip section if none due)*

## Stale flags
*(only if applicable — render zero or more lines)*
- welcome.md last updated <Xd> ago — `/evening` to refresh
- state.md last updated <Xh> ago — `/check-in`
- No timeline entry today yet
```

### Step 3 — Done

No closing summary, no next-step prompts. The output is the
output. The user reads and decides what to do next.

## Style rules

- **Headers are nouns, not actions.** "Chapter", "State",
  "Active tasks". Not "Your chapter" or "What you're doing".
- **Counts in parens.** "Active tasks (3)" — the number
  surfaces immediately.
- **Stale flags at the bottom.** They're hygiene, not signal.
- **Distillation, not dump.** "Yesterday: state stayed mid-day,
  one note about the cofounder call, no morning logged" — one
  line per day.
- **No emoji.** This is a snapshot, not a celebration.
- **Cite via relative paths from repo root** — `tasks/active/...`,
  `docs/decisions/...`. Phone markdown viewers handle these.

## What you must NOT do

- **Don't write anything.** No writes, period.
- **Don't ask questions.** This skill is render-and-stop.
- **Don't analyze.** "You've had three states logged today,
  energy trending down" is interpretation. Skip.
- **Don't suggest action items.** Stale flags surface
  staleness; they don't say "do this now".
- **Don't aggregate across 3+ months.** Three days is the
  window. Longer windows belong to retros and reviews.
- **Don't auto-commit.** (Nothing to commit anyway — read-only.)

## Edge cases

- **Empty repo state** (first day, nothing logged): render
  "*Repo is fresh. Run `/morning` to start logging.*"
- **No PHASES.md content** (template not filled in): render
  "*Chapter not yet set. Run `/plan` to think through one.*"
- **No active tasks**: render "*Nothing in `tasks/active/`.
  Pull from `tasks/backlog/`?*"
- **timeline/ doesn't exist yet** (no daily entries logged):
  skip the "Last 3 days" section, render nothing in its
  place.
- **Multiple chapters marked Active** (shouldn't happen but
  could): render all, flag the inconsistency: "*Multiple
  active chapters — fix in PHASES.md.*"
- **State.md is the unfilled template**: render "*State not
  yet checked in. `/check-in`?*"
- **All files are very stale**: surface in stale flags. Don't
  catastrophize — "*welcome.md is 14d old — that's fine if
  you've been away*".

## When NOT to use this skill

- **Daily kickoff (with a write)** → `/morning`.
- **Daily wrap** → `/evening`.
- **Logging a thought** → `/journal`.
- **Logging state** → `/check-in`.
- **Multi-week pattern review** → `/weekly-review`,
  `/monthly-review` *(future)*.
- **Full project state for handover** → `/handoff` *(life-
  flavor pending in Wave 2.5)*.

## What "done" looks like for an /oncall session

A single rendered snapshot, no files modified. The user knows
chapter / state / active / this-week / last-3-days / recent
decisions / staleness — without scrolling through the repo.
