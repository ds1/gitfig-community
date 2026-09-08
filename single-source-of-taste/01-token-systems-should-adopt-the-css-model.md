# Token Systems Should Adopt CSS's Model

*Source: originally published on the [design-token-context-model wiki](https://github.com/ds1/design-token-context-model/wiki/Token-Systems-Should-Adopt-CSS'-model) in January 2026. Moved here September 2026; this file is now the canonical copy.*

---

CSS applies specific resolution rules to styles. Design token systems typically don't. They treat tokens as flat key-value pairs with manual "variant" handling.

The Context Coordinate Model says apply CSS's approach (conditions + specificity + precedence) to the token layer itself.

Here's how most token systems actually work today:

Token systems use flat variants, not combinable dimensions

A typical token file looks like this:
```
  {
    "color.surface.default": {
      "light": "#ffffff",
      "dark": "#0a0a0a"
    }
  }
```
What if you need light + high contrast? You create another variant:
```
  {
    "color.surface.default": {
      "light": "#ffffff",
      "dark": "#0a0a0a",
      "light-high-contrast": "#ffffff",
      "dark-high-contrast": "#000000"
    }
  }
```
Now add mobile vs desktop. You get:
  * `light`
  * `dark`
  * `light-high-contrast`
  * `dark-high-contrast`
  * `light-mobile`
  * `dark-mobile`
  * `light-high-contrast-mobile`
  * `dark-high-contrast-mobile`

This is combinatorial explosion. You're manually creating every intersection instead of letting dimensions combine naturally.

CSS doesn't work this way. In CSS, you'd write:
```
  .surface { background: #ffffff; }

  @media (prefers-color-scheme: dark) {
    .surface { background: #0a0a0a; }
  }

  @media (prefers-contrast: high) {
    .surface { border: 2px solid; }
  }

  @media (prefers-color-scheme: dark) and (prefers-contrast: high) {
    .surface { background: #000000; }
  }
```
You only write the intersections that matter. The browser combines dimensions automatically. You don't pre-enumerate every possible combination.

What token systems lack:

1. **No compound conditions** — Most token formats can't express "when dark AND high-contrast." You fake it with manually-named variants.
2. **No specificity rules** — If two token definitions could apply, what wins? Most systems don't say. Teams handle it ad-hoc in code.
3. **No inheritance model** — CSS cascades from parent to child. Token systems typically don't. You manually pass context down.
4. **No precedence order** — CSS has `!important` and specificity. Token systems don't define "accessibility overrides appearance."

The evidence is in how teams work. When a team needs "dark mode with high contrast on mobile," they typically:
  * Create a new theme variant manually
  * Write conditional logic in application code (`if darkMode && highContrast && isMobile`)
  * Duplicate tokens across variant combinations

None of these would be necessary if the token system had CSS's model: independent dimensions that combine, with explicit rules for what wins when they intersect.
