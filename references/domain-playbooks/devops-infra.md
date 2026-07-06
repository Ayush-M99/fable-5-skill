# Playbook — DevOps / infrastructure

## Pipeline

1. **State the blast radius (best):** before touching infra, write: what
   this change affects, what breaks if it goes wrong, how to roll back, and
   how you'll know it worked (the exact check command). No rollback plan
   written = change doesn't happen.
2. **Everything as code (Sonnet):** infra in version-controlled files
   (Terraform/compose/K8s manifests/scripts). A manual console change is a
   defect even when it works — it will be invisible to the next session.
3. **Secrets discipline (Sonnet):** secrets in env/secret manager, never in
   code, compose files, or the ledger. `.env.example` with fake values is
   committed; real `.env` is gitignored. Grep for leaked keys before commit.
4. **Staged rollout (Sonnet):** local → staging/test env → production, with
   the step-1 check command run at each stage and its output pasted in the
   ledger.
5. **Observability before features (Sonnet):** health endpoint, structured
   logs, and one dashboard/alert per service BEFORE the service takes
   traffic. An unmonitored service is an outage you haven't met yet.
6. **Verify (Haiku, rubric below).**

## Verification rubric (binary)

- [ ] Change exists as committed code/config, zero manual console steps
      (or the manual steps are listed in the ledger with a TODO to codify)
- [ ] Rollback command written in the ledger BEFORE the change was applied
- [ ] The step-1 "how I'll know it worked" check was actually run; output
      is in the ledger
- [ ] `git grep` for key patterns (AKIA, api_key=, token=, BEGIN PRIVATE)
      returns nothing new — actually run it
- [ ] Health endpoint answers; logs are structured (one JSON/line) not
      print-soup
- [ ] Fresh-clone bootstrap works: a new machine can go from git clone to
      running via documented commands only
- [ ] Every service the change touches has an owner-facing alert or a
      written note saying why not
- [ ] Ledger updated: what changed, stage outputs, rollback path

## Known failure modes

1. **Snowflake servers.** Manual fixes accumulate; nothing reproducible.
   Everything-as-code rule exists for this.
2. **Rollback invented during the incident.** Step 1 requires it written
   before, when you're calm.
3. **Secret in git history.** Once committed, rotating is the only fix —
   the pre-commit grep is cheaper.
4. **Staging skipped "just this once".** The once is always the outage.
   Cheap models especially must never skip stages — the rule is absolute so
   no judgment is needed.
5. **Works-when-watched.** No alerts, so failures surface via users. Step 5.
6. **Docker/host drift.** Tested in container, deployed on host (or vice
   versa). The ledger's environment block states which is canonical —
   e.g. "Docker pg16 is the DB verify path", written down once, obeyed
   every session.
