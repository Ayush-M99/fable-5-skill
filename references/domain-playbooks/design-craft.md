# Playbook — Design craft (making interfaces beautiful without taste)

Companion to `frontend-ui.md` (that one is the build pipeline; this one is
the visual knowledge). Strong models carry taste; you will carry numbers and
banned patterns. Followed exactly, these get you to "distinctly good" —
which beats "generically fine" every time.

## Step 0 — Commit to a direction (best model, or the rules below)

Before any CSS: write three lines in the ledger —
- **Tone word:** pick ONE (editorial / brutalist / luxurious / playful /
  technical / warm / stark). Every later choice must agree with it.
- **Signature element:** the ONE memorable thing (an oversized display font,
  a signature color, a border style, a layout gimmick). One. More = noise.
- **What this will NOT look like:** name the cliché you're avoiding.

If no strong model is available to choose: match the tone word to domain
(dev tool → technical/stark; consumer/social → warm/playful; content/reading
→ editorial; portfolio/agency → one bold gimmick).

## Typography (carries 80% of design quality)

- Max TWO typefaces: one display (headings), one text (body). Pairing rule
  with no taste needed: expressive display + neutral text, never two
  expressive ones. Safe strong pairs: serif display + sans text, or heavy
  grotesque display + humanist sans text.
- NEVER default to the same tired stack every time (Inter-for-everything is
  an AI tell). Pick per the tone word: editorial → serif display;
  technical → mono or grotesque; playful → rounded; luxurious → light-weight
  serif with tight tracking.
- Type scale: ratio 1.25–1.333 from a 16–18px body. Headings jump at least
  two scale steps from body — timid 1.2x headings read as broken, not subtle.
- Body: line-height 1.5–1.7; line length 45–75 characters (`max-width: 65ch`
  is the default answer). Headings: line-height 1.05–1.2, letter-spacing
  slightly negative for large sizes (-0.01 to -0.03em).
- Hierarchy through weight AND size contrast: pair big+light against
  small+bold; 400 next to 700, not 500 next to 600.
- Real quotes (" "), real dashes (—), tabular numerals for data tables.

## Color (a formula, not a feeling)

- Structure: 60% neutral surface, 30% secondary/structure, 10% accent. ONE
  accent color; a second only for semantic states (error/success).
- Neutrals are never pure: no #FFFFFF page on light, no #000000 page on
  dark. Tint neutrals toward the accent hue (warm accent → warm grays).
  Dark mode surfaces: #0A0A0F–#1A1A22 territory, never black; text at
  85–90% white, never 100%.
- Text contrast ≥ 4.5:1 body, ≥ 3:1 large text — check it, don't eyeball it.
- Banned without explicit request: purple-to-blue gradients on white cards,
  rainbow gradients, the blue-500/purple-600 Tailwind default look. These
  are THE "AI-generated" tells.
- Derive states, don't invent them: hover = same hue, shift lightness ~8%;
  disabled = drop chroma, not just opacity.

## Space and layout

- One spacing scale, no exceptions: 4/8/12/16/24/32/48/64/96px. Any margin
  not on the scale is a defect.
- Whitespace is the cheapest way to look expensive: when in doubt, double
  the section padding (48–96px vertical between page sections).
- Group by proximity: related items ≤ 8–12px apart; unrelated groups ≥ 32px.
  Space BETWEEN groups must exceed space WITHIN them — this one rule fixes
  most amateur layouts.
- Align to a grid; mixed alignments on one screen (some centered, some left)
  is a defect. Default: left-align text; center only short hero content.
- Border radius: pick ONE value (+ one for pills/avatars) and use it
  everywhere. Mixed radii = sloppy.
- Depth: either borders OR shadows as the primary separator, not both
  heavily. Shadows: large blur, low opacity (0 4px 24px rgba(0,0,0,0.06–0.12));
  never the harsh default `box-shadow: 2px 2px 4px`.

## Motion

- UI transitions 150–250ms; entrances ≤ 400ms. Ease-out for things
  appearing, ease-in for leaving, never `linear` for UI.
- Animate only `transform` and `opacity` (compositor-cheap). Animating
  width/height/top/left is a defect.
- One coordinated entrance (stagger children 30–60ms) beats ten scattered
  animations. If everything moves, nothing matters.
- Hover states are mandatory on every interactive element — even just a
  120ms color/elevation shift. Respect `prefers-reduced-motion`.

## The anti-slop checklist (binary, run before "done")

- [ ] Tone word + signature element written in ledger, and visible in the
      result (someone could name your signature element from a screenshot)
- [ ] Not the same typeface you always reach for; display face ≠ text face
- [ ] No pure #FFF/#000 surfaces; neutrals tinted; one accent, used ≤ 10%
- [ ] No purple-gradient-on-white-card pattern anywhere
- [ ] All spacing on the scale; between-group space > within-group space
- [ ] Body text 16px+, 45–75ch, line-height ≥ 1.5; headings ≥ 2 scale steps
      above body
- [ ] One border radius value; one shadow style; one accent
- [ ] Every interactive element has hover + focus-visible states
- [ ] Empty/loading/error states styled, not browser-default
- [ ] Contrast checked ≥ 4.5:1; reduced-motion respected
- [ ] Screenshot at 375px and 1440px both look intentional

## Known failure modes

1. **Template-with-new-colors.** Generic hero + three feature cards +
   testimonial strip. Fix: the signature element from Step 0, applied to
   layout, not just color.
2. **Everything emphasized = nothing emphasized.** Six font weights, four
   colors, three radii. The "one of each" rules exist for this.
3. **Cramped sections.** Amateur tell #1. Double the vertical padding.
4. **Decoration before hierarchy.** Gradients and glass effects on a page
   where you can't tell what to read first. Hierarchy (size/weight/space)
   first, decoration last.
5. **Dark mode as inversion.** Colors that worked on white, flipped. Dark
   mode needs its own surface ladder and desaturated accents.
