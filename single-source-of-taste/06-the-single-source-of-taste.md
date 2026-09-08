# The Single Source of Taste: Aesthetic-Intent Policies in the Context Coordinate Model

### Draft 0.1 — September 8, 2026

*Source: drafted September 8, 2026 in [this conversation](https://claude.ai/share/5bb9babf-7a7b-47ae-84be-878192d36f35) as a companion to the wiki pages; first committed here.*

*Companion to [Context as Coordinate System](./02-context-as-coordinate-system.md) and the report [Design Systems: Single Source of Truth](https://github.com/ds1/design-token-context-model/blob/main/archive/Design%20Systems_%20Single%20Source%20of%20Truth.md). Assumes the reader accepts the thesis that tokens are resolution policies, not values.*

---

## Abstract

The Context Coordinate Model reframes tokens as **resolution policies** — functions mapping context coordinates to values — and argues that resolution becomes *testable*: a policy is correct when its resolved value achieves the policy's behavioral intent. That argument is sound, but its worked examples are exclusively **functional**: WCAG contrast ratios, vestibular safety, legibility thresholds. Each has a decision procedure. This document observes that a second, equally large class of policies has the *same policy form* but *no decision procedure* — policies whose intent is perceptual and evaluative rather than functional. This class is what practitioners call **taste**. The model already holds these policies; it simply cannot validate them. This document names the class **aesthetic intent**, introduces the **validation regime** as first-class policy metadata, and defines the **Single Source of Taste** as the design-owned authority that authors and adjudicates aesthetic-intent policies. The result is a governance layer that mirrors the Single Source of Truth in form while acknowledging where mechanical validation ends and authored judgment begins.

---

## Thesis

**Every resolution policy carries a behavioral intent, and behavioral intent bifurcates by validation regime. Functional intent is *verifiable* — machine-checkable against an external criterion. Aesthetic intent is *judged* — resolvable only by a designated authority. The Single Source of Taste is the principle that aesthetic-intent policies have exactly one canonical authoring authority, the way the Single Source of Truth holds that data has exactly one canonical store.**

This reframing has three implications:

1. The distinction between *truth* and *taste* is not a distinction of kind. Both are resolution policies. They differ only in the **nature of their success criterion**.
2. The resolution algorithm can enforce **conformance** to an aesthetic policy but can never certify that the policy is **good**. Design authority is therefore irreducible, not a temporary gap awaiting better tooling.
3. Taste is not a coordinate in the context space. It is an **authority over policies** — closer to a cascade origin than to a mode value.

---

## Argument 1: Behavioral Intent Bifurcates

The coordinate model establishes that tokens encode behavioral intent, not appearance: `color.error` intends to communicate error, and the specific red varies by context to preserve that intent. The model then claims resolution is testable — does `contrast.high` reach AAA, does `motion.reduced` remove triggers.

Observe what every one of those examples shares: an **external, agent-independent criterion** against which the resolved value can be checked without consulting a human. Contrast ratio is arithmetic. Vestibular safety is a documented threshold. Legibility has measurable floors.

Now consider policies of identical form whose intent admits no such check:

- `color.action.primary` intends to read as *confident and trustworthy*
- `spacing.section` intends to feel *composed rather than crowded*
- `elevation.card` intends to sit *quietly beneath* its neighbours, not compete with them

These are behavioral-intent policies in exactly the model's sense. They map coordinates to values in pursuit of a perceptual outcome. But no arithmetic certifies "trustworthy," and no threshold certifies "composed." The intent space is therefore not homogeneous. It bifurcates:

- **Functional intent** — the resolved value is correct when it satisfies an external criterion.
- **Aesthetic intent** — the resolved value is correct when a designated authority judges it to achieve the intent.

The model's testability argument silently covers only the first half. This document covers the second.

---

## Argument 2: Aesthetic Intent Shares Form but Not Validation Regime

Because both classes are policies, they share the entire resolution apparatus: predicates over coordinates, specificity, precedence, inheritance, volatility staging. Nothing about aesthetic intent requires a separate resolution engine. What differs is the **validation regime** — the procedure by which a resolved value is judged correct.

We make the regime explicit as first-class policy metadata:

- **`verifiable`** — a machine-checkable criterion exists; the authority is a *specification* (`wcag:AA:contrast>=4.5`). Correctness is decided at resolution time without human involvement.
- **`judged`** — no machine-checkable criterion exists; the authority is a *party* (`design-system-core`). Correctness is decided by that party, in advance (by authoring the policy) and on appeal (by adjudicating exceptions).

This is the minimal addition the model needs to accommodate taste without distorting it. A policy does not become un-systematic because it is `judged`. It remains authored, versioned, inherited, and precedence-ordered. It simply names a *who* instead of a *what* as its certifier.

---

## Argument 3: Taste is a Resolution Authority, Not a Coordinate

The model already resists treating named contexts as mere shortcuts, arguing they are subspace policies carrying overlays and constraints. Taste requires the analogous move, one level up.

Taste is frequently misplaced as a coordinate value — as if "on-brand" were a mode of some `brand` dimension. It is not. A coordinate describes *the conditions under which resolution happens*. Taste describes *who is entitled to author and adjudicate the aesthetic policies that resolve under those conditions*. In CSS terms, taste is not a selector or a media query; it is closest to **cascade origin** — the authority stratum from which a rule derives its standing.

The **Single Source of Taste** is therefore the claim that, for any given aesthetic-intent policy, exactly one authority is canonical. Divergent aesthetic judgments elsewhere in the system are not competing truths; they are unresolved appeals to that authority. This is the precise mirror of the Single Source of Truth: not "one value everywhere," but "one place entitled to say."

---

## Argument 4: The Judged Regime Extends the Resolution Signature

The model's uncertainty argument enriches resolution to return confidence and a volatility horizon:

```
resolve(token, coordinate) → {
  value: T,
  confidence: 0..1,
  volatility_horizon: Duration
}
```

Aesthetic intent extends this signature along two axes:

```
resolve(token, coordinate) → {
  value: T,
  confidence: 0..1,              // for judged policies: authorial conviction, not statistical certainty
  volatility_horizon: Duration,
  regime: verifiable | judged,
  authority: CriterionRef | AuthorityRef,
  escalation: Policy | ⊥         // where a low-conviction judged resolution is sent
}
```

Two properties deserve emphasis:

**Confidence changes meaning under the judged regime.** For a verifiable policy, confidence is the probability the resolution is correct. For a judged policy, confidence is the **authorial conviction** behind the aesthetic call — how settled the design authority considers this decision. A `0.6` on a `verifiable` policy means "probably passes." A `0.6` on a `judged` policy means "provisional; the authority has not committed."

**Low confidence escalates; it does not degrade.** The model prescribes graceful degradation — when confidence is low, prefer conservative values. That is correct for functional intent: a safe fallback still satisfies the criterion. It is *wrong* for aesthetic intent, because there is no conservative value that is guaranteed tasteful. A low-conviction aesthetic resolution must instead **escalate to its authority**, not fall back to a default. Validation splits accordingly:

```
validate(resolution, coordinate):
  if resolution.regime = verifiable:
    return resolution.authority.check(resolution.value, coordinate)   // arithmetic, at resolve time
  if resolution.regime = judged:
    if resolution.confidence ≥ threshold: return ACCEPT
    else: return ESCALATE(resolution.authority)                       // to the terminal resolver
```

---

## Argument 5: Aesthetic Rightness is Agent-Relative

The model's affordance argument holds that resolution is agent-relative: the right value is "the right value for this agent in this environment." Aesthetic rightness inherits this property, and the inheritance is load-bearing, because it reclaims territory often assumed to be un-encodable.

The classic objections to tokenizing taste are relational: the same hex reads darker on cream than on white; identical padding feels wrong in a dense table and right in a hero; a card's elevation must answer to its neighbours, not an absolute. These are routinely cited as proof that taste resists systematization.

Under agent-relative resolution, much of this is simply **more dimensions**. Perceived colour depends on `colorGamut`, `ambientLight`, and adjacent-surface luminance — all expressible as agent and environment dimensions. Relational elevation depends on `elevation-relative-to-neighbours` — a container-context dimension. The optical and relational layer of taste is therefore *largely encodable*, and the coordinate model is the right instrument for encoding it.

What remains after every such dimension is admitted is a **residue**: whether the fully-resolved, context-correct output is *good*. That residue is not a missing dimension. It is the judged intent itself. Argument 5 thus sets the boundary precisely: agent-relativity expands the encodable fraction of taste further than intuition suggests, and the judged residue is exactly what is left when expansion is exhausted.

---

## Argument 6: Taste Systematizes Through Promotion, Not Automation

The ontology's design philosophy already includes *learnable friction*: exceptions are logged and promoted to tokens when patterns emerge. Aesthetic intent is where this mechanism does its most important work, because it is how taste becomes systematic **without** becoming automated.

The lifecycle of an aesthetic decision:

1. **Override** — a designer resolves an aesthetic policy locally against a specific coordinate, logging a rationale and an authorial confidence.
2. **Signal** — recurrence of similar overrides across coordinates is detected. The friction is now a pattern.
3. **Promotion** — the authority reviews the pattern and, if it holds, authors a new aesthetic policy (or a new context type) capturing it. The ad-hoc judgment becomes canonical.
4. **Precedent** — the rationale is retained as interpretive context for future appeals, exactly as case law is retained.

Automation would attempt step 3 mechanically. The Single Source of Taste forbids this: promotion is an act of the authority, informed by the signal but not determined by it. The system *surfaces* the pattern; the designer *decides* whether it is taste. This preserves the distinction the whole document rests on — the engine learns where judgment recurs, but never usurps the judging.

---

## Argument 7: The Irreducibility Boundary

The coordinate model's ambition is to make resolution a formally specified, testable, portable artifact. Aesthetic intent marks the exact limit of that ambition, and the limit is principled rather than provisional.

For a `verifiable` policy, the resolution algorithm and its criterion together certify correctness end to end. For a `judged` policy, the algorithm can certify only **conformance** — that the resolved value obeys the authored aesthetic policy. It cannot certify **adequacy** — that the policy is any good. No enrichment of the algorithm closes this gap, because the gap is definitional: a `judged` policy is precisely one whose success has no machine-checkable criterion. If a criterion were found, the policy would reclassify as `verifiable`.

Therefore:

> No resolution algorithm eliminates the design authority. It can enforce taste once authored; it cannot originate or ratify taste. The terminal resolver of every aesthetic-intent policy is a party, not a procedure.

This is the boundary of the program, stated honestly. It is also the formal seat of the designer's role — and the reason that role *grows* rather than shrinks as building is democratized: when values and implementations become cheap to generate, the authored, judged policy layer becomes the scarce and defensible artifact.

---

## Placement in Resolution Precedence

Aesthetic intent is **not** a new row in the precedence order. It is orthogonal to coordinate specificity: a property of the policy, not of its predicate. Its interaction with the existing order is a **constraint relationship**, not a ranking:

- **Verifiable policies define the admissible set.** Accessibility overrides, at the top of the existing precedence order, bound which values are permitted at a given coordinate.
- **Judged policies choose within it.** Aesthetic intent resolves freely inside the feasible region that verifiable policies leave open.

Stated as a governance principle:

> Taste is sovereign over the aesthetic, and subordinate to access. It selects within the box that functional policies draw; it may not select outside it.

When an aesthetic resolution would violate a verifiable criterion (a "trustworthy" navy that fails AA on the required surface), the verifiable policy wins and the aesthetic policy must re-resolve within the admissible set — escalating to its authority if no admissible value satisfies the intent.

---

## Schema

Aesthetic and functional policies are distinguished by namespaced `$extensions`, leaving the W3C DTCG core untouched.

A **judged** (aesthetic) policy:

```json
{
  "color.action.primary": {
    "$type": "color",
    "$value": "{blue.500}",
    "$extensions": {
      "dtcm.intent": "aesthetic",
      "dtcm.regime": "judged",
      "dtcm.authority": "design-system-core",
      "dtcm.confidence": 0.9,
      "dtcm.rationale": "Primary action must read as trustworthy on financial surfaces",
      "dtcm.escalation": "design-system-core/review",
      "mode": {
        "light": "{blue.500}",
        "dark": "{blue.300}",
        "dim": "{blue.400}"
      }
    }
  }
}
```

A **verifiable** (functional) policy, for contrast:

```json
{
  "color.text.body": {
    "$type": "color",
    "$value": "{gray.900}",
    "$extensions": {
      "dtcm.intent": "functional",
      "dtcm.regime": "verifiable",
      "dtcm.authority": "wcag:AA:contrast>=4.5",
      "mode": {
        "light": "{gray.900}",
        "dark": "{gray.100}",
        "dim": "{gray.200}"
      }
    }
  }
}
```

The `dtcm.authority` field carries a **specification reference** under the verifiable regime and an **authority reference** under the judged regime. This single field is what encodes the truth/taste distinction at the data layer.

---

## Key Insights

**Insight 1: Truth and taste are one construct, split by regime.** The model need not treat them as separate systems. They share resolution end to end and diverge only at validation. This is why a Single Source of Taste can mirror the Single Source of Truth so exactly — they are the same machine with two certifiers.

**Insight 2: The `authority` field is the whole distinction.** A specification reference means verifiable; a party reference means judged. Everything else — confidence semantics, escalation-vs-degradation, promotion governance — follows from that one field.

**Insight 3: Agent-relativity is a land grab in taste's favour.** Much of what is dismissed as un-encodable aesthetic nuance is encodable as agent and environment dimensions. The coordinate model *expands* the codifiable fraction of taste; the residue it leaves is the judged intent itself, cleanly isolated.

**Insight 4: The Figma projection holds the shadow, not the source.** Because Figma's flat mode collections are a lossy projection of the policy space, and because aesthetic intent lives in policy metadata (`regime`, `authority`, `rationale`) that the projection discards, a Figma-and-code sync keeps the *rendered values* of taste true while dropping the *authority and reasoning* that make them taste. The canonical Single Source of Taste is the policy set, of which Figma is one view.

**Insight 5: Democratization inverts the value of the layers.** As generation makes values and implementations cheap, the authored judged-policy layer becomes the durable artifact. Authoring aesthetic policies is authoring taste; the authority that does so is the design function, now sitting at the scarce point in the pipeline.

---

## Relationship to the Single Source of Truth

| | Single Source of Truth | Single Source of Taste |
|---|---|---|
| Canonical object | Data / code | Aesthetic-intent policy set |
| Certifier | Tests, types, criteria | A design authority |
| Regime | `verifiable` | `judged` |
| Violation | Bug, regression | Incoherence, wrongness |
| On low confidence | Degrade to safe value | Escalate to authority |
| Divergence is | Debt to reconcile | Appeal to adjudicate |
| Enforced by | Resolution + criterion | Resolution + authored policy |
| Cannot be | Made incorrect silently | Certified good by machine |

The two are complementary strata of one policy layer. Verifiable policies draw the admissible region; judged policies choose within it. A mature system runs both through the same resolver and distinguishes them only by `dtcm.authority`.

---

## Conclusion

The Context Coordinate Model made resolution the core artifact and intent its object. In doing so it quietly assumed intent was verifiable, because its examples were. Lifting that assumption reveals a second class of policy the model already holds but cannot validate — aesthetic intent — and shows that the difference between engineering's truth and design's taste is not a difference of kind but of **validation regime**. Truth is a policy certified by a criterion; taste is a policy certified by an authority. Naming that authority, and requiring it to be singular, yields the **Single Source of Taste**: a governance layer isomorphic to the Single Source of Truth, running on the same resolution engine, distinguished by a single field.

The engine can then do everything for taste that it does for truth — author, version, inherit, order by precedence, resolve against coordinates, learn from recurring friction — except the one thing it must not do: decide that a resolved aesthetic is good. That decision is the irreducible seat of design, and it is exactly the capacity that rises in value as the production of working software approaches free.

---

## Glossary

**Admissible Set (Feasible Region)** The set of values permitted at a given coordinate by all applicable verifiable policies. Judged policies resolve within this set. See *Placement in Resolution Precedence*.

**Aesthetic Intent** A behavioral intent whose success has no machine-checkable criterion and is decided by a designated authority. Examples: reading as trustworthy, feeling composed, sitting quietly beneath neighbouring surfaces. Contrast with *functional intent*.

**Authorial Confidence** Under the judged regime, the `confidence` field denotes the design authority's conviction in an aesthetic call — how settled the decision is — not a statistical probability of correctness. Low authorial confidence triggers escalation, not degradation.

**Authority (of a policy)** The certifier of a policy's correctness. A *specification reference* (e.g., `wcag:AA:contrast>=4.5`) under the verifiable regime; an *authority reference* (e.g., `design-system-core`) under the judged regime. Carried in `dtcm.authority`. This field encodes the truth/taste distinction at the data layer.

**Behavioral Intent** (extends the term from *Context as Coordinate System*) What a policy aims to achieve perceptually or functionally, independent of value. In this document, behavioral intent **bifurcates** by validation regime into functional and aesthetic intent.

**Escalation** The routing of a low-confidence judged resolution to its authority for decision, in place of the graceful degradation used for verifiable policies. There is no conservative value guaranteed to be tasteful, so aesthetic uncertainty cannot be resolved by fallback.

**Functional Intent** A behavioral intent whose success is checkable against an external, agent-independent criterion (contrast ratio, vestibular threshold, legibility floor). Resolved under the verifiable regime.

**Irreducibility (of design authority)** The property that no resolution algorithm can originate or ratify taste. The algorithm enforces conformance to an authored aesthetic policy but cannot certify the policy's adequacy, because a judged policy is by definition one with no machine-checkable criterion. The terminal resolver is always a party, not a procedure.

**Judged Regime** The validation regime in which correctness is decided by a designated authority rather than a criterion. Applies to aesthetic-intent policies.

**Promotion** The elevation of a recurring aesthetic override into a canonical aesthetic policy (or context type) by the authority. Distinct from automation: the system surfaces the pattern; the authority decides whether it is taste.

**Single Source of Taste** The principle that every aesthetic-intent policy has exactly one canonical authoring and adjudicating authority. The mirror of the Single Source of Truth: not one value everywhere, but one place entitled to say. Divergent aesthetic judgments are appeals to that authority, not competing truths.

**Taste** The design-owned capacity to author and adjudicate aesthetic-intent policies. Not a coordinate value but an authority over policies — analogous to a cascade origin, not a selector or media query.

**Terminal Resolver** The authority that makes the final decision on a judged policy. Contrast with the resolution algorithm, which is the terminal resolver only for verifiable policies.

**Validation Regime** First-class policy metadata (`dtcm.regime`) recording how a policy's resolved value is judged correct: `verifiable` (by criterion) or `judged` (by authority). The minimal addition required to accommodate taste without a separate resolution engine.

**Verifiable Regime** The validation regime in which correctness is decided at resolution time by a machine-checkable criterion, with no human involvement. Applies to functional-intent policies.
