---
id: css.flexbox
type: concept
title: Flexbox Layout Model
description: One-dimensional layout system for arranging elements in rows or columns
tags: [css, flexbox, layout]
prerequisites:
  - css.box-model
  - css.display
related:
  - css.grid
  - css.responsive-design
resource: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout
timestamp: 2026-01-01
---

# Summary

Flexbox lays out children of a flex container along a single main axis (row or column) with a perpendicular cross axis. Flex items can grow, shrink, and align with `justify-content`, `align-items`, and `align-self`. It excels at distributing space and aligning items in one dimension.

# Mental model

Two axes govern everything:

```
flex-direction: row (default)
 main axis ──────────────────►
 ┌──────┬──────┬──────────────┐
 │ item │ item │  flex-grow   │  ▲ cross axis
 └──────┴──────┴──────────────┘  ▼

flex-direction: column
 ┌────────────┐
 │   item     │  main axis
 ├────────────┤     │
 │   item     │     ▼
 ├────────────┤
 │ flex-grow  │
 └────────────┘
 ◄── cross axis ──►
```

The container controls distribution (`justify-content` on main axis, `align-items` on cross axis). Items control their own flexibility via `flex-grow`, `flex-shrink`, and `flex-basis` (shorthand `flex`).

# Example usage

```css
.toolbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
}

.sidebar-layout {
  display: flex;
  min-height: 100dvh;
}

.sidebar {
  flex: 0 0 240px;   /* don't grow, don't shrink, basis 240px */
}

.main {
  flex: 1 1 auto;    /* take remaining space */
}

.card-row {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.card {
  flex: 1 1 280px;   /* responsive cards without media query */
}
```

# Common mistakes

- Applying flex properties to the container instead of items (`flex: 1` on children, not parent).
- Using flexbox for full page two-dimensional layouts when [Grid](/concepts/grid.md) is clearer.
- Forgetting `min-width: auto` default prevents flex items from shrinking below content size — often fixed with `min-width: 0`.
- Relying on `order` for document order semantics — hurts accessibility when it changes visual vs tab order.
- Omitting `flex-wrap` and expecting many items to shrink indefinitely.

# Related concepts

* [Display](/concepts/display.md) — `display: flex` establishes the flex formatting context
* [Grid](/concepts/grid.md) — two-dimensional alternative; often combined (grid page, flex components)
* [Responsive Design](/concepts/responsive-design.md) — flex + `flex-wrap` and `gap` for fluid component layouts
