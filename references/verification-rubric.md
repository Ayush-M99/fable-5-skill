# Verification rubric — template

Copy this into every system you build. Fill the brackets. Every check must be
answerable YES/NO by reading the output — no taste, no extra reasoning budget.
Haiku runs this exactly as written; there is no "think harder" fallback.

## Rubric for: [system name]

Output being checked: [what artifact — a post, a PR, an email, a video edit]

### Structural checks (does it contain the required parts?)
- [ ] Contains [required section/field 1]
- [ ] Contains [required section/field 2]
- [ ] [Field Z] is filled in, not a placeholder
- [ ] Length is between [min] and [max] [words/lines/seconds]

### Match checks (does it match the known-good example?)
- [ ] Format matches the example at [path/to/example] — same sections, same
      order
- [ ] Tone check by RULE, not feel: contains none of the banned words/phrases
      listed at [path]; uses [first/second] person throughout
- [ ] [Any other concrete "same as example Y" comparison]

### Failure-mode checks (does it avoid the known bad patterns?)
- [ ] Does NOT [known failure 1 — from failure-modes.md]
- [ ] Does NOT [known failure 2]

### Verdict rule (no judgment allowed)
- ALL boxes checked → PASS. Ship it.
- ANY box unchecked → FAIL. Fix ONLY the unchecked items, re-run the rubric.
  After 2 failed loops, stop and escalate to the user or a stronger model —
  do not keep looping.
