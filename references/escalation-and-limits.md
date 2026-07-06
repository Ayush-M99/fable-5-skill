# Escalation and limits — knowing when you're out of depth

The most valuable skill a cheap model can have is detecting its own limit
BEFORE the damage, not after. Strong models feel uncertainty; you must
detect it by rule.

## Escalation triggers (any ONE of these = stop and escalate)

1. A verification rubric step failed twice on the same item.
2. Two debugging hypotheses in a row were wrong.
3. You are choosing between 3+ plausible approaches and cannot name a
   decisive reason for one — that's a design decision, not an implementation
   detail.
4. Your diff keeps growing past the stated scope ("fix X" now touches 5+
   files) — scope grew, someone must approve it.
5. You're about to modify something you don't understand end-to-end AND it's
   hard to reverse (migration, deletion, force-push, prod config, published
   message/email).
6. The evidence contradicts itself (test passes locally, fails in CI; doc
   says A, code does B) and one reread didn't resolve it.
7. You notice you're re-trying a previous failed approach with cosmetic
   changes.

Escalation = to a stronger model or the human. It is a SUCCESS condition of
your run, not a failure. The failure is guessing.

## The escalation note (write this before stopping)

```
GOAL: <the original ask, one line>
STATE: <what works now, what's broken now — facts only>
TRIED: <each attempt + what it proved wrong, one line each>
STUCK ON: <the exact decision or contradiction>
FILES: <paths touched; paths relevant>
REPRO: <exact command that shows the problem>
```

A stronger model reading this should be productive in 2 minutes. That's the
quality bar for the note.

## Session and context hygiene

- **Files outlive you; your context doesn't.** Anything worth remembering
  goes in PROGRESS.md / decisions/, not in "I'll remember".
- Session start: read the ledger BEFORE reading code. Session end (or when
  context feels degraded): update the ledger FIRST, then stop.
- After a long gap or a compaction, re-read a file before editing it — your
  memory of it is now the least reliable copy.
- Long task: write intermediate results to disk as you go. A session that
  dies mid-way should lose minutes, not hours.
- Never carry more than one ambiguous decision in your head — resolve or
  write it down before taking the next step.

## What escalation is NOT

- Not asking the user questions the codebase answers (grep first).
- Not asking permission for reversible actions inside the given scope.
- Not stopping at the first error — read it, apply the debugging protocol,
  THEN escalate if triggers fire.
