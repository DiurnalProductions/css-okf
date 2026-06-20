---
id: css.positioning
type: concept
title: CSS Positioning
description: Placing elements relative to normal flow, ancestors, or the viewport using position and inset properties
tags: [css, positioning, layout, display]
prerequisites:
  - css.display
related:
  - css.display
  - css.box-model
  - css.responsive-design
resource: https://developer.mozilla.org/en-US/docs/Web/CSS/position
timestamp: 2026-01-01
---

# Summary

`position` controls how an element is placed. `static` follows normal flow. `relative` offsets from its flow position without removing it from flow. `absolute` removes from flow and positions against the nearest positioned ancestor. `fixed` anchors to the viewport. `sticky` toggles between relative and fixed based on scroll thresholds.

# Mental model

Normal flow stacks boxes; positioning lifts or shifts them:

```
static / relative in flow:
┌──────┐ ┌──────┐
│  A   │ │  B   │
└──────┘ └──────┘

absolute (out of flow) — parent needs position ≠ static:
┌─────────────────┐
│  parent         │
│     ┌───┐       │  top/left relative to padding edge
│     │ X │       │  of positioned ancestor
│     └───┘       │
└─────────────────┘

fixed — relative to viewport:
┌ viewport ─────────────┐
│  [fixed header]       │
│  scrolling content…   │
└───────────────────────┘

sticky — sticks when scroll hits threshold:
│  section header (sticky top: 0)  │ ← pins here while section scrolls
```

`top`, `right`, `bottom`, `left` (or logical `inset-*`) define offsets. `z-index` only affects stacking among positioned elements and flex/grid items with `z-index` other than `auto`.

# Example usage

```css
.card { position: relative; }
.card .badge {
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
}

.site-header {
  position: sticky;
  top: 0;
  z-index: 10;
}

.modal-backdrop {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.4);
}

.sidebar {
  position: fixed;
  left: 0;
  width: 280px;
  height: 100dvh;
}
```

# Common mistakes

- Using `absolute` without a positioned ancestor — element positions against the initial containing block (often the viewport), not the intended parent.
- Expecting `relative` to create a new stacking context only when combined with `z-index` — other properties (`opacity`, `transform`) also create contexts.
- Overusing `fixed` without accounting for mobile browser chrome (`100vh` vs `100dvh`).
- Forgetting that absolutely positioned elements don't expand their parent's height.
- Using large `z-index` values instead of understanding stacking context hierarchy.

# Related concepts

* [Display](/concepts/display.md) — formatting context affects how positioned elements interact with siblings
* [Box Model](/concepts/box-model.md) — offsets apply to margin edges of positioned boxes
* [Responsive Design](/concepts/responsive-design.md) — sticky and fixed behavior across viewport sizes
