# ORVIA Inclusive Design Extraction — v0.1

This branch is the derived-work layer for the mirrored InclusiveTechLab source and related ORVIA architecture/research extraction.

## Branch contract

- `main` = upstream source preservation only.
- `orvia-inclusive-extraction-v0.1` = extraction, normalization, relationship analysis, architecture mapping, and future validation work.
- No source file is renamed, deleted, or rewritten as part of extraction.
- No extracted record becomes `ORVIA_CORE` automatically.
- Architecture references and design-system examples do not create new dials or thresholds automatically.

## Machine source of truth

1. `SOURCE_REGISTRY.yaml`
2. `RAW_EXTRACTIONS.yaml`
3. `PARAMETER_REGISTRY.yaml`
4. `RELATIONSHIP_RULES.yaml`
5. `PRESETS.yaml`

## Supplemental records

- `DIAGNOSTIC_TOOL_ARCHITECTURE_RECORD.md` — Flutter DevTools diagnostic architecture extraction.
- `DEVTOOLS_ARCHITECTURE_MAP.yaml` — machine-readable diagnostic architecture map.
- `MATERIAL_DENSITY_SOURCE_RECORD.yaml` — source-native Material 2 density rules, quantitative values, applicability and relationship candidates.
- `MATERIAL_DENSITY_MAPPING.md` — human-readable mapping from Material's density model to ORVIA `VISUAL_DENSITY`.
- `MATERIAL_COMPONENTS_ANDROID_IMPLEMENTATION_RECORD.yaml` — Material 3 Android implementation tokens for motion, shape, elevation, state feedback, touch targets, component size classes, and focus examples.
- `MATERIAL_COMPONENTS_ANDROID_ORVIA_MAPPING.md` — mapping from Material Components Android implementation tokens to ORVIA resolver layers.

## Critical provenance rule

A source-native number, an implementation example, a WCAG criterion, an architecture pattern, a design-system rule, a platform token, and an ORVIA orchestration score are different evidence types. They must never be silently merged.

## Current first-pass scope

This pass focuses on:
- exclusion taxonomy;
- human capability domains;
- permanent/temporary/situational reasoning;
- interaction precision and drag dependency;
- color-information dependency;
- focus lifecycle;
- action-label specificity;
- reduced-motion handling;
- task resumability;
- preference persistence;
- spatial stability;
- WCAG-vs-beyond-WCAG traceability;
- diagnostic-tool applicability gating;
- capture/normalization/replay separation;
- baseline-vs-candidate comparison;
- portable diagnostic evidence;
- modular diagnostic extension architecture;
- component density;
- grid/component density coupling;
- touch-target density constraints;
- typographic line-height density;
- context-dependent density applicability;
- motion duration/easing/spring token resolution;
- shape/corner token resolution;
- elevation/depth token resolution;
- interaction-state feedback tokens;
- coordinated component-size classes.

## Next validation stages

The current records are intentionally conservative. Next passes should:
- complete source-wide extraction;
- deduplicate against the existing ORVIA parameter registry;
- cross-check normative claims directly against current W3C/WCAG sources;
- compare legacy Material 2 density guidance against current Material guidance;
- implement measurement adapters before inventing additional dials;
- build a composite evidence model for `VISUAL_DENSITY`;
- build token resolvers for motion, shape, surface depth, and state feedback;
- test whether `MOTION_SCHEME`, `MOTION_SPEED_CLASS`, and `COMPONENT_SIZE_CLASS` remain independently useful across non-Material systems;
- connect normalized evidence to token/component/responsive resolvers;
- test implementation consequences;
- identify dependency/collision rules;
- promote only independently useful, validated controls.
