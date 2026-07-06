# Playbook — Frontend / UI apps (web + mobile)

## Pipeline

1. **Spec (best):** one page before any code — screens list, user flows,
   data in/out per screen, chosen stack WITH pinned versions, and the visual
   direction named explicitly (pick a named style — e.g. "editorial",
   "neobrutalism" — never "make it look good"). If design skills are
   installed, NAME which one applies; don't let the model pick silently.
2. **Scaffold (Sonnet):** project skeleton, router, design tokens file,
   one fully-styled example screen. Stop and verify before mass-producing
   screens — the example screen is the visual contract for every other one.
3. **Build screens (Sonnet/Haiku):** each screen copies the example screen's
   patterns. New visual patterns require going back to step 1, not improvising.
4. **Wire data (Sonnet):** real data or realistic fixtures. Empty, loading,
   and error states for every data-bearing view.
5. **Verify (Haiku, rubric below):** run it, screenshot every screen, check.
6. **Log:** append to PROGRESS.md which style/skill combination produced the
   result — this is what makes good outcomes reproducible next session.

## Verification rubric (binary)

- [ ] App starts from a clean install (`npm ci`/fresh venv) with zero manual
      fixes — the actual command run and its output are in the ledger
- [ ] Every screen in the spec exists and is reachable by navigation
- [ ] Every data-bearing view has visible empty, loading, and error states
- [ ] No default-template look: theme tokens differ from framework defaults
      (check: primary color, font family, and border radius are all set
      explicitly in the tokens file)
- [ ] Interactive elements have hover/press states; touch targets ≥ 44px
      (mobile)
- [ ] Keyboard navigation reaches every control; images have alt text
- [ ] Console shows zero errors on a full click-through
- [ ] Versions pinned (lockfile committed); ledger updated with style combo

## Known failure modes

1. **Generic-template output.** Cause: visual direction never named. Fix is
   in step 1, not in polishing afterwards.
2. **Screen N contradicts screen 1's patterns.** Cause: skipped the
   example-screen contract. Mass production before verification.
3. **Works on my machine.** Cause: never tested from clean install. The
   rubric's first line exists for this.
4. **Good result, unreproducible.** Cause: winning style/skill combo never
   logged. Step 6 exists for this.
5. **Screenshot-and-complain loops forever.** Rule: after 3 adjustment
   rounds on the same screen, stop — the spec's visual direction was too
   vague; rewrite it (best model) instead of iterating (any model).
