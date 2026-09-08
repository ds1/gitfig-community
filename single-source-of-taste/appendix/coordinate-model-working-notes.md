# Context Coordinate Model: Theoretical Exploration

*Source: working notes originally kept as `archive/context-coordinate-model-exploration.md` in the [design-token-context-model](https://github.com/ds1/design-token-context-model) repository (January 2026). These notes were refined into [Context as Coordinate System](../02-context-as-coordinate-system.md); they are kept because they carry the formal predicate-matching, inheritance-merge, and coordinate-normalization details that the polished page summarizes.*

*Applying principles from embodied AI to the design token context ontology*

---

## Agent-Relative Resolution

The [embodied AI paper](./design-tokens-for-embodied-ai.md)'s most striking insight: affordances aren't properties of objects—they're **relations between agent capabilities and environmental structure**. A chair affords sitting *for a human*, not universally.

Translated to our context model: a token's resolution isn't just a function of environmental conditions—it's a function of the **intersection of environment and agent capabilities**:

```
value = f(environment, agent)
```

The "agent" in 2D design is the platform/device/user combination:
- Screen color gamut (can it render P3?)
- Pointer type (can it hover?)
- Input method (touch precision vs. mouse precision)
- Accessibility settings (user's contrast needs)

So our coordinate has two kinds of dimensions:
- **Environmental**: colorScheme, viewport width, ambient light
- **Agent capabilities**: color gamut support, pointer precision, screen density

Affordances emerge from the intersection. A `:hover` state is only an affordance on devices with hover capability. A subtle color distinction is only an affordance for users without color vision deficiency on displays with sufficient gamut.

---

## Uncertainty as First-Class

The paper insists uncertainty must be explicit—physical interaction is inherently stochastic. We've been treating resolution as deterministic: coordinate → value. But consider:

- `viewport: 768px` — is the user resizing? Will this change in 100ms?
- `interaction: hover` — on a touch device with long-press-to-hover, this state is ambiguous
- `colorScheme: dark` — system preference, but the user might override

Resolution could produce **distributions**, not point values:

```
resolve(token, coordinate) → { value, confidence, volatility_horizon }
```

Where `volatility_horizon` estimates how long this resolution is likely to remain valid. This enables smarter caching and precomputation.

---

## Behavior Under Interaction, Not Just Appearance

The paper distinguishes: 2D tokens encode **appearance**, but embodied tokens must encode **behavior under interaction**.

Our motion tokens already do this—they encode how things change over time. But the deeper point: even "static" tokens encode behavior. `surface.default` isn't a color—it's the answer to "what color should this surface be **when the user looks at it in this context**?"

The token is a **policy**, not a value. The resolution algorithm is policy evaluation.

This reframes compound conditions. Consider:

```
{ colorScheme: dark, interaction: hover } → blue-400
```

This isn't "the color when dark AND hovering." It's "the behavioral response to a hover event in a dark environment." The token encodes stimulus-response, not static mapping.

---

## Hierarchical Composition with Semantic Compression

The paper describes: motion primitives → skills → tasks → behaviors. Each level is a **semantic compression** of the level below.

In our model:
- **Primitives**: `blue-500`, `16px`, `200ms`
- **Semantics**: `accent.default`, `spacing.4`, `duration.normal`
- **Components**: `button.background`, `card.padding`, `modal.enter`
- **Patterns**: (unnamed in current model) — compound behaviors like "card hover lift effect"

But there's a level above: **context types**. `kiosk`, `print`, `mobile-dark` are semantic compressions of coordinate regions. They're not just shortcuts—they're **named behavioral policies**:

```
kiosk = "the behavioral policy appropriate for public touchscreen displays"
```

The coordinate model could formalize this: a context type is a **subspace** of the full coordinate space, with its own resolution rules that apply within that subspace.

---

## Simulation-Grounded Semantics

The paper argues token meanings should be grounded in simulatable physics—defined by behavior under simulation.

For UI: token meanings could be grounded in **observable user behavior**. A color token's meaning isn't the hex value—it's the perceptual response it produces. `color.error` means "produces recognition of error state in target user population."

This suggests validation criteria beyond "correct hex value":
- Does `contrast.high` actually produce sufficient contrast for low-vision users?
- Does `motion.reduced` actually reduce vestibular discomfort?
- Does `density.compact` actually fit more content without sacrificing usability?

The resolution algorithm becomes testable: given this coordinate, does the resolved value achieve its semantic intent?

---

## The Coordinate Space (Formalized)

A **context coordinate** is a point in a space where each axis is a dimension from the ontology:

```
C = (d₁, d₂, d₃, ..., d₉₃)
```

But most dimensions are **optional**. A coordinate is really a *partial function*—it maps some dimension names to values, leaving others undefined:

```
C: DimensionName → Value | ⊥
```

Where `⊥` means "unspecified." This is important: most tokens don't care about most dimensions. A spacing token probably ignores `colorScheme`. A color token probably ignores `density`.

---

## Token Rules as Predicates

A token isn't a single value—it's a set of **rules**, where each rule is:

```
(predicate, value)
```

The predicate is a partial coordinate that acts as a pattern:

```
predicate: DimensionName → Value | *
```

Where `*` means "any value, including ⊥."

A predicate **matches** a context coordinate when every specified dimension in the predicate equals the corresponding dimension in the coordinate:

```
matches(predicate, coordinate) =
  ∀d ∈ dom(predicate): predicate(d) = coordinate(d) ∨ predicate(d) = *
```

---

## The Resolution Problem

Given:
- A token with rules R = [(p₁, v₁), (p₂, v₂), ...]
- A context coordinate C

Find the value to use.

### Specificity

Intuition: a rule that matches more dimensions is more specific. If we have:

```
Rule A: { colorScheme: dark } → #0a0a0a
Rule B: { colorScheme: dark, contrast: high } → #000000
```

And the coordinate is `{ colorScheme: dark, contrast: high, density: compact }`, both rules match. But B is more specific—it cares about more of the coordinate.

**Specificity** = |dom(predicate)| = number of dimensions the predicate constrains.

### Dimension Precedence

For two rules with equal specificity constraining *different* dimensions, the tiebreaker is: which rule constrains a higher-precedence dimension?

The ontology defines a total ordering:

```
contrast > colorScheme > density > viewport > ...
```

Accessibility dimensions have highest precedence.

### The Resolution Algorithm

```
resolve(rules, coordinate):
  1. Filter to matching rules
  2. If one rule, return its value
  3. If zero rules, return default (or error)
  4. If multiple rules:
     a. Find max specificity
     b. Filter to rules with max specificity
     c. If one rule, return its value
     d. If multiple, compare by dimension precedence
     e. If still tied, it's an ambiguity (author error)
```

---

## Context Inheritance

Context coordinates are **assembled** from layers:

```
effective_coordinate = merge(
  system_defaults,
  platform_detected,
  user_preferences,
  document_context,
  ancestor_context[n],
  ...
  ancestor_context[1],
  local_context
)
```

Later layers override earlier ones.

**Combinable states** are different—they union rather than replace:

```
ancestor: { focus: true }
local: { selected: true }
effective: { focus: true, selected: true }  // both!
```

Versus exclusive dimensions:

```
ancestor: { density: compact }
local: { density: spacious }
effective: { density: spacious }  // local wins
```

---

## Volatility as a Dimension Property

Some dimensions change rarely (platform). Others change constantly (interaction state). This affects **when resolution happens**.

**Partial evaluation** by volatility: stable dimensions are resolved first, producing a simplified rule set that only varies on volatile dimensions.

```
// Pre-resolved for dark mode:
{ interaction: rest } → blue-400
{ interaction: hover } → blue-500
{ interaction: active } → blue-600
```

This is the resolution algorithm staged by volatility class.

---

## Cross-Dimensional Constraints

Some coordinate positions are **invalid** or **force** other dimensions:

```
if contrast = forced:
  elevation must be base
  colorScheme doesn't matter (system colors override)
```

Before resolution, we **normalize** the coordinate:

```
normalize(coordinate):
  if coordinate.contrast = forced:
    return { ...coordinate, elevation: base, colorScheme: ⊥ }
  ...
```

This reduces the effective dimensionality when certain modes are active.

---

## The Deepest Parallel

The embodied AI paper asks whether physical world knowledge can be systematized like visual design. We're asking whether **contextual variation** can be systematized.

Both are asking: **what are the minimal semantic units that compose into complex behavior?**

The answer may be that tokens aren't values—they're policies. Resolution isn't lookup—it's policy evaluation. And the context coordinate isn't state—it's the observation space against which policies are evaluated.
