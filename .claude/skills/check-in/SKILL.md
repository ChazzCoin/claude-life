---
name: check-in
description: Capture a multi-axis state snapshot — Mood, Energy, Focus, Body, Social, Context. Updates `.claude/state.md` (latest only, auto-loaded into CLAUDE.md) and appends a structured `## STATE HH:MM` block to today's `timeline/YYYY/MM/DD.md` for history. Foundational skill — defines the state-block format that `/oncall`, `/morning`, and future trend skills depend on. State informs Claude's posture (softer when depleted, more energetic when energetic) but never overrides explicit user instructions. Triggered by "/check-in", "log my state", "how am I right now", "update state", or when the user shares fresh state info ("I'm feeling X today").
---

# /check-in — Multi-axis state snapshot

Capture how I am, right now, across the axes that matter for
how Claude shows up. Updates two places:
- `.claude/state.md` — replaced with latest block (auto-loaded
  into CLAUDE.md so future sessions know my current state).
- `timeline/YYYY/MM/DD.md` — appends a `## STATE HH:MM` block
  for history.

Per `life-rules.md`: state informs *posture*, not *permissions*.
Claude reads state to calibrate tone, cadence, and what to
push on — not to gate behavior.

## Behavior contract

- **One check-in = one state block.** No bulk-check-in.
- **Append to timeline; replace state.md.** History accumulates
  in timeline; state.md only carries the latest.
- **Timestamp is local time, HH:MM, 24-hour.** Use `date '+%H:%M'`.
- **Empty axes are allowed.** If the user doesn't have a read on
  Body or Social, omit that line from the block.
- **Don't paraphrase the user's words.** "Drained but
  determined" stays "drained but determined". Don't sand the
  edges.
- **Don't diagnose.** State is descriptive, not therapeutic.
  "Mood: lonely" is fine. "Mood: experiencing social isolation
  pattern" is wrong.
- **No auto-commit.**

## State block format

```
## STATE HH:MM

Mood:    <free text — multiple feelings welcome>
Energy:  <n/10 or descriptive>
Focus:   <clear / scattered / foggy / hyper / etc>
Body:    <free text — what the body is reporting>
Social:  <isolated / connected / strained / overwhelmed / etc>
Context: <one line — what's going on right now>
Note:    <anything else — optional>
```

The exact axis labels matter. Trend skills grep `## STATE` and
extract by axis label. Keep them stable.

## Process

### Step 1 — Get the timestamp

Run `date '+%Y-%m-%d %H:%M'` to get today's date and current
time. Today's date determines the file path; HH:MM goes in
the block header.

### Step 2 — Prompt for the axes

If the user invoked `/check-in` without details, prompt:

```markdown
**Quick state check.** Fill in what you have a read on; skip
what you don't.

- Mood?
- Energy (1-10 or word)?
- Focus (clear / scattered / foggy / etc)?
- Body?
- Social?
- Context (what's going on right now)?
- Anything else?
```

If the user already shared state in the conversation ("I'm sad
today, energy is low"), reflect what you heard back as a draft
block and ask for confirmation/additions:

```markdown
Drafting from what you shared:

## STATE 14:32

Mood:    sad
Energy:  low

Want to fill in the rest, or log it like this?
```

### Step 3 — Show preview

```markdown
**About to log:**

\`\`\`
## STATE HH:MM

Mood:    ...
Energy:  ...
Focus:   ...
Body:    ...
Social:  ...
Context: ...
Note:    ...
\`\`\`

This will:
- Replace `.claude/state.md` with this block.
- Append it to `timeline/YYYY/MM/DD.md`.

Proceed? (yes / edit X / cancel)
```

### Step 4 — Write

On approval:

1. **Ensure today's timeline path exists.** Create
   `timeline/YYYY/MM/` if needed.
2. **Append to today's file** (`timeline/YYYY/MM/DD.md`),
   creating it if it doesn't exist. Add a blank line before
   the new `## STATE HH:MM` block if the file is non-empty.
   If creating fresh, prepend `# YYYY-MM-DD\n\n` as the file
   header.
3. **Rewrite `.claude/state.md`** with this template:

   ```markdown
   # State

   Latest check-in. Auto-loaded into `CLAUDE.md` so Claude reads
   it on session start and adjusts posture accordingly.

   > **State informs posture, not permissions.** Heavy state →
   > softer. Energetic state → more momentum. Never overrides
   > what I explicitly ask for.

   History accumulates as `## STATE HH:MM` blocks in today's
   `timeline/YYYY/MM/DD.md`.

   ---

   <STATE BLOCK GOES HERE>

   ---

   *Updated: <YYYY-MM-DD HH:MM>. Run `/check-in` to update.*
   ```

### Step 5 — Closing summary

```markdown
✓ Check-in logged.

- `.claude/state.md` updated.
- Block appended to `timeline/<path>`.

Working tree dirty. `git diff` to review.
```

## Style rules

- **Header is exact.** `## STATE HH:MM` — no extra text on the
  header line. The `STATE` token is what trend skills grep.
- **Axis labels are exact.** `Mood:`, `Energy:`, `Focus:`,
  `Body:`, `Social:`, `Context:`, `Note:`. Same casing every
  time.
- **One value per axis.** Multiple feelings go on one line
  comma-separated, not multiple lines.
- **Omit empty axes.** Don't write "Mood: (skipped)" or
  "Body: —". Just leave the axis out of the block.

## What you must NOT do

- **Don't add axes the user didn't ask for.** Mood / Energy /
  Focus / Body / Social / Context / Note is the canon. New
  axes need explicit user agreement (and probably a discussion
  about updating this skill).
- **Don't infer state from the conversation history without
  asking.** If the user seems sad based on tone, ask — don't
  log "Mood: sad" without confirmation.
- **Don't moralize about the state.** "Energy 2/10" is logged
  as "Energy 2/10". Not "Mood: drained — let's talk about why".
  Save the talking-about-it for after the log.
- **Don't auto-trigger other skills.** A heavy state doesn't
  auto-route to `/journal` or `/stuck`. Surface the option;
  let the user pick.
- **Don't archive prior state.md.** State.md is replace-only by
  design. The history lives in `timeline/`.
- **Don't auto-commit.**

## Edge cases

- **Today's timeline file doesn't exist.** Create
  `timeline/YYYY/MM/DD.md` with `# YYYY-MM-DD\n\n` as a
  readability anchor before appending the state block.
- **Multiple check-ins in the same minute.** Rare but possible.
  Append both blocks; they sort by file order.
- **User wants to edit the most recent block instead of
  appending.** Allowed if explicit ("edit my last check-in").
  Update both `.claude/state.md` AND the corresponding block
  in today's timeline file.
- **User says "I'm fine" for everything.** Log it as-is. Don't
  probe. "Mood: fine" is real data.
- **State contradicts what the user just said in conversation.**
  E.g. user says "I'm great!" then logs Mood: drained, Energy:
  2/10. Don't surface the contradiction — both can be true
  (great in spirit, drained in body) and the user has the
  right to log honestly.
- **Current time is past midnight.** Today's file is the new
  date, not the prior day's.

## When NOT to use this skill

- **Capturing a thought / reflection** → `/journal`.
- **Reviewing trends over time** → `/state-trend` *(future)*.
- **Snapshotting full life context** → `/oncall`.
- **Wrapping the day** → `/evening` (which may chain a final
  check-in).

## What "done" looks like for a /check-in session

A single `## STATE HH:MM` block written in two places (latest
in `.claude/state.md`, history-append in today's timeline).
Working tree dirty, uncommitted. Future Claude sessions read
`.claude/state.md` on start and calibrate posture accordingly.
