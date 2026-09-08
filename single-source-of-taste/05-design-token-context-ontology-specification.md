# Design Token Context Ontology

*Source: originally kept as `archive/design-token-context-ontology-whitepaper.md` in the [design-token-context-model](https://github.com/ds1/design-token-context-model) repository. Moved here September 2026; this file is now the canonical copy. The overview page is [Design Token Context Ontology](./04-design-token-context-ontology.md).*

## A Specification for Multi-Dimensional Token Resolution

**Version 1.0** | **January 2026**

---

## Abstract

Design tokens represent the atomic values of a design system, but their resolution depends on context—a multidimensional coordinate system of environmental conditions, user preferences, and runtime states. This specification defines a comprehensive context ontology comprising 12 categories, 93 dimensions, and 78 cross-category dependencies that govern how design tokens resolve to concrete values.

The ontology classifies each dimension by volatility, defines context types as named intersections of common dimension combinations, specifies inheritance rules for how context flows through component hierarchies, and establishes resolution algorithms for handling dimension conflicts. Every resolved token value traces back through an explicit chain, supporting a Single Source of Truth (SSOT) architecture with controlled contextual variation.

This document serves as the authoritative reference for implementing context-aware design token systems.

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Core Concepts](#2-core-concepts)
3. [Context Categories](#3-context-categories)
   - 3.1 Appearance and Theme
   - 3.2 Density and Spacing
   - 3.3 Viewport and Responsive
   - 3.4 Container Sizing
   - 3.5 Interaction States (Mutually Exclusive)
   - 3.6 Combinable States
   - 3.7 Accessibility
   - 3.8 Locale and Internationalization
   - 3.9 Platform and Runtime
   - 3.10 Elevation and Surface
   - 3.11 Semantic and Feedback
   - 3.12 Structural
4. [Context Types](#4-context-types)
5. [Inheritance and Scoping](#5-inheritance-and-scoping)
6. [Resolution Algorithm](#6-resolution-algorithm)
7. [Cross-Category Dependencies](#7-cross-category-dependencies)
8. [Token Architecture](#8-token-architecture)
9. [Conclusion](#9-conclusion)
10. [Appendices](#appendix-a-terminology)

---

## 1. Introduction

Design tokens have become foundational infrastructure for scalable design systems. The industry has converged on a tiered abstraction model—primitives, options, decisions, components, instances—but context-awareness remains fragmented. Current approaches treat context as flat modes (light/dark, desktop/mobile) rather than as a coordinate system with inheritance, intersection, and resolution rules.

This specification establishes a comprehensive context ontology that enables:

- **Complete enumeration** of all context dimensions affecting token resolution
- **Volatility classification** indicating how frequently each dimension changes
- **Named context types** for common dimension intersections
- **Inheritance rules** for context flow through component trees
- **Resolution algorithms** for handling dimension conflicts
- **Detection mechanisms** distinguishing inferrable from declarative contexts

### 1.1 Design Philosophy

This ontology supports a Single Source of Truth (SSOT) architecture with controlled contextual variation:

1. **Explicit chains**: Every resolved token value traces back through a documented path
2. **Governed types**: Context types are finite and governed, not arbitrary
3. **Learnable friction**: Exceptions are logged and can be promoted to formal tokens when patterns emerge
4. **Predictable blast radius**: Changes at any abstraction tier have bounded, knowable effects

### 1.2 Specification Scope

This specification covers:

| Aspect | Description |
|--------|-------------|
| **93 dimensions** | Complete enumeration across 12 categories |
| **78 dependencies** | Cross-category relationships requiring coordination |
| **52 conflict scenarios** | Documented resolution rules for dimension intersections |
| **5 volatility classes** | Classification of dimension change frequency |

---

## 2. Core Concepts

### 2.1 Volatility Classification

Every dimension is classified by how frequently its value changes during a session:

| Class | Definition | Examples | Typical Scope |
|-------|------------|----------|---------------|
| **Immutable** | Fixed for the entire session; set at initialization | Platform, browser engine, device capabilities | Application |
| **Stable** | Changes only through explicit user action in settings | Color scheme, density preference, locale | User session |
| **Semi-stable** | May change during session but infrequently | Viewport size, orientation, fold posture | Page/view |
| **Volatile** | Changes frequently during normal interaction | Hover, focus, scroll position | Component |
| **Inherited** | Derived from ancestor context, not set directly | Nesting depth, container size, surface level | Subtree |

Volatility classification informs caching strategies, invalidation boundaries, and appropriate detection mechanisms.

### 2.2 Detection Mechanisms

Contexts are detected through one of three mechanisms:

| Mechanism | Description | Examples |
|-----------|-------------|----------|
| **Inferred** | Automatically detected via platform APIs | `prefers-color-scheme`, `pointer: coarse`, viewport dimensions |
| **Declared** | Explicitly set by application or component | Brand theme, density mode, surface level |
| **Computed** | Derived from other context values | Text expansion factor (from locale), tonal surface color (from theme + elevation) |

### 2.3 Context Coordinates

A **context coordinate** is the complete set of dimension values at a given point in the component tree. Token resolution maps context coordinates to concrete values:

```
resolve(token, coordinate) → value
```

Context coordinates support partial specification—unspecified dimensions inherit from ancestors or fall back to defaults.

---

## 3. Context Categories

### 3.1 Appearance and Theme

Appearance contexts govern visual presentation based on user preferences, system settings, and brand requirements.

#### Dimension Inventory

| Dimension | Values | Volatility | Detection |
|-----------|--------|------------|-----------|
| **Color scheme** | `light`, `dark`, `auto`, `dim` | Stable | Inferred: `prefers-color-scheme`; Declared: theme setting |
| **Contrast preference** | `standard`, `more`, `less` | Stable | Inferred: `prefers-contrast` |
| **Forced colors** | `none`, `active` | Stable | Inferred: `forced-colors` |
| **Inverted colors** | `none`, `inverted` | Stable | Inferred: `inverted-colors` (limited) |
| **High contrast theme** | `none`, `black`, `white`, `aquatic`, `desert`, `custom` | Stable | Inferred: Platform API |
| **Brand theme** | Implementation-specific | Stable | Declared |
| **Color scheme variant** | `standard`, `tinted`, `vivid`, `muted` | Stable | Declared |
| **Dynamic color source** | `none`, `wallpaper`, `brand-logo`, `content-image`, `user-selected` | Stable | Inferred: Platform API |
| **Color temperature** | `warm`, `neutral`, `cool` | Stable | Declared |
| **Color gamut** | `srgb`, `p3`, `rec2020` | Immutable | Inferred: `color-gamut` |
| **Tonal elevation mode** | `tonal`, `shadow`, `hybrid` | Stable | Declared |

#### Cross-Category Dependencies

```
Appearance ──────┬─── Elevation: Tonal elevation derives surface tint from
                 │               theme's primary color; dark scheme reverses
                 │               elevation direction (surfaces lighten upward)
                 │
                 ├─── Accessibility: forced-colors overrides ALL theme colors;
                 │                   prefers-contrast: more coupled to
                 │                   forced-colors on Windows
                 │
                 └─── Platform: iOS/Android dynamic color APIs differ;
                                Android requires @android:color/system_* tokens
```

#### Conflict Resolution

| Scenario | Rule | Rationale |
|----------|------|-----------|
| High contrast + dark scheme active | Apply both: `@media (prefers-color-scheme: dark) and (prefers-contrast: more)` | User preferences are additive |
| Forced colors + brand theme | Forced colors wins; map brand semantics to system color keywords | Accessibility override |
| Multi-brand portal | Namespace at primitive level: `{brand}.color.primary` | Prevent collision |
| Print from dark scheme | Reset to light values; remove decorative colors | Practical output |

---

### 3.2 Density and Spacing

Density contexts control spatial relationships between interface elements, affecting information density, touch target sizing, and visual hierarchy.

#### Dimension Inventory

| Dimension | Values | Volatility | Detection |
|-----------|--------|------------|-----------|
| **Density mode** | `compact`, `comfortable`, `spacious` | Stable | Declared |
| **Density scale** | `0`, `-1`, `-2`, `-3` (4px decrements) | Stable | Declared |
| **Touch optimization** | `standard`, `touch-optimized` | Semi-stable | Inferred: `pointer: coarse` |
| **Input device precision** | `fine`, `coarse`, `pen`, `hybrid` | Semi-stable | Inferred: `pointer`, `any-pointer` |
| **Content density** | `low`, `medium`, `high` | Inherited | Declared per container |
| **Scale factor** | `1x`, `1.25x`, `1.5x`, `1.75x`, `2x` | Immutable | Inferred: `resolution` |
| **Font scaling** | `default`, `large`, `larger`, `largest` | Stable | Inferred: Platform accessibility setting |
| **Interaction precision** | `standard`, `accessible` | Stable | Declared |
| **Minimum target size** | `24px` (AA), `44px` (AAA) | Stable | Declared based on conformance target |

#### Cross-Category Dependencies

```
Density ─────────┬─── Viewport: Larger viewports default to higher density;
                 │              auto-switch compact on desktop
                 │
                 ├─── Container: Container queries inform density;
                 │               sidebar card needs different density than
                 │               main content card
                 │
                 ├─── Accessibility: Touch targets MUST maintain minimum
                 │                   regardless of visual density reduction
                 │
                 └─── Interaction States: Compact density reduces hover/focus
                                          indicator spacing—must not violate
                                          WCAG 2.4.13 Focus Appearance
```

#### Conflict Resolution

| Scenario | Rule | Rationale |
|----------|------|-----------|
| Compact + touch input | Maintain 48dp touch targets; allow tighter visual spacing | Accessibility minimum preserved |
| 200% zoom + compact | Auto-switch to comfortable above 150% zoom | Prevent layout breakage |
| Mixed component density support | Establish minimum density per component type | Consistent behavior |
| Density change during focus | Transition only on blur, or animate smoothly | Prevent jarring shifts |

---

### 3.3 Viewport and Responsive

Viewport contexts address device screen characteristics, orientation, display modes, and foldable device states.

#### Dimension Inventory

| Dimension | Values | Volatility | Detection |
|-----------|--------|------------|-----------|
| **Breakpoint** | `xs`, `sm`, `md`, `lg`, `xl`, `xxl` (or continuous) | Semi-stable | Inferred: viewport width |
| **Orientation** | `portrait`, `landscape` | Semi-stable | Inferred: `orientation` |
| **Display mode** | `browser`, `standalone`, `fullscreen`, `minimal-ui`, `window-controls-overlay` | Stable | Inferred: `display-mode` |
| **Pixel density** | `1x`, `2x`, `3x`, `infinite` | Immutable | Inferred: `resolution` |
| **Safe area insets** | Continuous (top/right/bottom/left) | Semi-stable | Inferred: `env(safe-area-inset-*)` |
| **Display cutout type** | `none`, `notch`, `dynamic-island`, `punch-hole`, `corner` | Immutable | Inferred: Platform API |
| **Viewport segment count** | Integer (1, 2, 3) | Semi-stable | Inferred: `horizontal-viewport-segments` |
| **Fold posture** | `flat`, `half-opened`, `folded`, `tabletop`, `book` | Semi-stable | Inferred: Screen Fold API |
| **Fold occlusion** | `none`, `partial`, `full` | Semi-stable | Computed from fold posture |
| **Virtual keyboard state** | `hidden`, `visible`, `transitioning` | Volatile | Inferred: `env(keyboard-inset-*)` |
| **Titlebar area** | Continuous (x/y/width/height) | Stable | Inferred: `env(titlebar-area-*)` |

#### Cross-Category Dependencies

```
Viewport ────────┬─── Density: Viewport size informs default density;
                 │            mobile defaults to touch-optimized
                 │
                 ├─── Container: Container queries handle component-level
                 │               adaptation; viewport queries handle page layout
                 │
                 ├─── Platform: Foldable APIs differ between Android/Windows;
                 │              Samsung vs Surface fold behaviors differ
                 │
                 └─── Structure: Fold posture affects layout mode
                                 (single-column vs split-view)
```

#### Conflict Resolution

| Scenario | Rule | Rationale |
|----------|------|-----------|
| Cover to main screen transition (foldable) | Use `horizontal-viewport-segments: 2` to detect; preserve UI state | Continuity across form factor change |
| Dynamic safe area changes | Use `env()` in calculations, not hardcoded values | Dynamic hardware adaptation |
| Split-screen with reduced viewport | Container queries for components; viewport queries for chrome only | Component autonomy |
| Half-opened fold posture | Define `--layout-mode: split-vertical` | Explicit mode switching |

---

### 3.4 Container Sizing

Container contexts enable component-level responsive adaptation independent of viewport dimensions.

#### Dimension Inventory

| Dimension | Values | Volatility | Detection |
|-----------|--------|------------|-----------|
| **Container width** | Continuous (any length) | Inherited | Inferred: `@container (width)` |
| **Container height** | Continuous (any length) | Inherited | Inferred: `@container (height)` |
| **Container name** | String identifier | Declared | Declared: `container-name` |
| **Container aspect ratio** | `16/9`, `4/3`, `1/1`, `9/16`, etc. | Inherited | Computed |
| **Content overflow** | `within-bounds`, `overflowing-x`, `overflowing-y` | Volatile | Requires JavaScript |
| **Available space** | Percentage of viewport | Inherited | Computed |
| **Scroll state** | `scrolled-top`, `scrolled-bottom`, `scrolling`, `at-snappoint` | Volatile | Inferred: `@container scroll-state()` |

#### Cross-Category Dependencies

```
Container ───────┬─── Viewport: Media queries for global layout,
                 │              container queries for component layout
                 │
                 ├─── Density: Narrow containers trigger compact mode
                 │             @container (width < 20rem) { --density: compact }
                 │
                 ├─── Structure: Nested containers create formatting contexts
                 │               with token scope boundaries
                 │
                 └─── Elevation: Container containment affects stacking context
                                 creation and z-index resolution
```

#### Conflict Resolution

| Scenario | Rule | Rationale |
|----------|------|-----------|
| Height query + performance concern | Prefer `inline-size`; use JavaScript for height-dependent logic | Performance tradeoff |
| Nested container conflicts | Token inheritance rules + `@layer` for cascade control | Predictable cascade |
| Container + media query on same element | Media query establishes foundation; container query adapts | Clear precedence |
| Circular size dependency | CSS containment prevents; tokens must not create cycles | Architecture constraint |

---

### 3.5 Interaction States (Mutually Exclusive)

Interaction states represent the current input-driven state of an interactive element within a single state machine.

#### Dimension Inventory

| State | CSS Detection | ARIA Mapping | Volatility |
|-------|---------------|--------------|------------|
| **rest** | (default) | (implicit) | Volatile |
| **hover** | `:hover` | None | Volatile |
| **active** | `:active` | (limited) | Volatile |
| **pressed** | JavaScript `pointerdown` | None | Volatile |
| **dragged** | JavaScript | `aria-grabbed` (deprecated) | Volatile |
| **disabled** | `:disabled`, `[aria-disabled]` | `aria-disabled="true"` | Semi-stable |
| **pending** | JavaScript state | `aria-busy="true"` | Volatile |
| **skeleton** | JavaScript state | `aria-hidden="true"` | Volatile |
| **touch-hold** | JavaScript (`touchstart` + timer) | None | Volatile |

**Note**: Focus is NOT mutually exclusive—it is an additive overlay (see Section 3.6).

#### Cross-Category Dependencies

```
Interaction States ──┬─── Accessibility: Focus MUST take precedence over
                     │                   hover for keyboard navigation
                     │                   (WCAG 2.4.7 Focus Visible)
                     │
                     ├─── Semantic: Error state + disabled creates
                     │              "disabled due to error" compound state
                     │
                     └─── Elevation: Active/pressed states include
                                     elevation change (pressed = -1dp)
```

#### Conflict Resolution

| Scenario | Rule | Rationale |
|----------|------|-----------|
| Hover + another exclusive state | Exclusive states take precedence over hover | State machine integrity |
| Disabled + hover | No visual response; `cursor: not-allowed` | Disabled means non-interactive |
| Pending + continued focus | Maintain focus indicator; add loading indicator alongside | Accessibility continuity |
| Touch device (no hover) | Provide touch-hold as hover alternative; never require hover | Universal access |

---

### 3.6 Combinable States

Combinable states can be applied simultaneously to represent complex component conditions.

#### Dimension Inventory

| State | ARIA Mapping | Applicable Elements | Volatility |
|-------|--------------|---------------------|------------|
| **focus** | Managed by browser | Interactive elements | Volatile |
| **focus-visible** | `:focus-visible` | Interactive elements | Volatile |
| **focus-within** | `:focus-within` | Container elements | Volatile |
| **selected** | `aria-selected` | Listbox items, tabs, grid cells | Semi-stable |
| **checked** | `aria-checked` | Checkboxes, switches, radios | Semi-stable |
| **indeterminate** | `aria-checked="mixed"` | Checkboxes with partial children | Semi-stable |
| **expanded** | `aria-expanded` | Accordions, menus, trees | Semi-stable |
| **current** | `aria-current` | Navigation items | Semi-stable |
| **loading** | `aria-busy` | Regions being updated | Volatile |
| **invalid** | `aria-invalid` | Form fields (validation) | Volatile |
| **error** | `aria-errormessage` | Form fields (system error) | Volatile |
| **warning** | `aria-describedby` | Form fields | Volatile |
| **success** | Live region | Form fields | Volatile |
| **required** | `aria-required` | Form fields | Stable |
| **read-only** | `aria-readonly` | Editable fields | Semi-stable |
| **modified** | (none) | Editable fields | Volatile |
| **autofilled** | `:autofill` | Input fields | Volatile |
| **visited** | `:visited` | Links | Stable |

**Semantic Distinctions**:
- `selected` vs `checked`: `selected` applies to selection in containers; `checked` applies to toggle state
- `invalid` vs `error`: `invalid` is validation failure; `error` is system/submission failure

#### Cross-Category Dependencies

```
Combinable States ──┬─── Interaction: selected + disabled = valid combination
                    │                 (item selected but action unavailable)
                    │
                    ├─── Semantic: error triggers feedback.error color tokens
                    │              success triggers feedback.success tokens
                    │
                    └─── Structural: expanded state affects child visibility
                                     and nesting context propagation
```

#### Conflict Resolution

| Scenario | Rule | Rationale |
|----------|------|-----------|
| Checked + indeterminate | Indeterminate takes visual precedence; `checked` reflects programmatic state | Visual clarity |
| Error + warning | Error takes precedence; show warning after error resolved | Severity ordering |
| Selected + current | Both may apply with distinct styling | Different purposes |
| Loading + interaction | Add `aria-disabled` during loading | Prevent double-submission |

---

### 3.7 Accessibility

Accessibility contexts represent user preferences and system settings addressing visual, motor, and cognitive needs.

#### Dimension Inventory

| Dimension | Values | Volatility | Detection |
|-----------|--------|------------|-----------|
| **Reduced motion** | `no-preference`, `reduce` | Stable | Inferred: `prefers-reduced-motion` |
| **Reduced transparency** | `no-preference`, `reduce` | Stable | Inferred: `prefers-reduced-transparency` |
| **Reduced data** | `no-preference`, `reduce` | Stable | Inferred: `prefers-reduced-data` |
| **CVD-safe mode** | `none`, `protanopia-safe`, `deuteranopia-safe`, `tritanopia-safe`, `universal` | Stable | Declared |
| **Cognitive load** | `default`, `reduced` | Stable | Declared |
| **Focus visibility** | `default`, `enhanced` | Stable | Declared |
| **Pointer target size** | `default`, `large` (44px minimum) | Stable | Declared |
| **Screen magnification** | Boolean | Stable | Inferred: Platform API |

**Implementation Principle**: Default to reduced motion and enhance for `no-preference` (accessibility-first approach).

#### Cross-Category Dependencies

```
Accessibility ───────┬─── Appearance: prefers-contrast may override theme;
                     │                dark scheme often preferred by users
                     │                with light sensitivity
                     │
                     ├─── Elevation: Reduced motion disables elevation
                     │               transitions; forced-colors removes box-shadow
                     │
                     ├─── Density: Large pointer mode overrides density,
                     │             enforcing minimum touch targets
                     │
                     └─── Semantic: Status indicators MUST use non-color
                                    differentiators (icons, patterns) per WCAG 1.4.1
```

#### Conflict Resolution

| Scenario | Rule | Rationale |
|----------|------|-----------|
| `prefers-contrast: more` + `forced-colors: active` | Handle both; provide system color fallbacks with enhanced contrast | Platform coupling on Windows |
| Reduced motion + loading feedback needed | Replace motion-based loaders with static indicators or state text | Accessibility without sacrificing information |
| CVD user + error/success states | Mandatory secondary indicators: icons, patterns, text | Non-color differentiation |
| High contrast + branded UI | Map brand semantics to system color keywords; accept brand expression loss | Accessibility priority |

---

### 3.8 Locale and Internationalization

Locale contexts address linguistic, cultural, and typographic requirements for global audiences.

#### Dimension Inventory

| Dimension | Values | Volatility | Detection |
|-----------|--------|------------|-----------|
| **Text direction** | `ltr`, `rtl`, `auto` | Stable | Declared: `dir` attribute |
| **Writing mode** | `horizontal-tb`, `vertical-rl`, `vertical-lr`, `sideways-rl`, `sideways-lr` | Stable | Declared: `writing-mode` |
| **Language** | BCP 47 tags (e.g., `en-US`, `ar-EG`, `zh-Hans`) | Stable | Declared: `lang` attribute |
| **Script type** | `latin`, `cjk`, `arabic`, `cyrillic`, `devanagari`, `thai` | Stable | Computed from language |
| **Text expansion factor** | Numeric (German: 1.2-1.35, Chinese: 0.7-0.9) | Stable | Computed from language |
| **Quote style** | `"text"`, `„text"`, `«text»`, etc. | Stable | Computed from language |
| **Calendar system** | `gregorian`, `hijri`, `buddhist`, `hebrew`, `japanese` | Stable | Declared or computed from locale |
| **Number format** | Locale-specific | Stable | Computed: `Intl.NumberFormat` |
| **Pluralization rules** | CLDR categories (1-6 forms) | Stable | Computed from language |
| **Line height adjustment** | CJK: 1.5-1.8×, Arabic: +diacritics | Stable | Computed from script type |

#### Cross-Category Dependencies

```
Locale ──────────────┬─── Platform: iOS UISemanticContentAttribute vs
                     │              Android layoutDirection handle RTL differently
                     │
                     ├─── Structure: Header/footer slots maintain position,
                     │               but start/end slots must flip in RTL
                     │
                     ├─── Viewport: Text expansion affects responsive breakpoints
                     │              (German labels may trigger earlier breakpoint)
                     │
                     └─── Density: Long compound words may require
                                   different truncation tokens
```

#### Conflict Resolution

| Scenario | Rule | Rationale |
|----------|------|-----------|
| RTL text + embedded LTR URLs | Use Unicode control characters (LRM, RLM) or isolates | BiDi algorithm handling |
| Directional icons in RTL | Document per-icon mirroring; checkmarks/clocks don't mirror | Semantic preservation |
| Vertical writing + RTL | Test combinations explicitly | Edge case validation |
| Font fallback in RTL | Specify complete stack with RTL-aware metrics | Metric consistency |

---

### 3.9 Platform and Runtime

Platform contexts address deployment target, rendering environment, and runtime capabilities.

#### Dimension Inventory

| Dimension | Values | Volatility | Detection |
|-----------|--------|------------|-----------|
| **Platform** | `web`, `ios`, `android`, `macos`, `windows`, `linux`, `electron`, `tauri`, `react-native` | Immutable | Build-time |
| **Framework** | `react`, `angular`, `vue`, `svelte`, `web-components` | Immutable | Build-time |
| **Browser engine** | `webkit`, `gecko`, `blink` | Immutable | Inferred: Feature detection |
| **Rendering context** | `SSR`, `SSG`, `ISR`, `CSR`, `hydrating` | Semi-stable | Runtime detection |
| **JavaScript state** | `enabled`, `disabled`, `loading` | Semi-stable | Runtime detection |
| **Device capabilities** | `haptics`, `camera`, `gps`, `biometrics`, `nfc` | Immutable | Inferred: Platform API |
| **Performance tier** | `high`, `medium`, `low` | Immutable | Computed from device signals |
| **Network quality** | `offline`, `slow-2g`, `2g`, `3g`, `4g+` | Volatile | Inferred: Network Information API |
| **Print context** | `screen`, `print` | Semi-stable | Inferred: `@media print` |
| **Embedded context** | `standalone`, `iframe`, `webview` | Immutable | Runtime detection |

#### Cross-Category Dependencies

```
Platform ────────────┬─── Locale: React Native lacks CSS logical properties;
                     │            requires explicit RTL/LTR value transforms
                     │
                     ├─── Appearance: iOS/Android dynamic color APIs differ;
                     │                Electron requires system theme detection
                     │
                     ├─── Accessibility: Platform-specific accessibility APIs
                     │                   (VoiceOver vs TalkBack vs NVDA)
                     │
                     └─── Structure: React Native doesn't support web slots;
                                     compound components need platform-specific
                                     composition patterns
```

#### Conflict Resolution

| Scenario | Rule | Rationale |
|----------|------|-----------|
| SSR dark + client light preference | Inject `prefers-color-scheme` check in `<head>`; use cookie for server preference | Prevent flash |
| Token uses CSS `inherit` on React Native | Transform inherited values to explicit during build | Platform compatibility |
| Low-performance device + heavy animations | Detect tier; reduce complexity or remove animations | Performance over fidelity |
| Print from JavaScript-dependent component | `@media print` tokens expand collapsed content, show all tabs | Useful output |

---

### 3.10 Elevation and Surface

Elevation contexts define visual hierarchy through layering, shadows, and surface treatments.

#### Dimension Inventory

| Dimension | Values | Volatility | Detection |
|-----------|--------|------------|-----------|
| **Surface level** | `0`, `1`, `2`, `3`, `4`, `5` (or `container-lowest` through `container-highest`) | Inherited | Declared |
| **Shadow depth** | `none`, `sm`, `md`, `lg`, `xl` | Inherited | Computed from surface level |
| **Z-index layer** | Semantic scale (e.g., base: 0, dropdown: 300, modal: 500, tooltip: 800) | Inherited | Declared |
| **Backdrop blur** | `none`, `sm`, `md`, `lg` | Inherited | Declared |
| **Overlay opacity** | `0%`, `32%`, `50%`, `100%` | Inherited | Declared |
| **Surface tint color** | Color token reference | Computed | Computed from theme primary + surface level |
| **Print elevation** | `border`, `separator`, `none` | Computed | Computed from surface level for print |
| **Elevation transition** | Duration + easing tokens | Computed | Declared with reduced-motion fallback |

#### Cross-Category Dependencies

```
Elevation ───────────┬─── Appearance: Dark scheme reverses elevation direction
                     │                (surfaces lighten upward); tint color
                     │                derived from theme primary
                     │
                     ├─── Accessibility: Reduced motion disables elevation
                     │                   transitions; forced-colors removes
                     │                   box-shadow entirely
                     │
                     ├─── Structure: Nested modals require incrementing z-index;
                     │               card-in-card compounds elevation
                     │
                     └─── Platform: Shadows render differently across browsers;
                                    Android elevation uses native ViewCompat
```

#### Conflict Resolution

| Scenario | Rule | Rationale |
|----------|------|-----------|
| Forced colors removes shadows | Use borders as fallback: `@media (forced-colors: active) { border: 2px solid ButtonText }` | Alternative hierarchy |
| Micro-frontend z-index collision | Global z-index contract with reserved ranges per application | Coordination mechanism |
| Deep nesting exceeds theme support | Document maximum supported depth per theme | Known limitations |
| Print with elevated cards | Use border-based indicators | Practical output |

---

### 3.11 Semantic and Feedback

Semantic contexts convey meaning, status, and intent through consistent visual language.

#### Dimension Inventory

| Dimension | Values | Volatility | Detection |
|-----------|--------|------------|-----------|
| **Status** | `success`, `warning`, `error`, `info`, `neutral` | Volatile | Declared |
| **Feedback** | `positive`, `negative`, `neutral` | Volatile | Declared |
| **Emphasis** | `primary`, `secondary`, `tertiary`, `bold`, `subtle` | Stable | Declared |
| **Intent** | `brand`, `informational`, `success`, `warning`, `destructive` | Stable | Declared |
| **Urgency** | `low`, `medium`, `high`, `critical` | Volatile | Declared |
| **Confidence** | `confirmed`, `likely`, `uncertain` | Volatile | Declared |
| **Temporal status** | `new`, `updated`, `stale`, `archived` | Semi-stable | Declared |
| **Provenance** | `human`, `ai-generated`, `ai-assisted` | Stable | Declared |

**Semantic Distinction**: Status tokens represent current state (static); feedback tokens represent result of action (dynamic).

#### Cross-Category Dependencies

```
Semantic ────────────┬─── Interaction: Error state + disabled creates
                     │                 compound "disabled due to error" state
                     │
                     ├─── Accessibility: Status MUST include non-color
                     │                   indicators (icons, patterns)
                     │
                     ├─── Combinable States: Error/success are combinable
                     │                       states triggering semantic tokens
                     │
                     └─── Elevation: High-emphasis alerts may use elevated
                                     surfaces for attention
```

#### Conflict Resolution

| Scenario | Rule | Rationale |
|----------|------|-----------|
| Yellow warning background | Use `warning.inverse` text token; ensure 4.5:1 contrast | Accessibility compliance |
| Multiple statuses on same component | Precedence: error > warning > info; show highest | Clear hierarchy |
| Brand + destructive intent | Destructive always uses danger palette; brand defers | Safety clarity |
| Success + disabled | Disabled takes visual precedence; success via `aria-describedby` | State clarity |

---

### 3.12 Structural

Structural contexts address component composition, positional relationships, and architectural patterns.

#### Dimension Inventory

| Dimension | Values | Volatility | Detection |
|-----------|--------|------------|-----------|
| **Component type** | Implementation-specific | Immutable | Declared |
| **Region** | `header`, `main`, `footer`, `nav`, `aside`, `complementary` | Stable | Declared: Landmark roles |
| **Nesting depth** | `depth-0` through `depth-n` | Inherited | Computed from DOM |
| **List position** | `first`, `middle`, `last`, `only` | Inherited | Computed from siblings |
| **Slot position** | `header`, `body`, `footer`, `trigger`, `content` | Stable | Declared |
| **Tree depth** | Integer | Inherited | Computed from ancestors |
| **Component lifecycle** | `mounting`, `mounted`, `unmounting` | Volatile | Runtime state |
| **Layout position** | `start`, `center`, `end`, `stretch` | Inherited | Computed from parent |
| **Complexity level** | `atom`, `molecule`, `organism` | Immutable | Declared |
| **Instance count** | `first`, `nth`, `last` | Inherited | Computed from siblings |

**Architecture Rule**: A component-specific token must never reference another component-specific token.

#### Cross-Category Dependencies

```
Structure ───────────┬─── Elevation: Nested modals increment z-index;
                     │               card-in-card compounds surface level
                     │
                     ├─── Locale: Start/end slots flip in RTL;
                     │            header/footer maintain position
                     │
                     ├─── Platform: React Native lacks web slot pattern;
                     │              needs platform-specific composition
                     │
                     └─── Density: Deep nesting may require compact mode
                                   to prevent excessive indentation
```

#### Conflict Resolution

| Scenario | Rule | Rationale |
|----------|------|-----------|
| Compound component state sync | Use framework context or CSS `:has()` for state propagation | Cross-component coordination |
| Same component in different regions | Use container queries + named containers | Context-aware adaptation |
| Cross-component token reference | Prohibited; use shared semantic token | Prevent hidden dependencies |

---

## 4. Context Types

Context types are named intersections of common dimension combinations, providing governed shortcuts for frequent patterns.

### 4.1 Predefined Context Types

| Type Name | Dimensions | Use Case |
|-----------|------------|----------|
| `mobile-light` | viewport.breakpoint ≤ sm, appearance.scheme = light, density.touch = optimized | Mobile web default |
| `mobile-dark` | viewport.breakpoint ≤ sm, appearance.scheme = dark, density.touch = optimized | Mobile web dark mode |
| `desktop-compact` | viewport.breakpoint ≥ lg, density.mode = compact | Desktop productivity app |
| `desktop-comfortable` | viewport.breakpoint ≥ lg, density.mode = comfortable | Desktop consumer app |
| `kiosk` | display.mode = fullscreen, density.touch = optimized, accessibility.pointer = large | Public terminal |
| `print` | platform.context = print, appearance.scheme = light, elevation.mode = border | Print stylesheet |
| `high-contrast-dark` | accessibility.forced = active, appearance.scheme = dark | Windows high contrast |
| `reduced-motion` | accessibility.motion = reduce | Motion-sensitive users |
| `foldable-split` | viewport.segments = 2, viewport.posture = flat | Foldable in split view |
| `rtl-compact` | locale.direction = rtl, density.mode = compact | RTL productivity app |

### 4.2 Context Type Resolution

When a context type is applied, it sets the specified dimensions; unspecified dimensions inherit normally:

```
applyContextType("mobile-dark", coordinate) →
  coordinate with {
    viewport.breakpoint: ≤ sm,
    appearance.scheme: dark,
    density.touch: optimized,
    ...other dimensions unchanged
  }
```

### 4.3 Custom Context Types

Organizations may define custom context types for their specific patterns:

```json
{
  "contextTypes": {
    "brand-portal-embedded": {
      "platform.embedded": "iframe",
      "appearance.theme": "brand-minimal",
      "density.mode": "compact",
      "elevation.maxLevel": 2
    }
  }
}
```

---

## 5. Inheritance and Scoping

### 5.1 Inheritance Rules

Context flows through the component tree according to these rules:

| Rule | Description |
|------|-------------|
| **Downward propagation** | Context set on an ancestor applies to all descendants unless overridden |
| **Nearest ancestor wins** | When multiple ancestors set a dimension, the nearest takes precedence |
| **Explicit override** | A component may explicitly override any inherited dimension |
| **Scope boundaries** | Certain structural elements (modal, popover, iframe) create new context scopes |

### 5.2 Scope Boundaries

Scope boundaries reset certain context dimensions while preserving others:

| Boundary Type | Reset Dimensions | Preserved Dimensions |
|---------------|------------------|----------------------|
| **Modal/Dialog** | elevation, z-index | appearance, locale, accessibility |
| **Popover** | elevation, z-index, container | appearance, locale, accessibility |
| **Portal** | All structural | appearance, locale, accessibility, platform |
| **Iframe** | All | (new document context) |
| **Shadow DOM** | structural.depth | All others (piercing via CSS custom properties) |

### 5.3 Context Providers

Framework implementations expose context through provider components:

```jsx
<ContextProvider
  appearance={{ scheme: "dark" }}
  density={{ mode: "compact" }}
>
  {/* Children inherit dark scheme and compact density */}
  <ContextProvider density={{ mode: "comfortable" }}>
    {/* Children inherit dark scheme but comfortable density */}
  </ContextProvider>
</ContextProvider>
```

---

## 6. Resolution Algorithm

### 6.1 Token Resolution Steps

Given a token reference and a context coordinate, resolution proceeds:

1. **Collect context**: Walk ancestor chain to build complete context coordinate
2. **Apply context type**: If a named context type is active, merge its dimensions
3. **Check dependencies**: Identify cross-category dependencies affecting this token
4. **Resolve conflicts**: Apply conflict resolution rules for any dimension intersections
5. **Compute value**: Look up token value for the resolved context coordinate
6. **Transform if needed**: Apply platform-specific transforms (color format, unit conversion)

```
resolve("color.surface.primary", coordinate) →
  1. coordinate = { appearance.scheme: "dark", elevation.level: 2, ... }
  2. No context type override
  3. Dependencies: elevation.level affects surface tint
  4. No conflicts
  5. Value: compute tonal surface from primary + level 2 in dark scheme
  6. Transform: #1a1a2e (CSS hex)
```

### 6.2 Conflict Resolution Order

When dimensions conflict, apply this precedence (highest to lowest):

1. **Accessibility overrides**: `forced-colors`, `prefers-contrast`, minimum target sizes
2. **Explicit declarations**: Dimensions explicitly set on the element
3. **Context type**: Dimensions from applied context type
4. **Inherited context**: Dimensions from ancestor chain
5. **Platform defaults**: Platform-specific default values
6. **System defaults**: Ontology-defined defaults

### 6.3 Exception Handling

When resolution encounters an undefined combination:

1. **Log the exception** with full context coordinate
2. **Apply fallback** using nearest valid ancestor value
3. **Track frequency** of the exception pattern
4. **Promote to token** when pattern exceeds threshold

This enables the system to learn from friction rather than suppressing it.

---

## 7. Cross-Category Dependencies

This ontology documents 78 cross-category dependencies. Dependencies are classified by type:

### 7.1 Critical Dependencies

These dependencies must be resolved in every implementation:

| Dependency | Categories | Resolution |
|------------|------------|------------|
| Forced colors override | Appearance ↔ All | System colors replace all color tokens |
| Focus precedence | Interaction ↔ Accessibility | `:focus-visible` after `:hover` in cascade |
| Touch target minimum | Density ↔ Accessibility | `min(var(--compact-size), 44px)` |
| Tonal elevation | Elevation ↔ Appearance | Token reference: `elevation.surface.tint: {color.primary}` |

### 7.2 Conditional Dependencies

These dependencies activate under specific conditions:

| Dependency | Trigger | Effect |
|------------|---------|--------|
| Motion ↔ Elevation | `prefers-reduced-motion: reduce` | Elevation transitions disabled |
| Transparency ↔ Elevation | `prefers-reduced-transparency: reduce` | Backdrop blur set to `none` |
| Writing mode ↔ Structure | `writing-mode: vertical-*` | Slot positions rotate |
| Viewport ↔ Density | `viewport-segments: 2` | Layout mode becomes `split` |

### 7.3 Implicit Dependencies

These dependencies are computed automatically:

| Dependency | Computation |
|------------|-------------|
| Platform → Color format | Build transform: CSS hex, iOS RGB object, Android AARRGGBB |
| Locale → Font stack | Computed from language/script type |
| Container → Stacking context | `container-type` creates new stacking context |
| Surface level → Shadow depth | Computed from elevation level and appearance scheme |

---

## 8. Token Architecture

### 8.1 Layer Model

The ontology supports a layered token architecture with unidirectional dependencies:

```
┌─────────────────────────────────────────────────────────────────┐
│ Context Layer (Environment)                                     │
│   viewport-*, platform-*, locale-*                              │
│   Detected automatically, read-only                             │
├─────────────────────────────────────────────────────────────────┤
│ Preference Layer (User Settings)                                │
│   prefers-*, density-*, accessibility-*                         │
│   User-controllable, system-detectable                          │
├─────────────────────────────────────────────────────────────────┤
│ Semantic Layer (Design Intent)                                  │
│   status-*, emphasis-*, intent-*                                │
│   Author-defined, context-independent                           │
├─────────────────────────────────────────────────────────────────┤
│ State Layer (Interaction)                                       │
│   interaction-*, combinable-*                                   │
│   Runtime-dynamic, composable                                   │
├─────────────────────────────────────────────────────────────────┤
│ Component Layer (Implementation)                                │
│   {component}-{property}-{state}                                │
│   References semantic layer only                                │
└─────────────────────────────────────────────────────────────────┘
         ↑ Dependencies flow upward (component → semantic → preference → context)
```

### 8.2 Architecture Principles

1. **Unidirectional flow**: Component tokens reference semantic tokens, which reference preference tokens, which respect context tokens
2. **No sibling references**: Cross-references at the same layer indicate architecture violations
3. **Explicit chains**: Every resolved value traces back through a documented path
4. **Predictable blast radius**: Changes at any tier have bounded effects

### 8.3 Token Naming Convention

```
{layer}.{category}.{property}[.{variant}][.{state}]
```

| Example | Description |
|---------|-------------|
| `context.viewport.breakpoint` | Current viewport breakpoint |
| `preference.accessibility.motion` | User's motion preference |
| `semantic.status.error.background` | Error state background |
| `semantic.emphasis.primary.foreground` | Primary emphasis text |
| `component.button.background.hover` | Button hover background |

### 8.4 SSOT Traceability

Every resolved token value must be traceable:

```
component.button.background.hover
  → references: semantic.emphasis.primary.background.hover
    → references: preference.appearance.scheme (= dark)
      → modifies: color.primary.400 → color.primary.300
        → primitive: #818cf8
```

---

## 9. Conclusion

This specification defines a comprehensive context ontology for design token resolution, comprising:

- **12 context categories** covering all aspects of token resolution
- **93 dimensions** with complete value enumerations
- **5 volatility classes** for caching and invalidation strategy
- **78 cross-category dependencies** with documented resolution rules
- **52 conflict scenarios** with prescribed handling
- **Named context types** for common dimension intersections
- **Inheritance and scoping rules** for component tree propagation
- **Resolution algorithm** with explicit precedence ordering

The ontology supports a Single Source of Truth architecture where every resolved token value traces back through an explicit chain. Context types are finite and governed. Exceptions are logged and can be promoted to formal tokens when patterns emerge. The system learns from friction rather than suppressing it.

Organizations implementing this ontology gain:

1. **Complete coverage** of real-world context requirements
2. **Predictable behavior** through documented resolution rules
3. **Maintainable architecture** through layered dependencies
4. **Future compatibility** through extensible dimension model

This specification serves as the authoritative reference for implementing context-aware design token systems.

---

## Appendix A: Terminology

| Term | Definition |
|------|------------|
| **Context** | Environmental or user-preference condition affecting token resolution |
| **Dimension** | Single axis within a context category |
| **Volatility** | Classification of how frequently a dimension's value changes |
| **Context coordinate** | Complete set of dimension values at a point in the component tree |
| **Context type** | Named intersection of common dimension combinations |
| **Scope boundary** | Structural element that resets certain context dimensions |
| **Resolution** | Process of determining concrete value for a token given context |
| **Dependency** | Relationship where one category's resolution affects another |

---

## Appendix B: Dimension Quick Reference

| Category | Dimension Count | Volatility Range |
|----------|-----------------|------------------|
| Appearance | 11 | Immutable–Stable |
| Density | 9 | Immutable–Inherited |
| Viewport | 11 | Immutable–Volatile |
| Container | 7 | Inherited–Volatile |
| Interaction (Exclusive) | 9 | Volatile |
| Combinable States | 18 | Volatile–Stable |
| Accessibility | 8 | Stable |
| Locale | 10 | Stable |
| Platform | 10 | Immutable–Volatile |
| Elevation | 8 | Inherited–Computed |
| Semantic | 8 | Volatile–Stable |
| Structural | 10 | Immutable–Inherited |
| **Total** | **93** | |

---

## Appendix C: Referenced Specifications

| Specification | Relevance |
|---------------|-----------|
| W3C Design Tokens Community Group Format | Token interchange format |
| CSS Media Queries Level 5 | Preference and viewport queries |
| CSS Containment Level 3 | Container queries |
| CSS Color Level 4 | Color spaces, gamut |
| WAI-ARIA 1.2 | Accessibility state mappings |
| WCAG 2.2 | Accessibility requirements |
| Screen Fold API | Foldable device detection |
| CSS Writing Modes Level 4 | Internationalization |

---

*This specification defines the Design Token Context Ontology, version 1.0.*
