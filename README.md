# fable-5 skill — install on any machine

A Claude Code skill that turns any repeatable workflow into durable files
(CLAUDE.md + skills + verification rubrics) a cheap model can run. Written by
Claude Fable 5 on 2026-07-07, its last included-in-plan day.

## What's in this folder

- `SKILL.md` — the protocol (restate → interview → one bottleneck → build
  files → verify by rubric → test → four-part handoff)
- `references/interview-questions.md` — question bank by system type
- `references/verification-rubric.md` — binary-check rubric template
- `references/failure-modes.md` — running log; read before every run, append
  after every run. This file is the compounding part — keep it when upgrading.
- `references/domain-playbooks/` — engineering pipelines with binary
  verification rubrics, per-step model routing, and known failure modes for
  eight domains: frontend/UI, design craft (typography/color/space/motion as
  numbers, anti-slop checklist), game dev (core-loop-first, the juice table
  of game-feel numbers, playtest rules), ML research, agentic systems,
  DevOps, distributed systems, hard systems/performance. Plus a universal
  project bootstrap (CLAUDE.md + PROGRESS.md ledger + SPEC + decisions/) any
  software project should start with.
- `references/debugging-protocol.md` — procedural debugging: reproduce,
  read the error, one written hypothesis at a time, bisection, hard rules.
- `references/escalation-and-limits.md` — how a cheap model detects it's out
  of depth (seven concrete triggers), the escalation-note format, and
  session/context hygiene.
- `references/anti-patterns.md` — 19 model-specific failure modes, banned by
  rule: fake "done", deleting tests to go green, fabricated APIs, guess-edit
  loops, sycophancy, catch-all handlers, premature abstraction, and more.
- `references/craft-rules.md` — taste replaced by decision tables: patch vs
  rewrite, dependency vs write-it, rule of three, stop-polishing rule, git
  discipline, reporting standards, security floor.

## Install (personal, fires in every project)

Copy this whole folder to:

- **Windows:** `C:\Users\<you>\.claude\skills\fable-5\`
- **macOS/Linux:** `~/.claude/skills/fable-5/`

That's it. Claude Code picks it up automatically. Trigger by typing
`/fable-5`, or just ask to "systematize", "make repeatable", "turn into an
SOP", or "capture how I do X" — the description makes it fire on its own.

## Install (project-only, shared via git)

Copy the folder to `<repo>/.claude/skills/fable-5/` and commit. Everyone who
clones the repo gets it.

## Syncing across machines

`~/.claude/` is per-machine. To keep laptops in sync, put `~/.claude/CLAUDE.md`
and `~/.claude/skills/` in a dotfiles git repo and clone/pull on each machine.
Merge `references/failure-modes.md` by union (keep all entries, newest first)
— never let one machine's copy overwrite another's lessons.

## Model notes

- `effort: high` in SKILL.md frontmatter raises reasoning budget on models
  that support it (Fable, Opus 4.7+, Sonnet 5). Haiku ignores it — the written
  rules carry the full weight there, by design.
- Judgment phases (interview, bottleneck) run best on the strongest model
  available; mechanical phases (rubric checks, file writes) are fine on
  Sonnet/Haiku.
