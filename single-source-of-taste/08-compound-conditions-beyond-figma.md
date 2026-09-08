# Compound Conditions Beyond Figma: A Code-Resident Resolver and Agentic Authoring for Non-Separable Judged Values

### Draft 0.1 — September 8, 2026

*Source: drafted September 8, 2026 in [this conversation](https://claude.ai/share/5bb9babf-7a7b-47ae-84be-878192d36f35) as a companion to the wiki pages; first committed here.*

*Fourth in the sequence with [Context as Coordinate System](./02-context-as-coordinate-system.md), [The Single Source of Taste](./06-the-single-source-of-taste.md), and [Carrying Judged Policies Through Figma](./07-carrying-judged-policies-through-figma.md). Picks up the residue the round-trip page named but did not resolve.*

> Note on scope. Figma claims here concern durable mechanisms (collections, per-collection modes, aliasing, scoping); exact per-plan mode limits should be verified against current Figma documentation. Claude Code claims are limited to documented capabilities — it runs in a repository, reads and writes files, runs shell commands, connects to MCP servers as a client, and can run headless for scheduled or CI use ([docs](https://code.claude.com/docs/en/mcp)). Anything beyond that is described at the level of capability, not asserted as a feature.

---

## Abstract

The round-trip page established that GitFig can make Figma a *faithful* projection of the Single Source of Taste, but it flagged a residue it could not carry: judged values that depend on an intersection of dimensions Figma's mode model cannot express. Those values live only in code, and a designer cannot edit them in-canvas. This page resolves the open question — does the residue shrink as Figma's mode model grows? — with a firm answer: **no, it is structural**, because a Figma variable is a one-dimensional lookup keyed to its own collection's modes, while compound conditions are N-dimensional, and closing the gap by materialising the product of modes is exactly the combinatorial explosion the coordinate model exists to reject. It then separates the residue into a *reducible* portion, removable by refactoring non-separable composition into separable composition, and an *irreducible* core that is precisely the locus of pure taste. For that core it proposes a practical solution — a **code-resident resolver** that owns compound resolution, paired with an **agentic authoring loop** — and analyses honestly where a Claude Code integration enables this and where it structurally cannot.

---

## Thesis

**A Figma variable resolves along one collection's mode axis; a compound condition resolves along several at once. This is a dimensionality mismatch, not a capacity shortfall, so the compound-condition residue cannot be represented in Figma at any plan tier without abandoning the compose-don't-enumerate principle. The practical resolution is to stop trying to put these values in Figma and instead let a code-resident resolver own them, using an agentic authoring loop to author, preview, and govern them — accepting that this relocates their authoring out of the design tool.**

Three implications:

1. The residue divides into a **reducible** part (removable by better token architecture) and an **irreducible** part (bespoke intersection values no composition predicts). Effort should be spent shrinking the first and governing the second, not representing either in Figma.
2. The coordinate model's claim that *the resolution algorithm is the core artifact* stops being theoretical here. A running resolver is the only home the irreducible residue has.
3. Claude Code can enable the authoring loop for code-resident judged values, but it cannot make Figma hold them, and it must never become their certifying authority.

---

## Argument 1: Separable versus Non-Separable Composition

Figma composes dimensions better than it is usually credited for, and understanding exactly how far that reaches locates the residue precisely.

A node can carry a mode per collection simultaneously — `Theme: dark` and `Density: dense` at once — and a variable in one collection can alias a variable in another. This means Figma natively handles **separable composition**: cases where the value is a function of independent per-dimension contributions.

```
separable:      value(d₁, d₂) = combine( f(d₁), g(d₂) )
```

A colour that varies by theme, used with spacing that varies by density, composes fine: each dimension resolves on its own axis and the two meet by reference. Layered collections plus aliasing cover this cleanly, and much of what looks like "compound" behaviour is in fact separable.

The residue is **non-separable composition**: cases where the intersection carries a bespoke value that is not any function of the independent per-dimension values.

```
non-separable:  value(d₁ ∧ d₂ ∧ d₃) = X,   where X ≠ combine( f(d₁), g(d₂), h(d₃) )
```

A worked aesthetic example: on a surface that is dark **and** dense **and** high-contrast, the primary action must become near-black with a hairline border — not the dark-mode navy, because at that specific compounding the navy both washes out against the darkened surround and crowds its dense neighbours. The correct value at the triple intersection is a judgment about *that combination*, not a blend of the three single-dimension values. A Figma variable cannot express this, because it has no slot keyed to a conjunction of modes across collections. The only way to force it is to create a collection whose modes are the cartesian product — `dark-dense-highContrast`, `dark-comfortable-standard`, and so on — which is the combinatorial explosion the [CSS-model argument](./01-token-systems-should-adopt-the-css-model.md) rejected.

---

## Argument 2: The Residue Is Structural, Not a Capacity Problem

The round-trip page left open whether raising Figma's mode limits would eventually dissolve the residue. It would not, for two compounding reasons.

**A variable is a one-dimensional function.** A variable maps *its own collection's mode* to a value. A compound condition is a function of *several dimensions at once*. Storing an N-argument function in a one-argument slot is a type mismatch, not a size problem. More modes does not add arguments; it only lengthens the single axis.

**Materialising the product defeats the model.** The only way to fake N-dimensional lookup with one-dimensional slots is to enumerate the product of modes as explicit combinations. That is precisely the pre-enumeration the coordinate model was built to avoid, and it is super-exponential: interacting dimensions multiply, and the resulting mode list is both un-authorable by hand and unmaintainable regardless of tier. Raising limits merely trades the projection gap for the explosion — swapping a representation Figma *can't* hold for one no human *should* author.

So capacity growth pushes the boundary outward and never removes it. The residue is bounded below by the structure of the mode model, not by a plan. This is the firm answer: **structural, not provisional.**

---

## Argument 3: Shrink Before You Route — Refactor Non-Separable into Separable

Before building machinery for the residue, shrink it. A large fraction of apparent non-separability is an artefact of missing intermediate structure and dissolves under refactoring.

Introduce an intermediate semantic layer that turns a conjunction into a chain of separable resolutions. The dark-and-high-contrast case often reduces if a `contrast-adjusted` alias layer is inserted: `color.action.primary` aliases a contrast-aware semantic, which itself resolves on the contrast axis, so theme and contrast each resolve on their own axis and compose by reference. What looked like a bespoke intersection becomes two separable steps — and separable steps are Figma-representable.

This refactoring is not a trick; it is good token architecture, and it should be exhausted first. But it does not reach zero. Some intersections resist decomposition because their value exists for reasons that are not a function of the parts — a designer's eye judges that *this specific combination* needs *this specific value*, and no intermediate layer reconstructs that judgment because the judgment is not compositional. The residue that survives refactoring is therefore not arbitrary leftover. It is disproportionately **pure taste**: the values no composition predicts and no criterion certifies. It is fitting, and not coincidental, that the hardest cases to tool are the ones most purely made of judgment.

---

## Argument 4: The Resolver Is the Home of the Residue

The coordinate model asserts that the resolution algorithm is the core artifact and that values are merely data the algorithm operates on. For the irreducible residue this stops being a stance and becomes the solution.

Build the resolver as a real runtime library implementing `resolve(policy, coordinate)` over the full coordinate space, compound predicates included. Then reframe where compound-condition values live: they do not live "in code as opposed to Figma"; they live **in the resolver**, which is the only place resolution actually happens. Figma's variables and the compiled CSS are both *projections consumed downstream* — neither is the seat of resolution. A compound-condition value never needed to enter Figma, because Figma was never the resolver; it is a view.

This inverts the framing of the residue helpfully. The problem is not "values Figma cannot hold." It is "values that belong to the resolver, and the resolver was never something Figma could be." The residue stops being a gap in the projection and becomes the native content of the core artifact. What remains is not representation but **authoring and governance**: how a human authors a value into the resolver, sees it, and has it ratified — without Figma as the surface.

---

## Argument 5: The Three Sub-Problems of a Code-Resident Judged Value

A compound-condition judged policy that lives in the resolver poses exactly three problems, and a solution must answer all three or it has not helped.

1. **Authoring.** The designer must write the bespoke intersection value together with its judged metadata — `authority`, `rationale`, `confidence`, a compound predicate, and a stable `dtcm.id`. Figma cannot hold it, so the authoring surface must be elsewhere.
2. **Preview.** The designer must *see* the value at its compound coordinate and judge whether it achieves the intent. This is the capability the round-trip page said Figma structurally lacks: Figma cannot apply a mode that does not exist, so it cannot render the intersection.
3. **Governance.** The edit must still flow through the adjudication pull request from the round-trip page, so a compound-condition change is owned, attributable, and revertible exactly like a representable one.

The rest of this page is how an agentic loop answers these three, and where it cannot.

---

## Argument 6: Claude Code as the Authoring Loop — What It Enables

Claude Code runs in the repository, reads and writes files, runs shell commands, connects to MCP servers, and can run headless for scheduled or CI use. Those documented capabilities map onto the three sub-problems directly.

**Authoring, from intent to conformant policy.** A designer describes the intersection in language — "when it's dark and dense and high-contrast, the primary action needs to be near-black with a hairline, because the navy washes out and crowds." Claude Code writes the compound-predicate rule into the token source in the `dtcm` schema, with `intent: aesthetic`, `regime: judged`, the compound predicate, a minted `dtcm.id`, and placeholders for `authority`, `rationale`, and `confidence` to be filled by a human. The designer authors in intent; the agent produces schema-conformant policy. This closes sub-problem 1 without Figma.

**Preview, via the resolver.** Claude Code can scaffold and run the resolver against the specific compound coordinate and render the result to a previewable artifact — a swatch, or the component snapshotted at that coordinate as HTML — so the designer sees the value Figma cannot show. This closes sub-problem 2, the one Figma structurally cannot.

**Governance, via the adjudication pull request.** Claude Code can commit to a branch and open the pull request that the round-trip page defined as the adjudication artifact, carrying `rationale`, `confidence`, author, and source `dtcm.id` in the body. Compound-condition edits become reviewable acts identical to representable ones. This closes sub-problem 3.

**Invariant enforcement in CI.** Run headless, Claude Code (or a plain CI check it writes) can guard the invariants the whole system depends on: every judged policy carries authority, rationale, and confidence; every compound-condition value still falls inside the admissible set that verifiable policies define, so accessibility continues to cap taste; no `dtcm.id` is orphaned; the resolver remains total.

An optional tightening: expose the resolver itself as an MCP server with tools like `resolve(coordinate)`, `preview(policy, coordinate)`, `list_unrepresented()`, and `draft_compound_policy(intent, predicate)`. Because Claude Code is an MCP client, the loop then becomes conversational — the designer talks, the agent calls the resolver's tools to draft, preview, and open the PR. This does not change what is possible; it makes the code-side loop pleasant.

---

## Argument 7: What Claude Code Cannot Do — The "Or Not"

An honest integration analysis has to state the limits as plainly as the capabilities, and here they are sharp.

**It cannot make Figma hold compound conditions.** The barrier is Figma's data model — a variable is a one-dimensional lookup — not a shortfall of tooling effort. Storing a value keyed to an N-dimensional conjunction in a one-dimensional slot is a type mismatch no agent can grind through; the only escape is materialising the product of modes, which is the explosion the model rejects. So there is no version of a Claude Code integration that syncs compound-condition values back into editable Figma variables. The agent routes around the projection gap; it does not close it.

**It cannot certify the value is good.** The irreducibility boundary from the Single Source of Taste holds unchanged. Claude Code can draft and preview a compound-condition value; it cannot ratify that the value achieves the aesthetic intent. Worse, a model fluent enough to propose a plausible "trustworthy near-black" can make a machine-originated value *look* authored — laundering machine aesthetics as taste. The guard must be explicit and structural:

> **Invariant.** No policy may carry a `regime: judged` value whose `authority` resolves to an automated agent. An agent may *draft* a judged value and its rationale; the `authority` must be a human party, and the adjudication pull request must be merged by that authority. The model's proposal is an input to judgment, never the judgment.

**Preview is a proxy, and proxies mislead at the edges.** The resolver-driven preview renders an approximation — HTML, a snapshot — that is not guaranteed faithful to the production runtime across every agent and environment, which is exactly the agent-relativity the coordinate model stresses. So the preview reduces, but does not eliminate, the "designer can't see it" problem. And it degrades precisely at compound coordinates, where intuition is weakest and fidelity matters most.

**Authoring has left the design tool.** This is the philosophical cost, and it should not be softened. The Single Source of Taste's appeal was that design owns taste in the tool design lives in. For the irreducible residue, Claude Code relocates authoring from direct manipulation in Figma to intent expressed in language, mediated by an agent, landing in code. Whether that still counts as design-owned is a real question. The most defensible reading is that it is a *new authoring modality* — intent-authoring — rather than a loss of authorship: the designer still originates and ratifies the judgment; they no longer touch the artifact. But it is candidly not Figma, and the residue is exactly where the design tool and the authored surface diverge.

---

## A Practical Protocol

**Authoring loop for a compound-condition judged value:**

```
1. Designer states the intersection intent in natural language.
2. Claude Code writes the compound-predicate policy into the token source:
     - dtcm.intent = aesthetic, dtcm.regime = judged
     - predicate = conjunction of dimension values
     - dtcm.id minted; authority/rationale/confidence left for the human
3. Claude Code runs the resolver at the compound coordinate and renders a preview.
4. Designer judges the preview:
     - accepts → fills authority + rationale + confidence (a human's)
     - rejects → restate intent; return to step 2
5. Claude Code opens the adjudication pull request (rationale, confidence, id, before/after).
6. The named authority reviews and merges (ratify) or declines (revert).
7. CI (headless) verifies invariants: metadata complete, value within admissible set,
   no orphaned id, resolver total.
```

**Guard executed on every pull request touching judged policies:**

```
validate_pr(pr):
  for policy in pr.changed_judged_policies:
    assert policy.authority is HumanParty        // never an agent
    assert policy.rationale ≠ ∅ and policy.confidence ∈ [0,1]
    assert admissible(resolve(policy, policy.predicate))   // accessibility caps taste
    assert policy.id resolves in the policy set  // no orphans
  assert resolver.is_total()
```

---

## Schema

**A compound-condition judged policy (lives in the resolver, never projected to Figma):**

```json
{
  "color.action.primary@dark∧dense∧high-contrast": {
    "$type": "color",
    "$value": "{gray.950}",
    "$extensions": {
      "dtcm.id": "pol_7c41…",
      "dtcm.intent": "aesthetic",
      "dtcm.regime": "judged",
      "dtcm.predicate": {
        "colorScheme": "dark",
        "density": "dense",
        "contrast": "high"
      },
      "dtcm.authority": "design-system-core",
      "dtcm.confidence": 0.8,
      "dtcm.rationale": "At this compounding the navy washes out and crowds; near-black with a hairline holds the action.",
      "dtcm.representable_in_figma": false,
      "dtcm.drafted_by": "claude-code",
      "dtcm.ratified_by": "@design-lead"
    }
  }
}
```

The `dtcm.drafted_by` and `dtcm.ratified_by` fields keep the invariant auditable: an agent may appear as drafter, never as authority or ratifier.

**The preview manifest Claude Code produces for the designer:**

```
policy:      pol_7c41…  (color.action.primary)
coordinate:  { colorScheme: dark, density: dense, contrast: high }
resolved:    {gray.950}  + border: hairline
admissible:  PASS (contrast vs surface ≥ AA)
preview:     ./previews/pol_7c41-dark-dense-highContrast.html
note:        not representable in Figma; adjudicate here
```

---

## Key Insights

**Insight 1: The residue is a dimensionality mismatch.** A variable is a one-argument function; a compound condition is many-argument. No plan tier reconciles that; only the resolver does. This is why the answer to "does it shrink as modes grow" is a firm no.

**Insight 2: Shrink, then route.** Most non-separability dissolves under an intermediate alias layer that turns a conjunction into separable steps. The irreducible remainder is disproportionately pure taste — no composition predicts it, no criterion certifies it — which is why it is both the hardest to tool and the last to leave human hands.

**Insight 3: The resolver was always the home.** Reframing compound values as belonging to the resolver rather than "stuck in code" turns a projection gap into native content of the core artifact. Figma is a view; it was never the seat of resolution.

**Insight 4: Claude Code enables the loop, not the representation.** It closes authoring, preview, and governance for code-resident judged values, and it can enforce invariants headless in CI. It cannot store an N-dimensional value in Figma's one-dimensional slot, and no effort changes that.

**Insight 5: The agent may draft but never certify.** The single most important invariant of the whole four-page program: a judged value's `authority` is a human party. An agent's proposal is an input to judgment. Let it draft; make the human ratify; keep the record.

**Insight 6: The residue is where the tool and the authored surface part ways.** For representable values, design authors in Figma. For the irreducible residue, design authors in intent, through an agent, into the resolver. That divergence is the honest cost of taste at its most compounded, and naming it is more useful than pretending the design tool reaches everywhere.

---

## Relationship to Prior Pages

- **[Context as Coordinate System](./02-context-as-coordinate-system.md)** said the resolution algorithm is the core artifact. This page cashes that in: the resolver is the only home for compound-condition values.
- **[The Single Source of Taste](./06-the-single-source-of-taste.md)** drew the irreducibility boundary. This page shows the compound residue sitting exactly on it, and adds the invariant that an agent may draft but not certify.
- **[Carrying Judged Policies Through Figma](./07-carrying-judged-policies-through-figma.md)** named the residue and the `unrepresented` flag. This page resolves what happens to it: shrink what is separable, and give the rest a code-resident, agent-assisted, human-ratified home.

---

## Conclusion

The compound-condition residue is the point at which "design owns taste in Figma" reaches its structural limit. A Figma variable resolves along one axis; a non-separable judged value resolves along several, and no growth in mode capacity reconciles the two — it only trades the projection gap for a combinatorial explosion the coordinate model was built to refuse. The practical response is not to represent these values but to relocate them: shrink the residue by refactoring separable composition into aliased chains, and give the irreducible remainder a home in the resolver, where resolution actually happens. A Claude Code integration can make that home habitable — authoring bespoke intersection values from intent, previewing them through the resolver, and routing them through the adjudication pull request, with invariants enforced headless in CI. What it cannot do is put those values back into editable Figma variables, or become the authority that certifies them good. So the residue leaves design with a changed but not diminished role: for the values most purely made of judgment, the designer authors in intent and ratifies in review, while the tool they draw in stops at the edge of what a one-dimensional slot can hold. That edge is not a defect to be tooled away. It is the visible boundary of taste itself.

---

## Glossary

**Admissible Set** (from the Single Source of Taste) The values permitted at a coordinate by verifiable policies. A compound-condition judged value must fall inside it; accessibility continues to cap taste even where Figma cannot represent the value.

**Agentic Authoring Loop** The cycle by which a designer states an intersection intent, an agent (Claude Code) drafts a conformant compound policy and renders a preview via the resolver, the designer ratifies with human authority and confidence, and the change is opened as an adjudication pull request.

**Code-Resident Resolver** A runtime library implementing `resolve(policy, coordinate)` over the full coordinate space, compound predicates included. The home of the irreducible residue and the operational form of the coordinate model's "resolution algorithm as core artifact."

**Compound Condition** A resolution rule keyed to a conjunction of dimension values (`dark ∧ dense ∧ high-contrast`). Representable in Figma only by materialising the product of modes, which the coordinate model rejects.

**Dimensionality Mismatch** The core reason the residue is structural: a Figma variable is a one-argument function of its collection's mode, while a compound condition is a many-argument function. Capacity growth lengthens the one axis; it never adds arguments.

**Draft-not-Certify Invariant** The rule that an automated agent may draft a judged value and its rationale but may never be its `authority`. The `authority` must resolve to a human party, and the adjudication pull request must be merged by that party.

**Irreducible Residue** The compound-condition judged values that survive refactoring because their value is not a function of the parts. Disproportionately pure taste — unpredicted by composition, uncertified by criterion.

**Intent-Authoring** The authoring modality in which a designer originates a judged value by expressing intent in language to an agent, rather than by direct manipulation in the design tool. The changed-but-not-lost form of authorship for the residue.

**Non-Separable Composition** A composition in which the intersection carries a bespoke value not equal to any combination of independent per-dimension values. The source of the residue. Contrast with separable composition.

**Reducible Residue** The portion of apparent non-separability removable by inserting an intermediate alias layer that turns a conjunction into separable, Figma-representable steps. Should be exhausted before building resolver machinery.

**Separable Composition** A composition in which the value is a function of independent per-dimension contributions, `value(d₁,d₂) = combine(f(d₁), g(d₂))`. Handled natively by Figma via layered collections and aliasing.

---

## References

- Figma Variables, modes, aliasing, and scoping — verify current capabilities and per-plan mode limits at Figma's official documentation.
- Claude Code capabilities (repository operation, file editing, shell commands, MCP client, headless mode): https://code.claude.com/docs/en/mcp and https://docs.claude.com/en/docs/claude-code/overview
- Model Context Protocol — for exposing the resolver as a tool server to an MCP client.
- W3C Design Tokens Community Group, Format Module — the `$extensions` mechanism used for `dtcm` metadata.
