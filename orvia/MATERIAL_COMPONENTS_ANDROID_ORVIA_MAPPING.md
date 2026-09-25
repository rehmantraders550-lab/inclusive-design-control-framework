# Material Components Android → ORVIA Mapping

**Pinned source:** `material-components/material-components-android@60ff09436d5d477a4b9d02940f31eb01e1250620`  
**Observed generated token version:** `34.0.0`

This repository is particularly valuable because it turns design-system ideas into **machine-readable implementation tokens**. It therefore sits between abstract ORVIA controls and actual component code.

## Main architectural contribution

The repository demonstrates that a mature design system should resolve broad design intent through coordinated token families:

```text
ORVIA CONTROL
      ↓
SEMANTIC MODE / CLASS
      ↓
SYSTEM TOKENS
      ↓
COMPONENT TOKENS
      ↓
PLATFORM IMPLEMENTATION
```

This is a stronger model than mapping a `1..10` dial directly to arbitrary CSS.

## Motion

The Android implementation exposes a duration ladder from **50ms to 1000ms**, named from `short1` through `extra_long4`.

It also defines multiple easing families and two spring schemes:

- **standard**
- **expressive**

Each spring scheme has **fast / default / slow** profiles, and each profile differentiates **spatial** motion from **effects**.

This is important for ORVIA because it proves that perceived motion cannot be represented by duration alone.

A more correct resolver is:

```text
MOTION_TEMPO
   ↓
duration class
+ easing family
+ spring profile
+ spatial/effect role
```

The source therefore strengthens the existing `MOTION_TEMPO` control rather than creating a competing numeric dial.

## Shape

Material's Android shape scale is explicit:

```text
none                 0dp
extra-small          4dp
small                8dp
medium              12dp
large               16dp
large-increased     20dp
extra-large         28dp
extra-large+        32dp
extra-extra-large   48dp
full                50%
```

The `50%` Full token is important: it shows why ORVIA `CORNER_ROUNDNESS` should remain a semantic magnitude rather than assuming every shape resolves to a fixed absolute radius.

## Surface depth

Material 3 Android defines six elevation levels:

```text
0 → 0dp
1 → 1dp
2 → 3dp
3 → 6dp
4 → 8dp
5 → 12dp
```

These are excellent source-native implementation anchors for `SURFACE_DEPTH`, but they remain Material/Android values, not universal elevation thresholds.

## Interaction states

The base Material 3 state tokens include:

```text
hover    0.08
focus    0.10
pressed  0.10
dragged  0.16
disabled 0.38
```

However, Android-specific resource variants intentionally change some values to compensate for `RippleDrawable` rendering behavior.

That produces a critical ORVIA rule:

> Preserve the semantic state target separately from the platform-specific rendering constant.

In practical terms, ORVIA should diagnose whether a state is perceivable and then let a platform resolver decide how to produce it.

## Touch target

The implementation defines:

```text
mtrl_min_touch_target_size = 48dp
```

This corroborates the 48dp touch-target guidance already captured from the Material 2 density source. It strengthens our evidence but still should not be promoted as a universal accessibility hard guardrail until current normative standards are cross-checked.

## Component size classes

The strongest new finding is the button-size token architecture.

Material defines:

```text
XSMALL
SMALL
MEDIUM
LARGE
XLARGE
```

The class does not merely change one height. It coordinates:

- icon size;
- leading/trailing space;
- icon-label spacing;
- typography role;
- outline width;
- corner role;
- pressed/selected shape behavior.

For example, icon size progresses `20 → 20 → 24 → 32 → 40dp`, while leading/trailing space progresses `12 → 16 → 24 → 48 → 64dp`.

This gives ORVIA a very useful implementation principle:

```text
COMPONENT SCALE ≠ one dimension

COMPONENT SCALE =
  geometry
+ iconography
+ typography
+ spacing
+ stroke
+ shape
```

This should eventually become a resolver layer rather than another free-floating visual dial.

## Candidate categorical controls

Three source-backed categorical candidates are now worth validating:

```yaml
MOTION_SCHEME:
  standard | expressive

MOTION_SPEED_CLASS:
  fast | default | slow

COMPONENT_SIZE_CLASS:
  xsmall | small | medium | large | xlarge
```

They are **not promoted to ORVIA_CORE** yet.

## Relationship to the Material density extraction

The previous Material 2 density record told us **when and why** density changes should be applied.

This Android repository tells us **how a production component system coordinates actual values**.

Together:

```text
MATERIAL DENSITY GUIDANCE
context + applicability + density relationships
                 ↓
ORVIA VISUAL_DENSITY
                 ↓
COMPONENT SIZE CLASS
                 ↓
coordinated token bundle
                 ↓
platform component implementation
```

This materially closes the gap between an abstract density score and implementable component behavior.

## ORVIA decision

No existing ORVIA numerical dial should be replaced by Material's native scales.

Instead, this source should feed the **token-resolution layer**:

- `MOTION_TEMPO` → motion duration/easing/spring resolver
- `CORNER_ROUNDNESS` → shape token resolver
- `SURFACE_DEPTH` → elevation/depth resolver
- `INTERACTION_EXPLICITNESS` → state-feedback resolver
- `VISUAL_DENSITY` → component-size and spacing resolver
- `INPUT_PRECISION_DEMAND` → target-geometry validator

That is the useful abstraction to preserve.
