# Playbook — Agentic systems / LLM apps

## Pipeline

1. **Contract first (best):** for every agent/LLM call site, write: input
   schema, output schema, failure behavior (timeout, malformed output,
   refusal), and the escalation path (retry? fallback model? human?).
   An LLM call without a written failure behavior is a defect.
2. **Provider abstraction (Sonnet):** all model calls go through one
   interface; no SDK calls scattered in business logic. Model name, params,
   and prompts are config, not code.
3. **Prompts as versioned files (Sonnet):** every prompt lives in a file
   with a version (`name@semver` pattern), never inline in code. Changing a
   prompt = new version + eval run, same as changing code = tests.
4. **Evals before scale (best writes, Sonnet/Haiku runs):** a fixed set of
   input→expected-property cases per prompt (not exact-match — property
   checks: "output parses as JSON", "cites a source", "refuses X").
   Minimum 10 cases per prompt before it ships. Eval score is logged per
   prompt version in the ledger.
5. **Tracing (Sonnet):** every agent run logs: prompt version, model id,
   input hash, output, latency, token counts. Debugging an agent without
   traces is guessing.
6. **Guardrails (best):** the actions the agent must NEVER take, written as
   hard code-level gates, not prompt instructions ("never auto-submit",
   "never bypass CAPTCHA/anti-bot", "human approves irreversible actions").
   Prompt-level "please don't" is not a guardrail.
7. **Verify (Haiku, rubric below).**

## Verification rubric (binary)

- [ ] Every LLM call site has schema-validated output parsing with a defined
      behavior on parse failure (not an unhandled exception)
- [ ] Zero inline prompts in code (grep for multi-line strings near model
      calls — actually run the grep)
- [ ] Every prompt file has a version and ≥10 eval cases; latest eval score
      logged in ledger
- [ ] Prompt changed ⇒ eval rerun in the same commit (check git log)
- [ ] Irreversible actions gated by code (find the gate, name the file:line
      in the ledger), not by prompt text
- [ ] Traces include prompt version + model id + token counts
- [ ] Fallback tested: kill the primary model key in a test env; system
      degrades per the written failure behavior, not with a stack trace
- [ ] Cost estimate per 1000 runs written in ledger (tokens × price)

## Known failure modes

1. **Prompt drift.** Someone "improves" a prompt inline, evals never rerun,
   quality silently regresses. Versioned files + eval-per-change kills this.
2. **Happy-path agents.** Demo works; first malformed output in production
   crashes the loop. The contract (step 1) is written for the failure path
   first.
3. **Guardrails in the prompt.** "You must never X" in a system prompt is a
   suggestion, not a gate. Real gates are code.
4. **Untraceable regressions.** "It got worse last week" with no traces =
   unfalsifiable. Step 5 before scale.
5. **Model-upgrade breakage.** New model version changes output format;
   nothing pinned, nothing evaled. Pin model ids; upgrading = eval run.
6. **Eval-by-vibes.** "Looks better to me" is not an eval. Property checks a
   Haiku can run ARE.
