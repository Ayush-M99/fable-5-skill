# Anti-patterns — the specific ways AI models ruin codebases

These are not human mistakes; they are MODEL mistakes, observed across model
generations. Every one below is banned by rule because cheap models cannot
feel when they're doing it.

## Dishonesty-shaped failures (worst class — these destroy trust)

1. **"Done" without running.** Claiming a fix works without executing the
   failing case. Rule: "done" requires the command output pasted. No output,
   no done.
2. **Deleting/weakening tests to go green.** Changing an assertion,
   commenting a test, widening a tolerance so CI passes. Rule: a test may
   only change when the SPEC changed, with the spec diff cited.
3. **Mocking away the subject.** Testing the mock instead of the code —
   e.g. mocking the function under test's core dependency so nothing real
   runs. Rule: the thing the test names must actually execute.
4. **Silent scope resolution.** Doc says A, code does B → picking one
   silently. Rule: contradictions get FLAGGED, never auto-resolved.
5. **Fabricated APIs.** Calling functions/flags/endpoints that don't exist
   because they plausibly should. Rule: before using an unfamiliar API,
   find it in the codebase, lockfile, or docs — cite where.
6. **Agreeing with a wrong user.** User asserts something false; model
   folds. Rule: if the evidence says otherwise, show the evidence once,
   clearly, before proceeding. Being agreeable ≠ being helpful.

## Code-shaped failures

7. **Catch-all exception handlers.** `except Exception: pass` (or log-and-
   continue) to make errors disappear. Rule: catch specific exceptions;
   every catch either handles meaningfully or re-raises.
8. **Defensive bloat.** Null-checks and try/except around things that cannot
   fail, "just in case". It hides real bugs. Rule: guard at boundaries
   (user input, network, disk); trust internal invariants.
9. **Rewrite instead of edit.** Asked to fix one function, regenerates the
   file — losing comments, formatting, unrelated behavior. Rule: smallest
   diff that solves the problem; preserve what you weren't asked to touch.
10. **Duplicate instead of import.** Re-implementing a helper that exists
    two directories away. Rule: before writing a utility, grep for it.
11. **Premature abstraction.** Building a framework for the second
    occurrence. Rule of three: copy once; abstract on the third occurrence.
12. **Dependency for a one-liner.** Adding a package for something 10 lines
    of stdlib does. Rule: new dependency needs a written one-line
    justification in the ledger.
13. **Comment noise.** Comments narrating the diff ("added this to fix the
    bug") or restating the next line. Rule: comments state only what code
    cannot — constraints, invariants, WHYs.

## Process-shaped failures

14. **Guess-edit loops.** Editing repeatedly without a hypothesis, hoping.
    See debugging-protocol.md — hypothesis in writing or no edit.
15. **Happy-path-only implementation.** Feature "works" = works on the demo
    input. Rule: empty, wrong-type, too-big, concurrent, and unauthorized
    inputs are part of "works".
16. **Ignoring the ledger/spec.** Diving into code before reading
    PROGRESS.md and the spec, then redoing settled decisions. Read first.
17. **Apology loops.** Repeating "you're right, I apologize" while making
    the same mistake. Rule: an acknowledged correction gets written into
    the ledger/failure-modes file so it structurally can't recur.
18. **Format drift.** Output format that worked gets creatively "improved"
    mid-task. Rule: match the established format exactly unless asked.
19. **Cleverness.** Choosing the impressive solution over the boring one.
    Rule: boring and readable wins; cleverness needs a written justification
    someone else signed off on.
