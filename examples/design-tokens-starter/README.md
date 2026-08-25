# Design Tokens Starter

A complete starter template for the [Design Tokens Workflow](https://docs.gitfig.com/tutorials/design-tokens-workflow) tutorial — expanded with a full base set of token files so you can test every token category GitFig supports.

## Files

```
tokens/
├── colors.json       # Brand, background, and text colors
├── palette.json      # Primitive color scales (gray/blue/green/red/amber 50–900) + alpha overlays
├── spacing.json      # Spacing scale (4px base)
├── typography.json   # Font families, sizes, and weights
├── radius.json       # Corner radius scale (none → full)
├── borders.json      # Border widths + opacity steps
├── elevation.json    # Z-index scale + CSS shadow recipes (string tokens)
├── motion.json       # Durations (ms) + easing curves (string tokens)
└── breakpoints.json  # Breakpoints, container sizing + boolean token examples
```

## Suggested mappings

Map each file to **Variables** with a matching collection name:

| File | Target | Collection Name |
|------|--------|-----------------|
| `tokens/palette.json` | Variables | `palette` |
| `tokens/colors.json` | Variables | `colors` |
| `tokens/spacing.json` | Variables | `spacing` |
| `tokens/typography.json` | Variables | `typography` |
| `tokens/radius.json` | Variables | `radius` |
| `tokens/borders.json` | Variables | `borders` |
| `tokens/elevation.json` | Variables | `elevation` |
| `tokens/motion.json` | Variables | `motion` |
| `tokens/breakpoints.json` | Variables | `breakpoints` |

## Usage

1. Copy this folder to your own repository, or use this repo directly
2. Open GitFig in Figma
3. Connect to your repository
4. Configure mappings for each JSON file → Variables
5. Pull to create Figma Variables
6. Follow the tutorial to modify and push changes

## Token Structure

All tokens use **W3C Design Tokens** format:

```json
{
  "token-name": {
    "$value": "the-value",
    "$type": "color|number|string|boolean",
    "$description": "Optional description"
  }
}
```

Notes:
- Each file opens with a `"$comment"` key stating its suggested mapping — GitFig ignores `$`-prefixed keys on import and preserves them on push, so you can annotate your own files the same way
- **Colors** become Figma color variables (8-digit hex like `#0000001A` carries alpha)
- **Numbers** become float variables (spacing, radius, z-index, durations)
- **Strings** become string variables (font names, easing curves, shadow recipes — Figma variables don't support shadow values directly)
- **Booleans** become boolean variables (useful for feature-flag-driven components)
