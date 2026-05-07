# Archive

Prior versions of files that have been fundamentally rewritten.
**Never `rm`.** When a file like `values.md`, `vision.md`,
`world-view.md`, or any other evolving doc gets a real shift,
the previous version copies here *before* the rewrite.

---

## Filename convention

`archive/<original-path-flattened>-YYYY-MM.md`

Examples:
- `archive/values-2026-05.md` — `.claude/values.md` snapshot before a rewrite
- `archive/vision-2024-08.md` — `.claude/vision.md` before a major shift
- `archive/memories-people-business-marco-2025-11.md` — Marco's
  file before being rebuilt after a falling-out

A future `/archive <file>` skill does this automatically.

## When to archive

Two patterns:

- **Evolutionary edits** (a new dated section in a memory file,
  a new value added) → don't archive. The within-file history
  is enough. Git tracks the rest.
- **Fundamental rewrites** (replacing `values.md` because what
  I value has actually shifted) → archive first.

The line is judgment. The skill asks; you decide.

## What this isn't

- Not for tasks you finished — those live in `tasks/done/`.
- Not for relationships that ended — those go to
  `memories/people/lost/`.
- Not for things you regret writing — those stay in place.
  We don't rewrite history.

Archive is for the evolution of *durable anchors* and similar
files where the *changed-over-time* dimension is itself part
of the value. You want to be able to read `archive/values-2020-01.md`
in a decade and remember who you were then.
