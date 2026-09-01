# How GitFig Compares

[GitFig](https://gitfig.com) connects a Figma file directly to a GitHub repository and gives Figma's native Variables and Styles a real Git workflow: change detection, staged commits, branches, and pull requests, all from inside Figma. Tokens land in your repository as standard W3C Design Tokens JSON that any DTCG-capable tool (Style Dictionary, your build pipeline, other design tools) can consume.

## GitFig vs Tokens Studio

Both tools put tokens in Git, but they start from opposite ends.

**Tokens Studio** is a token editor that lives in a plugin. You author tokens in its own store and format (token sets, themes, math and composite tokens), then apply them to Figma variables and styles through an export step. That model is powerful for token-first teams, but it means your design system has two representations, the plugin's and Figma's, and you manage the mapping between them.

**GitFig** treats Figma's native Variables and Styles as the source of truth inside Figma. You author tokens in Figma's own UI, the same Variables panel your components already consume, and GitFig adds the Git layer around them. It detects your edits, select the changes you want to commit, write a commit message, and push.

| | GitFig | Tokens Studio |
|---|---|---|
| **Where tokens are authored** | Figma's native Variables and Styles UI | The plugin's own token editor |
| **Source of truth in Figma** | Native Variables plus Color, Text, and Effect Styles | The plugin's token store, applied to Figma via export |
| **File format in Git** | Writes W3C Design Tokens; reads W3C, Style Dictionary, Tokens Studio, and custom JSON | Tokens Studio JSON, with a W3C DTCG option |
| **Commit workflow** | Per-change staging with checkboxes, commit messages, one-click push, undoable pulls | Push and pull of token files |
| **Branches and pull requests** | Create branches and open PRs from inside Figma, free | Branch switching on the paid plan; PRs on your Git host |
| **Variable modes / themes** | Every mode of a collection syncs as one mode-keyed file (free during the v1.3 preview) | Themes on the paid plan, applied via export |
| **Git hosts** | GitHub (github.com) | GitHub, GitLab, Bitbucket, Azure DevOps |
| **What you learn** | Git basics: commit, push, branch, PR | Its own token model: sets, themes, and export options |
| **Price** | Free during the v1.3 preview | Free tier; Git branching, multi-file sync, and advanced token types on a [paid plan](https://tokens.studio/pricing) |

Tokens Studio remains a good fit if you author math-driven or composite tokens that Figma variables cannot yet express, or if your repositories live on GitLab, Bitbucket, or Azure DevOps. If your tokens are (or should be) native Figma Variables and Styles, GitFig removes the parallel model and the export step.

Already on Tokens Studio? GitFig reads its format directly, so migration is one pull and one push: see the [migration guide](https://docs.gitfig.com/tutorials/migrating-from-tokens-studio) and the [tokens-studio-migration example](./examples/tokens-studio-migration/) in this repo.

## What about component variants, states, and visibility?

Component structure (variants, component properties, visibility bindings, interactive states) lives in Figma components. Design token tools, GitFig included, version tokens, not components. In future releases, GitFig may support component versioning and integrations.

With GitFig, your components consume Variables natively, i.e., bind a variable to a fill, a mode to a theme or state, a boolean variable to a visibility property. GitFig versions the variables and styles underneath, so component behavior stays a pure Figma concern and keeps working with every Figma feature, current and future. There is a single representation of your system because GitFig syncs Figma with GitHub. Either in the external code repo or in Figma, renaming a variable, adding a mode, or rebinding a component property, changes appear in GitFig's change list as ready to commit or pull.

If versioning component structure itself would help your team, open a [feature request](https://github.com/ds1/gitfig-community/discussions/categories/feature-requests): requests here directly shape the roadmap.

## GitFig vs the Figma Variables REST API

Figma's [Variables REST API](https://developers.figma.com/docs/rest-api/variables) can read and write variables, but it requires a Figma Enterprise plan, and you build and maintain the sync scripts, hosting, and conflict handling yourself. There is no in-Figma workflow: designers cannot see, stage, or commit changes from the canvas.

GitFig is a plugin, so it works on any Figma plan (the number of modes per collection follows your Figma plan's limit) and puts the whole workflow inside the file where the design work happens.

## GitFig vs manual export

Copying values by hand or maintaining JSON files manually works at very small scale, but there is no change detection, no history of who changed what and why, and drift between Figma and code is invisible until something ships wrong. GitFig gives every token change a commit, an author, and a reviewable diff.

## Learn more

- [Documentation](https://docs.gitfig.com)
- [Full comparison in the docs](https://docs.gitfig.com/reference/comparison)
- [Install GitFig](https://www.figma.com/community/plugin/1584467274034932618)
