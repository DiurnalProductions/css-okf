---
id: css.modern-css
type: concept
title: Modern CSS
description: Contemporary CSS features including custom properties, math functions, nesting, layers, and logical properties
tags: [css, custom-properties, nesting, clamp, layers]
prerequisites:
  - css.cascade
  - css.specificity
related:
  - css.responsive-design
  - css.flexbox
  - css.grid
  - css.animations
  - css.selectors
resource: https://developer.mozilla.org/en-US/docs/Web/CSS
timestamp: 2026-01-01
---

# Summary

Modern CSS extends the language with custom properties (variables), native nesting, cascade layers, math functions (`calc`, `clamp`, `min`, `max`), logical properties (`margin-inline`, `padding-block`), and improved color syntax. These features reduce preprocessor dependence and make responsive, themeable systems easier to maintain.

# Mental model

Layers of abstraction stack on the core cascade:

```
@layer reset, tokens, components, utilities;

@layer tokens {
  :root {
    --color-primary: oklch(55% 0.2 250);
    --space-md: clamp(0.75rem, 1.5vw, 1.25rem);
  }
}

@layer components {
  .button {
    padding-inline: var(--space-md);
    background: var(--color-primary);
  }
}

Nesting mirrors HTML structure (native in modern browsers):
.card {
  padding: var(--space-md);
  & .title { font-weight: 600; }
  &:hover { box-shadow: 0 2px 8px rgb(0 0 0 / 0.1); }
}
```

Custom properties inherit and cascade like other properties — they enable runtime theming via overrides on ancestors or `prefers-color-scheme`.

# Example usage

```css
@layer base, theme, components;

@layer theme {
  :root {
    --font-size-body: clamp(1rem, 0.9rem + 0.5vw, 1.125rem);
    --gutter: max(1rem, env(safe-area-inset-left));
    color-scheme: light dark;
  }

  [data-theme="dark"] {
    --surface: oklch(20% 0.02 260);
    --text: oklch(95% 0.01 260);
  }
}

@layer components {
  .dialog {
    width: min(90vw, 32rem);
    margin-inline: auto;
    padding-block: 1.5rem;
    padding-inline: var(--gutter);
  }
}

/* Math without preprocessor */
.sidebar {
  width: calc(240px + 2 * var(--gutter));
}

/* :has() — parent selection */
.form-field:has(:invalid) {
  border-color: var(--color-error, red);
}
```

# Common mistakes

- Treating custom properties like static preprocessor variables — they resolve at computed-value time and inherit.
- Nesting too deeply, recreating overly specific selectors — use `&` thoughtfully.
- Putting all styles in one `@layer` — defeats the purpose of predictable override ordering.
- Using `clamp()` without testing minimum and maximum at extreme viewport sizes.
- Assuming universal support for newest selectors (`:has`) without fallbacks where needed.

# Related concepts

* [Cascade](/concepts/cascade.md) — layers and inheritance govern custom property resolution
* [Specificity](/concepts/specificity.md) — `:where()` and layer order manage override cost
* [Responsive Design](/concepts/responsive-design.md) — `clamp()` and container queries are core tools
* [Animations](/concepts/animations.md) — custom properties can animate when registered or used in transitions
