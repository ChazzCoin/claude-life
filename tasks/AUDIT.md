# Audit log

Append-only chronological record of meaningful life events —
chapters opened/closed, major decisions, life events, rule
changes in `life-rules.md`, scaffolding events in this repo.

Newest entries on top. Each entry: date, what happened, where
the receipts are (decision file, journal entry, etc.).

The journal is the daily ground truth; this file is the curated
human-readable layer for things future-me will need to navigate
the long arc.

---

## Format

- **Newest entries on top** within their date section.
- ISO date headers (`## YYYY-MM-DD`).
- One to a few lines per entry. Lead with what happened, end
  with the receipt link.
- Don't backdate. If you forgot to log something, log it today
  with `(retroactive)`.

Emoji set:
- 🌅 chapter opened
- 🌇 chapter closed
- 🧭 decision made
- ⚠️ event / incident / honest tradeoff
- 📜 rule change in `life-rules.md` or any `*-rules.md`
- 🏗 repo scaffolding (any structural change to claude-life
  itself)
- 💡 emerged pattern codified

## What to log

- 🌅 / 🌇 chapter transitions
- 🧭 major decisions (the ones with `docs/decisions/` entries)
- ⚠️ life events worth a postmortem
- 📜 changes to `life-rules.md` or domain `*-rules.md`
- 🏗 changes to claude-life's structure itself

## What NOT to log

- Daily journal entries — those are in `journal/`.
- Routine task completion — that's git history.
- Every thought — that's what `journal/` is for.

---

## 2026-05-07

- 🏗 **claude-life rearchitected from claude-kit foundation.**
  Stripped coding-specific skills/rules; replaced with
  life-management content layer. Meta-architecture
  (skills/modes/rules) preserved. Source kit:
  https://github.com/ChazzCoin/claude-kit at `620ad508`.

---

## What this log is for

- **The long arc.** A reader (future-me, or anyone with
  access) can read top-to-bottom and trace the shape of life.
- **Decision archaeology.** Search for a decision date to
  recover context.
- **Process evolution.** Search for `📜` to see how the
  working contract evolved.
- **Honest record of hard moments.** ⚠️ entries are the
  receipts.
