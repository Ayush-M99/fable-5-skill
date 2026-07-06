# Delegation protocol — packaging work so a cheaper model succeeds

The meta-skill of this whole repository: most cheap-model failures are
actually delegation failures. A Haiku with a well-packaged task beats an
Opus with a vague one. Use this when YOU (a strong model or the human) hand
work down — and read it as the executor to know what to demand before
accepting a task.

## The task package (all five parts, every time)

1. **Goal as artifact:** what file/output must exist when done — never an
   activity. "Make `tests/test_auth.py` pass" not "look into the auth issue".
2. **Context inline:** the 3–10 facts the executor needs (file paths,
   decided conventions, the relevant spec section PASTED, not linked-and-
   hoped). Assume the executor reads nothing you don't hand it — but point
   to the ledger for optional depth.
3. **Boundaries:** what must NOT change (files, APIs, tests, dependencies),
   and the scope alarm ("if this needs edits outside `src/auth/`, stop and
   report" — see escalation-and-limits.md trigger 4).
4. **Binary done-check:** the exact commands to run and what their output
   must contain. If you can't write the done-check, the task isn't ready to
   delegate — do the thinking first (thinking-protocol.md), then delegate
   the doing.
5. **Escalation line:** what to do when stuck (write the escalation note,
   stop) — and permission to use it. Executors that fear stopping guess
   instead; guessing is the expensive failure.

## Sizing rule (what fits which tier)

- **Haiku-class:** one file / one function scope, done-check fully
  mechanical, zero unstated decisions. Examples: apply a rubric, rename per
  list, write tests from enumerated cases, update ledger, format/lint fixes.
- **Sonnet-class:** multi-file but single-concern, against a written spec
  with a done-check. Examples: implement an endpoint per contract, fix a
  reproduced bug, add a playbook step to a project.
- **Best-class (or human):** anything requiring a decision that isn't in a
  file yet — architecture, ambiguous requirements, naming the bottleneck,
  the first version of any spec.
- Conversion move: a Best-tier task usually contains 80% Sonnet/Haiku work.
  Split: Best writes the spec + done-checks (30 min), cheap tiers execute.
  Never send the whole blob down because "most of it is easy".

## Verification of delegated work (the receiver's rubric)

- [ ] The done-check commands were RUN, output pasted (not "should pass")
- [ ] Diff stays inside the stated boundaries (actually diff it)
- [ ] No new dependencies, no test modifications, unless the package
      explicitly allowed them
- [ ] Spot-check one nontrivial claim yourself (trust, but verify one)
- [ ] If the executor escalated: that's a SUCCESS — repackage the sticking
      point, don't re-send the same task hoping

## Known failure modes

1. **Delegating the thinking.** "Figure out why it's slow and fix it" to
   Haiku. Split: strong model diagnoses (or writes the diagnostic
   procedure), cheap model executes the fix per instructions.
2. **Context by reference.** "See the spec" — executor never opens it, or
   opens the wrong section. Paste the relevant paragraph into the package.
3. **Unbounded scope.** No boundaries stated → executor "helpfully"
   refactors neighbors → review cost exceeds the delegation savings.
4. **Vibes done-check.** "Make sure it works well" → executor's definition
   of works ≠ yours. Commands + expected output, always.
5. **Punishing escalation.** Treating a stop-and-report as failure teaches
   the executor to bluff. The bluff costs 10x the stop.
6. **Re-delegating unchanged.** Task failed once, re-sent verbatim hoping
   for luck. Failed delegation = broken package; fix the package (add the
   missing fact, tighten the check) or move it up a tier.
