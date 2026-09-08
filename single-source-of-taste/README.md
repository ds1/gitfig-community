# The Single Source of Taste

*A design-owned authority for the judgments a design system cannot test, built on a model of design tokens as resolution policies over a multidimensional context space.*

This folder is the canonical home for a body of work by [Dan Schmitz](https://github.com/ds1) that began as a context ontology for design tokens and arrived at a governance principle for taste. It gathers the theory pages that were previously spread across the [design-token-context-model](https://github.com/ds1/design-token-context-model) repository and its wiki, the three Single Source of Taste papers drafted in September 2026, and the conversation that produced them. GitFig is the tool that makes the work practical, which is why it lives here, in the GitFig community repository.

The documents are drafts and are meant to be argued with. Open a thread in [Discussions](https://github.com/ds1/gitfig-community/discussions) or a pull request against this folder.

## The argument in five steps

1. **Token systems lack what CSS has.** Flat mode switches force teams to pre-enumerate every combination of theme, contrast, density, and viewport. CSS solved this decades ago with conditions, specificity, and precedence. Tokens should adopt that model.
2. **Context is a coordinate; a token is a policy.** A token does not store a value. It maps a point in an N-dimensional context space to a value, and resolution is policy evaluation with specificity and precedence. The resolution algorithm, not the value table, is the core artifact. Figma's mode collections are a lossy projection of that space.
3. **The ontology names the space.** Twelve categories, ninety-three dimensions, five volatility classes, and an explicit resolution order give the coordinate system its axes, and ten W3C-format token collections show how the ontology projects into Figma today.
4. **Intent bifurcates by how it is validated.** Functional intent (contrast ratios, motion safety) is verifiable by a criterion. Aesthetic intent (reading as trustworthy, feeling composed) has the same policy form but can only be judged by an authority. The Single Source of Taste is the principle that every aesthetic-intent policy has exactly one canonical authority, mirroring the Single Source of Truth. Taste chooses inside the box that accessibility draws.
5. **Taste has to survive the tool, and past it.** Figma can hold the rendered value of a judgment but not its authority, confidence, or rationale, so a faithful round-trip needs stable policy identity, plugin-resident metadata, and a pull request as the act of adjudication. Values that depend on compound conditions cannot be represented in Figma at all; they belong in a code-resident resolver, authored from intent with an agent that may draft but never certify.

## Reading order

| # | Document | What it establishes | Status |
|---|---|---|---|
| 01 | [Token Systems Should Adopt CSS's Model](./01-token-systems-should-adopt-the-css-model.md) | The combinatorial-explosion problem and the four things token formats lack: compound conditions, specificity, inheritance, precedence | Wiki page, January 2026 |
| 02 | [Context as Coordinate System](./02-context-as-coordinate-system.md) | Tokens are resolution policies; context is a coordinate; resolution is agent-relative, uncertain, and staged by volatility; the algorithm is the core artifact | Wiki page, January 2026 |
| 03 | [Coordinate Model and the Three-Tier Taxonomy](./03-coordinate-model-and-the-three-tier-taxonomy.md) | The coordinate model formalizes what happens inside the semantic tier; it does not replace primitives, semantics, and components | Wiki page, January 2026 |
| 04 | [Design Token Context Ontology](./04-design-token-context-ontology.md) | Overview of the ontology and its ten token collections, resolution precedence, and named context types | Wiki page, January 2026 |
| 05 | [Design Token Context Ontology: Specification](./05-design-token-context-ontology-specification.md) | The full specification: 12 categories, 93 dimensions, volatility, context types, inheritance, resolution algorithm, cross-category dependencies | Version 1.0, January 2026 |
| 06 | [The Single Source of Taste](./06-the-single-source-of-taste.md) | Aesthetic intent as a policy class; the `verifiable` and `judged` validation regimes; the `dtcm.authority` field; escalation instead of degradation; the irreducibility boundary | Draft 0.1, September 2026 |
| 07 | [Carrying Judged Policies Through Figma](./07-carrying-judged-policies-through-figma.md) | The projection gap and the annotation gap; stable policy identity; plugin data as record and description badge as view; the pull request as adjudication | Draft 0.1, September 2026 |
| 08 | [Compound Conditions Beyond Figma](./08-compound-conditions-beyond-figma.md) | Why the compound-condition residue is structural; separable versus non-separable composition; the code-resident resolver; the agentic authoring loop and the draft-not-certify invariant | Draft 0.1, September 2026 |

Read 01, 02, and 06 for the argument. Read 04 and 05 when you need the axes. Read 07 and 08 when you are building tooling.

The [Glossary](./GLOSSARY.md) indexes every term the pages define, with each page's wording side by side where a later page extends an earlier one.

## Appendix

- [From Polytruth to Taste: Origins](./appendix/origins-from-polytruth-to-taste.md) distills the conversation that produced this work, from a contrarian "polytruth" architecture through the engineering-versus-design comparison to the moment the Single Source of Taste was named and then corrected by the coordinate model.
- [Coordinate Model Working Notes](./appendix/coordinate-model-working-notes.md) are the notes that became page 02, retained for their formal predicate-matching, inheritance-merge, and normalization details.
- [Design Tokens for Embodied AI](./appendix/design-tokens-for-embodied-ai.md) is the research report whose affordance and behavior-under-interaction ideas page 02 borrows.
- [Soundbites](./appendix/soundbites.md) are short spoken-word statements of the ideas, with a reference list of what GitFig 1.3 does.

## What this asks of GitFig, and of Figma

GitFig today syncs Figma's native Variables and Styles with W3C Design Tokens JSON in GitHub, including every mode of a collection as one mode-keyed file, and lets designers stage changes, branch, and open pull requests without leaving Figma. In the language of these pages, that keeps the **projection** of the policy space true across design and code. See [COMPARISON.md](../COMPARISON.md) for the current feature set.

Pages 07 and 08 describe what a tool would need to keep the **judgment** true as well. None of it ships in GitFig today; it is a design brief, not a changelog.

- A stable policy identity (`dtcm.id`) written into every variable-mode cell a policy projects into, so a push can reattach an edit to its source policy instead of matching by name.
- A plugin-resident metadata surface that stores `regime`, `authority`, `confidence`, `rationale`, and `escalation` in namespaced shared plugin data, and renders a read-only badge into the variable description on pull.
- Asymmetric push: a changed verifiable value writes normally and lets CI re-check its criterion; a changed judged value opens a pull request carrying the edit-time rationale and confidence, and merging that pull request is the adjudication.
- An `unrepresented` record and in-canvas warning for values whose true resolution depends on a compound condition Figma cannot express.
- CI invariants: every judged policy names a human authority, every judged value falls inside the admissible set that accessibility policies define, and no policy identity is orphaned.

The one feature request the whole sequence implies is aimed at Figma rather than GitFig: a native place on a variable to say that a value is a judgment, and whose. Until that exists, taste's provenance is only as portable as the plugin that carries it.

## Provenance and what changed in the move

Every document here was reconciled from a prior location. The table records where each came from and what was edited beyond the provenance line added under each title. No argument was changed.

| Document | Came from | Edits in the move |
|---|---|---|
| 01 | Wiki page *Token Systems Should Adopt CSS' model* | Added a title heading; the wiki page had none |
| 02 | Wiki page *Context as Coordinate System* | None |
| 03 | Wiki page *Context Coordinate Model and the Three-Tier Token Taxonomy* | Restored the attribution to the *Design Systems: Single Source of Truth* report that the wiki version had dropped, and linked it |
| 04 | Wiki page *Design Token Context Ontology* (a copy of the source repository README) | Pointed the whitepaper link at 05 and the token links at the source repository; replaced a GitFig quick start that linked to the wrong repository with the current Mapping and Pull flow; added the projection caveat; moved the MIT notice into this README |
| 05 | `archive/design-token-context-ontology-whitepaper.md` in the source repository, never published on the wiki | None |
| 06, 07, 08 | Drafted September 8, 2026; never committed anywhere | Companion links that pointed at placeholder wiki URLs now point at the files in this folder and at the archived report |
| Appendix: working notes | `archive/context-coordinate-model-exploration.md` in the source repository | None |
| Appendix: embodied AI | Wiki page *Design Tokens for Embodied AI* | None |
| Appendix: origins, soundbites | The September 8, 2026 conversation | New documents distilled from the transcript |

The source repository's own README still links to wiki pages under slugs that never existed, and its wiki still carries the January pages. Those should become pointers to this folder; see [issue #30](https://github.com/ds1/gitfig-community/issues/30).

## License

The documents that came from the design-token-context-model repository were published there under the MIT License, and they remain under MIT here. The three September 2026 papers and the appendix documents written for this folder are released under the same terms.
