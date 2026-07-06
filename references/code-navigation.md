# Code navigation — understanding an unfamiliar codebase before touching it

Editing code you haven't mapped is how cheap models make plausible-looking
changes that break invisible contracts. Mapping is procedural.

## The 15-minute recon (before any edit in a new repo)

1. **Read the repo's own instructions first:** README, CLAUDE.md/AGENTS.md,
   CONTRIBUTING, PROGRESS/ledger files. Settled decisions live there;
   re-deriving them wastes the session and re-litigates closed questions.
2. **Map the skeleton:** top-level directory listing + the build/config
   files (package.json / pyproject / Cargo.toml / Makefile). The dependency
   list and the scripts section tell you the stack, the entry points, and
   how to run tests — before reading any source.
3. **Find the entry points:** main/index/app files, route registrations,
   CLI command definitions. Trace ONE request/command end-to-end at skim
   depth — this single trace teaches the architecture better than reading
   ten files fully.
4. **Learn the local dialect:** open the 2–3 files most similar to what
   you'll write. Note: naming style, error handling pattern, test style,
   import organization. Your code must look native, not transplanted.

## Search strategy (cheapest first)

- Filename/symbol guess (`glob`, IDE symbol search) → grep exact strings
  (error messages, API paths, config keys are the best anchors — they're
  unique) → grep identifiers → read files. Full-file reading is the LAST
  resort, not the first move.
- Error message in hand? Grep its most distinctive substring — quotes and
  placeholders stripped. This finds the throw site in one move.
- Feature in hand? Find its UI string or route path, grep it, walk inward.
- Before writing any helper: grep for it existing (`format`, `parse`,
  `retry`, `slug` …). Duplicating an existing utility is a defect
  (anti-patterns.md #10).

## Building the change map (before editing)

- List every file the change should touch, WITH a one-line reason each.
  A "fix X" change map that reaches 5+ files = scope alarm
  (escalation-and-limits.md trigger 4).
- Find who calls what you're changing: grep the function/endpoint name;
  every caller is either compatible with your change or part of it.
- Find the tests that cover the area BEFORE editing; run them; they are
  your before-picture. No tests there? Write the pinning test first —
  it proves current behavior before you change it.

## Reading rules

- Read the data structures/schemas first, logic second — code is easy to
  follow once you know the shapes it moves.
- Trust code over comments and docs when they disagree — then flag the
  disagreement (never silently resolve; anti-patterns.md #4).
- A function's contract = its callers' expectations, not its docstring.
  Two callers using it differently = a trap; note it.
- Note every surprise in the ledger as you explore ("expected X, found Y").
  Surprises are either your misunderstanding (fix your model) or the
  codebase's debt (flag it). Both are valuable; neither should evaporate.
