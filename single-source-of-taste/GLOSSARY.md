# Glossary

A single alphabetical index of every term defined across the documents in this folder. Each entry keeps the wording of the page that defined it and names that page in brackets. Where more than one page defines the same term, every definition is listed in reading order, so you can see how a later page specializes or extends an earlier one; those entries are marked **(multiple pages)**.

Source pages:

- **[02]** [Context as Coordinate System](./02-context-as-coordinate-system.md)
- **[05]** [Ontology Specification](./05-design-token-context-ontology-specification.md)
- **[06]** [The Single Source of Taste](./06-the-single-source-of-taste.md)
- **[07]** [Carrying Judged Policies Through Figma](./07-carrying-judged-policies-through-figma.md)
- **[08]** [Compound Conditions Beyond Figma](./08-compound-conditions-beyond-figma.md)

Pages 01, 03, and 04 introduce no glossary terms of their own. The appendix documents keep their own vocabulary and are not indexed here.

---

## A

**Accessibility Dimension** [02] A context dimension encoding user accessibility needs or preferences. Examples: `contrast`, `motion`, `forcedColors`. Accessibility dimensions have highest precedence in the resolution algorithm because they encode fundamental agent capabilities that constrain which values can achieve behavioral intent.

**Adjudication Pull Request** [07] A pull request opened on push of a changed judged value, carrying the edit-time rationale, confidence, author, and source policy identity. Merging ratifies the new aesthetic resolution; declining reverts it. Realises the Single Source of Taste's escalation requirement using GitFig's existing pull-request-from-Figma capability.

**Admissible Set (Feasible Region) / Admissible Set** (multiple pages)

- [06] The set of values permitted at a given coordinate by all applicable verifiable policies. Judged policies resolve within this set. See *Placement in Resolution Precedence*.
- [08] (from the Single Source of Taste) The values permitted at a coordinate by verifiable policies. A compound-condition judged value must fall inside it; accessibility continues to cap taste even where Figma cannot represent the value.

**Aesthetic Intent** [06] A behavioral intent whose success has no machine-checkable criterion and is decided by a designated authority. Examples: reading as trustworthy, feeling composed, sitting quietly beneath neighbouring surfaces. Contrast with *functional intent*.

**Affordance** [02] A relation between an agent and an environment that specifies what actions are possible. From Gibson's ecological psychology. In UI: a hover state is an affordance only for devices with hover capability. Affordances are agent-relative, not objective properties of objects or interfaces.

**Agent** [02] The platform/device/user combination that consumes resolved token values. Includes environmental capabilities (display gamut, pointer type) and user characteristics (accessibility needs, preferences). Resolution is agent-relative: the same environment produces different values for different agents.

**Agent Capability** [02] A property of an agent that determines what affordances are available and what values can achieve behavioral intent. Examples: display color gamut, pointer precision, screen reader presence. Agent capabilities constrain resolution—a P3 color is only achievable on P3-capable displays.

**Agent Class** [02] A category of agents with similar capabilities. Examples: mobile-touch, desktop-pointer, screen-reader. Allows resolution rules to target classes rather than enumerating individual agent configurations.

**Agent Dimension** [02] A context dimension encoding agent capabilities rather than environmental state. Examples: `pointerType`, `colorGamut`, `screenReaderActive`. Contrast with environmental dimensions like `colorScheme` or `viewportWidth`.

**Agent-Relative** [02] The property of depending on agent capabilities, not just environmental state. Resolution is agent-relative: the same token in the same environment resolves differently for different agents. Affordances are agent-relative: what's possible depends on who's acting.

**Agentic Authoring Loop** [08] The cycle by which a designer states an intersection intent, an agent (Claude Code) drafts a conformant compound policy and renders a preview via the resolver, the designer ratifies with human authority and confidence, and the change is opened as an adjudication pull request.

**Algorithmic Evaluation** [02] Resolution treated as execution of an algorithm rather than simple lookup. The resolution algorithm filters, sorts by specificity, applies precedence, and handles defaults. Contrasts with flat mode systems where resolution is direct value retrieval.

**Annotation Gap** [07] The absence in Figma of any native, structured, designer-visible field for judged-policy metadata (regime, authority, confidence, rationale). Distinct from the projection gap; closed by carrying metadata in plugin data and rendering it into descriptions.

**Appearance** [02] The visual presentation of an interface element. In the coordinate model, appearance is distinguished from behavior—tokens encode behavioral intent (perceptual outcome), not just appearance (visual specification).

**Authorial Confidence** [06] Under the judged regime, the `confidence` field denotes the design authority's conviction in an aesthetic call — how settled the decision is — not a statistical probability of correctness. Low authorial confidence triggers escalation, not degradation.

**Authority (of a policy)** [06] The certifier of a policy's correctness. A *specification reference* (e.g., `wcag:AA:contrast>=4.5`) under the verifiable regime; an *authority reference* (e.g., `design-system-core`) under the judged regime. Carried in `dtcm.authority`. This field encodes the truth/taste distinction at the data layer.

## B

**Behavior-Under-Interaction** [02] How something responds to action, not just how it appears statically. From embodied AI: a material's properties are its response to force, not its color. For tokens: even "static" color tokens encode behavioral response to perception in context.

**Behavioral Intent** (multiple pages)

- [02] What a token is trying to achieve perceptually or functionally, independent of the specific value used. `color.error` intends to communicate error state; the specific red varies by context. Behavioral intent is constant; implementation values vary.
- [06] (extends the term from *Context as Coordinate System*) What a policy aims to achieve perceptually or functionally, independent of value. In this document, behavioral intent **bifurcates** by validation regime into functional and aesthetic intent.

**Behavioral Policy** [02] A specification that maps conditions to outcomes. Tokens are behavioral policies: given this context coordinate, produce this perceptual outcome. The policy encodes intent; resolution evaluates the policy to produce concrete values.

**⊥ (Bottom)** [02] The symbol for "undefined" or "no value" in logic and type theory. In the coordinate model, `C(dimension) → ⊥` means the coordinate does not specify that dimension. Pronounced "bottom."

## C

**Carrier** [07] Any location in a Figma file where metadata could be stored: variable description, code-syntax slots, naming convention, or shared plugin data. No single carrier is both structured and designer-visible.

**Cartesian Product** [02] The set of all possible combinations of elements from multiple sets. If Theme has 2 values and Density has 3, their Cartesian product has 2 × 3 = 6 combinations. Flat mode systems treat context as a Cartesian product of independent dimensions.

**Cascade** [02] In CSS, the precedence ordering that determines which rule wins when multiple rules match. Origin (user-agent, author, user), importance, specificity, and source order form the cascade. The coordinate model's precedence ordering serves a similar function.

**Category Error** [02] A logical fallacy of treating something as belonging to a category it doesn't belong to. Treating context as a tuple of independent modes is a category error—context is a single complex state, not separable independent selections.

**Certain** [02] A dimension value that is definitively known, not inferred. `platform: iOS` is certain after detection. Contrast with inferred values that are probabilistic. Certainty affects resolution confidence.

**Code-Resident Resolver** [08] A runtime library implementing `resolve(policy, coordinate)` over the full coordinate space, compound predicates included. The home of the irreducible residue and the operational form of the coordinate model's "resolution algorithm as core artifact."

**Cognitive Science** [02] The interdisciplinary study of mind and intelligence. Relevant to token theory through concepts like core knowledge (innate physical primitives), mental simulation, and object-centric cognition. Informs how humans perceive and process interface elements.

**Composable** [02] The property of combining smaller units into larger wholes while preserving meaning. Tokens should be composable: resolution of a composed token equals composition of resolutions. Composability enables building complex systems from simple parts.

**Compound Condition** (multiple pages)

- [02] A token rule that triggers only when multiple dimensions align. Example: `{ colorScheme: dark, contrast: high } → value`. Not expressible in flat mode systems where dimensions are independent. Enables nuanced context-dependent behavior.
- [08] A resolution rule keyed to a conjunction of dimension values (`dark ∧ dense ∧ high-contrast`). Representable in Figma only by materialising the product of modes, which the coordinate model rejects.

**Construct** [02] A programmatic entity that can be created, manipulated, and referenced. Context types should be first-class constructs—not just documentation but runtime-inspectable, composable entities in the system.

**Context** (multiple pages)

- [02] The complete set of conditions under which a token resolves. Includes environmental state, agent capabilities, inherited values, and local overrides. Modeled as a coordinate in N-dimensional space.
- [05] Environmental or user-preference condition affecting token resolution

**Context Coordinate / Context coordinate** (multiple pages)

- [02] A point in the N-dimensional space of all context dimensions. Formally, a partial function `C: DimensionName → Value | ⊥` that maps dimension names to values, leaving unspecified dimensions undefined. Represents the complete contextual state against which tokens resolve.
- [05] Complete set of dimension values at a point in the component tree

**Context Type / Context type** (multiple pages)

- [02] A named subspace of the coordinate space with optional policy overlays. Examples: `kiosk`, `print`, `mobile-dark`. More than shortcuts—they can carry constraints and resolution overrides beyond their coordinate positions. Can form hierarchies through inheritance.
- [05] Named intersection of common dimension combinations

**Contextual Variation** [02] The phenomenon of tokens resolving to different values under different conditions. The central problem the coordinate model addresses. Treated as afterthought in flat mode systems; treated as primary concern in the coordinate model.

**Coordinate Model** [02] The theoretical framework treating context as a point in N-dimensional space and tokens as resolution policies over that space. Contrasts with flat mode systems that treat dimensions as independent switches.

**Cross-Dimensional Constraint** [02] A rule that forces or restricts dimension values based on other dimensions. Example: `if contrast = forced then elevation must be base`. Constraints normalize coordinates before resolution, reducing effective dimensionality.

## D

**Dependency** [05] Relationship where one category's resolution affects another

**Deterministic** [02] Producing the same output for the same input, every time. The resolution algorithm should be deterministic: same token + same coordinate = same value. Contrast with stochastic/non-deterministic processes that may vary.

**Dimension** (multiple pages)

- [02] A single axis in the context coordinate space. Examples: `colorScheme`, `density`, `viewport`, `interaction`. Each dimension has a finite set of possible values. The ontology defines 93 dimensions across 12 categories.
- [05] Single axis within a context category

**Dimensionality Mismatch** [08] The core reason the residue is structural: a Figma variable is a one-argument function of its collection's mode, while a compound condition is a many-argument function. Capacity growth lengthens the one axis; it never adds arguments.

**DOM Coordinate Space** [02] In CSS, the implicit space over which selectors operate. Axes include element type, classes, IDs, attributes, pseudo-classes, and pseudo-elements. Selectors are predicates over this space; specificity orders matches.

**Draft-not-Certify Invariant** [08] The rule that an automated agent may draft a judged value and its rationale but may never be its `authority`. The `authority` must resolve to a human party, and the adjudication pull request must be merged by that party.

## E

**Embodied AI** [02] Artificial intelligence systems that interact with the physical world through sensors and actuators. Relevant to token theory through concepts like affordances, agent-relative perception, and behavior-under-interaction.

**Environmental Dimension** [02] A context dimension encoding environmental state rather than agent capabilities. Examples: `colorScheme`, `ambientLight`, `viewportWidth`. Detected from the environment; contrast with agent dimensions encoding capabilities.

**Escalation** [06] The routing of a low-confidence judged resolution to its authority for decision, in place of the graceful degradation used for verifiable policies. There is no conservative value guaranteed to be tasteful, so aesthetic uncertainty cannot be resolved by fallback.

**Escalation-on-Edit** [07] The rule that an edit to a judged value is an appeal, not a write: it must supply conviction and open an adjudication pull request rather than degrade to a default. The round-trip form of the Single Source of Taste's escalate-don't-degrade principle.

**Evaluation Strategy** [02] A method for determining when and how to compute values. Volatility classification defines an evaluation strategy: resolve immutable dimensions at build time, stable at session start, volatile at interaction time.

**Explicit Ambiguity** [02] When two rules of equal specificity and incomparable precedence both match a coordinate. A detectable authoring error—the system should flag this at definition time. Contrast with implicit ambiguity hidden by arbitrary tiebreakers.

**Expressiveness** [02] The range of distinctions a system can represent. The coordinate model has greater expressiveness than flat modes: compound conditions, specificity ordering, cross-dimensional constraints. More expressiveness enables more nuanced behavior.

## F

**Faithful Round-Trip** [07] A round-trip in which every value is preserved or explicitly flagged as unrepresentable, every judged edit is adjudicated, and every judgment is legible in-canvas. Weaker than lossless-and-native because carried metadata remains invisible to consumers without the plugin.

**First-Class** [02] Having full status as a programmatic entity—can be passed as arguments, returned from functions, assigned to variables, inspected at runtime. Context types should be first-class constructs, not just documentation strings.

**Functional Intent** [06] A behavioral intent whose success is checkable against an external, agent-independent criterion (contrast ratio, vestibular threshold, legibility floor). Resolved under the verifiable regime.

## G

**Gamut** [02] The range of colors a display can reproduce. sRGB, P3, and Rec. 2020 are progressively wider gamuts. An agent capability dimension—P3 colors only resolve correctly on P3-capable displays.

**Graceful Degradation** [02] The property of falling back to acceptable behavior when ideal behavior isn't possible. In resolution: when confidence is low or capabilities are limited, prefer conservative values that work broadly over optimal values that might fail.

**Graceful Fallback** [02] Less specific rules catching coordinates not covered by more specific rules. Enables progressive enhancement: define specific behavior where needed, let general rules handle the rest.

## I

**Implementation Value** [02] The concrete value (hex color, pixel count, milliseconds) that implements behavioral intent in a specific context. The same behavioral intent requires different implementation values across contexts.

**Inferred** [02] A dimension value derived probabilistically rather than detected with certainty. "User probably prefers high contrast based on system settings" is inferred. Inferred values have lower confidence than certain values.

**Inheritance** [02] The mechanism by which context flows from ancestors to descendants. A `<CompactRegion>` wrapper makes descendants inherit `density: compact`. Combinable states union; exclusive dimensions override.

**Intelligent Caching** [02] Caching strategy informed by volatility. Cache resolutions with long volatility horizons; recompute resolutions with short horizons. Avoids stale values while minimizing computation.

**Intent-Authoring** [08] The authoring modality in which a designer originates a judged value by expressing intent in language to an agent, rather than by direct manipulation in the design tool. The changed-but-not-lost form of authorship for the residue.

**Irreducibility (of design authority)** [06] The property that no resolution algorithm can originate or ratify taste. The algorithm enforces conformance to an authored aesthetic policy but cannot certify the policy's adequacy, because a judged policy is by definition one with no machine-checkable criterion. The terminal resolver is always a party, not a procedure.

**Irreducible Residue** [08] The compound-condition judged values that survive refactoring because their value is not a function of the parts. Disproportionately pure taste — unpredicted by composition, uncertified by criterion.

## J

**Judged Regime** [06] The validation regime in which correctness is decided by a designated authority rather than a criterion. Applies to aesthetic-intent policies.

## L

**Lossless-and-Native Round-Trip** [07] The unattainable ideal in which judged policies survive through Figma with full structure and metadata, visible to every consumer without special tooling. Blocked by the projection and annotation gaps; would require a native Figma metadata field.

## M

**Minimal Semantic Unit** [02] The smallest meaningful building block in a system. In traditional token theory: colors, spacings, durations. In the coordinate model: resolution policies over context coordinates.

**Mode** [02] A named value within a dimension. "dark" is a mode of the `colorScheme` dimension. "compact" is a mode of the `density` dimension. Flat mode systems treat dimensions as independent mode switches.

**Mode Switch** [02] A toggle that selects one mode from a dimension's options. Flat mode systems model context as a set of independent mode switches. The coordinate model critiques this as a category error.

**Monotonic** [02] In resolution: adding specificity to a rule doesn't change unrelated resolutions. If rule R resolves coordinate C to value V, adding a more specific rule R' for a different coordinate C' shouldn't change R's resolution of C.

## N

**Non-Deterministic (Stochastic)** [02] Potentially producing different outputs for the same input. The antonym of deterministic. Resolution should generally be deterministic, though uncertainty representation acknowledges probabilistic confidence.

**Non-Separable Composition** [08] A composition in which the intersection carries a bespoke value not equal to any combination of independent per-dimension values. The source of the residue. Contrast with separable composition.

## O

**Observation Space** [02] In reinforcement learning, the set of all possible observations an agent can receive. The context coordinate is the observation space against which token policies are evaluated—the complete perceptible state.

## P

**Partial Evaluation** [02] A compilation technique that evaluates parts of a program early when some inputs are known. In token resolution: resolve stable dimensions at build/load time, producing simplified rule sets that only vary on volatile dimensions.

**Partial Function** [02] A function that may not produce a value for all inputs. Context coordinates are partial functions because they typically specify only a few dimensions, leaving others undefined (⊥). Contrast with total functions.

**Perception** [02] The process by which agents interpret sensory information. Token behavioral intent targets perception—`color.error` should be perceived as indicating error, regardless of the specific wavelength used.

**Perceptual Outcome** [02] The experienced result of a resolved token value. Behavioral intent specifies perceptual outcomes ("communicate error state"), not implementation values ("#ff0000"). Same outcome may require different values for different agents.

**Policy** [02] A token's complete specification, consisting of rules that map context predicates to values. Tokens are policies, not values. The policy encodes behavioral intent; resolution evaluates the policy against a coordinate to produce a concrete value.

**Policy Identity (`dtcm.id`)** [07] An opaque, stable identifier minted when a policy is first authored and written to every variable-mode cell the policy projects into. The precondition for push: it maps a Figma edit back to its source policy by identity rather than by name.

**Precedence** [02] The ordering of dimensions that determines which rule wins when multiple rules of equal specificity match. Accessibility dimensions have highest precedence. Defined by the ontology as a total ordering over dimensions.

**Precedence Ordering** [02] The total ordering of dimensions from highest to lowest priority. When specificity ties, the rule constraining higher-precedence dimensions wins. Ensures deterministic resolution without arbitrary tiebreakers.

**Precomputation** [02] Resolving values before they're needed, typically at build time for immutable dimensions. Eliminates runtime work, reduces latency, enables optimization. Part of volatility-staged evaluation.

**Predicate** [02] The condition part of a token rule. A partial coordinate pattern that either matches or doesn't match a given context coordinate. Example: `{ colorScheme: dark, density: compact }` matches any coordinate where colorScheme is dark AND density is compact.

**Primitive** [02] A foundational value that semantic tokens reference. `blue-500: #3b82f6` is a primitive. Primitives are raw values; semantic tokens map intent to primitives; component tokens apply semantics to specific uses.

**Projection** [02] A mapping from a higher-dimensional space to a lower-dimensional one, losing information. Figma's flat collections are a projection of the full coordinate space—useful but lossy. Compound conditions and cross-dimensional constraints are lost in projection.

**Projection Gap** [07] The inability of Figma's mode model to represent compound conditions, specificity, or precedence, such that some judged values are unrepresentable as variables and live only in code. Recorded in an `unrepresented` field and surfaced as an in-canvas warning.

**Promotion** [06] The elevation of a recurring aesthetic override into a canonical aesthetic policy (or context type) by the authority. Distinct from automation: the system surfaces the pattern; the authority decides whether it is taste.

**Pseudo-States** [02] In CSS, dynamic states accessed via pseudo-class selectors: `:hover`, `:focus`, `:active`, `:disabled`. Part of the DOM coordinate space. Correspond to volatile interaction dimensions in the token coordinate model.

## R

**Reducible Residue** [08] The portion of apparent non-separability removable by inserting an intermediate alias layer that turns a conjunction into separable, Figma-representable steps. Should be exhausted before building resolver machinery.

**Rendered Badge** [07] A compact, human-readable summary of judged metadata that GitFig writes into a variable's description on pull. A derived *view*, regenerated each pull, never read back as source.

**Resolution** (multiple pages)

- [02] The process of evaluating a token (policy) against a context coordinate to produce a concrete value. The resolution algorithm filters rules to those whose predicates match, then selects among matches by specificity and precedence.
- [05] Process of determining concrete value for a token given context

**Resolution Algorithm** [02] The formal procedure that evaluates tokens against coordinates. The core artifact of the coordinate model. Properties: deterministic, total (always produces a value via defaults), monotonic, composable.

## S

**Scope boundary** [05] Structural element that resets certain context dimensions

**Selectors** [02] In CSS, patterns that identify which elements a rule applies to. Selectors are predicates over the DOM coordinate space. The coordinate model generalizes this: token predicates are selectors over context coordinate space.

**Semantic Intent** [02] The meaning a token is designed to convey, independent of implementation. `spacing.comfortable` intends appropriate spacing for relaxed scanning; the pixel value varies by viewport and density. Synonym for behavioral intent.

**Semantic Unit** [02] A meaningful abstraction above raw values. `color.action.primary` is a semantic unit; `#0066cc` is a primitive. Semantic units encode intent; primitives store values.

**Separable Composition** [08] A composition in which the value is a function of independent per-dimension contributions, `value(d₁,d₂) = combine(f(d₁), g(d₂))`. Handled natively by Figma via layered collections and aliasing.

**Shared Plugin Data** [07] Figma's mechanism for a plugin to persist namespaced key–value data on documents and nodes. Structured and round-trip-safe, but invisible in the native UI. The system of record for judged metadata; invisibility to non-GitFig consumers is the design's hard ceiling.

**Single Source of Taste** [06] The principle that every aesthetic-intent policy has exactly one canonical authoring and adjudicating authority. The mirror of the Single Source of Truth: not one value everywhere, but one place entitled to say. Divergent aesthetic judgments are appeals to that authority, not competing truths.

**Specificity** [02] The number of dimensions a predicate constrains. `{ colorScheme: dark, density: compact }` has specificity 2. When multiple rules match a coordinate, higher specificity wins. Borrowed from CSS selector specificity.

**Subspace** [02] A region of the coordinate space defined by fixing some dimensions. Context types define named subspaces. Example: the `kiosk` subspace might be all coordinates where `viewport=large ∧ density=spacious ∧ contrast=high`.

**System of Record (split)** [07] The principle that the *value* of a judged policy may be edited in Figma, while its *metadata* is recorded only in plugin data plus the adjudication pull request. Writing metadata back from a variable is a loss.

## T

**Taste** [06] The design-owned capacity to author and adjudicate aesthetic-intent policies. Not a coordinate value but an authority over policies — analogous to a cascade origin, not a selector or media query.

**Terminal Resolver** [06] The authority that makes the final decision on a judged policy. Contrast with the resolution algorithm, which is the terminal resolver only for verifiable policies.

**Total** [02] A function that produces a value for every valid input (contrast with partial). The resolution algorithm should be total: every valid coordinate produces a value, via explicit rules or defaults. No coordinate should cause resolution to fail.

**Tuple** [02] An ordered sequence of values. `(dark, compact, mobile)` is a tuple of mode selections. Flat mode systems treat context as a tuple of independent selections—the category error the coordinate model corrects.

## U

**Uncertainty** [02] Lack of certainty about dimension values or resolution correctness. Explicit uncertainty representation enables confidence scores, volatility horizons, and graceful degradation. Acknowledges that physical/contextual interaction is inherently probabilistic.

**Unrepresented** [07] A recorded list of the coordinate conditions under which a policy's value could not be projected into Figma's modes. The projection gap, made explicit and visible so designers do not edit values whose true resolution Figma cannot show.

## V

**Validation Regime** [06] First-class policy metadata (`dtcm.regime`) recording how a policy's resolved value is judged correct: `verifiable` (by criterion) or `judged` (by authority). The minimal addition required to accommodate taste without a separate resolution engine.

**Verifiable Regime** [06] The validation regime in which correctness is decided at resolution time by a machine-checkable criterion, with no human involvement. Applies to functional-intent policies.

**Volatility** (multiple pages)

- [02] How frequently a dimension's value changes at runtime. Classification: immutable (never changes), stable (changes rarely), semi-stable (changes occasionally), volatile (changes constantly). Determines caching strategy and evaluation staging.
- [05] Classification of how frequently a dimension's value changes

**Volatility Classification** [02] The categorization of dimensions by change frequency: immutable, stable, semi-stable, volatile. Metadata that drives evaluation strategy, caching policy, and partial evaluation staging.

**Volatility Horizon** [02] An estimate of how long a resolved value remains valid before the context coordinate might change. Enables intelligent caching: values with long horizons can be cached; values with short horizons should be recomputed.

**Volatility-Aware Evaluation** [02] Resolution that accounts for volatility classification. Stable dimensions are resolved early and cached; volatile dimensions are resolved late and recomputed. Optimizes performance while ensuring correctness.
