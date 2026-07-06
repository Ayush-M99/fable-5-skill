# Playbook — Game development (fun by procedure)

Fun feels like magic; most of it is mechanics with known numbers. This
playbook encodes them so a cheap model ships a game that FEELS good, not
just runs.

## Pipeline

1. **Core loop on paper (best):** one paragraph before any code — the
   30-second loop the player repeats (act → feedback → reward → repeat),
   the fail state, and the ONE verb the game is about (jump, shoot, match,
   build). A game whose core verb isn't fun stays unfun under any amount of
   content. Scope rule: whatever's planned, CUT HALF before starting —
   unfinished-but-polished beats finished-but-flat.
2. **Greybox prototype (Sonnet):** rectangles and circles only. No art, no
   menus, no story. Goal: the core verb playable in the first session.
   Gate: someone plays the greybox for 2 minutes voluntarily → proceed.
   Boring with rectangles = boring with art; fix the loop, not the visuals.
3. **Game feel pass (Sonnet, numbers below):** apply the juice table to the
   core verb BEFORE adding content. This pass is where "feels amazing"
   lives, and it's pure numbers.
4. **Content + difficulty (Sonnet/Haiku):** levels/waves/progression.
   Teach → test → twist per mechanic: introduce it safely, then require it,
   then combine it with an old one. First fail should come from the player's
   choice, not from unread instructions.
5. **Verify (Haiku, rubric below) + playtest.** Playtest rule: watch
   silently, never explain. Every point where the tester asks "what do I
   do?" is a design defect, logged in the ledger.

## Game feel numbers (the juice table — copy these, tune later)

| Technique | Default value | Applies to |
|---|---|---|
| Input latency | action starts same frame as input; animation may lag, logic may not | everything |
| Coyote time | jump allowed 80–120ms after leaving a ledge | platformers |
| Input buffering | inputs pressed ≤ 100–150ms early still fire | all action games |
| Asymmetric jump | fall gravity 1.5–2x rise gravity; cut velocity ~50% on button release | platformers |
| Hitstop | freeze both parties 30–80ms on heavy hits | combat |
| Screenshake | trauma-based (shake = trauma², decay ~1.5/s), rotational > positional, ≤ 300ms | impacts |
| Squash & stretch | 10–20% scale deform on land/hit/bounce, spring back ~100ms | anything that moves |
| Particles | every impact spawns something (dust, sparks); lifetime ≤ 1s, pooled | everything |
| Sound layering | one action = 2–3 layered sounds with ±10% random pitch | every action |
| Death/restart | restart input available within ~500ms of death; no unskippable death animation | all |
| Muzzle flash / attack anticipation | 2–4 frames anticipation, exaggerated release | combat |

Rule: the core verb gets AT LEAST four rows of this table before any content
is added.

## Architecture rules (so it stays shippable)

- Fixed timestep for logic (60Hz), interpolated rendering; never tie physics
  to framerate.
- Explicit state machines for game states (menu/playing/paused/dead) and
  entity states — no boolean soups (`isJumping && !isDead && canMove`).
- Object pooling for anything spawned more than once per second (bullets,
  particles).
- Frame budget 16ms: profile before optimizing (see
  `systems-performance.md`); usual suspects are draw calls, per-frame
  allocation/GC, and unpooled spawns.
- Separate data from behavior (entity configs as data files) — content
  tuning must not require code edits.
- Save/checkpoint from day one if the game exceeds one sitting.

## Verification rubric (binary)

- [ ] Core loop paragraph + core verb written in ledger before code (check
      dates)
- [ ] Greybox gate passed: a real person played 2+ minutes voluntarily,
      noted in ledger
- [ ] ≥ 4 juice-table rows implemented on the core verb, named in ledger
- [ ] Input → visible reaction in ≤ 1 frame (test: record + step through)
- [ ] Death → able to retry in ≤ 1s; game over never dead-ends (restart
      without reload)
- [ ] Every mechanic has teach → test → twist placement, listed in ledger
- [ ] No physics/logic behavior change between 30/60/144 fps (actually test
      capped)
- [ ] One full playthrough with zero console errors; pause works everywhere
- [ ] A stranger can start playing without instructions (playtest note
      exists)
- [ ] Frame time ≤ 16ms in the heaviest scene (profiler output in ledger)

## Known failure modes

1. **Engine before game.** Weeks of "systems" with nothing playable.
   Greybox gate at step 2 exists for this — playable in session one.
2. **Art before fun.** Polishing sprites on a boring loop. Rectangles rule.
3. **Scope kills everything.** The #1 game-dev killer at every skill level.
   Cut-half rule at step 1; a cheap model must enforce it mechanically.
4. **Juice skipped as "polish later".** Later never comes; game ships flat.
   Step 3 sits BEFORE content on purpose.
5. **Difficulty tuned for the developer.** Author plays 100x better than a
   new player. Number: a first-time tester should survive ≥ 60 seconds.
6. **Framerate-coupled logic.** Works on the dev machine, breaks on a fast
   monitor. Fixed timestep rule.
7. **Tutorial as text wall.** If it needs a paragraph, redesign the level to
   teach it silently (safe intro → required use).
