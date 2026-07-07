---
type: Concept
title: CSS Specificity
description: A weighting system that ranks selectors when the cascade must break ties
tags: [css, specificity, cascade, selectors]
prerequisites:
  - concepts/cascade
related:
  - concepts/selectors
  - concepts/modern-css
resource: "https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_cascade/Specificity"
timestamp: 2026-01-01
---

# Summary

Specificity compares selectors as a tuple: inline styles, then ID selectors, then class/attribute/pseudo-class selectors, then type/pseudo-element selectors. The selector with the higher tuple wins when importance and origin are equal. Universal (`*`) and combinators contribute zero.

# Mental model

Each selector category adds weight — compare columns left to right like version numbers:

```
Selector                    IDs  Classes/Attrs  Types
────────────────────────────────────────────────────
*                           0    0            0
li                          0    0            1
ul li                       0    0            2
ul.nav li                   0    1            2
#main nav li.active         1    1            2
style="color:red"           ∞    (inline beats any selector rule)
```

`:not()` itself adds nothing, but its argument's specificity counts. `:is()`, `:where()`, and `:has()` follow current Selectors Level 4 rules — `:where()` contributes zero; `:is()` takes the specificity of its most specific argument.

# Example usage

```css
/* (0, 1, 0) beats (0, 0, 1) */
.intro { color: teal; }
p { color: gray; }

/* (0, 2, 1) beats (0, 1, 1) */
nav.main .link { font-weight: 600; }
nav .link { font-weight: 400; }

/* Inline style on element beats (0, 1, 0) from stylesheet */
/* <p class="intro" style="color: red"> */

/* !important does not change specificity — it changes cascade layer */
```

# Common mistakes

- Duplicating IDs or chaining classes solely to "beat" another rule — creates specificity debt.
- Believing `!important` increases specificity — it does not; it changes importance in the [cascade](/concepts/cascade.md).
- Treating `:not(.foo)` as zero specificity — the `.foo` inside counts.
- Using ID selectors in component styles that must be themeable or overridden downstream.
- Comparing specificity as a single integer in legacy tools — modern browsers use the tuple model.

# Related concepts

* [Cascade](/concepts/cascade.md) — uses specificity after origin and importance
* [Selectors](/concepts/selectors.md) — selector syntax determines specificity contribution
* [Modern CSS](/concepts/modern-css.md) — `:where()` for intentional zero-specificity hooks
