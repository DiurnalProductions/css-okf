---
type: Concept
title: CSS Grid Layout
description: "Two-dimensional layout system with explicit rows, columns, tracks, and named areas"
tags: [css, grid, layout]
prerequisites:
  - concepts/display
  - concepts/flexbox
related:
  - concepts/flexbox
  - concepts/responsive-design
resource: "https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout"
timestamp: 2026-07-06
---

# Summary

Grid divides a container into rows and columns (tracks) defined by `grid-template-rows`, `grid-template-columns`, and optional `grid-template-areas`. Items are placed by line numbers, spans, or area names. Grid handles two-dimensional alignment simultaneously — ideal for page shells and complex component layouts.

# Mental model

Think spreadsheet with explicit tracks and mergeable cells:

```
grid-template-columns: 200px 1fr 1fr;
grid-template-areas:
  "header header header"
  "nav    main   aside"
  "footer footer footer";

┌──────── header ────────┐
├────┬──────────┬────────┤
│nav │   main   │ aside  │
├────┴──────────┴────────┤
└──────── footer ────────┘

Implicit tracks appear when items exceed the explicit grid.
fr units distribute free space proportionally.
```

Line numbers start at 1; negative indices count from the end. `gap` (or `row-gap` / `column-gap`) separates tracks without margin hacks.

# Example usage

```css
.page {
  display: grid;
  grid-template-columns: minmax(200px, 1fr) 3fr;
  grid-template-rows: auto 1fr auto;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  min-height: 100dvh;
  gap: 1rem;
}

.header { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main { grid-area: main; }
.footer { grid-area: footer; }

/* Spanning without named areas */
.feature {
  grid-column: 1 / -1;   /* full width */
}

.gallery {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
  gap: 1rem;
}
```

# Common mistakes

- Mixing up `grid-template-areas` names — every row must have the same number of cells and consistent area names.
- Using flexbox mental model only — grid items don't need `flex: 1`; placement and track sizing drive layout.
- Forgetting that grid children become blockified for sizing purposes.
- Over-specifying line numbers when `grid-area` or `subgrid` (supported in all major browsers since late 2023) would simplify.
- Expecting implicit row heights to match explicit row sizing without `grid-auto-rows`.

# Related concepts

* [Flexbox](/concepts/flexbox.md) — one-dimensional layouts inside grid cells
* [Display](/concepts/display.md) — `display: grid` creates the grid formatting context
* [Responsive Design](/concepts/responsive-design.md) — `auto-fill`, `minmax()`, and media queries reshape grids
