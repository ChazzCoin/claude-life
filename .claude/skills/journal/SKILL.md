---
name: journal
description: Append to today's `timeline/YYYY/MM/DD.md` entry. Freeform — the user writes whatever shape the moment needs (prose, bullets, a fragment, a poem). Skill adds a `## NOTE HH:MM` header timestamp before the user's content so today's file stays readable as multiple time-stamped blocks. Distinct from `/check-in` (which writes structured state blocks). Triggered by "/journal", "let me write", "I want to journal something", "today is...", or any signal that the user wants to capture freeform thinking-in-time.
---

# /journal — Append to today's timeline entry

Today's `timeline/YYYY/MM/DD.md` is the catch-all for the day.
This skill is the lightweight way to add to it: timestamp it,
append it, get out of the way.

The format inside the entry is yours — prose, bullets, a poem,
fragments, a transcript of what someone said. The skill only
adds a `## NOTE HH:MM` header so multi-block files stay
readable.

The header is a *time marker, not a template* — the content
beneath it is whatever shape you write.

## Behavior contract

- **Append, never replace.** Each invocation adds a block.
- **Header is `## NOTE HH:MM`.** Distinguishes prose blocks
  from state blocks (`## STATE HH:MM`) and morning/evening
  blocks (`## MORNING HH:MM` / `## EVENING HH:MM`).
- **No template inside the block.** Whatever the user writes
  is what gets written. Don't structure their thinking.
- **No paraphrasing.** Quote the user verbatim if they shared
  content earlier in the conversation. Don't sand or
  summarize.
- **No commentary inside the entry.** The skill writes the
  user's words, not Claude's reflections on them. (Reflections
  can happen in conversation; they don't go in the file.)
- **Quick mode supported.** If the user runs `/journal <text>`
  with content inline, append it directly without prompting.
- **No auto-commit.**

## Process

### Step 1 — Get the timestamp

Run `date '+%Y-%m-%d %H:%M'`. Today's date for the file path,
HH:MM for the block header.

### Step 2 — Get content

**Mode A — Inline:** if the user passed text with the command
(`/journal Today was hard.`), use that directly. Skip prompt.

**Mode B — From prior conversation:** if the user's recent
turns contained journal-shaped content ("Today was hard. I
spent four hours on the fundraise and it didn't move...") and
they then said "/journal", quote the relevant block back and
ask for confirmation:

```markdown
Going to log this verbatim:

> Today was hard. I spent four hours on the fundraise and it
> didn't move. <continue>

Append to today's timeline? (yes / edit / no)
```

**Mode C — Empty invocation:** if the user just ran `/journal`
with no content, prompt:

```markdown
What do you want to journal? Whatever shape — prose, bullets,
fragments, a poem. Length doesn't matter. I'll add a `## NOTE
HH:MM` header and append.
```

Wait for the response. Then proceed.

### Step 3 — Preview (if non-trivial)

If the content is more than ~5 lines or contains anything that
looks load-bearing (a name, a date, a decision), show preview:

```markdown
**About to append to `timeline/<path>`:**

\`\`\`markdown
## NOTE HH:MM

<content>
\`\`\`

Proceed? (yes / edit / cancel)
```

For short entries (<5 lines, no flagged content), append
without preview.

### Step 4 — Write

Append `## NOTE HH:MM\n\n<content>\n` to today's
`timeline/YYYY/MM/DD.md`. If the file doesn't exist, create
it with `# YYYY-MM-DD\n\n` as a readability anchor first,
then append.

If the file is non-empty, ensure a blank line separates the
new block from prior content.

### Step 5 — Closing summary

```markdown
✓ Logged.

`timeline/<path>` — `## NOTE HH:MM` appended (<line count> lines).
```

## Style rules

- **Header exact.** `## NOTE HH:MM`. No suffix, no extra text.
- **Quote the user verbatim.** Especially when pulling from
  conversation history. Don't normalize phrasing, fix grammar,
  or trim.
- **Don't add closing summaries inside the entry.** "All in
  all, today was..." is Claude voice, not user voice. Skip.

## What you must NOT do

- **Don't paraphrase the user.** Verbatim or nothing.
- **Don't structure the user's thinking.** No "I notice
  you're feeling X — let me organize this into themes".
- **Don't comment on the content.** This is logging, not
  conversation.
- **Don't auto-trigger other skills.** Even if the entry
  reads like a decision or a regret — don't preemptively
  route. Suggest in the closing summary if relevant; don't
  auto-act.
- **Don't moralize.** Same rule as `/check-in`.
- **Don't auto-commit.**

## Edge cases

- **Entry is mostly state.** If the entry reads like state
  ("I'm tired, energy's low"), suggest `/check-in` *as an
  option* — but still log what they wrote if they confirm.
  Don't redirect unilaterally.
- **Entry contains another person's words verbatim** (a text,
  an email, a transcript). Log it. The user's job to decide
  what's safe to record.
- **Entry contains a credential / secret.** Refuse and flag.
  "I see what looks like an API key — don't log that here.
  Use a password vault."
- **Multiple entries in same minute.** Both append; second
  block goes below first. Timestamp collisions don't matter
  for readability.
- **User wants to write a long-form piece.** Fine. The entry
  is whatever length it is. No upper bound.
- **Entry references a person.** Don't auto-link to
  `memories/people/`. Cross-linking is manual; suggest in
  closing summary if it'd help: "*This mentions Marco — want
  me to add a backlink to `memories/people/business/marco.md`?*"

## When NOT to use this skill

- **Logging structured state** → `/check-in`.
- **Recording a decision** → `/decision`.
- **Recording a regret in hindsight** → `/regret`.
- **Capturing a learned lesson** → `/lessons`.
- **Logging a specific past event with its own date/slug** →
  edit `timeline/YYYY/MM/DD-<slug>.md` directly.
- **Daily kickoff** → `/morning`.
- **Daily wrap** → `/evening`.

## What "done" looks like for a /journal session

A `## NOTE HH:MM` block appended to today's
`timeline/YYYY/MM/DD.md` containing exactly what the user
wrote, verbatim. Working tree dirty. The user knows where
the entry landed.
