# Design Tokens Starter

A complete starter template for the [Design Tokens Workflow](https://docs.gitfig.com/tutorials/design-tokens-workflow) tutorial.

## Files

```
tokens/
├── colors.json      # Brand, background, and text colors
├── spacing.json     # Spacing scale (4px base)
└── typography.json  # Font families, sizes, and weights
```

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
    "$type": "color|number|string",
    "$description": "Optional description"
  }
}
```
