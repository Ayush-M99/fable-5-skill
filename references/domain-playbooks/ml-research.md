# Playbook — ML / deep learning research

## Pipeline

1. **Hypothesis file (best):** before any training run — one file stating:
   the question, the metric that answers it, the baseline number to beat,
   and the decision rule ("if metric < X, abandon; if ≥ Y, scale up").
   No run starts without a falsifiable claim written down.
2. **Data audit (Sonnet):** row counts, class balance, leakage check
   (train/test overlap, temporal leakage), 10 hand-inspected samples pasted
   into the ledger. Data problems found after training are 10x costlier.
3. **Baseline first (Sonnet):** simplest thing that could work (majority
   class, linear model, off-the-shelf pretrained). Its score goes in the
   ledger. A fancy model that can't beat the logged baseline is a negative
   result, not a draft.
4. **Experiment loop (Sonnet runs, best interprets):** ONE variable changes
   per run. Every run gets: config file (not CLI flags), fixed seed, git
   commit hash, and a row in the experiments table (run id, config, seed,
   metric, wall time, one-line takeaway).
5. **Verify (Haiku, rubric below).**
6. **Write-up (best):** what was claimed, what was measured, what the
   decision rule says to do next. Negative results get written up too.

## Verification rubric (binary)

- [ ] Hypothesis file exists with metric + baseline + decision rule
- [ ] Every experiment row has: config path, seed, commit hash, metric value
- [ ] Same config + same seed reruns to the same metric (±1% or a stated
      tolerance) — actually rerun once to check
- [ ] Test set touched exactly once per hypothesis, never for model selection
- [ ] Baseline score logged BEFORE any complex model's score
- [ ] No data leakage: dedup check between train/test logged in ledger
- [ ] Plots have axes labels, and the plotting script is committed (no
      screenshot-only results)
- [ ] Ledger updated: takeaway line per run, next decision per decision rule

## Known failure modes

1. **Metric drift.** Started measuring accuracy, quietly switched to F1
   mid-project because it looked better. The hypothesis file's metric is
   law; changing it requires a new hypothesis file.
2. **Seed roulette.** Rerunning until a good number appears. Fixed seeds +
   logged runs make this visible.
3. **Baseline skipped.** Weeks on a transformer that loses to logistic
   regression. Step 3 is mandatory.
4. **Comparing runs that differ in 2+ variables.** The one-variable rule
   exists for this; a cheap model must refuse to interpret a two-variable
   diff and escalate instead.
5. **Untracked environment.** CUDA/library version changed mid-project,
   results shifted. Environment (exact versions) lives in the ledger's
   environment block, checked at session start.
6. **GPU hours burned on unverified data.** Step 2 before step 4, always.
