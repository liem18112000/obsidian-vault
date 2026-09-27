---
title: "CSS grid ::before counter plus N children overflows columns and crushes the last child"
created: 2026-09-09
type: gotcha
status: seedling
source: "session 2026-09-09"
tags: [css, grid, layout, gotcha, pseudo-element]
---

# CSS grid ::before counter plus N children overflows columns and crushes the last child

In a CSS grid row that uses a `::before` counter/badge as its first cell, the `::before` counts as a grid item. If the number of real child elements plus that pseudo-element **exceeds the column count**, grid auto-placement wraps the extra child onto the next row starting in **column 1** — typically a narrow `auto`/badge column — so its text gets crushed into a sliver and the row "looks broken".

**Example:** `.row{display:grid;grid-template-columns:auto 1fr auto}` with `::before` + `<b>` + `<span>` + `<p>` = 4 items in 3 columns. The `<p>` lands in column 1 under the badge.

**Fix:** pin the content children to their intended tracks explicitly — `b,p{grid-column:2}` and the trailing badge `{grid-column:3;grid-row:1/span 2}` — instead of relying on auto-placement. (Or wrap the text children in one container so the row has exactly as many items as columns.)

Watch for it whenever a list item is a grid with a generated counter and stacked title+description.
