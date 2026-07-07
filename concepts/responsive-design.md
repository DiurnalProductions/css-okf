---
type: Concept
title: Responsive Web Design
description: Adapting layout and typography to viewport and container size using fluid units and conditional rules
tags: [css, responsive, media-queries, container-queries]
prerequisites:
  - concepts/flexbox
  - concepts/grid
  - concepts/display
related:
  - concepts/flexbox
  - concepts/grid
  - concepts/modern-css
resource: "https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Responsive_Design"
timestamp: 2026-01-01
---

# Summary

Responsive design makes interfaces usable across screen sizes. Techniques include fluid widths, flexible images, relative units, media queries (`@media`) keyed to viewport features, and container queries (`@container`) keyed to a parent element's size. Mobile-first CSS starts with base styles for small viewports and adds rules as space increases.

# Mental model

Two query surfaces — viewport vs component:

```
Viewport (media queries)
┌──────────────────────────────────────┐
│  @media (min-width: 768px) { … }     │  entire browser width
└──────────────────────────────────────┘

Container (container queries)
┌──────── card (container) ──────────┐
│  @container (min-width: 400px)     │  card's own width, not viewport
│  { switch layout }                 │
└────────────────────────────────────┘

Fluid typography scales between bounds:
font-size: clamp(1rem, 2vw + 0.5rem, 1.5rem);
```

Layout systems ([Flexbox](/concepts/flexbox.md), [Grid](/concepts/grid.md)) provide intrinsic responsiveness; queries add breakpoints where behavior should change discretely.

# Example usage

```css
/* Mobile-first */
.layout {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
}

@media (min-width: 48rem) {
  .layout {
    grid-template-columns: 240px 1fr;
  }
}

/* Container queries */
.card-wrapper {
  container-type: inline-size;
  container-name: card;
}

@container card (min-width: 32rem) {
  .card {
    display: grid;
    grid-template-columns: 120px 1fr;
  }
}

/* Fluid spacing and type — see Modern CSS */
:root {
  --space: clamp(0.75rem, 2vw, 1.5rem);
}

img, video {
  max-width: 100%;
  height: auto;
}
```

# Common mistakes

- Designing desktop-first with many `max-width` overrides — harder to maintain than mobile-first `min-width`.
- Tying component layout only to viewport width when sidebar or split-pane layouts change available space without resizing the window.
- Using fixed pixel breakpoints without testing content-driven break points.
- Forgetting `meta viewport` on HTML documents (outside CSS but required for mobile scaling).
- Hiding critical content at small breakpoints instead of restructuring layout.

# Related concepts

* [Flexbox](/concepts/flexbox.md) — `flex-wrap`, `gap`, and flexible bases reduce breakpoint needs
* [Grid](/concepts/grid.md) — `auto-fill` / `minmax()` for responsive track lists
* [Modern CSS](/concepts/modern-css.md) — `clamp()`, custom properties, and logical properties support fluid design
