# Playbook — Security hardening (defensive checklist)

Extends craft-rules.md's security floor into a working procedure. Scope:
DEFENSIVE hardening of your own systems. Anything offensive-shaped, or
designing auth/crypto from scratch, is an automatic escalation
(escalation-and-limits.md trigger 5) — this playbook is the checklist tier,
not the expert tier.

## Threat-model-lite (10 minutes, before building anything exposed)

Write four lists in the ledger:
1. **Assets:** what's worth stealing/breaking here (data, credentials,
   compute, reputation).
2. **Entry points:** every place untrusted input arrives (routes, file
   uploads, webhooks, query params, LLM outputs, third-party callbacks).
3. **Actors:** anonymous user / authenticated user / admin / insider /
   automated bot — which entry points can each reach?
4. **Worst plausible event:** the single scenario you most need to not
   happen. Design review focuses there first.

## Input handling (every entry point, no exceptions)

- Validate at the boundary: schema/type/length/range BEFORE any logic.
  Allowlist what's valid; never blocklist what's known-bad.
- Parameterized queries only (SQL/NoSQL). String-built queries are
  reviewer-blocking, even in scripts.
- Output encoding per context: HTML-escape for HTML, parameterize for SQL,
  exec arrays (never shell strings) for commands.
- File uploads: validate type by content not extension, cap size, store
  outside the web root under generated names, never execute.
- Paths from users: resolve, then verify the result is inside the allowed
  directory — string prefix checks alone don't survive `..` and symlinks.
- LLM output IS untrusted input: schema-validate before acting on it; tool
  calls it requests pass the same authz as a user would
  (see agentic-systems.md guardrails).

## AuthN / AuthZ (use established; verify usage)

- Passwords: argon2id or bcrypt via a maintained library — never hand-rolled
  hashing, never reversible storage.
- Sessions/tokens: expiry + revocation path (denylist/rotation); tokens in
  httpOnly + Secure cookies where applicable; JWTs verified with pinned
  algorithm (the token's own `alg` header is attacker input).
- Authorization on EVERY server endpoint — UI hiding is not authz. The
  classic miss is object-level: `/orders/123` must check the order BELONGS
  to the caller, not just that the caller is logged in (IDOR).
- Rate limiting on auth endpoints and anything expensive; generic error
  messages on login (no "user exists but wrong password").

## Secrets and supply chain

- Secrets: env/secret manager only; `.env` gitignored; pre-commit grep
  (see devops-infra.md rubric). A secret that ever touched git history is
  rotated, not deleted.
- Dependencies: lockfiles committed; `npm audit`/`pip-audit`/`cargo audit`
  in CI; new dependency = one-line justification (craft-rules.md) + a look
  at its maintenance pulse.
- Never log secrets, tokens, passwords, or full card/PII fields — add
  redaction at the logger, not at each call site.

## Verification rubric (binary)

- [ ] Threat-model-lite (4 lists) in ledger before first deploy
- [ ] Grep: no string-built SQL, no eval/exec on user input, no shell
      string concat with user data — zero hits each
- [ ] Every mutating endpoint checks authz including object ownership
      (test: user A requests user B's resource id → 403/404)
- [ ] Auth endpoints rate-limited (test proves it)
- [ ] Secrets grep clean; lockfile committed; audit tool wired in CI
- [ ] Uploads (if any): content-type verified, size-capped, non-executable
      storage
- [ ] Error responses leak no stack traces/paths/SQL in production mode
- [ ] Logs redact credentials (grep log output in tests for a planted
      secret)

## Known failure modes

1. **Auth ≠ authz.** Logged-in user reads everyone's data via id iteration
   (IDOR) — the most common real-world API hole. Object-ownership test is
   mandatory.
2. **Validation on the client only.** The API trusts the UI; curl doesn't
   use the UI.
3. **Blocklist mentality.** Filtering `<script>` while attackers use the
   hundred other vectors. Allowlist + context encoding.
4. **Secrets in "temporary" code.** The debug print with the token ships.
   Logger-level redaction catches what discipline misses.
5. **Trusting LLM output.** Agent output executed/queried raw = injection
   with extra steps. Same boundary rules as user input.
6. **Security review after launch.** The 10-minute threat model exists
   because 10 minutes before beats 10 days after.
