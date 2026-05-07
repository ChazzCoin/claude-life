# claude-life

This is my private life-management system. Single user, single
private repo, accessed from any device with markdown support.

I am Chazz Romeo. I'm CTO and co-founder of VSI and rAI.

This repo holds:
- **My values + vision** — `.claude/values.md`, `.claude/vision.md`
- **My current chapter and goals** — `tasks/PHASES.md`,
  `tasks/ROADMAP.md`
- **My daily journal** — `journal/`
- **What I know about my people, places, events** — `memories/`
- **Decisions, retros, lessons, regrets, audits** — `docs/`
- **The skills + modes that shape how Claude collaborates here**
  — `.claude/`

If you're reading this, you're either me, or you have my
explicit permission. There is no public version of this repo.

The external face — what others can see about my life — is
projected via an explicit `api/` layer (Wave 4, see
[PHASES.md](tasks/PHASES.md)).

---

## Start a session

Open Claude Code in this directory. It auto-loads `CLAUDE.md`
and the primitives. From there:

- `/morning` — daily kickoff *(Wave 2)*
- `/journal` — start a journal entry *(Wave 2)*
- `/oncall` — what's live in life right now *(Wave 2)*
- `/skills` — list every skill
- `/mode <name>` — switch driving mode (`task`, `recovery`,
  `planner`, `normal`)

For the architecture and conventions, read
[`CLAUDE.md`](CLAUDE.md).

## Build status

- ✅ **Wave 1: Foundation** — strip + replace coding-flavored
  kit layer with life-shaped scaffolding.
- 📋 **Wave 2: Daily skills** — `/journal`, `/morning`,
  `/evening`, `/weekly-review`, `/oncall`, `/check-in`,
  `/goals`, etc.
- 📋 **Wave 3: Domain rules** — `vsi-rules.md`, `rai-rules.md`,
  `personal-rules.md`. Built after Wave 2 has proven what each
  domain actually needs.
- 📋 **Wave 4: API / publish** — external projection layer.
  Deferred until 1-3 settle.
