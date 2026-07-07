---
type: Concept
title: CSS Box Model
description: "How margin, border, padding, and content boxes determine element size and spacing"
tags: [css, box-model, layout]
prerequisites:
  - concepts/specificity
related:
  - concepts/display
  - concepts/flexbox
  - concepts/grid
resource: "https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_box_model"
timestamp: 2026-01-01
---

# Summary

Every element is a rectangular box composed of content, padding, border, and margin areas. `width` and `height` by default apply to the content box (`box-sizing: content-box`). `box-sizing: border-box` includes padding and border in the specified width and height, which simplifies layout math.

# Mental model

Boxes nest from inside out:

```
        margin (transparent, separates from neighbors)
    ┌───────────────────────────────────┐
    │           border                    │
    │  ┌─────────────────────────────┐   │
    │  │         padding              │   │
    │  │  ┌───────────────────────┐  │   │
    │  │  │      content          │  │   │
    │  │  │   (text, images)      │  │   │
    │  │  └───────────────────────┘  │   │
    │  └─────────────────────────────┘   │
    └───────────────────────────────────┘
```

Vertical margins between adjacent block-level siblings collapse — the larger margin wins, not the sum. Padding and border never collapse. Percentage margins and padding resolve against the *width* of the containing block.

# Example usage

```css
*, *::before, *::after {
  box-sizing: border-box;
}

.card {
  width: 320px;
  padding: 1rem;
  border: 1px solid #ccc;
  margin-block: 1.5rem;
}

/* content-box (default): total width = width + padding + border */
.legacy { box-sizing: content-box; width: 300px; padding: 20px; }

/* border-box: total width ≈ width including padding + border */
.modern { box-sizing: border-box; width: 300px; padding: 20px; }
```

# Common mistakes

- Setting `width: 100%` with horizontal padding on `content-box`, causing overflow.
- Expecting vertical margins to always add — collapse surprises spacing between paragraphs.
- Forgetting that inline non-replaced elements ignore vertical margin (and width/height).
- Using margin to size internal spacing when padding is more appropriate (background won't extend into margin).
- Ignoring `min-width`, `max-width`, and `overflow` when content exceeds the content box.

# Related concepts

* [Display](/concepts/display.md) — block vs inline affects how box model properties apply
* [Flexbox](/concepts/flexbox.md) — flex items have adjusted sizing and margin behavior
* [Grid](/concepts/grid.md) — grid items sit in track-sized areas defined by the grid container
