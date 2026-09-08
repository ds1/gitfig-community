# Context as Coordinate System: A Theoretical Foundation for Design Token Resolution

*Source: originally published on the [design-token-context-model wiki](https://github.com/ds1/design-token-context-model/wiki/Context-as-Coordinate-System:-A-Theoretical-Foundation-for-Design-Token-Resolution) in January 2026. Moved here September 2026; this file is now the canonical copy.*

## Abstract

Design tokens have succeeded by identifying minimal semantic units for visual specification. Yet their treatment of context — the conditions under which tokens resolve to values — remains primitive: flat mode switches that cannot compose, inherit, or express complex conditions. This document argues that context should be modeled as a **multidimensional coordinate system**, with tokens defined as **resolution policies** over that space. Drawing on principles from embodied AI and cognitive science, I propose that this reframing transforms tokens from static value mappings into behavioral specifications, enabling systematic treatment of contextual variation at scale.

---

## Thesis

**Design tokens are not values. They are policies that map context coordinates to values. The resolution algorithm is policy evaluation, and the context coordinate is the observation space against which policies execute.**

This reframing has three implications:

1. Context is not a set of independent mode switches but a **single point in N-dimensional space**
2. Token definitions are not value lookups but **conditional rule sets with specificity and precedence**
3. Resolution is not instantaneous retrieval but **algorithmic evaluation** that can be staged, cached, and validated

---

## Argument 1: The Coordinate Model Corrects a Category Error

### The Problem with Flat Modes

Current design systems treat context dimensions as independent:

- Theme: light | dark
- Density: compact | comfortable | spacious
- Viewport: mobile | tablet | desktop

This implies the total context space is the Cartesian product: 2 × 3 × 3 = 18 combinations. But the model offers no way to express that these combinations interact. A token cannot specify different behavior for "dark AND compact" versus "dark AND spacious"; it can only specify "dark" behavior and "compact" behavior independently.

This is a **category error**. Context is not a tuple of independent selections. It is a single complex state that happens to be describable along multiple axes. The weather is not "temperature: cold" plus "humidity: high" as independent facts; it is a single atmospheric state that we decompose for description.

### The Coordinate Correction

A context coordinate is a partial function from dimension names to values:

```
C: DimensionName → Value | ⊥
```

A token rule is a predicate-value pair where the predicate matches coordinates:

```
{ colorScheme: dark, density: compact } → value₁
{ colorScheme: dark } → value₂
```

Both rules might match a given coordinate. The resolution algorithm determines which wins based on **specificity** (more constrained predicates beat less constrained) and **precedence** (some dimensions outrank others in tiebreaks).

This model supports:
- **Compound conditions**: Rules that trigger only when multiple dimensions align
- **Graceful fallback**: Less specific rules catch coordinates not covered by specific ones
- **Explicit ambiguity**: When two rules of equal specificity match, that's a detectable authoring error

### Supporting Evidence

CSS already works this way. Selectors are predicates over the DOM coordinate space (element type, class, ID, attributes, pseudo-states). Specificity determines which rule wins. The cascade provides precedence ordering. Design tokens lack this expressiveness despite CSS having demonstrated its necessity for decades.

---

## Argument 2: Tokens Encode Behavior, Not Appearance

### The Static Value Illusion

The conventional view: a token like `color.surface.default` stores a color value. In dark mode, it stores a different color value. Resolution is lookup.

This framing obscures what tokens actually do. Tokens answer questions. They don't store colors.

> "What color should this surface be when a user perceives it in this context?"

The answer depends on:
- The user's color scheme preference
- The user's contrast needs
- The display's color gamut
- The surface's elevation relative to surrounding surfaces
- Whether the element is in a disabled, error, or selected state

The token encodes a **behavioral policy**: given these conditions, produce this perceptual outcome.

### Behavior Under Interaction

Embodied AI literature distinguishes appearance from behavior-under-interaction. A material's properties aren't its color; they are how it responds to force, friction, deformation.

For UI tokens:
- A **color token** specifies perceptual response, not wavelength
- A **spacing token** specifies relational structure, not pixel count
- A **motion token** specifies temporal behavior, not duration value

This is why the same token resolves differently across contexts. The policy's goal (perceptual outcome) is constant; the implementation (value) varies to achieve that goal under different conditions.

### Implication: Testable Resolution

If tokens encode behavioral intent, resolution becomes testable:

- Does `contrast.high` achieve WCAG AAA ratios for the target population?
- Does `motion.reduced` eliminate vestibular triggers?
- Does `density.compact` preserve usability metrics?

The resolution algorithm isn't just "correct" when it returns the specified value. It is correct when the resolved value achieves the token's semantic intent in the given context.

---

## Argument 3: Resolution is Agent-Relative

### Affordances Require Agents

Gibson's affordance theory, central to embodied AI, holds that affordances are not object properties but **relations between agents and environments**. A chair affords sitting for a human, not for an elephant.

Translated to design tokens: resolution depends not just on environmental context but on **agent capabilities**:

- A `:hover` state is only meaningful for devices with hover capability
- A P3 color gamut value is only achievable on displays that support it
- A 12px font size is only legible for users without visual impairment

The context coordinate must include both:
- **Environmental dimensions**: color scheme, ambient light, viewport size
- **Agent dimensions**: display gamut, pointer type, user accessibility needs

### The Intersection Produces Affordances

The resolved value is "the right value for this agent in this environment." The same environmental context produces different resolutions for different agents.

This explains why accessibility dimensions have highest precedence in the resolution algorithm. They encode fundamental agent capabilities that constrain which values can achieve the token's behavioral intent.

### Platform-Agnosticism Revisited

Traditional token platform-agnosticism means: specify once, render on iOS/Android/web. Agent-relative resolution means: specify the behavioral intent once, resolve to the appropriate implementation for each agent class.

The token `color.action.primary` doesn't mean "#0066cc on web and UIColor.systemBlue on iOS." It means "produce primary action affordance perception", which happens to require different values for different agent/environment combinations.

---

## Argument 4: Uncertainty Deserves Explicit Representation

### The Determinism Assumption

Current token models assume deterministic resolution: coordinate → value. But consider:

- `viewport: 768px` — The user may be resizing. Is this value stable?
- `colorScheme: dark` — System preference, but app override possible
- `interaction: hover` — On touch devices, this state is ambiguous

Resolution confidence varies. Some coordinates are stable (platform = iOS); others are volatile (interaction = hover). Some values are certain (prefers-color-scheme is dark); others are inferred (user probably prefers high contrast based on system settings).

### Resolution with Confidence

A richer resolution model:

```
resolve(token, coordinate) → {
  value: T,
  confidence: 0..1,
  volatility_horizon: Duration
}
```

Where:
- `confidence` indicates certainty that this is the correct resolution
- `volatility_horizon` estimates how long the resolution remains valid

This enables:
- **Intelligent caching**: Cache stable resolutions, recompute volatile ones
- **Graceful degradation**: When confidence is low, prefer conservative values
- **Precomputation**: Resolve stable dimensions at build time, defer volatile dimensions to runtime

### Volatility-Staged Evaluation

Volatility classification (immutable → stable → semi-stable → volatile) is an evaluation strategy:

1. **Build time**: Resolve immutable dimensions (platform, locale)
2. **Session start**: Resolve stable dimensions (user preferences)
3. **Interaction time**: Resolve volatile dimensions (hover, focus)

This is **partial evaluation** of the resolution algorithm, staging computation by volatility class.

---

## Argument 5: Context Types are Subspace Policies

### Named Contexts as Compression

Named contexts, like `mobile-dark`, `kiosk`, `print`, appear to be shortcuts — convenient names for common coordinate combinations.

However, they're more than shortcuts. They're **named subspaces** of the coordinate space, each with potentially distinct resolution behavior.

### Subspace Policies

Consider `kiosk` mode:
- Coordinates: viewport=large, colorScheme=dark, density=spacious, contrast=high
- But also: touch targets must be larger, text must be readable from distance, sessions are untrusted

The `kiosk` context type is more than a coordinate position. It is actually a **policy overlay** that might:
- Override normal resolution for certain tokens
- Impose constraints (minimum touch target size)
- Activate additional rules (session isolation)

### Hierarchical Context Types

Context types can nest:

```
kiosk extends desktop-dark {
  density: spacious,
  contrast: high,
  + additional constraints
}
```

This is inheritance in the coordinate space—context types form a taxonomy, with more specific types inheriting from and overriding more general ones.

---

## Argument 6: The Resolution Algorithm is the Core Artifact

### Inversion of Emphasis

Current practice emphasizes token values. The resolution algorithm is implicit and usually just "look up the value for the current mode."

The coordinate model inverts this. **The resolution algorithm is the core artifact.** Token values are just data the algorithm operates on.

### Algorithm Properties

A well-designed resolution algorithm should be:

1. **Deterministic**: Same coordinate always produces same value (modulo uncertainty)
2. **Total**: Every valid coordinate produces a value (via defaults)
3. **Monotonic**: Adding specificity to a rule doesn't change unrelated resolutions
4. **Composable**: Resolution of composed tokens equals composition of resolutions

### Algorithm as Specification

The resolution algorithm can be formally specified:

```
resolve(token, coordinate):
  rules ← token.rules.filter(r → r.predicate.matches(coordinate))
  if rules.empty: return token.default
  if rules.size = 1: return rules[0].value

  maxSpecificity ← max(rules, by: r → |r.predicate|)
  mostSpecific ← rules.filter(r → |r.predicate| = maxSpecificity)
  if mostSpecific.size = 1: return mostSpecific[0].value

  return mostSpecific.maxBy(r → precedenceScore(r.predicate))
```

This specification is implementable, testable, and portable across platforms.

---

## Key Insights

### Insight 1: The Figma Model is a Projection

Figma's Variable Collections with modes aren't wrong; they're a **projection** of the full coordinate space onto orthogonal axes. This projection:

- Loses compound conditions (dark + high contrast)
- Loses specificity ordering
- Loses cross-dimensional constraints

The projection is useful for design tooling but shouldn't constrain the source-of-truth representation.

**Recommendation**: Author tokens in the full coordinate model; generate Figma-compatible projections as build artifacts.

### Insight 2: CSS Already Solved This (Partially)

CSS selectors + specificity + cascade implement a coordinate resolution system. Design tokens should learn from CSS:

- Selectors → Predicates over context dimensions
- Specificity → Rule ordering by predicate constraint count
- Cascade → Precedence ordering by dimension importance

CSS lacks: explicit context dimensions, cross-dimensional constraints, volatility-aware evaluation.

### Insight 3: Tokens are Policies, Resolution is Evaluation

Reframing tokens as policies and resolution as evaluation:

- Clarifies what tokens *mean* (behavioral intent, not static value)
- Enables testability (does resolved value achieve intent?)
- Supports agent-relativity (same policy, different evaluation per agent)
- Motivates uncertainty representation (policies can have confidence)

### Insight 4: Volatility Enables Staging

The volatility classification is an evaluation strategy. Stable dimensions resolve early; volatile dimensions resolve late. This:

- Optimizes performance (cache stable resolutions)
- Reduces complexity (volatile evaluation operates on pre-simplified rule sets)
- Enables build-time optimization (eliminate runtime work for immutable dimensions)

### Insight 5: Context Types are First-Class

Named context types shouldn't be documentation shortcuts. They should be:

- First-class runtime constructs
- Hierarchically composable
- Capable of carrying policy overlays beyond coordinate positions

---

## Conclusion

The design token community has focused on what tokens store. The deeper question is how tokens resolve. By modeling context as a coordinate system and resolution as policy evaluation, we gain:

1. **Expressiveness**: Compound conditions, specificity, precedence
2. **Correctness**: Testable behavioral intent, not just value matching
3. **Performance**: Volatility-staged evaluation, intelligent caching
4. **Generality**: Agent-relative resolution, uncertainty representation

This is a proposal for a new **conceptual foundation**; one that treats **contextual variation** as the central problem rather than an afterthought, and **resolution algorithms** as the core artifact rather than an implementation detail.

Design tokens, the minimal semantic units that compose into complex visual behavior, aren't colors, spacings, and durations. They're **resolution policies over context coordinates**. The design system's job is to evaluate policies, rather than store values.

---

## Glossary

**Accessibility Dimension**
A context dimension encoding user accessibility needs or preferences. Examples: `contrast`, `motion`, `forcedColors`. Accessibility dimensions have highest precedence in the resolution algorithm because they encode fundamental agent capabilities that constrain which values can achieve behavioral intent.

**Affordance**
A relation between an agent and an environment that specifies what actions are possible. From Gibson's ecological psychology. In UI: a hover state is an affordance only for devices with hover capability. Affordances are agent-relative, not objective properties of objects or interfaces.

**Agent**
The platform/device/user combination that consumes resolved token values. Includes environmental capabilities (display gamut, pointer type) and user characteristics (accessibility needs, preferences). Resolution is agent-relative: the same environment produces different values for different agents.

**Agent Capability**
A property of an agent that determines what affordances are available and what values can achieve behavioral intent. Examples: display color gamut, pointer precision, screen reader presence. Agent capabilities constrain resolution—a P3 color is only achievable on P3-capable displays.

**Agent Class**
A category of agents with similar capabilities. Examples: mobile-touch, desktop-pointer, screen-reader. Allows resolution rules to target classes rather than enumerating individual agent configurations.

**Agent Dimension**
A context dimension encoding agent capabilities rather than environmental state. Examples: `pointerType`, `colorGamut`, `screenReaderActive`. Contrast with environmental dimensions like `colorScheme` or `viewportWidth`.

**Agent-Relative**
The property of depending on agent capabilities, not just environmental state. Resolution is agent-relative: the same token in the same environment resolves differently for different agents. Affordances are agent-relative: what's possible depends on who's acting.

**Algorithmic Evaluation**
Resolution treated as execution of an algorithm rather than simple lookup. The resolution algorithm filters, sorts by specificity, applies precedence, and handles defaults. Contrasts with flat mode systems where resolution is direct value retrieval.

**Appearance**
The visual presentation of an interface element. In the coordinate model, appearance is distinguished from behavior—tokens encode behavioral intent (perceptual outcome), not just appearance (visual specification).

**Behavioral Intent**
What a token is trying to achieve perceptually or functionally, independent of the specific value used. `color.error` intends to communicate error state; the specific red varies by context. Behavioral intent is constant; implementation values vary.

**Behavioral Policy**
A specification that maps conditions to outcomes. Tokens are behavioral policies: given this context coordinate, produce this perceptual outcome. The policy encodes intent; resolution evaluates the policy to produce concrete values.

**Behavior-Under-Interaction**
How something responds to action, not just how it appears statically. From embodied AI: a material's properties are its response to force, not its color. For tokens: even "static" color tokens encode behavioral response to perception in context.

**⊥ (Bottom)**
The symbol for "undefined" or "no value" in logic and type theory. In the coordinate model, `C(dimension) → ⊥` means the coordinate does not specify that dimension. Pronounced "bottom."

**Cartesian Product**
The set of all possible combinations of elements from multiple sets. If Theme has 2 values and Density has 3, their Cartesian product has 2 × 3 = 6 combinations. Flat mode systems treat context as a Cartesian product of independent dimensions.

**Cascade**
In CSS, the precedence ordering that determines which rule wins when multiple rules match. Origin (user-agent, author, user), importance, specificity, and source order form the cascade. The coordinate model's precedence ordering serves a similar function.

**Category Error**
A logical fallacy of treating something as belonging to a category it doesn't belong to. Treating context as a tuple of independent modes is a category error—context is a single complex state, not separable independent selections.

**Certain**
A dimension value that is definitively known, not inferred. `platform: iOS` is certain after detection. Contrast with inferred values that are probabilistic. Certainty affects resolution confidence.

**Cognitive Science**
The interdisciplinary study of mind and intelligence. Relevant to token theory through concepts like core knowledge (innate physical primitives), mental simulation, and object-centric cognition. Informs how humans perceive and process interface elements.

**Composable**
The property of combining smaller units into larger wholes while preserving meaning. Tokens should be composable: resolution of a composed token equals composition of resolutions. Composability enables building complex systems from simple parts.

**Compound Condition**
A token rule that triggers only when multiple dimensions align. Example: `{ colorScheme: dark, contrast: high } → value`. Not expressible in flat mode systems where dimensions are independent. Enables nuanced context-dependent behavior.

**Construct**
A programmatic entity that can be created, manipulated, and referenced. Context types should be first-class constructs—not just documentation but runtime-inspectable, composable entities in the system.

**Context**
The complete set of conditions under which a token resolves. Includes environmental state, agent capabilities, inherited values, and local overrides. Modeled as a coordinate in N-dimensional space.

**Context Coordinate**
A point in the N-dimensional space of all context dimensions. Formally, a partial function `C: DimensionName → Value | ⊥` that maps dimension names to values, leaving unspecified dimensions undefined. Represents the complete contextual state against which tokens resolve.

**Context Type**
A named subspace of the coordinate space with optional policy overlays. Examples: `kiosk`, `print`, `mobile-dark`. More than shortcuts—they can carry constraints and resolution overrides beyond their coordinate positions. Can form hierarchies through inheritance.

**Contextual Variation**
The phenomenon of tokens resolving to different values under different conditions. The central problem the coordinate model addresses. Treated as afterthought in flat mode systems; treated as primary concern in the coordinate model.

**Coordinate Model**
The theoretical framework treating context as a point in N-dimensional space and tokens as resolution policies over that space. Contrasts with flat mode systems that treat dimensions as independent switches.

**Cross-Dimensional Constraint**
A rule that forces or restricts dimension values based on other dimensions. Example: `if contrast = forced then elevation must be base`. Constraints normalize coordinates before resolution, reducing effective dimensionality.

**Deterministic**
Producing the same output for the same input, every time. The resolution algorithm should be deterministic: same token + same coordinate = same value. Contrast with stochastic/non-deterministic processes that may vary.

**Dimension**
A single axis in the context coordinate space. Examples: `colorScheme`, `density`, `viewport`, `interaction`. Each dimension has a finite set of possible values. The ontology defines 93 dimensions across 12 categories.

**DOM Coordinate Space**
In CSS, the implicit space over which selectors operate. Axes include element type, classes, IDs, attributes, pseudo-classes, and pseudo-elements. Selectors are predicates over this space; specificity orders matches.

**Embodied AI**
Artificial intelligence systems that interact with the physical world through sensors and actuators. Relevant to token theory through concepts like affordances, agent-relative perception, and behavior-under-interaction.

**Environmental Dimension**
A context dimension encoding environmental state rather than agent capabilities. Examples: `colorScheme`, `ambientLight`, `viewportWidth`. Detected from the environment; contrast with agent dimensions encoding capabilities.

**Evaluation Strategy**
A method for determining when and how to compute values. Volatility classification defines an evaluation strategy: resolve immutable dimensions at build time, stable at session start, volatile at interaction time.

**Explicit Ambiguity**
When two rules of equal specificity and incomparable precedence both match a coordinate. A detectable authoring error—the system should flag this at definition time. Contrast with implicit ambiguity hidden by arbitrary tiebreakers.

**Expressiveness**
The range of distinctions a system can represent. The coordinate model has greater expressiveness than flat modes: compound conditions, specificity ordering, cross-dimensional constraints. More expressiveness enables more nuanced behavior.

**First-Class**
Having full status as a programmatic entity—can be passed as arguments, returned from functions, assigned to variables, inspected at runtime. Context types should be first-class constructs, not just documentation strings.

**Gamut**
The range of colors a display can reproduce. sRGB, P3, and Rec. 2020 are progressively wider gamuts. An agent capability dimension—P3 colors only resolve correctly on P3-capable displays.

**Graceful Degradation**
The property of falling back to acceptable behavior when ideal behavior isn't possible. In resolution: when confidence is low or capabilities are limited, prefer conservative values that work broadly over optimal values that might fail.

**Graceful Fallback**
Less specific rules catching coordinates not covered by more specific rules. Enables progressive enhancement: define specific behavior where needed, let general rules handle the rest.

**Implementation Value**
The concrete value (hex color, pixel count, milliseconds) that implements behavioral intent in a specific context. The same behavioral intent requires different implementation values across contexts.

**Inferred**
A dimension value derived probabilistically rather than detected with certainty. "User probably prefers high contrast based on system settings" is inferred. Inferred values have lower confidence than certain values.

**Inheritance**
The mechanism by which context flows from ancestors to descendants. A `<CompactRegion>` wrapper makes descendants inherit `density: compact`. Combinable states union; exclusive dimensions override.

**Intelligent Caching**
Caching strategy informed by volatility. Cache resolutions with long volatility horizons; recompute resolutions with short horizons. Avoids stale values while minimizing computation.

**Minimal Semantic Unit**
The smallest meaningful building block in a system. In traditional token theory: colors, spacings, durations. In the coordinate model: resolution policies over context coordinates.

**Mode**
A named value within a dimension. "dark" is a mode of the `colorScheme` dimension. "compact" is a mode of the `density` dimension. Flat mode systems treat dimensions as independent mode switches.

**Mode Switch**
A toggle that selects one mode from a dimension's options. Flat mode systems model context as a set of independent mode switches. The coordinate model critiques this as a category error.

**Monotonic**
In resolution: adding specificity to a rule doesn't change unrelated resolutions. If rule R resolves coordinate C to value V, adding a more specific rule R' for a different coordinate C' shouldn't change R's resolution of C.

**Non-Deterministic (Stochastic)**
Potentially producing different outputs for the same input. The antonym of deterministic. Resolution should generally be deterministic, though uncertainty representation acknowledges probabilistic confidence.

**Observation Space**
In reinforcement learning, the set of all possible observations an agent can receive. The context coordinate is the observation space against which token policies are evaluated—the complete perceptible state.

**Partial Evaluation**
A compilation technique that evaluates parts of a program early when some inputs are known. In token resolution: resolve stable dimensions at build/load time, producing simplified rule sets that only vary on volatile dimensions.

**Partial Function**
A function that may not produce a value for all inputs. Context coordinates are partial functions because they typically specify only a few dimensions, leaving others undefined (⊥). Contrast with total functions.

**Perception**
The process by which agents interpret sensory information. Token behavioral intent targets perception—`color.error` should be perceived as indicating error, regardless of the specific wavelength used.

**Perceptual Outcome**
The experienced result of a resolved token value. Behavioral intent specifies perceptual outcomes ("communicate error state"), not implementation values ("#ff0000"). Same outcome may require different values for different agents.

**Policy**
A token's complete specification, consisting of rules that map context predicates to values. Tokens are policies, not values. The policy encodes behavioral intent; resolution evaluates the policy against a coordinate to produce a concrete value.

**Precedence**
The ordering of dimensions that determines which rule wins when multiple rules of equal specificity match. Accessibility dimensions have highest precedence. Defined by the ontology as a total ordering over dimensions.

**Precedence Ordering**
The total ordering of dimensions from highest to lowest priority. When specificity ties, the rule constraining higher-precedence dimensions wins. Ensures deterministic resolution without arbitrary tiebreakers.

**Precomputation**
Resolving values before they're needed, typically at build time for immutable dimensions. Eliminates runtime work, reduces latency, enables optimization. Part of volatility-staged evaluation.

**Predicate**
The condition part of a token rule. A partial coordinate pattern that either matches or doesn't match a given context coordinate. Example: `{ colorScheme: dark, density: compact }` matches any coordinate where colorScheme is dark AND density is compact.

**Primitive**
A foundational value that semantic tokens reference. `blue-500: #3b82f6` is a primitive. Primitives are raw values; semantic tokens map intent to primitives; component tokens apply semantics to specific uses.

**Projection**
A mapping from a higher-dimensional space to a lower-dimensional one, losing information. Figma's flat collections are a projection of the full coordinate space—useful but lossy. Compound conditions and cross-dimensional constraints are lost in projection.

**Pseudo-States**
In CSS, dynamic states accessed via pseudo-class selectors: `:hover`, `:focus`, `:active`, `:disabled`. Part of the DOM coordinate space. Correspond to volatile interaction dimensions in the token coordinate model.

**Resolution**
The process of evaluating a token (policy) against a context coordinate to produce a concrete value. The resolution algorithm filters rules to those whose predicates match, then selects among matches by specificity and precedence.

**Resolution Algorithm**
The formal procedure that evaluates tokens against coordinates. The core artifact of the coordinate model. Properties: deterministic, total (always produces a value via defaults), monotonic, composable.

**Selectors**
In CSS, patterns that identify which elements a rule applies to. Selectors are predicates over the DOM coordinate space. The coordinate model generalizes this: token predicates are selectors over context coordinate space.

**Semantic Intent**
The meaning a token is designed to convey, independent of implementation. `spacing.comfortable` intends appropriate spacing for relaxed scanning; the pixel value varies by viewport and density. Synonym for behavioral intent.

**Semantic Unit**
A meaningful abstraction above raw values. `color.action.primary` is a semantic unit; `#0066cc` is a primitive. Semantic units encode intent; primitives store values.

**Specificity**
The number of dimensions a predicate constrains. `{ colorScheme: dark, density: compact }` has specificity 2. When multiple rules match a coordinate, higher specificity wins. Borrowed from CSS selector specificity.

**Subspace**
A region of the coordinate space defined by fixing some dimensions. Context types define named subspaces. Example: the `kiosk` subspace might be all coordinates where `viewport=large ∧ density=spacious ∧ contrast=high`.

**Total**
A function that produces a value for every valid input (contrast with partial). The resolution algorithm should be total: every valid coordinate produces a value, via explicit rules or defaults. No coordinate should cause resolution to fail.

**Tuple**
An ordered sequence of values. `(dark, compact, mobile)` is a tuple of mode selections. Flat mode systems treat context as a tuple of independent selections—the category error the coordinate model corrects.

**Uncertainty**
Lack of certainty about dimension values or resolution correctness. Explicit uncertainty representation enables confidence scores, volatility horizons, and graceful degradation. Acknowledges that physical/contextual interaction is inherently probabilistic.

**Volatility**
How frequently a dimension's value changes at runtime. Classification: immutable (never changes), stable (changes rarely), semi-stable (changes occasionally), volatile (changes constantly). Determines caching strategy and evaluation staging.

**Volatility-Aware Evaluation**
Resolution that accounts for volatility classification. Stable dimensions are resolved early and cached; volatile dimensions are resolved late and recomputed. Optimizes performance while ensuring correctness.

**Volatility Classification**
The categorization of dimensions by change frequency: immutable, stable, semi-stable, volatile. Metadata that drives evaluation strategy, caching policy, and partial evaluation staging.

**Volatility Horizon**
An estimate of how long a resolved value remains valid before the context coordinate might change. Enables intelligent caching: values with long horizons can be cached; values with short horizons should be recomputed.
