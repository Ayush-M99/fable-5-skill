# Legacy code and refactoring — changing what you're afraid of

Legacy code = code without tests, whoever wrote it. Rules for changing it
without breaking what nobody remembers.

## The prime directive

Behavior is preserved unless the task explicitly says change it — including
the weird behaviors. That "bug" may be load-bearing: something downstream
depends on the off-by-one, the odd rounding, the double-encoded string.
Found a genuine bug while refactoring? FLAG it, ledger it, fix it in a
SEPARATE commit after the refactor lands — never silently inside one.

## Procedure

1. **Characterization tests first (Sonnet):** before touching anything,
   pin CURRENT behavior — feed the code real inputs, assert whatever it
   actually returns (even if it looks wrong). These tests define "unchanged".
   No characterization tests = no refactor, only rewriting-and-praying.
2. **Find the seams (Sonnet):** the places you can substitute behavior
   without editing the scary core — function boundaries, injectable
   dependencies, env/config. Change AT seams; create a seam first (extract
   function, introduce parameter) when none exists — that extraction is
   itself a tiny, test-pinned step.
3. **Small steps, always green (Sonnet/Haiku):** rename → run tests →
   extract → run tests → move → run tests. Each step ≤ a few minutes,
   committed. A refactor with a broken intermediate state longer than one
   step has become a rewrite (see craft-rules.md table — that needs
   approval, not momentum).
4. **Strangler pattern for big replacements (best plans, Sonnet executes):**
   new implementation grows BESIDE the old behind a switch (flag/route/
   adapter); traffic moves incrementally (one caller, one route, 1%);
   old code dies only after the new path has run real load. Big-bang
   replacement of a working system is the highest-risk move in software —
   a cheap model must refuse it and propose strangler instead.
5. **Leave it better, boundedly:** improve what the task touched; ledger
   the rest as future items. "While I'm here" beyond the change map is
   scope creep (escalation-and-limits.md trigger 4).

## Verification rubric (binary)

- [ ] Characterization tests existed and passed BEFORE the first change
      (check commit order)
- [ ] Every intermediate commit is green (run the suite on each, or CI does)
- [ ] Zero behavior diffs: characterization suite passes unchanged after —
      any intentional behavior change is in its own commit, flagged
- [ ] Public interfaces unchanged, or every caller updated in the same
      change with the interface
- [ ] Dead code found along the way: deleted (if provably unreachable) or
      ledgered — never left commented out
- [ ] The refactor's motivation written in one sentence in the commit/ledger
      (a refactor that can't say why is churn)

## Known failure modes

1. **Rewrite wearing a refactor costume.** "Cleaning up" becomes 40 files.
   The craft-rules patch-vs-rewrite table gates this; momentum doesn't.
2. **Fixing bugs mid-refactor.** Behavior change hides inside a 400-line
   "no-op" diff; downstream breaks; git bisect points at "refactor: cleanup".
   Prime directive.
3. **Trusting the docs over the code.** Legacy docs describe intentions,
   not behavior. Characterization tests measure what IS.
4. **Deleting the weird thing.** The unexplained sleep(100), the re-fetch,
   the seemingly redundant check — assume Chesterton's fence: understand why
   it exists (git blame, callers) before removing; if unexplainable, remove
   it in its own labeled commit so the revert is surgical.
5. **All-at-once migrations.** Old system off, new system on, weekend gone.
   Strangler, always, for anything with real users.
