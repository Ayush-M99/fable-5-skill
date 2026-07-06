# Writing craft — prose, docs, and copy that respect the reader

Applies to READMEs, docs, commit messages, UI copy, reports, emails.
Writing quality is mostly deletion and ordering — both are procedural.

## The universal rules

1. **Point first.** The first sentence carries the conclusion; everything
   after is support. Test: would the reader who stops after one sentence
   still get the message?
2. **One idea per sentence, one topic per paragraph.** A sentence with two
   "and"s is two sentences. A paragraph you can't title is two paragraphs.
3. **Concrete beats abstract, always.** "Improves performance" → "cuts cold
   start from 3.2s to 400ms". Numbers, names, examples — every abstract
   claim gets one within a sentence.
4. **Cut ruthlessly, by rule:** delete "very / really / quite / basically /
   just / actually"; delete "in order to" (→ "to"); delete "the fact that";
   delete openers that restate the question. Target: second draft is 25%
   shorter than the first with nothing lost.
5. **Active voice, named actor.** "The request is validated" hides WHO —
   "the gateway validates the request" doesn't. Passive is allowed only
   when the actor truly doesn't matter.
6. **No unexplained jargon on first use;** define or link. Acronyms spelled
   out once.

## Structure by document type

- **README:** what it is (1 line) → what it looks like working (screenshot /
  example output) → install → minimal usage example → configuration →
  contributing. A README whose first screen doesn't show what the thing DOES
  is broken.
- **Technical doc/spec:** context (why this exists) → the design → the
  contracts (exact) → the failure modes → open questions, explicitly
  labeled. Concrete numbers; "TBD" banned in normative sections.
- **Report/summary to a human:** outcome first ("X works / X fails because
  Y"), then evidence, then detail for those who keep reading. Never
  narrative order ("first I looked at…") — nobody reads a diary for a
  verdict.
- **Commit message:** first line = what + where, imperative, ≤ 72 chars;
  body = why, and what would break if reverted.
- **Error messages (UI or log):** what happened + what the user/operator
  should DO next. An error message without a next action is a dead end.

## UI microcopy

- Buttons say the action's outcome, not the mechanism: "Save changes", not
  "Submit"; "Delete 3 files", not "OK".
- Empty states teach: what this area is for + the one action to fill it.
  Never a bare "No data".
- Confirmations state consequence: "This permanently deletes X. It cannot
  be undone." — the scary part first, not in fine print.
- Error copy: never blame the user, never show raw codes without a plain
  sentence, always offer the retry/fix path.
- Sentence case everywhere; exclamation marks: one per app, if that.

## Verification rubric (binary)

- [ ] First sentence of the document/section states the conclusion or
      purpose (read it alone and check)
- [ ] Zero filler words from the banned list (grep for them)
- [ ] Every abstract claim has a number, name, or example within a sentence
- [ ] No paragraph longer than 6 lines; no sentence longer than ~30 words
- [ ] Reader's next action is explicit at the end (docs: what to read/do
      next; report: the decision needed; error: the fix)
- [ ] Read it aloud once — anywhere you stumble, the reader stumbles; fix it
