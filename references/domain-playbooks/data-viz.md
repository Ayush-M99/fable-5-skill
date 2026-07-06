# Playbook — Data visualization (charts that inform instead of decorate)

A chart is a sentence about data. Decide the sentence first; the chart type,
colors, and layout all follow by rule.

## Step 1 — The sentence (any model)

Write the one-line claim the chart must make ("errors spiked after the
deploy", "segment B converts 2x segment A"). No sentence = no chart; a
table is more honest than an undirected graphic.

## Chart choice (lookup table, not taste)

| The sentence is about… | Use | Never |
|---|---|---|
| Change over time | Line (area only for cumulative/stacked share) | Bar-per-day walls |
| Comparison between categories | Horizontal bar, sorted by value | Vertical bars with rotated labels; radar |
| Part-of-whole, ≤ 5 parts | Stacked bar (pie acceptable at ≤ 3 slices) | Pie with 6+ slices, donuts with legends |
| Distribution | Histogram / box / violin | 3D anything |
| Correlation | Scatter (+ trend line if claimed) | Dual-axis line pairs (they fabricate correlation) |
| Flow/composition change | Sankey / slope chart | Exploded pies |
| Single number that matters | Big stat tile + sparkline for context | A gauge |

- More than ~6 series on one chart = split into small multiples (same
  scale, same axes, gridded) — the single strongest upgrade for cluttered
  charts.

## Honesty rules (violations are defects, not style choices)

- Bar charts start at zero, always. Line charts may zoom, but the axis
  range must be shown and the zoom must not be the source of the claim.
- Time axes: uniform intervals; missing periods shown as gaps, not skipped.
- Sorted by value unless the axis has natural order (time, ordinal).
- No dual y-axes (two charts instead). No 3D. No truncated axes to
  dramatize. Annotate real events; don't let the reader guess causes.
- Uncertainty shown when it exists (error bars/bands); n stated when small.

## Color (inherits design-craft.md, plus)

- Sequential data → one hue, varying lightness. Diverging data → two hues
  through a neutral midpoint. Categories → max 6 distinct hues, then group
  into "other".
- ONE series gets the accent color — the one the sentence is about;
  everything else gray. "Gray + one accent" outperforms rainbow defaults in
  every readability test that matters.
- Red/green never the only distinction (8% of male readers can't see it) —
  pair with position, shape, or label.
- Direct-label lines at their endpoints where possible; legends make eyes
  ping-pong.

## Dashboard layout

- Answer-ordered: top-left = the single number/most important chart; detail
  and diagnostics below the fold. A dashboard is a briefing, not an archive.
- Max ~6 charts per view; every chart passes Step 1 (has a sentence) or is
  removed.
- Same metric = same color, same scale, same unit format on every chart in
  the product (a lookup constant, not per-chart choices).
- Numbers: thousands separators, SI abbreviations (12.4k), 1–2 significant
  decimals, units on axes not in titles.

## Verification rubric (binary)

- [ ] The claim sentence is written, and the chart title states it (titles
      are claims — "Errors spiked after the 3.2 deploy" — not "Error count")
- [ ] Chart type matches the lookup table row for that claim
- [ ] All bar charts start at zero; no dual axes; no 3D anywhere
- [ ] Categories sorted by value (or natural order, stated)
- [ ] ≤ 6 series per chart; accent-vs-gray rule applied
- [ ] Not red/green-only anywhere (simulate or check)
- [ ] Axes labeled with units; source and date range stated on the figure
- [ ] Same metric styled identically across all views (grep the config)
