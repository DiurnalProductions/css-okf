---
type: Concept
title: CSS Display
description: "How elements generate boxes and participate in block, inline, flex, or grid formatting contexts"
tags: [css, display, layout, flexbox, grid]
prerequisites:
  - concepts/box-model
related:
  - concepts/positioning
  - concepts/flexbox
  - concepts/grid
resource: "https://developer.mozilla.org/en-US/docs/Web/CSS/display"
timestamp: 2026-01-01
---

# Summary

The `display` property defines an element's outer display type (how it participates in parent layout) and inner display type (how children are laid out). `block` stacks vertically; `inline` flows with text; `flex` and `grid` establish formatting contexts for one- and two-dimensional child layout. `none` removes the element from rendering entirely.

# Mental model

Display switches the layout engine for an element's children:

```
block formatting context          inline formatting context
┌─────────────┐                   The quick brown fox
│   Block A   │                   jumps over [inline] text
├─────────────┤                   wrapping line by line.
│   Block B   │
└─────────────┘

flex formatting context           grid formatting context
┌────┬────┬────┐                  ┌───────┬───────┐
│ 1  │ 2  │ 3  │  main axis →     │ area  │ area  │
└────┴────┴────┘                  ├───────┼───────┤
                                  │ area  │ area  │
                                  └───────┴───────┘
```

`display: block` on a former inline element makes it a block-level box. Two-value syntax (`display: block flex`) separates outer and inner types explicitly in modern CSS.

# Example usage

```css
/* Classic values */
nav { display: block; }
span.badge { display: inline-block; }  /* inline outside, block inside */

/* Layout contexts */
.toolbar { display: flex; gap: 0.5rem; }
.dashboard { display: grid; grid-template-columns: 240px 1fr; }

/* Two-value syntax */
.wrapper { display: block flow; }
.cards { display: block flex; flex-wrap: wrap; }

/* Remove from layout and accessibility tree considerations */
[hidden] { display: none; }
```

# Common mistakes

- Using `display: none` for toggling visibility when `visibility: hidden` or `hidden` attribute semantics differ (screen readers, focus).
- Expecting `inline-block` to ignore whitespace gaps between elements in HTML source.
- Applying flex/grid properties to children without setting `display: flex` or `display: grid` on the parent.
- Replacing semantic HTML with `div` + `display` when native elements (`nav`, `button`) provide behavior.
- Confusing `inline-flex` / `inline-grid` (outer inline, inner flex/grid) with block-level flex/grid containers.

# Related concepts

* [Box Model](/concepts/box-model.md) — sizing boxes once display context is established
* [Positioning](/concepts/positioning.md) — positioned boxes still depend on their formatting context
* [Flexbox](/concepts/flexbox.md) — one-dimensional layout inside flex containers
* [Grid](/concepts/grid.md) — two-dimensional layout inside grid containers
