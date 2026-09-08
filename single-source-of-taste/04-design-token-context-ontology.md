# Design Token Context Ontology
### Draft 0.1 January 20, 2026

*Source: originally published on the [design-token-context-model wiki](https://github.com/ds1/design-token-context-model/wiki/Design-Token-Context-Ontology) in January 2026. Moved here September 2026; this file is now the canonical copy.*

Exploration of a robust specification for multi-dimensional design token resolution with full Figma Variables support.

## The Problem

Design tokens have become foundational infrastructure for scalable design systems. The industry has converged on a tiered abstraction model—primitives, options, decisions, components, instances—but **context-awareness remains fragmented**.

Current systems treat context as flat modes (`light`/`dark`, `desktop`/`mobile`) rather than as a **multidimensional coordinate system** with inheritance, intersection, and resolution rules. When a button needs to resolve its background color, the answer depends on:

- Color scheme preference (light, dark, high-contrast)
- Density mode (compact, comfortable, spacious)
- Interaction state (rest, hover, active, disabled)
- Accessibility requirements (forced colors, reduced motion)
- Platform capabilities (color gamut, touch input)
- Locale considerations (RTL, text expansion)
- Container context (available space, scroll position)
- And dozens more dimensions...

**This project provides the missing specification.**

## What's Included

### [Specification](./05-design-token-context-ontology-specification.md)

The complete specification defining:

| Aspect | Description |
|--------|-------------|
| **12 Context Categories** | Appearance, Density, Viewport, Container, Interaction States, Combinable States, Accessibility, Locale, Platform, Elevation, Semantic, Structural |
| **93 Dimensions** | Complete enumeration with values, volatility class, and detection mechanism |
| **5 Volatility Classes** | Immutable → Stable → Semi-stable → Volatile → Inherited |
| **78 Cross-Category Dependencies** | Documented relationships requiring coordination |
| **52 Conflict Scenarios** | Resolution rules for dimension intersections |
| **Named Context Types** | Governed shortcuts for common patterns (mobile-dark, kiosk, print) |
| **Resolution Algorithm** | Explicit precedence ordering for token resolution |

### [Token Collections](https://github.com/ds1/design-token-context-model/tree/main/tokens)

W3C Design Tokens format implementation with 10 collections (~2,750 tokens), kept in the [design-token-context-model](https://github.com/ds1/design-token-context-model/tree/main/tokens) repository:

```
tokens/
├── collections/
│   ├── primitives.json  # Raw color palette (blue-500, gray-200, etc.)
│   ├── theme.json       # Light / Dark / Dim color schemes
│   ├── density.json     # Compact / Comfortable / Spacious spacing
│   ├── viewport.json    # Mobile / Tablet / Desktop layouts
│   ├── contrast.json    # Standard / High / Forced accessibility
│   ├── motion.json      # Full / Reduced / None animations
│   ├── elevation.json   # Base / Raised / Elevated surfaces
│   ├── typography.json  # Default / Editorial / Technical styles
│   ├── state.json       # Rest / Hover / Active / Disabled states
│   └── brand.json       # Primary / Secondary / Partner identities
└── manifest.json        # Collection metadata & GitFig mappings
```

## Collections Overview

| Collection | Modes | Purpose |
|------------|-------|---------|
| **Primitives** | — | Raw color palette: 22 scales (gray→rose) × 11 steps (50-950) |
| **Theme** | light, dark, dim | Color schemes with surfaces, foregrounds, borders, status colors |
| **Density** | compact, comfortable, spacious | Spacing, sizing, touch targets, component dimensions |
| **Viewport** | mobile, tablet, desktop | Responsive layouts, grids, breakpoints, navigation patterns |
| **Contrast** | standard, high, forced | Accessibility colors, focus rings, border widths |
| **Motion** | full, reduced, none | Durations, easings, animations, scroll behaviors |
| **Elevation** | base, raised, elevated | Shadows, z-indices, glass effects, surface treatments |
| **Typography** | default, editorial, technical | Font families, text styles, line heights, spacing |
| **State** | rest, hover, active, disabled | Interaction states for buttons, inputs, links, cards |
| **Brand** | primary, secondary, partner | Brand colors, gradients, radii, typography per brand |

## Architecture

Each collection represents an **independent context dimension** with its own modes:

```
┌──────────────────────────────────────────────────────────────────────────┐
│  Collection: Theme                                                       │
│  Modes: [light, dark, dim]                                               │
│  Detection: prefers-color-scheme, user preference                        │
├──────────────────────────────────────────────────────────────────────────┤
│  Collection: Density                                                     │
│  Modes: [compact, comfortable, spacious]                                 │
│  Detection: pointer type, user preference                                │
├──────────────────────────────────────────────────────────────────────────┤
│  Collection: Viewport                                                    │
│  Modes: [mobile, tablet, desktop]                                        │
│  Detection: viewport width, container queries                            │
├──────────────────────────────────────────────────────────────────────────┤
│  ...additional collections...                                            │
└──────────────────────────────────────────────────────────────────────────┘

Each token has explicit values for ALL modes in its collection:

{
  "surface.default": {
    "$type": "color",
    "$value": "#ffffff",
    "$extensions": {
      "mode": {
        "light": "#ffffff",
        "dark": "#0a0a0a",
        "dim": "#1a1a1a"
      }
    }
  }
}
```

## Quick Start

### Import to Figma with GitFig

1. Install the [GitFig](https://www.figma.com/community/plugin/1584467274034932618) plugin in Figma and connect a repository containing the `tokens/` folder
2. In GitFig's **Mapping** section, add a Variables target for each collection file, using the collection name from `manifest.json`
3. **Pull**: each collection file becomes a Figma Variable Collection, and each `mode` key becomes a mode (see the [variable-modes example](../examples/variable-modes/) for the file shape GitFig expects)

Note that this projects the ontology onto Figma's flat mode model. Compound conditions, specificity, and precedence do not survive the projection; see [Context as Coordinate System](./02-context-as-coordinate-system.md#insight-1-the-figma-model-is-a-projection) and [Carrying Judged Policies Through Figma](./07-carrying-judged-policies-through-figma.md).

### Use with Style Dictionary

```json
{
  "source": ["tokens/collections/*.json"],
  "platforms": {
    "css": {
      "transformGroup": "css",
      "buildPath": "dist/",
      "files": [{ "destination": "tokens.css", "format": "css/variables" }]
    }
  }
}
```

### Use Directly

```javascript
import primitives from './tokens/collections/primitives.json';
import theme from './tokens/collections/theme.json';
import density from './tokens/collections/density.json';

// Access raw color primitives
const blue500 = primitives.blue['500'].$value; // #3b82f6
const gray200 = primitives.gray['200'].$value; // #e5e5e5

// Get light mode surface color
const surfaceLight = theme.surface.default.$extensions.mode.light; // #ffffff

// Get dark mode surface color
const surfaceDark = theme.surface.default.$extensions.mode.dark; // #0a0a0a

// Get spacing for comfortable density
const spacing4 = density.spacing['4'].$extensions.mode.comfortable; // 16px

// Get spacing for compact density
const spacing4Compact = density.spacing['4'].$extensions.mode.compact; // 12px
```

## Design Philosophy

This ontology supports a **Single Source of Truth (SSOT) architecture** with controlled contextual variation:

1. **Explicit chains** — Every resolved token value traces back through a documented path
2. **Governed types** — Context types are finite and governed, not arbitrary
3. **Learnable friction** — Exceptions are logged and promoted to tokens when patterns emerge
4. **Predictable blast radius** — Changes at any tier have bounded, knowable effects
5. **Full mode coverage** — Every token has explicit values for all modes (no fallbacks needed)

## Resolution Precedence

When dimensions conflict, resolution follows this order (highest to lowest):

1. **Accessibility overrides** — `forced-colors`, `prefers-contrast`, minimum targets
2. **Explicit declarations** — Dimensions set directly on element
3. **Context type** — Dimensions from applied named context type
4. **Inherited context** — Dimensions from ancestor chain
5. **Platform defaults** — Platform-specific default values
6. **System defaults** — Ontology-defined defaults

## Named Context Types

Pre-defined combinations for common scenarios:

| Context Type | Dimensions |
|--------------|------------|
| `mobile-dark` | viewport: mobile, theme: dark, density: comfortable |
| `desktop-light` | viewport: desktop, theme: light, density: comfortable |
| `kiosk` | viewport: desktop, theme: dark, density: spacious, contrast: high |
| `print` | theme: light, contrast: high, motion: none, elevation: base |
| `high-contrast` | contrast: high, elevation: base |
| `reduced-motion` | motion: reduced |
| `compact-ui` | density: compact, typography: technical |
| `editorial-reading` | density: spacious, typography: editorial |


## Contributing

Issues and pull requests welcome. See the [specification](./05-design-token-context-ontology-specification.md) for the complete ontology that implementations should follow.
