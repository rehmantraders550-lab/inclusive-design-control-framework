# ORVIA Inclusive Design Extraction — v0.1

This branch is the derived-work layer for the mirrored InclusiveTechLab source.

## Branch contract

- `main` = upstream source preservation only.
- `orvia-inclusive-extraction-v0.1` = extraction, normalization, relationship analysis, and future validation work.
- No source file is renamed, deleted, or rewritten as part of extraction.
- No extracted record becomes `ORVIA_CORE` automatically.

## Machine source of truth

1. `SOURCE_REGISTRY.yaml`
2. `RAW_EXTRACTIONS.yaml`
3. `PARAMETER_REGISTRY.yaml`
4. `RELATIONSHIP_RULES.yaml`
5. `PRESETS.yaml`

## Critical provenance rule

A source-native number, an implementation example, a WCAG criterion, and an ORVIA orchestration score are different evidence types. They must never be silently merged.

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
- WCAG-vs-beyond-WCAG traceability.

## Next validation stages

The current records are intentionally conservative. Next passes should:
- complete source-wide extraction;
- deduplicate against the existing ORVIA parameter registry;
- cross-check normative claims directly against current W3C/WCAG sources;
- test implementation consequences;
- identify dependency/collision rules;
- promote only independently useful, validated controls.
