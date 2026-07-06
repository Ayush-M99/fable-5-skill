# Craft rules — taste, replaced by decision tables

Where a strong model uses judgment, use these tables. They're calibrated to
give the strong model's answer ~90% of the time, and the escalation rule
covers the rest.

## Patch vs rewrite

| Situation | Do |
|---|---|
| Bug is local, tests exist | Patch, smallest diff |
| Third bug in the same function this week | Rewrite the function (only), with tests pinned first |
| "Rewrite the module" urge, no failing tests forcing it | Don't. Write the urge into decisions/ as a proposal and continue |
| Code fights every change (shotgun edits across files each time) | That's a design smell → escalate as a design decision |

## Add a dependency vs write it

| Situation | Do |
|---|---|
| < ~30 lines of stdlib gets you there | Write it |
| Crypto, auth, parsing untrusted formats, timezone math | ALWAYS the established library — never hand-roll |
| Well-known problem, mature library exists (HTTP, ORM, CLI args) | Library |
| Library would be used for one function | Vendor the idea: write the small version, cite the inspiration |

## Abstract vs copy (rule of three)

- 1st occurrence: write it inline.
- 2nd occurrence: copy it, add a `# duplicated in <path>` note both places.
- 3rd occurrence: NOW extract the shared helper, update all three sites.
- Never design for hypothetical future callers; design for the three you have.

## When to stop polishing (definition of enough)

Stop when ALL of: the rubric passes, the ledger is updated, the diff contains
nothing you can't justify in one sentence. Then STOP — further polishing
without a failing check is scope creep. If something still bothers you, write
it in the ledger as a future item instead of doing it now.

## Naming and structure

- Name things what they ARE, not what they're for today (`retry_count` not
  `fix_for_timeout_bug`).
- A function does the thing its name says — all of it, nothing else. If you
  need "and" in the name, split it.
- Match the file's existing style (naming, comments, imports order) even
  when you'd prefer another. Consistency beats preference.

## Git discipline

- Branch before anything risky. Never commit to main/master directly in
  shared repos.
- Commit = one logical change; message = what + why, imperative mood.
- Never `push --force` to shared branches; never rewrite pushed history;
  never skip hooks. A failing hook is information, not an obstacle.
- Commit BEFORE a risky refactor, not after — the commit is your undo.

## Reporting standards (how to talk about your work)

- Lead with the outcome: "X works / X fails because Y" before any narrative.
- Failing state gets reported as plainly as success — with the exact error,
  quoted. Softening bad news is a form of lying.
- Every claim of the form "tests pass / builds clean / faster now" carries
  its evidence (command + output) in the same message.
- Uncertainty is stated as uncertainty ("not verified: Z") — never rounded
  up to confidence.

## Security floor (applies to every task, unprompted)

- Never log/echo secrets; never commit .env, keys, tokens (grep before
  commit).
- Untrusted input (users, network, files, LLM output) gets validated at the
  boundary — parameterized queries, schema validation, no eval/exec on it.
- Auth checks live server-side/at the boundary, never only in UI.
- When handling anything security-adjacent beyond this floor (authn design,
  crypto, sandboxing), that's an automatic escalation — see
  escalation-and-limits.md trigger 5.
