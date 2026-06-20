---
id: css.selectors
type: concept
title: CSS Selectors
description: Patterns that match elements in the DOM so declarations can be applied
tags: [css, selectors, cascade]
prerequisites: []
related:
  - css.cascade
  - css.specificity
resource: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_selectors
timestamp: 2026-01-01
---

# Summary

Selectors define which elements a CSS rule targets. A rule is a selector list followed by a declaration block. Matching is evaluated against the document tree; only elements that match receive the declarations.

# Mental model

Think of selectors as filters on the DOM tree:

```
Document
 └── body
      ├── h1          ← type selector `h1`
      ├── p.intro     ← `.intro` (class)
      └── nav#main    ← `#main` (ID)
```

Combinators express relationships between matched nodes:

```
article p        → any p descendant of article
ul > li          → direct child li of ul
h2 + p           → p immediately after h2
img ~ figcaption → figcaption after img (siblings)
```

Pseudo-classes (`:hover`, `:focus`, `:nth-child()`) and pseudo-elements (`::before`, `::after`) extend matching to state and generated content without extra markup.

Specificity of a selector directly affects [cascade](/concepts/cascade.md) outcomes — see [Specificity](/concepts/specificity.md).

# Example usage

```css
/* Type */
p { line-height: 1.6; }

/* Class — reusable, preferred for styling hooks */
.card { border-radius: 8px; }

/* ID — unique per page; high specificity */
#site-header { position: sticky; }

/* Attribute */
input[type="email"] { border-color: blue; }

/* Combinator */
nav a { text-decoration: none; }
nav > ul > li { display: inline-block; }

/* Pseudo-class */
button:disabled { opacity: 0.5; }

/* Pseudo-element */
blockquote::before { content: "\201C"; }
```

# Common mistakes

- Using IDs for styling when classes would keep specificity lower and styles easier to override.
- Overly long selector chains that tightly couple styles to HTML structure and break when markup changes.
- Assuming `div.class` and `.class` behave differently for matching — they match the same elements; the type adds specificity.
- Forgetting that selector lists are OR'd: `h1, h2, h3` applies to any of those elements.
- Relying on `!important` in selectors — importance is a declaration concern, not part of selector syntax.

# Related concepts

* [Cascade](/concepts/cascade.md) — resolves conflicts among rules that match the same element
* [Specificity](/concepts/specificity.md) — weights selectors when the cascade needs a tie-breaker
