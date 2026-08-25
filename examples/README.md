# GitFig Examples

Ready-to-use example files to help you get started with GitFig tutorials.

## How to Use

1. Copy the example files to your own GitHub repository
2. In GitFig's Mapping section, hover a file → **Add Target** → set the Collection Name from the folder's README (each example lists its suggested mappings) → Save, then **Pull**
3. Follow the corresponding tutorial in the [GitFig Documentation](https://docs.gitfig.com)

Or fork this repo and use it directly with GitFig!

---

## Examples

### 📁 [design-tokens-starter](./design-tokens-starter/)

**For tutorial:** [Design Tokens Workflow](https://docs.gitfig.com/tutorials/design-tokens-workflow)

A complete starter template with an expanded base set of token files:
- `tokens/colors.json` - Brand colors, backgrounds, text colors
- `tokens/palette.json` - Primitive color scales (50-900) + alpha overlays
- `tokens/spacing.json` - Spacing scale (xs to xl)
- `tokens/typography.json` - Font families, sizes, weights
- `tokens/radius.json` - Corner radius scale
- `tokens/borders.json` - Border widths + opacity steps
- `tokens/elevation.json` - Z-index scale + shadow recipes
- `tokens/motion.json` - Durations + easing curves
- `tokens/breakpoints.json` - Breakpoints + boolean token examples

### 📁 [multi-brand-themes](./multi-brand-themes/)

**For tutorial:** [Multi-Brand Themes](https://docs.gitfig.com/tutorials/multi-brand-themes)

Example showing how to structure tokens for multiple brand themes:
- `tokens/base.json` - Base/neutral tokens (main branch)
- `tokens/brand-a.json` - Brand A theme (blue, rounded)
- `tokens/brand-b.json` - Brand B theme (green, sharp)
- `tokens/brand-c.json` - Brand C theme (purple, soft)

### 📁 [tokens-studio-migration](./tokens-studio-migration/)

**For tutorial:** [Migrating from Tokens Studio](https://docs.gitfig.com/tutorials/migrating-from-tokens-studio)

Sample Tokens Studio format files for testing migration:
- `tokens.json` - Tokens Studio format (before migration)
- `tokens-w3c.json` - W3C Design Tokens format (after migration)

---

## Quick Start

```bash
# Clone this repo
git clone https://github.com/ds1/gitfig-community.git

# Or copy specific examples to your repo
```

Then connect to it with GitFig in Figma!
