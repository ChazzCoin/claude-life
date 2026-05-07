---
name: morning
description: Daily kickoff. Renders a morning brief from `.claude/welcome.md`, `tasks/active/`, current chapter (`tasks/PHASES.md`), and `.claude/state.md`. Prompts for today's intention and appends a `## MORNING HH:MM` block to today's `timeline/YYYY/MM/DD.md`. Read-mostly skill — minimal writing, lots of orientation. Surfaces `/check-in` if state is stale (>12 hours). Triggered by "/morning", "good morning", "start my day", or session-start when the user signals beginning-of-day.
---

# /morning — Daily kickoff brief

Render the morning context (where I left off, what's active,
current chapter, current state). Prompt for today's intention.
Log the intention as a timeline entry block. Get the user
oriented in 60 seconds.

## Behavior contract

- **Read-heavy, write-light.** This skill primarily reads and
  renders. The only write is one `## MORNING HH:MM` block to
  today's timeline.
- **Don't fabricate.** If `welcome.md` is empty or stale, say
  so. If no chapter is active, say so. If no tasks are active,
  say so.
- **Surface options, don't act.** Suggest `/check-in` if state
  is stale. Suggest `/journal` if the user wants to write.
  Don't auto-trigger.
- **One MORNING block per day.** If today's timeline already
  has a `## MORNING HH:MM` block, ask whether to overwrite,
  add a second one, or skip the write.
- **No auto-commit.**

## Process

### Step 1 — Get today's date and time

`date '+%Y-%m-%d %H:%M %A'` — date, time, day name. Day name
is human-readable in the brief.

### Step 2 — Read sources in parallel

- `.claude/welcome.md`
- `.claude/state.md`
- `tasks/PHASES.md` (extract the active chapter)
- `tasks/active/*.md` (list all)
- Today's `timeline/YYYY/MM/DD.md` *(check if MORNING block
  already exists)*
- Yesterday's `timeline/YYYY/MM/DD.md` *(if exists, optional —
  for the "yesterday" line)*

### Step 3 — Render the brief

```markdown
# Good morning — <YYYY-MM-DD> <DayName>

## Where you left off
<from welcome.md, the "Where I left off" section>

## Current chapter
**<chapter name>** — <North Star>

## Active tasks (<N>)
- TASK-NNN — <title>
- TASK-NNN — <title>
*(or: "Nothing in `tasks/active/`. Pull from backlog?")*

## Last state check-in
<from state.md, summarized in 1-2 lines, with timestamp>
*(if older than 12 hours, add: "*State is from <Xh ago> — want
to /check-in?*")*

## Yesterday
<one-line distillation of yesterday's timeline if present;
skip section if absent>

---

**What's the intention for today?** One line — the thing that
matters most, even if other things also happen.
```

### Step 4 — Get the intention

Wait for the user's response. If they skip ("nothing specific"
/ "just getting through it"), log that as-is — it's data.

### Step 5 — Preview the write

```markdown
**About to log to `timeline/<path>`:**

\`\`\`
## MORNING HH:MM

Intention: <user's intention>
\`\`\`

*(If the user's response was longer than one line, render the
full block.)*

Proceed? (yes / edit / skip the write)
```

### Step 6 — Write

Append `## MORNING HH:MM\n\nIntention: <text>\n` to today's
timeline. Create the file if needed (with `# YYYY-MM-DD\n\n`
header).

### Step 7 — Closing summary

```markdown
✓ Morning logged. Today's intention is on the record.

Next moves:
- `/check-in` if state is stale
- Pick a task: <best candidate from active/, terse>
- `/journal` if anything else wants to be written

Have a good one.
```

## Style rules

- **Day name in header.** "Tuesday" beats "2026-05-07" alone
  for human readability.
- **Counts surface.** "Active tasks (3)" — the number matters.
- **Empty states render honestly.** "Nothing in `tasks/active/`"
  beats rendering nothing.
- **Yesterday line is one sentence max.** Distill, don't dump.
- **No emoji in the brief output.** Stays text-clean for phone
  reading.

## What you must NOT do

- **Don't lecture about the intention.** If the user says "just
  survive today," log "just survive today." Don't reframe to
  "build resilience".
- **Don't auto-trigger /check-in even when state is stale.**
  Surface; let the user pick.
- **Don't pre-pick a task.** "Pick a task" is the user's call.
  Surfacing the best candidate is fine; assigning is not.
- **Don't write a long brief.** Aim for ~15-25 lines.
  Phone-readable.
- **Don't fabricate a yesterday summary if no yesterday file
  exists.** Skip the section.
- **Don't auto-commit.**

## Edge cases

- **First-ever morning** (no welcome.md content yet): render
  "*This is the first /morning. Welcome.md is empty — that's
  fine. Today's intention?*" and skip the where-you-left-off.
- **Welcome.md is hand-edited and the structure differs from
  the template.** Render whatever's there as-is. Don't
  reformat.
- **State is from yesterday or older.** Surface "*State is
  from <X> — /check-in?*" prominently.
- **MORNING block already exists in today's file.** Ask:
  - **Overwrite** — replace it.
  - **Append second** — add a new block (rare; mid-day
    re-orient might want this).
  - **Skip** — render the brief but don't write.
- **No active chapter** (PHASES.md has only "Queued" or empty
  state): "*No active chapter. Run `/plan` to think through
  one?*"
- **It's actually 11pm, not morning.** Run anyway. The user
  may be in a different timezone or just running /morning to
  get the brief. Don't gate on time.

## When NOT to use this skill

- **Mid-day re-orient** → `/oncall`.
- **End of day** → `/evening`.
- **Just want to log a thought** → `/journal`.
- **Just want to log state** → `/check-in`.

## What "done" looks like for a /morning session

A rendered brief on screen + (optionally) one `## MORNING HH:MM`
block in today's timeline. The user knows the day's intention
and the next move. Working tree dirty if a write happened;
clean if the user skipped the write.
