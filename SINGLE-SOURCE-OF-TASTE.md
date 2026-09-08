# The Single Source of Taste

> A short concept piece on where GitFig fits in a world where anyone can build. For the long-form argument, see [The Single Source of Taste](https://github.com/ds1/design-token-context-model) writeup in the Design Token Context Model project.

## Building got cheap. Judgment did not.

For most of software's history, the hard part was execution. Turning a design into working code took engineers, time, and a lossy handoff at every step. Design systems grew up to protect that crossing: a single source of truth, so a color or a spacing value lived in one place and every screen referenced it instead of copying it.

That world is ending. When a designer, a founder, or a model can describe an interface and get production-plausible code in seconds, execution stops being the bottleneck. Anyone can build almost anything.

So the scarce thing is no longer building. It is knowing what is worth building, and whether the built thing is any good. That is taste, and it has always lived in the hardest place to scale: people's heads.

## Truth was one problem. Taste is the next one.

A single source of truth kept your values from drifting. Two copies of a hex code will always disagree eventually, so you master it once and reference it everywhere.

Taste drifts worse, because it was never written down at all. It lives in the senior designer's eye and the reviewer's flinch at a wrong radius. When execution was slow, that was survivable: few things got built, and a small skilled group built them, so their taste spread by proximity. Democratized building removes that quiet quality gate. Now many people and many tools produce work fast, and none of them come with your standard attached.

A single source of taste is the same move as a single source of truth, aimed one level up. Make the standard explicit. Master it in one place. Let every maker reference it instead of reinventing it. The difference is what you are normalizing: not the values, but the judgment behind them.

## Why this needs Git, and why that used to rule design out

There was always one good reason to keep the canonical standard in code and not in the design tool: code had governance and design did not. Git gave engineers branches, review, history, rollback, and a clear sense of what a change would affect. Design tools gave you a beautiful canvas and no way to audit it. So even teams that agreed taste starts in design handed the master copy to code, because you cannot run a source of anything on a surface that cannot show its history or gate a change.

That was the whole argument. GitFig removes it.

GitFig connects a Figma file directly to a GitHub repository and gives Figma's native Variables and Styles a real Git workflow: change detection, staged commits, branches, pull requests, full history, and bi-directional sync so the standard stays true in both homes at once. A change to your taste becomes a branch and a reviewed commit, not a diffuse cultural event nobody can trace. Variable modes (light and dark, multi-brand, density) sync as structured, mode-keyed files, so the standard carries its context with it rather than flattening to a single look.

Once the design surface has the rigor that made code trustworthy, the reason to master taste anywhere else goes away. Taste starts in design. Now design can govern it.

## The short version

- When building was hard, the design system was your single source of truth.
- Now that building is easy, the design system becomes your single source of taste: the governed standard that every maker, human and machine, builds against.
- Taste can only be a real source if it can be versioned, reviewed, and traced. GitFig is what gives the design surface that rigor, so designers can hold the standard instead of filing requests against it.

Learn more at [gitfig.com](https://gitfig.com) and [docs.gitfig.com](https://docs.gitfig.com).
