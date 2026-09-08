# Soundbites: The Single Source of Taste

*Source: prepared September 8, 2026 in [this conversation](https://claude.ai/share/5bb9babf-7a7b-47ae-84be-878192d36f35) as quote material for a live podcast. Quotes are verbatim from that session, grouped by theme. The GitFig facts at the bottom are reference material, not quotes, and have been aligned with [COMPARISON.md](../../COMPARISON.md) and [docs.gitfig.com](https://docs.gitfig.com).*

Each line below is a self-contained statement of one idea from the [Single Source of Taste](../06-the-single-source-of-taste.md) sequence. The page that supports each group is linked so a listener can follow up.

## The core reframe

Supporting pages: [The Single Source of Taste](../06-the-single-source-of-taste.md), [Context as Coordinate System](../02-context-as-coordinate-system.md).

- "Single Source of Truth answers *does it work*. Single Source of Taste answers *is it right*. For the first time, both can live in one system."
- "Truth and taste aren't different kinds of things. They're the same thing, a policy, certified two different ways: one by a test, one by a person."
- "A design token was never a color. It's a policy that decides what the color should be, given the context."

## Taste versus truth

Supporting page: [The Single Source of Taste](../06-the-single-source-of-taste.md), Arguments 1, 2, and 7 and the section on resolution precedence.

- "You can lint truth. You can't lint taste. No test ever comes back green on 'beautiful.'"
- "When something's a bug, a test catches it. When something's just wrong, only a person can say so, and that person is the designer."
- "Taste is sovereign over beauty and subordinate to access. It chooses inside the box that accessibility draws."

## Why this matters now

Supporting pages: [The Single Source of Taste](../06-the-single-source-of-taste.md), Insight 5; [Compound Conditions Beyond Figma](../08-compound-conditions-beyond-figma.md), Argument 7.

- "When AI makes working software almost free, the scarce thing isn't code anymore. It's judgment. Taste becomes the bottleneck, and the moat."
- "Democratized building doesn't make designers less important. It moves them to the one job that can't be automated: deciding what's good."
- "Anyone can generate a working interface now. Almost no one can generate a right one. That gap is the whole future of design."

## The honest edge

Supporting pages: [The Single Source of Taste](../06-the-single-source-of-taste.md), Argument 7; [Compound Conditions Beyond Figma](../08-compound-conditions-beyond-figma.md), Arguments 3 and 7.

- "A design system can enforce that you followed the taste. It can never certify the taste was any good. That's the irreducible seat of the designer."
- "Let the AI draft the taste. Never let it sign the taste. The moment a machine becomes the authority, you're laundering machine aesthetics as judgment."
- "The values no formula predicts and no test can check, those are the ones made of pure taste. They're the hardest to automate and the last to leave human hands."

## Bridging the tool and the theory

Supporting page: [Carrying Judged Policies Through Figma](../07-carrying-judged-policies-through-figma.md).

- "GitFig keeps the values in sync. The Single Source of Taste is about keeping the judgment in sync, and that's the part the industry hasn't built yet."
- "If tokens are where taste gets codified, GitFig is what keeps that layer true across design and code. It's the plumbing the Single Source of Taste runs on."

## GitFig: why it matters

- "Design tokens have lived in two worlds, designers in Figma and developers in code, and keeping them in sync has been manual, lossy, and error-prone. GitFig makes it one world."
- "It brings the thing engineering has had for decades, branches, pull requests, review, to design, without making designers leave the tool they think in."
- "Every design change becomes reviewable, revertible, and attributable. Design decisions finally have a history."
- "Copy-pasting hex values is how design systems drift. GitFig ends the drift."
- "A pull request from Figma turns a design change into an act of record, exactly the way a code change already is."

## GitFig 1.3: what it does (reference, not quotes)

These are the current capabilities as described in [COMPARISON.md](../../COMPARISON.md) and the [documentation](https://docs.gitfig.com). GitFig v1.3.0 shipped on August 29, 2026; per the [changelog](https://docs.gitfig.com/changelog), the release's headline items were two-way Variable Modes sync with mode-keyed token files, Styles sync on Pull and Push, and the GitFig Pro preview. Everything else below was carried forward from earlier releases, so check the changelog before describing an item as new in a specific release.

- Bi-directional sync between a Figma file and a GitHub repository: Pull creates or updates Figma Variables and Styles from JSON token files, and Push writes Figma Variables and Styles back as W3C Design Tokens JSON.
- Works on Figma's native Variables and Color, Text, and Effect Styles, so the same Variables panel your components already consume is the design-side surface.
- Change detection like a working tree, with per-change staging, commit messages, and one-click push. Pulls are undoable.
- Branches and pull requests from inside Figma, so design review happens where code review already happens.
- Variable modes: every mode of a collection syncs as one mode-keyed file (free during the v1.3 preview).
- Reads W3C Design Tokens, Style Dictionary, Tokens Studio, and custom JSON; writes W3C Design Tokens.
- Connect any GitHub repository you have access to.
