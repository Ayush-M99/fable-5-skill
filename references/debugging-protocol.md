# Debugging protocol — for any model, any bug

Strong models debug by instinct; you will debug by procedure. The procedure
wins more often than instinct anyway.

## The loop (never skip a step, never reorder)

1. **Reproduce first.** Run the failing thing yourself and see it fail.
   No reproduction = no debugging; you'd be editing blind. If you cannot
   reproduce, that IS the finding — report it, don't "fix" anyway.
2. **Read the actual error.** The full text, the line numbers, the bottom
   frame of the stack that's in YOUR code. Quote it exactly in your notes.
   Most bugs are solved by believing the error message instead of skimming it.
3. **State ONE hypothesis in writing** before changing anything: "I believe X
   because the error says Y." If you can't fill the "because", gather more
   evidence (add a log line, print the value, check the version) — don't
   guess-edit.
4. **Make the smallest change that tests the hypothesis.** One change. Run
   again.
5. **Judge honestly:** fixed → remove debug scaffolding, run the FULL test
   suite (not just the failing test), done. Not fixed → the hypothesis was
   wrong; write down what you learned, return to step 3 with a NEW
   hypothesis. Never re-try the same idea with cosmetic edits.

## Bisection (when the cause is unknown)

- Recent regression → `git bisect` or manually walk commits. The bug entered
  at a specific commit; find it instead of theorizing.
- Big input fails → shrink the input by halves until minimal failing case.
- Long pipeline fails → check the midpoint's intermediate output; recurse
  into the broken half.

## Hard rules

- Never fix what you haven't reproduced.
- Never claim fixed without rerunning the original failing command and
  pasting its passing output.
- Two consecutive wrong hypotheses → STOP. Write the escalation note
  (see `escalation-and-limits.md`) — a third guess is where cheap models
  start damaging the codebase.
- The bug is in your code, not the compiler/framework/library, until you
  have a minimal reproduction that proves otherwise.
- Something that "should be impossible" happening = one of your assumptions
  is false. List your assumptions and test them one by one; don't stare at
  the code.
- Fixing the symptom is allowed ONLY with a written note naming the actual
  root cause and why it's deferred.

## After every fix

- Add the regression test that would have caught it.
- If the bug pattern is general (not a typo), append one line to the
  project's PROGRESS.md gotchas so no future session re-earns it.
