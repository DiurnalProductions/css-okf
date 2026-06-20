---
id: css.animations
type: concept
title: CSS Animations and Transitions
description: Time-based visual changes using transitions, keyframe animations, and motion preferences
tags: [css, animations, transitions, keyframes]
prerequisites:
  - css.display
related:
  - css.modern-css
  - css.responsive-design
resource: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animations
timestamp: 2026-01-01
---

# Summary

CSS can interpolate property values over time. `transition` animates between states when properties change (often on `:hover` or class toggles). `@keyframes` defines multi-step animations applied via `animation`. Respect `prefers-reduced-motion` for accessibility. Only animatable properties produce smooth results — layout properties (`width`, `top`) are costly; `transform` and `opacity` are preferred.

# Mental model

Transitions react to change; animations run on a timeline:

```
Transition (A → B on state change):
rest ──hover──► hovered
opacity: 1        opacity: 0.8
      ╲________╱  over transition-duration

Keyframes (named timeline):
@keyframes fade-slide {
  0%   { opacity: 0; transform: translateY(8px); }
  100% { opacity: 1; transform: translateY(0); }
}

animation: fade-slide 300ms ease-out;

Performance layers:
  cheap:  transform, opacity, filter (composited)
  costly:  width, height, top, left (layout/paint)
```

# Example usage

```css
.button {
  transition: background-color 150ms ease, transform 150ms ease;
}

.button:hover {
  transform: translateY(-1px);
}

.button:active {
  transform: translateY(0);
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

.spinner {
  animation: spin 0.8s linear infinite;
}

@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

# Common mistakes

- Animating `width`/`height`/`margin` causing layout thrashing — prefer `transform: scale()`.
- Transitions on `display: none` — not interpolable; use `opacity` + `visibility` or `allow-discrete` where supported.
- Long durations on UI feedback (>300ms feels sluggish for micro-interactions).
- Ignoring `prefers-reduced-motion` — required for accessible motion design.
- Chaining many simultaneous animations without `will-change` discipline — can hurt GPU memory; use sparingly.

# Related concepts

* [Display](/concepts/display.md) — `display` changes are discrete; plan enter/exit patterns carefully
* [Modern CSS](/concepts/modern-css.md) — custom properties and `@property` enable animatable variables
* [Responsive Design](/concepts/responsive-design.md) — motion and layout changes may differ by viewport context
