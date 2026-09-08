# Carrying Judged Policies Through Figma: A Round-Trip Representation for the Single Source of Taste

### Draft 0.1 — September 8, 2026

*Source: drafted September 8, 2026 in [this conversation](https://claude.ai/share/5bb9babf-7a7b-47ae-84be-878192d36f35) as a companion to the wiki pages; first committed here.*

*Companion to [The Single Source of Taste](./06-the-single-source-of-taste.md) and [Context as Coordinate System](./02-context-as-coordinate-system.md). Assumes the reader accepts that tokens are resolution policies, that those policies carry a validation regime (`verifiable` | `judged`), and that judged policies carry `authority`, `confidence`, and `rationale`.*

> Note on scope. This page reasons about durable Figma and GitFig mechanisms: variable collections, modes, aliasing, scoping, per-variable descriptions and code-syntax slots, namespaced plugin data, and GitFig's pull / push / branch / pull-request model. It deliberately avoids committing to volatile specifics — exact per-plan mode limits and the current set of variable-able properties — which should be verified against current Figma documentation before implementation.

---

## Abstract

The Single Source of Taste locates taste in a policy layer: aesthetic-intent policies certified by a design authority and annotated with `authority`, `confidence`, and `rationale`. GitFig keeps Figma and GitHub in sync, which raises the obvious hope that Figma could be the design-owned home of that layer. The coordinate model already warns that Figma's flat mode collections are a **projection** of the policy space — lossy in structure. This page shows the loss is worse than structural: Figma also has no native, structured, designer-visible home for judged-policy **metadata**, so a naive round-trip preserves the *rendered value* of a taste decision while silently discarding the *authority and reasoning* that made it taste. The page distinguishes the two losses, shows why the **push** direction is where taste leaks, inventories Figma's candidate carriers and proves none suffices alone, and specifies a round-trip that is **faithful** — value and metadata preserved and re-adjudicated — even though it can never be **lossless-and-native**. The keystone is turning GitFig's existing "pull request from Figma" into the adjudication artifact the Single Source of Taste already requires.

---

## Thesis

**A lossless-and-native round-trip of judged policies through Figma is impossible, because Figma projects both the *structure* of a policy (compound conditions) and its *annotation* (regime, authority, rationale) out of existence. A *faithful* round-trip is achievable, but only if GitFig treats Figma as a rendered, annotated projection with three additions: stable policy identity, a plugin-resident metadata surface, and pull-request-as-adjudication for edits to judged values.**

Three implications:

1. GitFig's bidirectionality must be **asymmetric** for judged policies. Pull *projects and annotates*; push *reattaches and adjudicates*. The two directions are not inverses.
2. The system of record splits. The **value** may be edited in Figma; the **metadata** never originates there. Metadata's record is plugin data plus the pull request, not the variable.
3. Taste's provenance is only ever as portable as GitFig itself. Any consumer reading the Figma file without the plugin sees the value and not the authority. This is the hard ceiling.

---

## Argument 1: Two Gaps, Not One

"Figma loses information" is usually said once, structurally. For taste it must be said twice, because there are two independent losses with different fixes.

**The projection gap (structural).** Figma expresses context as modes within a collection: one value per mode. It has no mechanism for compound conditions, specificity, or precedence. A judged policy whose value depends on *dark AND dense AND high-contrast* cannot be represented as a variable value unless that exact intersection happens to exist as a defined mode across the relevant collections — which projection generally cannot guarantee. This is the coordinate model's existing point, sharpened: the *value itself* may be unrepresentable, not merely its rules.

**The annotation gap (metadata).** Even for a value that fits a single mode cleanly, the judged-policy metadata — `regime`, `authority`, `confidence`, `rationale`, `escalation` — has no native, structured, designer-visible field in Figma. There is nowhere in the variable UI that means "this value is a judgment, owned by design-system-core, held with 0.9 conviction, because primary actions must read as trustworthy."

The projection gap is about **what Figma can represent**. The annotation gap is about **what Figma can carry alongside what it represents**. Carrying judged policies through the round-trip means solving both, and they do not share a solution.

---

## Argument 2: The Round-Trip Is Asymmetric — Push Is Where Taste Leaks

Pull (GitHub → Figma) is the safe direction. The full policy set is the source; generating Figma is projection plus annotation. Information is *reduced* on the way in, but the source remains intact in Git, so nothing is lost that was not already redundant.

Push (Figma → GitHub) is where taste is destroyed by naivety. A designer changes a colour in Figma. On push, a naive GitFig writes the new value to the token file. But Figma had no field for *why* or *by whose authority*, so the push either overwrites `rationale`/`confidence` with nothing, or leaves stale metadata attached to a value it no longer describes. Either way the value survives and the taste evaporates. The decision that a change *is a taste decision* — the thing the Single Source of Taste exists to govern — is exactly what a value-only push cannot capture.

The Single Source of Taste already prescribes the correct behaviour for this moment, in a different guise. Its spec says a low-conviction judged resolution must **escalate to its authority**, not degrade to a default. An *edit to a judged value* is that moment: a designer is asserting a new aesthetic resolution. Push must therefore treat such an edit as an appeal to be adjudicated, not a value to be written. That is the seam this page threads.

---

## Argument 3: Figma Offers Three Carriers, and None Suffices Alone

Where could judged metadata live inside a Figma file? There are three candidate carriers, plus the absent fourth everyone wishes for.

| Carrier | Structured | Survives round-trip | Visible to designer | Verdict for judged metadata |
| --- | --- | --- | --- | --- |
| Variable **description** (free text) | No — one string | Yes | Yes, in the variable panel | Partial: good home for a *rendered badge*, wrong as a *source* |
| **Code-syntax** slots (per-platform strings) | Semi — fixed slots | Yes | Yes, in Dev Mode | Poor: semantically the wrong field; abuse invites confusion |
| **Name / mode-name** convention | No | Yes | Yes | Poor: pollutes the namespace, brittle, collides with tooling |
| **Shared plugin data** (namespaced key–value) | Yes | Yes, if the plugin maintains it | **No** — hidden from native UI | Strong for fidelity; invisible without the plugin |
| A native "policy metadata" field | — | — | — | Does not exist |

The table yields the design. No single carrier is both **structured** and **visible**. Shared plugin data is structured and round-trip-safe but invisible; the description is visible but unstructured and singular. A faithful representation must therefore **combine** them and assign them distinct jobs:

- **Shared plugin data is the record.** It holds the full `dtcm` metadata block, structured and complete.
- **The description is the view.** GitFig renders a compact, human-readable badge into it on pull, so the authority and conviction are legible in-canvas.

One is authoritative and hidden; the other is derived and seen. They must never be confused, and the derived one must never be read back as source.

---

## Argument 4: Stable Policy Identity Is the Precondition for Push

Push cannot reattach a Figma edit to its source policy by name-matching, because projection breaks the name-to-policy bijection: one policy may fan out into several variable-mode cells, and names can be edited in-canvas. Reattachment requires a **stable identity** that survives projection and renaming.

We introduce `dtcm.id` — an opaque, stable identifier minted when a policy is first authored and written into shared plugin data on every variable-mode cell the policy projects into. Push then maps a changed cell back to its origin by `id`, never by name. The consequences are strict and deliberate:

- An edited cell whose `id` resolves to a policy is a **reattachment**: merge value, preserve metadata.
- An edited cell whose `id` resolves to nothing is an **orphan**: flag it; never silently create a new policy from a Figma edit. Silent creation is how shadow taste enters a system.

Identity is what makes push a controlled reattachment rather than a guess.

---

## Argument 5: Judged Metadata Needs a Plugin-Resident Surface

Because Figma has no native field for judged metadata (Argument 3) and edits to judged values must carry conviction (Argument 2), GitFig cannot merely *read* Figma — it must *offer a surface* where the designer supplies and edits the metadata. This is a plugin panel, not a Figma feature, and it is non-optional: without it, the metadata is either uneditable (frozen at whatever Git held) or smuggled through the wrong field.

The surface has two duties:

1. **Standing edit.** Let the authority view and revise `authority`, `confidence`, `rationale`, and `escalation` for a selected variable, writing to shared plugin data.
2. **Edit-time prompt.** When a designer changes a judged value and pushes, require confirmation or supply of `rationale` and `confidence` for *this* change, before a pull request may open.

The panel is where the annotation gap is actually closed. Figma provides the canvas and the value; GitFig provides the judgment surface.

---

## Argument 6: The Pull Request Is the Adjudication Artifact — Keystone

GitFig already creates pull requests from Figma for team review. The Single Source of Taste already demands that judged edits escalate to an authority. These are the same mechanism, and recognising that is the design's keystone.

On push of a changed **judged** value, GitFig opens a pull request whose body carries the edit-time `rationale` and `confidence`, the acting author, the source `dtcm.id`, and a before/after of the value. The pull request *is* the appeal. Merging it *is* the adjudication. Declining it returns the policy to its prior resolution. The Git history of the token file thereby becomes the case law the Single Source of Taste's promotion mechanism relies on — every taste change is a reviewable, attributable, revertible act, exactly as every truth change already is.

This aligns the two regimes at the level of workflow, not just data:

- A changed **verifiable** value pushes normally; its criterion re-checks in CI. No adjudication needed — the machine decides.
- A changed **judged** value pushes as a pull request; its authority decides. The human decides, on the record.

The distinction the Single Source of Taste draws in the schema (`dtcm.authority` is a spec vs a party) becomes the distinction GitFig draws in the pipeline (auto-merge on green vs review-by-authority).

---

## Argument 7: Taste Must Not Be Invisible

A representation that carried metadata only in hidden plugin data would preserve taste for the *tool* while hiding it from the *designer*, which defeats the design-owned premise. Visibility is a first-class requirement, met by rendering — never by relocating the source.

On pull, GitFig renders a badge into each judged variable's description, for example:

```
◈ judged · authority: design-system-core · conviction: 0.9
  "Primary action must read as trustworthy on financial surfaces"
  ⚠ resolves further in code (dark+dense+high-contrast) — edit with care
```

The badge is derived, read-only-by-convention, and regenerated on every pull. It gives the designer three things at a glance: that the value is a judgment rather than a fact, who owns it, and whether it is fully represented in Figma or resolves further in code. The last line is the projection gap made visible, so a designer never unknowingly edits a value whose real resolution Figma cannot show.

---

## Argument 8: The Residue — What Still Cannot Round-Trip

Honesty about the ceiling. Three things remain irreducibly lossy, and they bound the claim to *faithful* rather than *lossless-and-native*.

**Compound-condition values.** A judged value depending on an intersection Figma cannot express as a mode lives only in code. Figma shows the base value and a warning badge; the designer cannot edit the compound case in-canvas. Faithfulness here means *flagging* the unrepresentable, not representing it.

**Metadata invisibility to native consumers.** Any reader of the Figma file without GitFig — native Dev Mode handoff, other plugins, direct API access — sees the value but not the regime, authority, or rationale, because those live in namespaced plugin data. Taste's provenance is carried, not native. It is therefore only as portable as GitFig, and it does not survive tools that do not know to look. This is the deep limit and no amount of plugin cleverness removes it; only a native Figma metadata field would.

**Plan-gated representational capacity.** How much of the projection can be represented at all is bounded by how many modes a collection may hold, which is plan-dependent. On lower tiers, more of the policy space collapses into code-only residue. (Verify current limits against Figma documentation.)

The takeaway: GitFig can make Figma a *faithful* projection of the Single Source of Taste — every value round-trips or is flagged, every judged edit is adjudicated, every judgment is legible in-canvas — but it cannot make Figma the *native* home of taste. The canonical source remains the policy set in Git; Figma is its best-annotated view.

---

## The Round-Trip Protocol

**Pull — GitHub → Figma (project and annotate):**

```
pull(policySet):
  for policy in policySet:
    projection ← project(policy)                      // flatten to Figma-expressible modes
    variable   ← upsertVariable(policy.name, collectionFor(policy.dimension))
    for (mode, value) in projection.representable:
      setModeValue(variable, mode, value)
    writeSharedPluginData(variable, "dtcm", {
      id, intent, regime, authority, confidence, rationale, escalation,
      unrepresented: projection.dropped               // compound cases Figma can't hold
    })
    if policy.regime = judged:
      variable.description ← renderBadge(authority, confidence, rationale, projection.dropped)
```

**Push — Figma → GitHub (reattach and adjudicate):**

```
push(changedVariables):
  for variable in changedVariables:
    meta   ← readSharedPluginData(variable, "dtcm")
    policy ← policySet.lookupById(meta.id)            // identity, never name
    if policy = ⊥:
      flag("orphaned edit — no source policy"); continue   // never silently create

    newValue ← currentModeValue(variable)
    if newValue = policy.value: continue              // no change

    if policy.regime = verifiable:
      writeValue(policy, newValue)                    // criterion re-checks in CI
    else:                                             // judged: escalation-on-edit
      (rationale, confidence) ← requirePluginPrompt()
      openPullRequest({
        id: policy.id, before: policy.value, after: newValue,
        rationale, confidence, author, authority: policy.authority
      })                                              // the PR is the adjudication
    // metadata is NEVER written back from the variable; its record is plugin data + PR
```

---

## Schema

**Shared plugin data on a variable (the record):**

```json
{
  "namespace": "dtcm",
  "data": {
    "id": "pol_9f2a…",
    "intent": "aesthetic",
    "regime": "judged",
    "authority": "design-system-core",
    "confidence": 0.9,
    "rationale": "Primary action must read as trustworthy on financial surfaces",
    "escalation": "design-system-core/review",
    "unrepresented": ["colorScheme:dark ∧ density:dense ∧ contrast:high"]
  }
}
```

**The pull request body for a judged edit (the adjudication):**

```
Policy: pol_9f2a…  (color.action.primary)
Regime: judged   Authority: design-system-core
Change: {blue.500} → {blue.600}   (mode: light)
Conviction: 0.75
Rationale: "Deeper navy reads as more institutional after brand shift"
Author: @designer
—
Merging ratifies this resolution. Declining reverts to {blue.500}.
```

The `unrepresented` array is the projection gap, recorded; the pull request is the annotation gap, resolved on the record.

---

## Key Insights

**Insight 1: Two gaps need two mechanisms.** The projection gap is closed by *flagging* (record `unrepresented`, warn in-canvas); the annotation gap is closed by *carrying and adjudicating* (plugin data + pull request). Conflating them yields a design that solves neither.

**Insight 2: Split the system of record.** Value may live in Figma; metadata never does. Plugin data plus the pull request is the metadata's record. A push that writes metadata back from a variable has already lost.

**Insight 3: Reuse the PR you already have.** GitFig's pull-request-from-Figma and the Single Source of Taste's escalation requirement are the same act. Judged edits become pull requests; the Git log becomes the case law promotion depends on.

**Insight 4: Faithful, not lossless-native.** Every value round-trips or is flagged; every judged edit is adjudicated; every judgment is legible in-canvas. But metadata carried in plugin data is invisible to any consumer without GitFig, so taste's provenance is tool-bound. Only a native Figma metadata field would remove this ceiling.

**Insight 5: Identity precedes bidirectionality.** Without a stable `dtcm.id` on every projected cell, push cannot tell reattachment from creation, and metadata orphans. Identity is the precondition, not a detail.

---

## Relationship to Prior Pages

- **[Context as Coordinate System](./02-context-as-coordinate-system.md)** established that Figma modes are a projection. This page shows the projection also drops metadata, and specifies what must be added back.
- **[The Single Source of Taste](./06-the-single-source-of-taste.md)** established judged policies and their metadata. This page is its delivery mechanism: how that metadata survives contact with the design tool.
- **[Design Systems: Single Source of Truth](https://github.com/ds1/design-token-context-model/blob/main/archive/Design%20Systems_%20Single%20Source%20of%20Truth.md)** established propagation from a canonical source. This page keeps the source canonical in Git and makes Figma a faithful, adjudicated view rather than a competing store.

---

## Conclusion

The appeal of a Figma-and-code sync is that design might finally own taste in the tool design lives in. This page tempers that appeal without abandoning it. Figma cannot be the native home of judged policies: it projects away their compound structure and has no field for their authority or reasoning. But it can be their *faithful projection* — if GitFig mints stable policy identities, carries metadata in namespaced plugin data, renders that metadata into visible badges, flags the values it cannot represent, and turns every edit to a judged value into a pull request that its authority adjudicates. The canonical Single Source of Taste stays in Git as a policy set; GitFig makes Figma the best-annotated view of it that the platform permits. The residue — provenance invisible to tools that do not know to look — is the honest price, and it names the one feature that would settle the matter: a native place in Figma to say that a value is a judgment, and whose.

---

## Glossary

**Adjudication Pull Request** A pull request opened on push of a changed judged value, carrying the edit-time rationale, confidence, author, and source policy identity. Merging ratifies the new aesthetic resolution; declining reverts it. Realises the Single Source of Taste's escalation requirement using GitFig's existing pull-request-from-Figma capability.

**Annotation Gap** The absence in Figma of any native, structured, designer-visible field for judged-policy metadata (regime, authority, confidence, rationale). Distinct from the projection gap; closed by carrying metadata in plugin data and rendering it into descriptions.

**Carrier** Any location in a Figma file where metadata could be stored: variable description, code-syntax slots, naming convention, or shared plugin data. No single carrier is both structured and designer-visible.

**Escalation-on-Edit** The rule that an edit to a judged value is an appeal, not a write: it must supply conviction and open an adjudication pull request rather than degrade to a default. The round-trip form of the Single Source of Taste's escalate-don't-degrade principle.

**Faithful Round-Trip** A round-trip in which every value is preserved or explicitly flagged as unrepresentable, every judged edit is adjudicated, and every judgment is legible in-canvas. Weaker than lossless-and-native because carried metadata remains invisible to consumers without the plugin.

**Lossless-and-Native Round-Trip** The unattainable ideal in which judged policies survive through Figma with full structure and metadata, visible to every consumer without special tooling. Blocked by the projection and annotation gaps; would require a native Figma metadata field.

**Policy Identity (`dtcm.id`)** An opaque, stable identifier minted when a policy is first authored and written to every variable-mode cell the policy projects into. The precondition for push: it maps a Figma edit back to its source policy by identity rather than by name.

**Projection Gap** The inability of Figma's mode model to represent compound conditions, specificity, or precedence, such that some judged values are unrepresentable as variables and live only in code. Recorded in an `unrepresented` field and surfaced as an in-canvas warning.

**Rendered Badge** A compact, human-readable summary of judged metadata that GitFig writes into a variable's description on pull. A derived *view*, regenerated each pull, never read back as source.

**Shared Plugin Data** Figma's mechanism for a plugin to persist namespaced key–value data on documents and nodes. Structured and round-trip-safe, but invisible in the native UI. The system of record for judged metadata; invisibility to non-GitFig consumers is the design's hard ceiling.

**System of Record (split)** The principle that the *value* of a judged policy may be edited in Figma, while its *metadata* is recorded only in plugin data plus the adjudication pull request. Writing metadata back from a variable is a loss.

**Unrepresented** A recorded list of the coordinate conditions under which a policy's value could not be projected into Figma's modes. The projection gap, made explicit and visible so designers do not edit values whose true resolution Figma cannot show.
