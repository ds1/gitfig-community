# Multi-Brand Themes Example

Example files for the [Multi-Brand Themes](https://docs.gitfig.com/tutorials/multi-brand-themes) tutorial.

## Concept

This example shows how to use Git branches to manage multiple brand themes:

| Branch | Theme | Primary Color | Style |
|--------|-------|---------------|-------|
| `main` | Base/Neutral | Black | Default |
| `theme/brand-a` | Brand A | Blue (#0066FF) | Rounded corners |
| `theme/brand-b` | Brand B | Green (#00AA55) | Sharp corners |
| `theme/brand-c` | Brand C | Purple (#8B5CF6) | Soft shadows |

## Files

- `tokens/base.json` - Neutral base tokens (use on main branch)
- `tokens/brand-a.json` - Brand A customizations
- `tokens/brand-b.json` - Brand B customizations
- `tokens/brand-c.json` - Brand C customizations

## How to Use

### Option 1: Single Branch Per Theme

1. Create branches: `theme/brand-a`, `theme/brand-b`, `theme/brand-c`
2. On each branch, copy the corresponding brand file to `tokens/theme.json`
3. Switch branches in GitFig to load different themes

### Option 2: Use This Repo Directly

1. Connect GitFig to this repo
2. Map `examples/multi-brand-themes/tokens/base.json` to Variables
3. Create branches and swap in brand-specific files

## Switching Themes in Figma

1. Open GitFig
2. Click the branch selector
3. Choose your theme branch
4. Pull to load those token values
5. Your designs update automatically!
