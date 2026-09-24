# Material Design 2 Density — ORVIA Mapping Note

This source is materially more useful for **VISUAL_DENSITY** than Flutter DevTools because it contains source-native quantitative density behavior rather than only diagnostic architecture.

## Core finding

Material 2 does not treat density as one vague aesthetic score. It separates at least four interacting mechanisms:

1. **component compactness** — component dimensions decrease in discrete steps;
2. **layout/grid spaciousness** — margins and gutters counterbalance dense components;
3. **interactive target geometry** — touchability places a floor under density;
4. **typographic line-height** — text density can be adjusted independently.

That means ORVIA should not calculate `VISUAL_DENSITY` from a single proxy such as average spacing.

A more defensible model is:

```text
VISUAL_DENSITY
   ├─ component compactness
   ├─ information count / viewport
   ├─ grid / gutter compactness
   ├─ typographic compression
   ├─ target spacing
   └─ disclosure / visible-control count
```

Material only supplies authoritative first-party evidence for some of those subdimensions.

## Source-native numeric controls

Material's density scale starts at `0` for default density. Higher component density moves through negative values such as `-1`, `-2`, and `-3`. Each step decreases component height by **4dp**.

This is an **inverse scale**, so it must not be directly copied into ORVIA's `1..10` envelopes.

The source also gives geometry constraints and examples:

- stacked internal elements use **4dp increments**;
- default touch targets are **48 × 48dp** with **8dp** separation for touch-capable contexts;
- the page notes **44 × 44dp** for iOS;
- line height is explicitly treated as a density mechanism;
- the page demonstrates that an overly dense chip at `-4` breaks, but that is a component example rather than a universal system minimum.

## Most important relationship rule

Material explicitly warns against increasing **component density** and **grid density** together.

As components become denser, margins and gutters should become more generous. This is a high-value ORVIA relationship because it shows that density controls are **coupled rather than independent**.

Proposed relationship:

```text
component_density ↑
        ⇒
layout_grid_density ↓
        ⇒
margins/gutters ↑
```

The purpose is to preserve scanning and grouping.

## Applicability

Higher density is positioned for contexts such as data-rich applications, tables, and long forms, where seeing more information at once improves relational context.

The same source discourages aggressive density in focused or precision-sensitive interactions and examples such as date-picker targeting or warning dialogs where readability suffers.

This should become an **applicability gate**, not merely an aesthetic preference.

## ORVIA consequence

Do **not** create a second competing `VISUAL_DENSITY` dial.

Instead:

- retain the existing `VISUAL_DENSITY: 1..10` orchestration envelope;
- add deterministic measurements beneath it;
- preserve Material's own density scale as source-native evidence;
- derive ORVIA classifications only after context, input mode, target geometry, and grid counterbalance are known.

Candidate measurement layer:

```yaml
visual_density_evidence:
  visible_controls_per_viewport:
  visible_information_units_per_viewport:
  mean_component_height:
  component_height_vs_baseline:
  median_vertical_gap:
  margin_to_viewport_ratio:
  gutter_to_column_ratio:
  line_height_to_font_size_ratio:
  min_interactive_target:
  min_target_gap:
```

This gives the AI observable evidence instead of asking it to decide that a page "looks dense."

## Governance note

This is **Material Design 2** guidance. It is first-party design-system evidence, but it should not be elevated into a universal or current cross-platform standard without checking current Material, WCAG/W3C, and platform-specific guidance.

The numerical values have therefore been recorded as source-native evidence and candidates, not silently promoted to ORVIA core.
