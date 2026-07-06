# Thinking protocol — how to decompose problems like a stronger model

The closest thing to "raw intelligence" that survives in writing. Strong
models do these moves implicitly; run them explicitly, in order, on paper
(the ledger), not in your head.

## Before starting ANYTHING non-trivial

1. **Restate the problem in your own words** — one sentence. If you can't,
   you don't understand it yet; ask or read more.
2. **Name the actual output.** Not the activity ("investigate X") but the
   artifact ("a list of the 3 causes with evidence", "a passing test").
   Work that can't name its output isn't work yet.
3. **Write what's already known and what's assumed.** Two lists. Every
   assumption is a risk; test the load-bearing ones FIRST, not when they
   explode later.
4. **Work backward from done.** What must be true at the end? What's the
   step just before that? Chain backward until you reach something you can
   do right now. Forward planning drifts; backward planning can't.
5. **Find the cheapest disproof.** Before hours of building, what 5-minute
   check would prove the approach wrong? Run it first. (Does the API even
   return that field? Does the library even support that mode?)

## Decomposition moves (when the problem is too big)

- **Vertical slice first:** the thinnest end-to-end path through the whole
  system (one input → one real output), before any component is complete.
  It flushes integration surprises while they're cheap.
- **Split by "what varies":** separate the part that's always the same
  (build once) from the part that changes per case (make it data/config).
- **Invariant hunting:** what must stay true no matter what? (Money sums to
  zero, IDs never reused, the queue never loses a message.) State the
  invariants, then design so breaking them is impossible rather than
  discouraged.
- **Enumerate the cases, don't discover them:** inputs are empty / one /
  many / too-many / malformed / hostile / concurrent. Write the list before
  coding; each becomes a test.
- **Reduce to a solved problem:** most tasks are a known thing wearing a
  costume (a cache, a state machine, a queue, a join, a graph traversal).
  Name the underlying shape before inventing a bespoke solution.

## Sanity checks (run these on your own conclusions)

- **Order-of-magnitude test:** estimate the number before measuring
  (requests/day, rows, ms). Off by 10x from the measurement = your mental
  model is wrong somewhere — find where before proceeding.
- **The inversion test:** "what would have to be true for the OPPOSITE
  conclusion?" If you can't say what evidence would change your mind, you're
  pattern-matching, not reasoning.
- **The second-order question:** after this change works, what does it make
  worse? (Cache → staleness. Retry → duplicates. Index → slower writes.)
  Every fix buys its improvement with a cost; name the cost.
- **The N=1 trap:** one passing example proves almost nothing. The rule is
  three diverse cases (typical, edge, hostile) before believing a pattern.
- **Confusion is data:** the moment something is surprising, STOP and chase
  it. Surprise = your model of the system diverges from the system. That gap
  is where the bug lives. Never push past confusion to "make progress".

## Planning multi-step work

- Order steps by information gained, not by natural sequence: do the step
  that could invalidate the plan earliest, first.
- Every plan gets a "step 0": the smallest thing that produces a real,
  checkable result today. Plans without a step 0 are essays.
- At each step boundary, compare state against the plan — drift gets flagged
  and re-planned, not absorbed silently.
- Two plans compared beat one plan polished: sketch a second approach in
  three lines before committing to the first. If you can't produce a second
  approach, you don't understand the solution space; if you can, the
  comparison usually reveals the first one's weakness.

## When reasoning stalls

- Explain the problem in writing as if to a junior — the explanation
  produces the answer more often than thinking harder does.
- Swap the question: "why doesn't X work?" → "under what conditions WOULD X
  work?" — then check which condition is missing.
- Shrink until trivial: keep removing pieces until the problem becomes
  obvious, solve it there, add pieces back one at a time.
- Budget rule: 30 minutes of genuine stall = escalation trigger (see
  escalation-and-limits.md). Stalling silently is the only wrong move.
