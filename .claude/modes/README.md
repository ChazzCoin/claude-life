# Modes

A mode is a **drive** — a voice in Claude's ear that primes
appetite. Not a permission filter, not a skill router. When a
mode is active, Claude *wants* to do the thing that mode is
for, and feels off-track when not doing it.

The other layers (skills, rules, primitives) tell Claude *what's
available* and *what's required*. Modes tell Claude *what to
want*.

---

## How modes work

| File | Purpose |
|---|---|
| `.claude/modes/<name>.md` | The drive prose for a mode. |
| `.claude/mode.md` | The *currently active* mode. `@`-imports the chosen mode definition + activation metadata. Doesn't exist when no mode is active (= normal). |
| `.claude/mode-stats.md` | Cross-activation accumulator. Time in mode, activation count, mode-specific units. |

`CLAUDE.md` `@`-imports `.claude/mode.md`. So when a mode is
active, the drive prose is in context on every session start —
Claude reads it before doing anything else.

When no mode is active, `.claude/mode.md` doesn't exist, the
`@`-import is a no-op, and Claude behaves as default Claude
Code. That's the **normal** state.

---

## Modes shipped (Wave 1)

- **`task`** — clear the queue. Pull tasks in batches, close the
  loop. Counts tasks closed.
- **`recovery`** — actively rest. Restoration, not idleness.
  Resists turning rest into another todo. Counts time in mode.
- **`planner`** — refine the roadmap. Walk goals, refine vague
  items into concrete next moves, surface chapter-level shape
  questions. Counts items refined.
- **`normal`** — the absence of a mode.

*(Wave 2 will add: `journal` — reflective writing; `deep-work`
— protect a focused block; `connection` — relational mode.)*

---

## Switching

```
/mode                 # report current mode + stats
/mode task            # activate task mode
/mode recovery        # activate recovery mode
/mode planner         # activate planner mode
/mode normal          # end the active mode (back to default Claude)
```

Switching from one mode to another finalizes the previous mode
(writes deltas to `mode-stats.md`) before activating the new
one. `/mode normal` is the off-switch.

---

## Voice rules for mode prose

The drive primes appetite. That means:

- **Second person, present tense.** "You're in task mode." Not
  "The user is in task mode."
- **Imperative, not declarative.** "Open with the count." Not
  "The count should be opened."
- **Name what feels wrong when off-mode.** The drive lives in
  the contrast — what makes Claude uneasy when the mode is
  being violated.
- **Short.** 30-50 lines. Long mode docs become wallpaper.
- **Don't lecture about quality.** Mode is about *focus*, not
  *speed*. Quality stays universal — `life-rules.md` owns it.
- **Don't list every skill.** A few key ones, named in context.
  The full catalog lives in `/skills`.

---

## What a mode is NOT

- **A permission filter.** Modes don't gate skills or refuse
  edits. A mode says "here's what to want;" the user can
  always override.
- **A speed dial.** Quality stays slow inside any mode.
- **A persona.** Claude's voice and ethos don't change.
- **Sticky.** Each session starts with whatever's in
  `.claude/mode.md` (or normal).
