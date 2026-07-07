---
type: Concept
title: The CSS Cascade
description: The algorithm that determines which declaration wins when multiple rules apply to the same element
tags: [css, cascade, specificity, inheritance]
prerequisites:
  - concepts/selectors
related:
  - concepts/specificity
  - concepts/modern-css
resource: "https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade"
timestamp: 2026-01-01
---

# Summary

When several rules set the same property on one element, the cascade picks a single winning value. Resolution order considers origin and importance first, then specificity, then source order. Inherited values apply only when no cascade winner exists for that property on the element itself.

# Mental model

Picture layers of rules pouring onto an element. Higher layers drown lower ones:

```
┌─────────────────────────────────────┐
│ 1. Origin & importance              │  user !important > author !important
│    (stylesheet source + !important) │  > author > user > browser
├─────────────────────────────────────┤
│ 2. Specificity                      │  inline style > ID > class > type
├─────────────────────────────────────┤
│ 3. Source order                     │  later rule in same layer wins
├─────────────────────────────────────┤
│ 4. Inheritance                      │  parent value if property inherits
└─────────────────────────────────────┘
```

Only declarations that match the element (via [selectors](/concepts/selectors.md)) enter the competition. `@layer` (see [Modern CSS](/concepts/modern-css.md)) adds an explicit ordering step among author stylesheets.

# Example usage

```css
/* Same specificity — source order wins */
p { color: black; }
p { color: navy; }   /* navy applied */

/* Higher specificity wins regardless of order */
.text { color: green; }
#main p.text { color: red; }   /* red applied */

/* !important overrides normal author rules */
.alert { color: orange !important; }
body .alert { color: red; }   /* orange still wins */

/* Inherited when not set on child */
body { font-family: system-ui, sans-serif; }
/* <p> inside body inherits font-family unless overridden */
```

# Common mistakes

- Fighting specificity with longer selectors instead of fixing architecture or using cascade layers.
- Sprinkling `!important` as a default — it breaks predictable override patterns and maintenance.
- Assuming the last rule in the entire project always wins — specificity and origin trump file order.
- Expecting all properties to inherit — `margin`, `padding`, `border`, and `background` do not inherit by default.
- Ignoring user-agent (browser) styles that still participate until overridden.

# Related concepts

* [Selectors](/concepts/selectors.md) — determine which rules are candidates
* [Specificity](/concepts/specificity.md) — the numeric tie-breaker within the cascade
* [Modern CSS](/concepts/modern-css.md) — `@layer` and custom properties interact with cascade behavior
