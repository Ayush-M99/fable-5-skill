# Testing strategy — what to test, what not to, and how to not lie to yourself

Tests exist to let the NEXT change be made fearlessly. Write them for that
reader, not for coverage numbers.

## What to test (priority order)

1. **Behavior at the public boundary** — the function/endpoint/CLI as its
   callers see it. Test the contract, not the implementation: a good test
   survives a full rewrite of the internals.
2. **The money paths:** whatever the project exists to do (the checkout,
   the export, the inference call) gets the deepest tests.
3. **The case enumeration** from thinking-protocol.md: empty / one / many /
   too-many / malformed / hostile / concurrent. Each named case = one test.
4. **Every fixed bug** gets its regression test in the same commit
   (debugging-protocol.md) — bugs recur in families.
5. **Error paths:** what callers see on failure (the error code, the
   rollback, the retry) — the happy path is the EASY 20%.

## What NOT to test

- Private helpers through their internals (test through the public surface).
- The framework/library itself (trust the dependency; test YOUR use of it).
- Exact strings that change for cosmetic reasons (test the data, not the
  formatting) — except contracts where the string IS the API.
- Trivial getters/pass-throughs. Coverage theater wastes the budget the
  money paths need.

## Structure rules

- Arrange–Act–Assert, visually separated; ONE behavior per test; the test
  name states the expectation in plain words
  (`retry_stops_after_three_failures`, not `test_retry_2`).
- Tests are independent: any subset, any order, parallel-safe. Shared
  mutable fixtures are a defect.
- Determinism is non-negotiable: fixed seeds, frozen clocks, no real
  network. A flaky test is worse than no test — it teaches everyone to
  ignore red. Rule: a test that flakes twice gets fixed or deleted TODAY,
  never retried-until-green.
- Fakes over mocks where possible (fake in-memory impl passing the same
  conformance suite as the real one — see distributed-systems.md). Mock
  assertions on call counts/arguments couple tests to implementation;
  use them only at true external boundaries.
- Test data: minimal and meaningful — the reader should see WHY that input
  produces that output. Builders/factories over 40-line fixture blobs.

## Honesty rules (the ones cheap models break)

- A test you watched fail is worth ten you haven't: new test → break the
  code (or write test first) → see red → fix → see green. A test born green
  proves nothing (it may test nothing).
- NEVER weaken an assertion to pass CI (anti-patterns.md #2). Tolerance
  widening, `assertTrue(True)`, commenting out — all reviewer-blocking.
- Coverage is a flashlight, not a goal: use it to find untested money
  paths; never write tests whose only purpose is the percentage.
- If code is hard to test, that's design feedback (hidden dependencies,
  global state) — flag it; don't force the test with mock pyramids.

## Verification rubric (binary)

- [ ] Every public behavior changed in this diff has a test exercising it
- [ ] Each new test was observed red before green (or written before code)
- [ ] The named edge cases (empty/many/malformed/concurrent) appear as tests
- [ ] No test asserts on private internals or mock call-shapes except at
      external boundaries
- [ ] Full suite run twice → identical results (no flakes)
- [ ] No assertion weakened relative to the previous commit (diff the tests)
