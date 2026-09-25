# Empty State Illustration Kit → ORVIA Mapping

**Source:** Figma Community file `KxeJeFASeMwg6EiW0EzeUp`  
**Inspected node:** `71:17946`  
**Status:** partial visual/structural extraction

This source is useful for a different reason than Material or Flutter DevTools. It does not give us authoritative spacing, motion, accessibility, or implementation tokens. It gives us **semantic illustration patterns for system states**.

## What the source actually exposes

The Figma metadata identifies a broad set of named states, including:

- No Message
- No Internet
- Wallet is Empty
- No Results
- No Chat History
- No Upcoming Trips
- network connection failure
- Payment successfully
- Payment Failed
- Search with no results
- Camera Access
- Enable Push Notifications
- Enable Location

That makes the kit useful as evidence for **state-family coverage**.

## Directly inspected visual samples

Four illustrations were rendered directly during this pass.

### No Results

The illustration uses one dominant metaphor: a document combined with a magnifier. Small decorative marks surround the object, while one warm accent carries most of the visual emphasis.

### Camera Access

The semantic object is centered inside concentric dashed rings. The rings behave as a contextual device rather than as the subject itself.

### Enable Location

The permission-state construction again uses the orbital/ring treatment around the location/map metaphor. This repetition suggests a shared visual grammar for at least some permission states.

### Payment Failed

Instead of a literal payment icon, this illustration uses a narrative mini-scene with a person and a red flag on a simplified terrain surface. It therefore communicates failure through story/metaphor rather than only iconography.

## Important distinction

The source demonstrates that **empty-state semantics and illustration composition are separate choices**.

A useful ORVIA resolver would therefore look more like:

```text
SYSTEM STATE
    ↓
SEMANTIC FAMILY
    ↓
METAPHOR MODE
    ↓
NARRATIVE COMPLEXITY
    ↓
BACKDROP MODE
    ↓
ACCENT STRATEGY
    ↓
ILLUSTRATION IMPLEMENTATION
```

rather than:

```text
empty state
    ↓
generic illustration
```

## Candidate categorical controls

The source supports further validation of these ORVIA synthesis candidates:

```yaml
EMPTY_STATE_SEMANTIC_FAMILY:
  absence
  search_no_result
  connectivity
  transaction
  permission
  history_activity
  error_system

EMPTY_STATE_METAPHOR_MODE:
  direct_object
  ui_object
  mini_scene
  symbolic_system

EMPTY_STATE_BACKDROP_MODE:
  none
  orbital
  surface_scene

EMPTY_STATE_NARRATIVE_COMPLEXITY:
  iconic
  object_metaphor
  narrative_scene
```

These are **not source-native Figma variables**. They are ORVIA abstractions derived from the observed examples and must stay marked as synthesis until cross-source validation.

## Measurement layer

If ORVIA later audits empty-state illustrations automatically, useful deterministic evidence includes:

```yaml
illustration_bbox_to_slot_ratio:
dominant_subject_count:
decorative_particle_count:
accent_color_count:
accent_area_ratio:
backdrop_presence:
orbit_or_ring_count:
embedded_text_count:
```

Those measurements can support classification without asking an AI to decide only by impression whether an illustration is "simple" or "busy."

## What this source does not justify

This pass does not support:

- exact color tokens;
- exact typography;
- universal empty-state dimensions;
- universal illustration style rules;
- accessibility thresholds;
- assumptions that the whole kit uses one identical construction.

The Figma connector exposed metadata successfully, but high-fidelity design context and variable extraction required an active selected layer for this community file. Further screenshot inspection then reached the Starter-plan MCP limit. The record therefore remains deliberately partial.

## ORVIA decision

Do not add a new numerical design dial from this kit.

Its strongest contribution is a **categorical semantic/composition layer** that can sit beneath empty-state generation and evaluation, while the existing ORVIA visual envelopes continue to govern broader page-level design.
