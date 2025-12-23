# Tokens Studio Migration Example

Example files for the [Migrating from Tokens Studio](https://docs.gitfig.com/tutorials/migrating-from-tokens-studio) tutorial.

## What's Included

- `tokens.json` - Sample Tokens Studio format file with colors, typography, and spacing
- `tokens-w3c.json` - The same tokens converted to W3C Design Tokens format

## Tokens Studio Format

Tokens Studio uses a slightly different format than W3C Design Tokens:

```json
{
  "colors": {
    "primary": {
      "value": "#0066FF",
      "type": "color"
    }
  }
}
```

## W3C Design Tokens Format

GitFig uses W3C Design Tokens format (with `$` prefix):

```json
{
  "colors": {
    "primary": {
      "$value": "#0066FF",
      "$type": "color"
    }
  }
}
```

## Key Differences

| Feature | Tokens Studio | W3C (GitFig) |
|---------|--------------|--------------|
| Value key | `value` | `$value` |
| Type key | `type` | `$type` |
| Description | `description` | `$description` |
| References | `{colors.primary}` | `{colors.primary}` (same) |

## Migration Steps

1. Export your tokens from Tokens Studio
2. Use GitFig's auto-detection (it handles Tokens Studio format)
3. Or manually convert by adding `$` prefix to keys
4. Push to GitHub to standardize on W3C format

See the [full migration guide](https://docs.gitfig.com/tutorials/migrating-from-tokens-studio) for detailed steps.
