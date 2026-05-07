---
name: evening
description: Daily wrap. Reads today's `timeline/YYYY/MM/DD.md`, prompts for closing reflections (what stayed open, what surprised you, what tomorrow needs to start with), appends an `## EVENING HH:MM` block, and rewrites `.claude/welcome.md` so tomorrow's first read is oriented. The bookend to `/morning`. Triggered by "/evening", "wrap the day", "end of day", "good night", "before I go to bed".
---

# /evening — Daily wrap and welcome.md rewrite

Close the day. Capture what stayed open. Rewrite `welcome.md`
so tomorrow's first session reads oriented context, not stale
yesterday-context.

## Behavior contract

- **Two writes:**
  1. `## EVENING HH:MM` block appended to today's
     `timeline/YYYY/MM/DD.md`.
  2. `.claude/welcome.md` rewritten in full (replace, not
     append). Future Claude sessions read welcome.md on start.
- **Read today's full timeline first.** The evening block
  references the day's content; can't compose without
  reading first.
- **Be willing to sit with the day.** If the user gave a
  hard answer to "what surprised you", don't pivot to
  next-steps unless they ask. Witness, then write.
- **No auto-commit.**

## Process

### Step 1 — Get timestamp

`date '+%Y-%m-%d %H:%M'`.

### Step 2 — Read today's timeline

Read today's `timeline/YYYY/MM/DD.md` in full. Note:
- State blocks logged today (count + latest)
- NOTE blocks logged today (count, brief content)
- MORNING block (the day's intention)
- Other freeform content

### Step 3 — Show today's recap

```markdown
**Today's timeline so far:**

- MORNING <HH:MM>: "<intention>"
- <N> state check-ins (latest at <HH:MM>: Mood/Energy summary)
- <N> notes
- <other blocks if any>

**A few questions to wrap:**

1. **What stayed open?** Threads still in motion, things you
   didn't get to.
2. **What surprised you?** Anything — a feeling, a person, a
   thing that worked, a thing that didn't.
3. **What does tomorrow need to start with?** The thing
   tomorrow's `/morning` should surface first.

Skip any that don't apply.
```

Wait for answers.

### Step 4 — Compose the EVENING block

```
## EVENING HH:MM

Stayed open: <user's answer or "—" if skipped>
Surprises:   <user's answer or "—">
Tomorrow:    <user's answer or "—">
<optional free-form closing line if the user added one>
```

### Step 5 — Compose the welcome.md rewrite

```markdown
# 👋 Welcome back

> First file Claude reads on session start. Updated by
> `/evening` (or by hand). Stays small.

---

## Where I left off

<2-3 sentence synthesis from today's intention + what stayed
open + tomorrow line. Concrete, not abstract.>

## Heads up

<Bullets — sharp-edge items the next session needs to know.
Pulled from "what stayed open" + "surprises" if any are
unfinished/risky. Empty is fine if there are none.>
- ...

## What to read first

- [`tasks/active/`](../tasks/active/) — anything in flight
- [`timeline/<today>.md`](../timeline/<today>.md) — today's record
- [`.claude/state.md`](state.md) — last state check-in

## Active mode

- `<from .claude/mode.md if present, else "normal">`

---

*Last updated by `/evening` on <YYYY-MM-DD>.*
```

### Step 6 — Preview both writes

```markdown
**About to write:**

1. Append to `timeline/<today>.md`:

   \`\`\`
   ## EVENING HH:MM

   <block>
   \`\`\`

2. Rewrite `.claude/welcome.md` (~15 lines):

   \`\`\`
   <full file content>
   \`\`\`

Proceed? (yes / edit / skip the welcome rewrite)
```

### Step 7 — Write

1. Append EVENING block to today's timeline.
2. Replace `.claude/welcome.md` content with the new version.

### Step 8 — Closing summary

```markdown
✓ Day wrapped.

- `timeline/<today>.md` — EVENING block added.
- `.claude/welcome.md` — rewritten for tomorrow's morning read.

Get some sleep.
```

## Style rules

- **Block axes are: Stayed open, Surprises, Tomorrow.** Same
  spelling and capitalization for grep-ability.
- **Welcome.md is short.** ~15 lines. Tomorrow needs the
  punch, not a wall.
- **"—" for skipped axes.** Honest about what wasn't shared.
- **"Get some sleep" closing is fine.** The vibe is
  warm-but-direct (peer who has my back). Not "Sweet dreams!"
  — that's flattery. Not "Goodnight." — that's stilted.

## What you must NOT do

- **Don't moralize about what stayed open.** "Three things
  stayed open" is data, not failure.
- **Don't suggest the user push through to finish things.**
  Evening is for closing, not pushing.
- **Don't auto-trigger anything for tomorrow.** The /morning
  skill handles tomorrow.
- **Don't summarize feelings.** "It sounds like you're tired"
  is unwelcome. The user already wrote what they wrote.
- **Don't auto-commit.**

## Edge cases

- **Today's timeline file doesn't exist.** Means the day was
  silent — no morning, no notes, no state. Ask: "*Quiet day —
  do you still want to log an evening, or skip?*"
- **EVENING block already exists for today.** Ask whether to
  overwrite or skip. (Multiple evenings is rare and
  confusing.)
- **User skips all three questions.** Log a minimal block:
  ```
  ## EVENING HH:MM

  (silent close)
  ```
  Welcome.md still rewrites with what's available.
- **The user shares something heavy in answer to a question
  ("what surprised me — that I'm not okay").** Witness first.
  Don't rush to write the block. Acknowledge: "*That's worth
  sitting with. Want to talk about it before logging, or just
  log it as-is?*"
- **State has shifted significantly during the day.** Welcome's
  "Heads up" should mention it: e.g. "*State went from energetic
  → drained over the day — surfacing here so tomorrow's morning
  doesn't assume yesterday's energy.*"

## When NOT to use this skill

- **Mid-day check-in** → `/check-in`.
- **Logging a specific thought** → `/journal`.
- **End-of-week reflection** → `/weekly-review` *(future)*.
- **Stepping away for longer (vacation, leave)** → use the
  surviving (and life-flavor-pending) `/handoff`.

## What "done" looks like for an /evening session

`## EVENING HH:MM` block in today's timeline + a rewritten
`.claude/welcome.md` reflecting where things actually stand.
Working tree dirty. Tomorrow's `/morning` will pick up clean
context.
