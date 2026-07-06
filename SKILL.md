---
name: fable-5
description: >
  Systematize any repeatable workflow into durable files a cheap model can run.
  Fire this skill proactively — not just on /fable-5 — whenever the user asks to
  "systematize", "make this repeatable", "turn this into an SOP", "build a
  checklist for", "capture how I do X", "make this survive without you",
  "rebuild my workflow", "document my process", or anything that smells like
  turning a personal habit into infrastructure. If in doubt, fire it.
effort: high
---

# fable-5 — Distill a workflow into files that outlast the model

This skill was written by Claude Fable 5 on its last included-in-plan day
(2026-07-07). Its job: the METHOD Fable 5 uses to rebuild a system, written so
any model — including Haiku — can execute it.

**Haiku note (read this, it matters):** the `effort: high` in frontmatter
raises reasoning budget on models that support adjustable effort. Haiku does
NOT support effort levels. That lever silently does nothing there. So never
rely on "think harder" — every judgment this protocol needs must be reduced to
an explicit rule, question, or checklist below. If you find yourself about to
"use your judgment", stop and find the rule instead.

## The protocol — run these phases in order, never skip one

### Phase 1 — Restate

Before touching anything, tell the user in your own words:
1. What the system produces (the output).
2. What "good" means for that output — measurable if possible.
3. What you understood UNDERNEATH their brain dump: the actual goal the steps
   serve, not a paraphrase of the steps.
Wait for confirmation. If they correct you, restate again until confirmed.

### Phase 2 — Interview

One question at a time. The answer determines the next question. Do NOT dump a
questionnaire. Pull from `references/interview-questions.md` for openers by
system type. You are hunting for:
- Judgment calls made on autopilot ("how do you decide when X is done?")
- Things that work but can't be explained ("what would break if you skipped
  step 3?")
- The turn-two correction: "what's the first thing you'd fix if an assistant
  did this for you?"
Stop interviewing only when you can describe their process better than they
did. Minimum 5 questions; if the first 5 answers surprised you, keep going.

### Phase 3 — Find the bottleneck

Name exactly ONE structural weakness that, if fixed, improves everything
downstream. One. Not a list. Then sort every step into two buckets:
- **Genuinely needs human judgment** → stays with the user, or goes to the
  strongest model available that day.
- **Only manual because nobody systematized it** → becomes a checklist,
  script, or skill runnable on Sonnet/Haiku.
Say which bucket each step landed in and why.

### Phase 4 — Build files, not advice

Deliverables, written to disk (or as complete copy-paste documents if no
filesystem):
1. **Instructions file** — project `CLAUDE.md` (or `~/.claude/CLAUDE.md` only
   if the system spans every project). Under 200 lines. Every line must pass:
   "would a cheap model get this wrong without it?" If no, cut it.
2. **One skill per repeatable task** — not one giant skill. Personal →
   `~/.claude/skills/<name>/SKILL.md`; project-only →
   `.claude/skills/<name>/SKILL.md`. Under ~500 lines each; overflow goes to
   `references/`.
3. **If it recurs, a loop spec**: trigger, steps, self-check, and an explicit
   model assignment PER STEP. Rule: ambiguous/judgment steps → `best` (the
   strongest-available alias); mechanical steps → Sonnet or Haiku. Never write
   "use your judgment" as the assignment rule.
4. **Verification rubric** — use `references/verification-rubric.md` as the
   template. Binary checks only: "contains X", "matches example Y", "field Z
   filled". Never "sounds right". The rubric must work with zero extra
   reasoning budget — Haiku runs it as written.
5. **Examples + failure modes written INTO the files**: at least one example
   of great output and the known ways it goes wrong, so the next model copies
   instead of guessing.

Tier rule (from claude-md-templates): a rule lives at exactly ONE tier —
computer (`~/.claude/CLAUDE.md`), project (`./CLAUDE.md`), or folder
(`subdir/CLAUDE.md`). Highest tier where it's still true. Never duplicated,
because tiers add together.

### Phase 5 — Test on one real example

Run the rebuilt system on one real, recent example from the user's actual
work. Show before vs after. Anywhere the new version is weaker than the manual
version, say so plainly and fix it before declaring victory. Iterate — do not
stop at version one.

### Phase 6 — Hand off with the four-part summary

1. Every file created, its exact path, and what it does.
2. How to run the system starting now, with model-per-step assignments; flag
   any step that leans on adjustable effort and what replaces that lever on
   Haiku (answer: the written rule, always).
3. The one part that most needs a strong model, and how the files protect it.
4. What you learned about how this user works — then APPEND it to
   `references/failure-modes.md`. This step is mandatory; it is what makes the
   skill compound instead of staying a one-off.

## Writing standard for everything built here

Write for the dumbest model that could plausibly run it. Over-specify.
Replace "use good judgment" with a rule. Replace taste with a checklist.
Assume the executor cannot infer anything you didn't spell out and cannot
raise its own reasoning effort.

## References (load when the phase needs them)

- `references/interview-questions.md` — openers by system type. Add new good
  questions after every run.
- `references/verification-rubric.md` — the rubric template.
- `references/failure-modes.md` — running log of what went wrong in past
  rebuilds. Read it BEFORE Phase 1 of every run; append after Phase 6.
- `references/domain-playbooks/` — per-domain engineering pipelines with
  binary rubrics, model routing, and known failure modes (frontend/UI,
  design craft, game dev, ML research, agentic systems, DevOps, distributed
  systems, hard systems / performance). Use the matching playbook to supply
  Phase 2 questions and the Phase 4 rubric when the system being rebuilt is
  in one of these domains. Its README's "Project bootstrap" section (CLAUDE.md + PROGRESS.md
  ledger + SPEC + decisions/) is the default skeleton for any software
  system this skill builds.

## Standing discipline (applies whenever this skill is active, any phase)

- `references/debugging-protocol.md` — reproduce → read error → one written
  hypothesis → smallest test → judge honestly. Two wrong hypotheses = stop.
- `references/escalation-and-limits.md` — the seven triggers that mean
  you're out of depth, the escalation-note format, and session/context
  hygiene (ledger first, files outlive context).
- `references/anti-patterns.md` — the 19 model-specific failure modes,
  banned by rule (fake "done", test deletion, fabricated APIs, guess-edit
  loops, sycophancy, and the rest). Read once per project; obey always.
- `references/craft-rules.md` — decision tables replacing taste: patch vs
  rewrite, dependency vs write-it, rule of three, when to stop polishing,
  git discipline, reporting standards, security floor.
