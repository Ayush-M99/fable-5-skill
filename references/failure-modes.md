# Failure modes — running log

Read before every run of the fable-5 skill. Append after every run (Phase 6,
step 4). Newest entries at the top.

## 2026-07-07 (later, same session) — from the all-domains rebuild

6. **Invented brain dumps lose to evidence on disk.** First fill of the
   master prompt guessed the user's workflow from their installed tools. The
   user's actual repos (on a different drive) showed a spec-first engineer
   with a spec-is-law project, progress ledgers, error catalogues, fake/real
   conformance testing. Lesson: before describing how a user works, LOOK AT
   THEIR REPOS — recent projects beat installed-tools inference every time.
   Ask where the code lives if the obvious drives look empty.
7. **The user's best project IS the spec.** Their best-run repo already
   contained the target system (bootstrap files, ledger discipline, binary
   DoD); the rebuild's job was extraction and generalization, not invention.
   Lesson: hunt for the user's best-run artifact first and mine it — the bar
   and the patterns are already theirs, which beats anything imported.
8. **Discipline that isn't portable doesn't propagate.** One repo had full
   discipline; three sibling repos had none. Nothing hand-built inside one
   project transfers by itself. Lesson: the fix for "my best project does
   this well" is always extraction to the computer tier / a shared skill,
   never "do it again by hand next project".
9. **Scope grows mid-rebuild.** "One system" became "all my domains" in one
   message. Lesson: absorb it by generalizing the structure (playbooks per
   domain, one shared skeleton) rather than building six separate systems.

## 2026-07-07 — seeded by Fable 5, from the session that built this skill

1. **The template arrived unfilled.** The user pasted the systematization
   prompt with its [Name it / My goal / brain dump] placeholders still blank,
   plus a second overlapping prompt, and asked for everything at once. Lesson:
   when the system-to-rebuild is missing, do NOT invent one. Build the
   reusable parts (skill, global file, prompt), and explicitly ask for the
   filled-in system before running Phases 1–5.
2. **Two prompts saying the same thing drift apart.** The user kept two
   near-duplicate versions of the same mega-prompt. Lesson: merge to ONE
   canonical file and delete/ignore the rest — duplicated instructions rot
   independently (same reason a rule lives at exactly one CLAUDE.md tier).
3. **Wrong tool for a big one-off.** The task arrived through /loop (a
   recurring-task command) because the intended command rejected long input.
   Lesson: treat the content as the intent, not the wrapper it arrived in;
   don't schedule a loop for a one-shot job.
4. **"Bank the judgment" requests skip the interview.** The user's focus was
   preserving the model's discipline, so the interview about THEIR workflow
   never happened — meaning the global CLAUDE.md has no cross-project user
   preferences yet. Lesson: on the next real rebuild, run Phase 2 fully and
   promote only the preferences true for EVERY project into
   `~/.claude/CLAUDE.md`.
5. **Deadline pressure produces bloat.** "Last day of the good model" tempts
   you to write everything down. Lesson: the test stays "would a cheap model
   get this wrong without this line?" — deadline doesn't change it.

## (append future entries above this line's date heading, newest first)
