# From Polytruth to Taste: Origins of the Single Source of Taste

*Source: distilled from [a conversation](https://claude.ai/share/5bb9babf-7a7b-47ae-84be-878192d36f35) between Dan Schmitz and Claude that ran from January 10 to September 8, 2026. This page records the path of the argument so the finished pages can be read against the questions that produced them. Tables and quoted structures are condensed from the transcript; nothing has been added to the positions taken there.*

---

## Timeline

| When | What happened | Where it landed |
|---|---|---|
| January 10, 2026 | A thought experiment: what would a software architecture look like if it were contrarian to the Single Source of Truth? | The "polytruth" seed, below |
| January 11 | The question narrowed to design tokens, engineering versus design. A three-way comparison of Single Source of Truth, contextual, and polytruth philosophies across seventeen dimensions, then three worked scenarios | Part 2 and Part 3 |
| January 11 | The scenarios moved the author toward the Single Source of Truth, keeping polytruth's exception logging. A five-tier token ontology with an exception log followed, then the observation that context deserves its own ontology | Part 4 and Part 5; became the [ontology](../04-design-token-context-ontology.md) and its [specification](../05-design-token-context-ontology-specification.md) |
| January 11 | The context ontology was exported as a working draft and a project prompt for systematic stress-testing | The `archive/` folder of [design-token-context-model](https://github.com/ds1/design-token-context-model) |
| January 20 and 21 | The stress-tested ontology, the coordinate-system thesis, and the companion pages were published on the design-token-context-model wiki | Pages [01](../01-token-systems-should-adopt-the-css-model.md) through [05](../05-design-token-context-ontology-specification.md) |
| September 8 | The Single Source of Taste was named, corrected against the wiki, and written up as three papers | Pages [06](../06-the-single-source-of-taste.md), [07](../07-carrying-judged-policies-through-figma.md), and [08](../08-compound-conditions-beyond-figma.md) |

---

## Part 1: The contrarian seed

The opening question was whether truth has to be singular. The Single Source of Truth assumes truth is singular, stable, and context-independent, but a person's shipping address legitimately differs for their bank, employer, and doctor. A "polytruth" architecture would embrace divergence instead of fighting it:

1. **Federated authority.** Each domain owns its own interpretation of shared data.
2. **Conflict as signal, not error.** Disagreement between systems is information worth surfacing, not something to paper over.
3. **Temporal plurality.** Every observer sees a valid truth for their causal moment, the way Git has no single "real" state, only perspectives on a graph.
4. **Probabilistic data.** Replace `status: active` with `status: {active: 0.94, churning: 0.06}` and let consumers apply their own thresholds.
5. **Truth negotiation protocols.** Interoperating systems negotiate a shared interpretation instead of one deferring to another.

The trade was named immediately: reasoning simplicity for resilience and autonomy.

---

## Part 2: Engineering, design, and polytruth on design tokens

Design tokens sit on the fault line between two disciplines with different epistemologies. Engineering culture is Platonic: there is an ideal `color.primary.500`, deviation is error, consistency is quality. Design culture is phenomenological: the same hex reads darker on cream than on white, identical padding feels different in a dense table and a hero, so the token is a useful fiction and consistency is an aesthetic choice, not a law.

The comparison was expanded across seventeen dimensions with a third column for the theoretical polytruth architecture.

| Dimension | Engineering (Single Source of Truth) | Design (contextual) | Polytruth |
|---|---|---|---|
| What a token represents | A canonical, immutable binding; the token *is* the value | A named intention; the hex is one implementation of it | A negotiated consensus with documented dissent |
| How overrides are read | Technical debt or defect; eliminate through better tokenization | Professional judgment; the override *is* the design decision | Signal requiring classification; feed it back into the system |
| Where the system boundary lies | The shipped artifact | The perceived experience | The feedback loop from definition through perception back to refinement |
| Optimization target | Reproducibility and predictability | Appropriateness and resonance | Coherence with acknowledged, bounded variance |
| Source of authority | The token file; authority flows downward | The design rationale; intent trumps implementation | Distributed with an explicit hierarchy of sovereign domains |
| Change management | Propagation from the source | Translation and re-evaluation per context | Renegotiation of every contextual agreement |
| Conflict resolution | Hierarchy; the more abstract token wins | Judgment by the designer closest to the context | Protocol; explicit rules encode how authorities interact |
| Versioning | Snapshots of truth; old versions are legacy | Evolutionary branches; versions are dialects | Layered precedent; history stays interpretively valid |
| Purpose of documentation | Contract specification | Design rationale, a teaching document | A negotiation record; case law |
| How success is measured | Consistency metrics a linter can verify | Experience quality, which needs human judgment | Adaptation rate and family resemblance |
| Characteristic failure | Brittleness and shadow systems | Fragmentation and lost rationale | Bureaucratic overhead and classification debates |
| Relationship to context | Context-blind by design; new needs spawn new tokens | Context-primary; every use is a context | Context-typed; contexts are first-class entities with resolution rules |
| Relationship to time | Synchronic; the current value is the only truth | Diachronic; values carry historical reasoning | Time-layered; past decisions remain a living resource |
| Governance | Centralized design systems team | Federated across product teams | A constitutional "court" that interprets principles and records precedent |
| Abstraction philosophy | Earlier and higher is better | Appropriate depth; a literal `16px` can communicate better than a token | High abstraction with tracked escape hatches |
| Testing and validation | Automated verification | Holistic review and critique | Automated for consensus cases, human for exceptions, with the reasons captured |
| Onboarding | Learn the rules | Learn the intent | Learn the jurisprudence |

Each philosophy answers a different meta-question:

- **Engineering:** how do we scale design decisions across an organization without a designer in every implementation?
- **Design:** how do we preserve quality and intentionality when decisions are implemented by people who were not in the room?
- **Polytruth:** how do we build a system that learns from the tension between those two needs rather than suppressing it?

---

## Part 3: Three scenarios

Three scenarios tested the philosophies against concrete token problems.

**A primary button across four contexts** (marketing hero, dense data table, mobile bottom sheet, confirmation modal). The Single Source of Truth answered by adding `button.primary.compact` and `button.primary.touch` tokens and a decision tree. The contextual approach clarified the token's intent and let each designer interpret it, documenting rationale that would later be lost when they left. Polytruth defined `dense-data`, `touch-primary`, and `marketing-surface` as first-class context types with named authorities and approval dates, and made the button's padding a set of context-keyed resolutions.

**A brand evolution across six surfaces.** Five coupled token families (color, typography, spacing, radius, elevation) had to move together under an eight-week deadline with App Store buffers, contractual notice periods, and merchant freezes. The Single Source of Truth shipped a coordinated v2.0.0 release train. The contextual approach shipped intent guidance and let each product interpret it. Polytruth classified the changes into three migration clusters, ran a context-by-context renegotiation with logged dissent, and came out with a new `type.family.numeric` token and a precedent that embedded contexts may prioritize host integration over brand expression.

**A conflict between card padding and touch-target minimums.** The one-sentence contrast that closed the scenario: the Single Source of Truth says the hierarchy decided and padding yields to accessibility; the contextual approach says the designer decided this context does not need mobile touch targets; polytruth says the protocol decided based on context type, and the decision is logged and reviewable.

---

## Part 4: The pivot to a Single Source of Truth with an exception log

The scenarios moved the author toward the Single Source of Truth, while keeping polytruth's learning mechanism. The question became: what is the structure that carries maximal abstraction down to specific instances without losing that learning?

The answer was a five-tier ontology (primitives, options, decisions, components, instances) with two rules that later pages depend on:

- **Instances are the only place context-specific values live**, and they are explicit, not overrides. `instance.button.context.dense.padding.block` is a declared token, not a patch.
- **A parallel exception log captures signals.** Each exception records the component, context, affected token, system value, actual value, rationale, author, date, and a status that moves through `logged`, `under_review`, `rejected`, `accepted`, or `promoted`. Three or more similar exceptions trigger governance review, and a promoted exception becomes a new instance token.

This is the "learnable friction" principle that appears in the ontology's design philosophy and that the [Single Source of Taste](../06-the-single-source-of-taste.md) later reuses as the promotion lifecycle for aesthetic decisions.

---

## Part 5: Context deserves its own ontology

Asked whether these tiers were the industry's state of the art, the conversation surveyed Salesforce Lightning, IBM Carbon, Adobe Spectrum, Material Design 3, Tokens Studio, and the W3C draft, and found a consensus on three tiers minimum but no formal treatment of context. Figma Variables had introduced modes, Spectrum had scale and platform, Material had density, but nobody had a comprehensive context taxonomy. Context was being reduced to theme, platform, and maybe density.

The response was the first draft of the context ontology: eighteen dimensions across platform, input, viewport, density, appearance, accessibility, temporal state, environment, and localization, each with values and the token families it affects. Four ideas from that draft survived into the specification unchanged in spirit:

- **Volatility hierarchy.** Immutable, stable, semi-stable, volatile, and inherited dimensions each resolve at a different time: build, session start, reactive update, scoped declaration, cascade.
- **Dimensional resolution.** Each token declares which dimensions it cares about; the most specific matching clause wins, with ties broken by declaration order. This is the resolution algorithm that [Context as Coordinate System](../02-context-as-coordinate-system.md) formalizes.
- **Context types** as named intersections with defaults and effects, including `dense_data`, `touch_primary`, `glanceable`, `embedded_foreign`, and `accessibility_enhanced`.
- **Inheritance rules** distinguishing full, constrained, non-inherited, and inverse inheritance, and a split between declared, system-detected, environment-detected, and inferred dimensions.

The draft was exported with a project system prompt for systematic stress-testing, and the stress-tested result became the [specification](../05-design-token-context-ontology-specification.md) with twelve categories and ninety-three dimensions.

---

## Part 6: Naming the Single Source of Taste

On September 8, 2026 the author introduced a new concept: taste is a design-owned quality and standard, and because GitFig now keeps Figma and GitHub in sync, the production and management of taste can happen in Figma. How should a Single Source of Taste be framed to mirror the Single Source of Truth in a landscape where building is democratized and the designer's role grows?

The first framing treated a shipped product as having two orthogonal quality dimensions with two sources of authority.

| | Truth (does it work?) | Taste (is it right?) |
|---|---|---|
| Authority | Engineering | Design |
| Lives in | Codebase | Design layer |
| Canonical store | Git repository | Figma, synced |
| Governance | Code review, tests, CI | Design review, critique, tokens |
| Violations | Bugs, regressions | Incoherence, ugliness, wrongness |
| Verification | Automated | Judgment |
| Change unit | Commit | Design decision |

Reading the GitFig documentation sharpened the claim. GitFig does not keep the codebase true in Figma; it keeps the **token layer** true in Figma. So the Single Source of Taste is a precise claim about one layer: tokens are the crystallized, machine-readable form of taste, and that layer can now be design-authored and code-synchronized. The four principles of the Single Source of Truth were mirrored over that surface: one canonical location, reference never copy, change at the source and propagate, divergence is debt. A hardcoded hex in a component became "taste drift," a violation for the same structural reason a stale data copy is.

The honest boundary was stated at the same time: truth is verifiable and taste is not. You can enforce conformance to tokens automatically; you cannot enforce that the tokens are good. The token layer canonicalizes only the encodable fraction of taste.

---

## Part 7: The correction from the coordinate model

The author then asked for the response to be reformed after reading the design-token-context-model wiki, noting that it was their own repository. Four corrections came out of that reading, and each became a load-bearing part of the finished papers.

**Taste and truth are the same construct.** The coordinate thesis holds that tokens are policies mapping context coordinates to values. A judgment such as "the primary action should read as confident and trustworthy here" has exactly that form. Truth and taste are therefore not cousins but the same kind of object, differing only in the nature of their success criterion. This became [Argument 1](../06-the-single-source-of-taste.md#argument-1-behavioral-intent-bifurcates) of the Single Source of Taste.

**The wiki had a taste-shaped hole.** Every testability example in the coordinate thesis was functional and had a decision procedure. Aesthetic intent has identical policy form and no decision procedure. The model already contained this class of policy and had not named it. Naming it was the contribution.

**Agent-relativity moves the boundary.** The residue that the first framing called un-encodable (hex on cream, elevation relative to neighbors) is partly encodable as agent and environment dimensions. What stays outside is only the final judgment of whether the resolved output is good. This became Argument 5.

**The GitFig premise needed correcting.** The thesis is explicit that Figma's mode collections are a projection of the coordinate space, and the source repository's own token files bear that out. So the canonical source of taste is the policy set plus the resolution algorithm, and GitFig keeps the projection true, not taste itself. This became Insight 4 of the Single Source of Taste and the whole of [Carrying Judged Policies Through Figma](../07-carrying-judged-policies-through-figma.md).

The tightest statement of the corrected concept:

```
Truth  = resolve(policy, coordinate) → value, criterion: does it work? (verifiable)
Taste  = resolve(policy, coordinate) → value, criterion: is it right?  (judged)
SSOTaste = design authors the policy layer;
           accessibility policies outrank aesthetic ones;
           GitFig projects it into Figma and code as views.
```

Three frontier problems were named and each got a paper: the implementation trails the theory (the resolver is specified, not running), Figma cannot represent the policy space so GitFig cannot host taste alone, and the engine can enforce conformance to taste but never taste itself. The third is the irreducibility boundary. The second became page 07. The compound-condition residue that page 07 flagged became page 08, whose answer to "does the residue shrink as Figma's mode model grows" is a firm no.

---

## What survived from polytruth, and what did not

The finished work is a Single Source of Truth architecture, not a polytruth one, but several polytruth ideas survived in changed form.

| Polytruth idea | Fate |
|---|---|
| Conflict as signal | Survived as the exception log, the ontology's "learnable friction," and the promotion lifecycle for aesthetic overrides |
| Probabilistic data | Survived as `confidence` on resolutions, reinterpreted for judged policies as authorial conviction |
| Layered precedent and case law | Survived as retained rationale and as the pull request history that page 07 calls the case law promotion depends on |
| Explicit hierarchy among authorities | Survived as accessibility outranking aesthetics: taste is sovereign over the aesthetic and subordinate to access |
| Federated authority | Rejected. The Single Source of Taste requires exactly one canonical authority per aesthetic policy; divergent judgments are appeals, not competing truths |
| Truth negotiation protocols | Rejected as a runtime mechanism; the pull request is the only negotiation, and its outcome is a single ratified resolution |
