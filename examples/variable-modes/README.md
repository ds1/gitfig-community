# Variable Modes Example

**For docs:** [Variable Modes](https://docs.gitfig.com/design-sync/variable-modes)

A two-collection setup showing mode-keyed sync: a single-mode **Primitives** collection referenced by a **Semantic** collection with `light` and `dark` modes. Variable modes sync is free for everyone during the v1.3 preview.

## Files

```
tokens/
├── primitives.json  # Single-mode collection: map as Variables, "First mode only"
└── semantic.json    # Mode-keyed (light/dark): map as Variables, "All modes"
```

## Suggested mappings

Pull `primitives.json` first so the alias targets exist when `semantic.json` is pulled.

| File | Target | Collection Name | Variable Modes |
|------|--------|-----------------|----------------|
| `tokens/primitives.json` | Variables | `Primitives` | First mode only |
| `tokens/semantic.json` | Variables | `Semantic` | All modes |

Requires a Figma plan that allows more than one mode per collection (Professional or higher). On the Starter plan the second mode is skipped and reported.

## What to look for

- `semantic.json` root keys are **mode names** (`light`, `dark`); each mode has an identical structure with different values.
- Values like `{Primitives.gray.900}` are **alias references**. After pulling, the Semantic variables are bound to the Primitives variables, and toggling the mode in Figma switches every resolved value.
- `color.action.primary` carries a `$description` and an `$extensions` block in both modes. The description lands on the Figma variable; `$extensions` is left untouched on push (GitFig only owns `$value`, `$type`, and `$description`).
- `space.inset` shows a number alias across modes (same primitive in both, so the value is mode-independent while still living in the mode-keyed file).
- Push after editing a `dark` value in Figma: the diff touches one file and one mode.

## Styles alongside modes

Styles have no modes, so they sync as flat files next to the mode-keyed one. Create one paint style and one text style in Figma, then add two more mappings:

| Target | File |
|--------|------|
| Color Styles | `tokens/color-styles.json` |
| Text Styles | `tokens/text-styles.json` |

Push writes them flat (`"Brand-Primary": { "$value": "#3B82F6", "$type": "color" }`) in the same atomic commit as the variables. Pull creates or updates the styles by name; a text style needs its font available in the file, otherwise that entry is skipped with a warning.

## What a correct round-trip looks like

Use this to confirm your setup end to end.

1. Pull `primitives.json` then `semantic.json`. Figma has a **Primitives** collection (12 variables, 1 mode) and a **Semantic** collection with exactly two modes, `light` and `dark`, holding 8 variables each. `Semantic/color/text/primary` shows as an alias to `Primitives/gray/900`, and `Primitives/gray/900` carries the description `Darkest gray, café-grade contrast ✓`.
2. Push with no edits. GitFig reports no changes, and `$extensions` is still present in both files on GitHub.
3. Change one `dark` value in Figma and push. The staging list shows a single change with a `dark` badge; the commit touches only `semantic.json`, only inside `"dark"`.
4. Delete both collections in Figma and pull again. Both are recreated with their modes, aliases re-bound, and descriptions intact.
5. Point a mapping set to **First mode only** at `semantic.json`. GitFig refuses with the mode-keyed file guard and creates no variables; switch that mapping to **All modes** to import it.

## Try the mode reconciliation

Add a third root key to `semantic.json` on GitHub, for example `"dim": {}`, and pull. GitFig adds a `dim` mode to the Semantic collection; Figma seeds it from the default mode, and your next push fills in its values.
