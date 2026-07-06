# Playbook — Distributed systems / backends

## Pipeline

1. **Contracts first (best):** API schemas, message formats, DB schema, and
   state machines written BEFORE handlers. Every entity's states and legal
   transitions enumerated. Every failure gets a stable error code from one
   catalogue (CareerOS pattern: `CAR-*` codes, one file, every failure maps
   to one) — never ad-hoc error strings.
2. **Failure semantics per boundary (best):** for every network hop, decide
   and write down: timeout value, retry policy (with backoff + cap),
   idempotency strategy, and what the caller sees on permanent failure.
   "We'll handle errors later" is the project-killing sentence.
3. **Single-node correctness (Sonnet):** business logic tested with fakes
   for every external dependency (CareerOS pattern: Fake providers +
   conformance test mixins shared between fake and real — the fake is
   guaranteed to behave like the real one because both pass the same suite).
4. **Integration against real deps (Sonnet):** dockerized real DB/queue,
   migrations checked both directions (`upgrade head` + drift check = zero
   diffs — the CareerOS B-001 pattern).
5. **Chaos pass (Sonnet):** kill the DB mid-request, duplicate a message,
   deliver out of order, make a dependency slow-but-alive. The written
   failure semantics from step 2 must be what actually happens.
6. **Verify (Haiku, rubric below).**

## Verification rubric (binary)

- [ ] Every write endpoint is idempotent OR documented why it doesn't need
      to be (retry of the same request cannot double-apply — test exists)
- [ ] Every network call has an explicit timeout (grep for calls without
      one — actually run it); no infinite retries anywhere
- [ ] Every failure path returns a catalogued error code; grep finds no
      ad-hoc error strings in responses
- [ ] State machines: no code path creates a transition not in the spec's
      enumeration (test iterates transitions)
- [ ] Migrations: fresh DB → `upgrade head` clean; ORM/schema drift check =
      zero diffs; downgrade one step works
- [ ] Fakes and real implementations pass the same conformance suite
- [ ] Chaos results logged: duplicate delivery, out-of-order, dep-down each
      produce the documented behavior
- [ ] Load sanity: p99 latency + error rate at expected traffic measured
      once and written in ledger (any tool, even a crude script)

## Known failure modes

1. **Retry without idempotency = duplicate writes.** The classic. Step 2
   forces the pair decision together.
2. **Missing timeout = one slow dependency freezes the fleet.** The grep in
   the rubric is mandatory because this is invisible in happy-path tests.
3. **Distributed transaction fantasies.** Two services "atomically" updated
   without an outbox/saga. If step 1's state machine needs cross-service
   atomicity, that's a `best`-model design session, not an implementation
   detail.
4. **Fake drifts from real.** Tests green, production broken. Shared
   conformance suite (step 3) is the only known cure.
5. **Error handling as afterthought.** Error catalogue retrofitted = every
   caller rewritten. Step 1 does it while it's cheap.
6. **Ordering assumed.** Queues deliver out of order eventually. The chaos
   pass proves the code doesn't care.
