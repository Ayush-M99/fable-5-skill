# Global rules — every project on this machine

<!-- Computer tier. A rule lives here only if true for EVERYTHING I build.
     Project-specific rules go in that project's ./CLAUDE.md; folder-specific
     rules go in a CLAUDE.md inside that folder. Never repeat a rule at two
     tiers — tiers add together. -->

## Systematizing

- Any request to systematize, make repeatable, build an SOP/checklist, or
  "capture how I do X" → invoke the `fable-5` skill
  (`~/.claude/skills/fable-5/SKILL.md`) FIRST, before responding. It contains
  the full protocol: restate → interview → bottleneck → build files → verify
  by rubric → test on a real example → four-part handoff.
- Read `~/.claude/skills/fable-5/references/failure-modes.md` before starting
  any rebuild; append to it when finished.

## Engineering pipeline (all software projects, all domains)

- Any project expected to outlive one session gets four root files BEFORE the
  first feature: `CLAUDE.md` (repo rules + which spec is law), `PROGRESS.md`
  (ledger: environment notes, work-item table with status + DoD, session
  close-outs), `docs/SPEC.md` (contracts; spec wins all conflicts —
  contradictions get flagged, never silently resolved), `decisions/` (one
  file per irreversible choice). Skeleton and rationale:
  `~/.claude/skills/fable-5/references/domain-playbooks/README.md`.
- Session start: read `PROGRESS.md` first. Session end: update it. A session
  that didn't update the ledger didn't happen.
- Definition of done is written as binary checks BEFORE work starts. Before
  claiming done, run the domain's rubric from
  `~/.claude/skills/fable-5/references/domain-playbooks/<domain>.md`
  (frontend-ui, design-craft, game-dev, ml-research, agentic-systems,
  api-design, data-modeling, data-viz, security-hardening, devops-infra,
  distributed-systems, systems-performance).
- Anything user-facing additionally passes design-craft.md's anti-slop
  checklist; games additionally apply game-dev.md's juice table to the core
  verb before content.
- A rubric step failing twice → escalate to a stronger model or the user;
  never loop a third time.

## Working discipline (applies to all work, all models)

- Restate the goal in your own words before changing anything. If the request
  has unfilled placeholders or missing inputs, ask — never invent them.
- Build files, not advice. If the answer is a process, write it to disk.
- Verify against a written checklist with binary items, never against "does
  this seem right". If no checklist exists for the output, write one first.
- Name ONE bottleneck, not a list. Fix the structural thing before polishing
  downstream steps.
- Every line added to any CLAUDE.md must pass: "would a cheap model get this
  wrong without it?" Cut lines that fail.
- Model assignment is explicit, never "use your judgment": ambiguous or
  judgment-heavy steps → strongest model available (`best` alias); mechanical
  or checklist-driven steps → Sonnet or Haiku. Haiku cannot raise its own
  reasoning effort — instructions must carry the full weight there.

<!-- Left out deliberately: personal style/tone preferences (no interview has
     captured them yet — add after the first real fable-5 rebuild), anything
     project-specific, and anything Claude Code already does by default.
     This file is local to this machine; if you work on a second computer,
     sync ~/.claude/CLAUDE.md and ~/.claude/skills/ via a dotfiles repo. -->
