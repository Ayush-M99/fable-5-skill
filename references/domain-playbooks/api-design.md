# Playbook — API design (interfaces that survive their consumers)

An API is a promise. Everything here exists so the promise stays cheap to
keep. Applies to REST/RPC/GraphQL externally and library interfaces
internally.

## Pipeline

1. **Consumer first (best):** write the calls you WISH existed — 5–10
   example requests/responses for the real use cases — before any schema.
   Design from the caller's side; implementation convenience never leaks
   into the interface.
2. **Contracts (best):** resources, verbs, status codes, error shape,
   pagination, auth — written as a spec (OpenAPI or equivalent) before
   handlers. The spec is law (bootstrap rule).
3. **Implement (Sonnet):** handlers against the spec; generated
   client/server stubs where the ecosystem supports it.
4. **Contract tests (Sonnet):** every example from step 1 becomes a test;
   spec-vs-implementation drift check in CI.
5. **Verify (Haiku, rubric below).**

## Naming and shape rules

- Nouns for resources (`/orders/{id}`), plural, kebab/lowercase in paths;
  verbs only for true actions that aren't CRUD (`/orders/{id}/cancel`).
- Consistency beats elegance: one casing for JSON fields everywhere
  (pick snake_case or camelCase ONCE, per ecosystem, write it in the spec).
- IDs are opaque strings to consumers (UUIDs/prefixed ids like `ord_x7f…` —
  prefixes make logs and bug reports self-identifying). Never expose
  auto-increment integers externally.
- Timestamps: UTC ISO-8601, suffix `_at`; booleans ask a question
  (`is_active`); money as integer minor units + currency, never floats.
- Every list endpoint is paginated FROM DAY ONE (cursor-based by default;
  offset only for small, static sets). Retrofitting pagination is a
  breaking change; shipping without it is a time bomb.

## Errors (one shape, everywhere)

```json
{ "error": { "code": "ORDER_NOT_FOUND", "message": "human sentence",
             "details": [ ... ], "request_id": "req_..." } }
```
- Stable machine code (from the project's error catalogue) + human message +
  request id for support. Same shape for 4xx and 5xx.
- Status codes honestly: 400 malformed / 401 unauthenticated / 403
  unauthorized / 404 absent / 409 conflict / 422 valid-shape-invalid-
  content / 429 rate limited. Never 200-with-error-body.
- Validation errors list ALL failures at once, per field — not one per
  round-trip.

## Evolution (the part everyone gets wrong)

- Additive changes only, forever: new optional fields, new endpoints, new
  enum values (consumers must be told to ignore unknown fields/values from
  day one — write it in the spec).
- Breaking = new version (path `/v2/` or header), with the old one kept on
  a published deprecation clock. Renaming a field is: add new + deprecate
  old + remove in next major — never rename in place.
- Idempotency: all writes accept an `Idempotency-Key` (or are naturally
  idempotent PUTs) — see distributed-systems.md.
- Rate limits + max page size + max body size stated in the spec, enforced,
  and returned in headers — limits added later break clients that never
  knew them.

## Verification rubric (binary)

- [ ] Example calls written before schema (check ledger dates)
- [ ] Every endpoint in the spec; drift check spec↔implementation passes
- [ ] One JSON casing throughout (grep for the other one — zero hits)
- [ ] Every list endpoint paginated; max page size enforced + tested
- [ ] Error shape identical everywhere incl. auth failures and 500s (test
      asserts the shape, not just the code)
- [ ] No auto-increment ids in any response (grep)
- [ ] Writes idempotent (retry test exists) or documented why not
- [ ] Unknown-field tolerance stated in spec; enum docs say "may grow"
- [ ] Breaking-change policy (versioning + deprecation window) written in
      the spec
