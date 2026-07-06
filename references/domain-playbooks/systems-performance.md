# Playbook — Hard systems / performance-critical work

## Pipeline

1. **Numbers first (best):** write the target before the code: throughput,
   latency budget, memory ceiling, and the measurement method. "Fast" is
   not a spec; "p99 < 5ms at 10k ops/s on this hardware" is.
2. **Measure before optimizing (Sonnet):** profile the REAL workload;
   paste the profile summary in the ledger. The hot spot is where the
   profiler says, not where intuition says — this rule exists because
   intuition here is wrong more than half the time, for every model tier
   and most humans.
3. **Benchmark harness before changes (Sonnet):** a repeatable benchmark
   (fixed input, fixed iterations, warm-up, reports mean + p99) committed
   to the repo. Every optimization claim cites before/after numbers from
   THIS harness, same machine, same config.
4. **One optimization per commit (Sonnet/Haiku):** each commit = one change
   + its benchmark delta in the commit message. Regressions bisect cleanly.
5. **Correctness net (Sonnet):** tests pass before AND after each
   optimization; UB/race checkers where the language has them (ASan/TSan,
   miri, -race) run in CI, not just locally.
6. **Verify (Haiku, rubric below).**

## Verification rubric (binary)

- [ ] Target numbers written in spec/ledger BEFORE optimization started
- [ ] Profile output (not a paraphrase) exists in ledger; the optimized code
      is in the profiler's top entries
- [ ] Benchmark harness committed; run instructions documented; results
      reproducible within stated tolerance across 3 runs
- [ ] Every optimization commit message contains before/after numbers
- [ ] Full test suite green after each optimization
- [ ] Sanitizer/race-detector pass logged (or written N/A with reason)
- [ ] Final numbers vs target table in ledger: met / not met per metric
- [ ] Anything still below target has a named next step or a written
      "accepted, because…" — not silence

## Known failure modes

1. **Optimizing the cold path.** No profile, strong opinions. Step 2 is
   non-negotiable; a cheap model must refuse to optimize without a profile
   in the ledger.
2. **Benchmark theater.** Different machine/config between before and after;
   numbers meaningless. Same-harness rule.
3. **Fast but wrong.** Optimization breaks an edge case; tests were skipped
   "because it's just perf". Step 5.
4. **Heroic unreadable code for 2%.** Rule: an optimization under 10% gain
   that increases complexity needs a written justification in decisions/,
   else revert.
5. **Targets invented after the fact.** "We got 3x faster!" — than what,
   toward what need? Step 1 prevents victory laps over meaningless numbers.
