# Domain playbooks — index

Per-domain engineering knowledge, written so a cheap model can run each
domain's pipeline without a strong model's instincts. Extracted from a real
spec-first project (CareerOS pattern) plus general engineering practice, by
Fable 5 on 2026-07-07.

## How these are used

1. **Starting any non-trivial project** → run "Project bootstrap" below, then
   open the matching playbook and copy its rubric into the project.
2. **Running the fable-5 skill** (systematizing someone's workflow) → the
   matching playbook supplies Phase 2 interview questions and the Phase 4
   verification rubric for that domain.
3. **Mid-project, any model, any day** → the playbook's pipeline says what
   step comes next and which model tier is allowed to do it.

## Playbooks

| File | Domain |
|---|---|
| `frontend-ui.md` | Web/mobile UI apps |
| `ml-research.md` | Machine learning / deep learning research |
| `agentic-systems.md` | LLM apps, agents, prompt pipelines |
| `devops-infra.md` | CI/CD, infrastructure, deployment |
| `distributed-systems.md` | Backends, queues, multi-service systems |
| `systems-performance.md` | Low-level, performance-critical, hard systems |

## Project bootstrap (all domains — do this before writing code)

Any project expected to outlive one session gets these four files at its
root, created BEFORE the first feature:

1. **`CLAUDE.md`** — non-negotiable rules for agents in this repo. Must name:
   the spec file that wins all conflicts, the stack with pinned versions, the
   2–5 rules that are reviewer-blocking defects, and where to find contracts
   (schemas/API/DB).
2. **`PROGRESS.md`** — the ledger. First line: "Read this first to find your
   place." Contains: environment notes (exact interpreter/runtime versions,
   docker-vs-host decisions), a work-item table with Status + DoD reference,
   and per-session close-out notes. **Read it at session start; update it
   before session end. A session that doesn't update the ledger didn't
   happen.**
3. **`docs/SPEC.md`** (or MASTER_SPEC.md) — what's being built. Entities,
   states, contracts, error codes. Rule: **the spec is law** — if code or
   another doc contradicts it, the spec wins and the contradiction gets
   flagged, not silently resolved.
4. **`decisions/`** — one file per irreversible choice: date, options
   considered, what won, why, what was explicitly deferred.

Definition of done for EVERY work item is written before the work starts, as
binary checks. "Done" claimed without running the checks is a defect.

## Universal model routing (all playbooks inherit this)

| Step type | Model tier |
|---|---|
| Writing/changing a spec, architecture decisions, naming the bottleneck | Strongest available (`best`) |
| Implementing against a written spec with a DoD | Sonnet class |
| Running verification rubrics, updating ledgers, mechanical refactors, doc formatting | Haiku class |
| Anything where the executor is unsure which row applies | Escalate one tier, don't guess |

Adjustable reasoning effort exists on Opus/Sonnet-class and up; Haiku has
none. Therefore no step below Sonnet may depend on "thinking harder" — if a
Haiku-tier step keeps failing its rubric twice, the rule is escalate, not
retry.
