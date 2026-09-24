# Fluent UI React Accordion Snapshot → ORVIA Mapping

**Source:** user-provided Accordion documentation snapshot  
**Version/commit:** not supplied  
**Package identifiers visible in the source:** `@fluentui/react-components`, `@fluentui/react-icons`

This source is useful for ORVIA primarily as a **component-behavior contract**, not as a source of universal numerical design envelopes.

## 1. State architecture

The Accordion exposes three separable state dimensions:

```text
STATE OWNERSHIP
controlled ↔ uncontrolled

DISCLOSURE CONCURRENCY
single-open ↔ multiple-open

CLOSURE POLICY
at-least-one-open ↔ all-closed-allowed
```

That separation is valuable. ORVIA should not represent "accordion behavior" with one generic setting.

The source also makes controlled behavior explicit through the `openItems` + `onToggle` pair, while `defaultOpenItems` represents uncontrolled initialization.

## 2. Pattern-fit rule

The most important source-native interaction rule is not a styling rule at all:

```text
IF arrow-key navigation is a hard requirement
THEN prefer Tree
RATHER THAN forcing arrow navigation into Accordion
```

This is a strong example of an **applicability resolver**. ORVIA should first decide whether a component pattern fits the interaction requirement before tuning its visual parameters.

## 3. Semantic structure

The header can render as `h1` through `h6`, and the source recommends a proper heading level in markup.

This should be treated as a semantic-structure guardrail candidate:

```text
visual disclosure header
        ↓
document hierarchy check
        ↓
semantic heading assignment
```

The exact heading level cannot be selected from the component in isolation; it depends on document hierarchy.

## 4. Component sizing

AccordionHeader provides four source-native size classes:

```text
small
medium
large
extra-large
```

The source describes these as heading-spacing sizes but does not supply exact dimensions.

This is useful cross-system evidence for ORVIA's emerging semantic component-size layer, especially because Material Components Android independently exposed named component size classes. However, the vocabularies differ, so ORVIA should normalize by semantic magnitude rather than copy names blindly.

## 5. Motion

`AccordionPanel.collapseMotion` can resolve at least:

```text
duration
easing
opacity animation
custom collapse rendering
```

The source's interactive example initializes duration at **1000ms** and exposes a demo slider from **100ms to 2000ms in 50ms steps**, using `motionTokens.curveDecelerateMid`.

Those values are **demo controls**, not recommended motion thresholds.

The useful ORVIA lesson is therefore structural:

```text
MOTION_TEMPO
   ≠ duration alone

collapse motion =
  duration
+ easing
+ opacity behavior
+ implementation strategy
```

This reinforces the resolver architecture already extracted from Material Components Android.

## 6. Density interaction

Accordion disclosure can change how much information is simultaneously visible.

```text
MULTIPLE_OPEN
    → potentially more visible content
    → higher local information exposure

SINGLE_OPEN
    → bounded visible disclosure
```

This relationship is an ORVIA inference, not a density rule stated by the source. It should therefore feed `VISUAL_DENSITY` evidence only after the rendered state is actually measured.

## 7. Candidate controls and guardrails

The source strengthens four candidates without promoting them:

```yaml
DISCLOSURE_CONCURRENCY:
  single_open | multiple_open

DISCLOSURE_COLLAPSIBILITY:
  all_closed_allowed | all_closed_not_allowed

SEMANTIC_HIERARCHY:
  validate heading role against document structure

NAVIGATION_PATTERN_FIT:
  reject accordion when required interaction model is tree-like
```

`COMPONENT_SIZE_CLASS` also gains additional cross-system evidence, but no universal class vocabulary is promoted.

## 8. What this source does not establish

The snapshot does not provide enough evidence for:

- exact header spacing dimensions;
- a default motion duration;
- reduced-motion handling;
- touch-target size;
- focus-ring geometry;
- color or contrast requirements;
- universal accordion accessibility compliance.

Those remain unresolved rather than inferred.

## 9. ORVIA consequence

The deeper architectural value of this component is:

```text
USER TASK / INTERACTION REQUIREMENT
                ↓
PATTERN FIT
                ↓
SEMANTIC STRUCTURE
                ↓
STATE MODEL
                ↓
SIZE / ICON / DISCLOSURE OPTIONS
                ↓
MOTION RESOLUTION
                ↓
RENDERED EVIDENCE
                ↓
VALIDATION
```

That is more useful than treating an accordion as a visual component with a few CSS knobs.
