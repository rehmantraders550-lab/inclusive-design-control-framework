# ORVIA Diagnostic Tool Architecture Record v0.1

**Status:** ARCHITECTURE REFERENCE — no parameter promotion  
**Primary architecture source:** Dart & Flutter DevTools  
**Pinned source commit:** `405689bc49f9c7ae0d7ca77b3d9b0f698745989b`  
**Mapped against:** ORVIA Design Parameter Extraction Protocol v0.1.0 DRAFT, the current ODVE numerical/categorical control model, and the InclusiveTechLab extraction already present on this branch.

---

## 1. Purpose

This record extracts reusable diagnostic architecture from Flutter DevTools without copying Flutter-specific product behavior into ORVIA.

The objective is to strengthen the missing bridge between:

```text
design control
→ measurable evidence
→ applicability
→ classification
→ diagnosis
→ implementation consequence
→ validation
```

This record deliberately **does not add a new numerical dial**.

Flutter DevTools is treated as implementation evidence for how a mature diagnostic platform structures tools, evidence, replay, comparison, and extensibility. It is not treated as a normative accessibility or UI/UX standard.

---

## 2. Source boundary

### 2.1 Evidence files used

| Source file | Architecture evidence extracted |
|---|---|
| `shared/framework/screen.dart` | Tool identity and applicability/capability metadata |
| `shared/offline/offline_data.dart` | Capture → serialize → replay separation |
| `screens/performance/performance_screen.dart` | Runtime performance diagnostic workflow and offline loading |
| `screens/accessibility/accessibility_controller.dart` | Semantics-tree inspection and accessibility-state controls |
| `screens/app_size/app_size_screen.dart` | Analysis vs baseline/candidate diff workflow |
| `screens/network/har_builder.dart` | Portable standards-based diagnostic artifact generation |
| `devtools_extensions/README.md` | Modular extension model, simulated environment, distribution workflow |
| `devtools_extensions/extension_config_spec.md` | Minimum metadata contract for pluggable tools |
| `AGENTS.md` | Repository-level quality, reuse, documentation, test, and UI implementation discipline |

### 2.2 Authority limit

The source proves architecture and implementation patterns inside Flutter DevTools. It does **not** prove:

- universal UX thresholds;
- universal accessibility criteria;
- a numerical definition for `DESIGN_VARIANCE`, `VISUAL_DENSITY`, or `MOTION_INTENSITY`;
- web-browser performance thresholds;
- cross-framework applicability of Flutter VM-service behavior.

Any ORVIA use of these patterns must therefore remain tagged as `ARCHITECTURE_REUSE` unless independently validated by a normative or research source.

---

## 3. Reusable architecture patterns

### DTA-001 — Applicability gate before execution

Flutter DevTools describes each screen/tool with explicit capability metadata rather than assuming that every diagnostic applies everywhere.

Observed metadata includes:

```text
requiresConnection
requiresDartVm
requiresFlutter
requiresDebugBuild
requiresAdvancedDeveloperMode
supportsWebServerDevice
worksWithOfflineData
requiresLibrary
```

### ORVIA adoption

Every diagnostic control should gain an applicability block before evaluation:

```yaml
applicability:
  interface_classes: []
  input_modes: []
  platform_requirements: []
  runtime_requirements: []
  source_requirements: []
  activation_conditions: []
  exclusions: []
```

This should be evaluated **before** score resolution. A non-applicable control should return `NOT_APPLICABLE`, not a low score or a warning.

This directly prevents false-positive advice such as grading hover behavior on touch-only UI or treating drag constraints as relevant where no drag interaction exists.

---

### DTA-002 — Separate capture from interpretation

Flutter DevTools can serialize screen-specific diagnostic data as JSON, reload it later, and reconstruct the analysis without requiring the original live connection.

The reusable pattern is:

```text
LIVE SYSTEM
   ↓
CAPTURE
   ↓
SERIALIZABLE EVIDENCE
   ↓
NORMALIZATION
   ↓
ANALYSIS / REPLAY
```

### ORVIA adoption

ORVIA should not bind AI interpretation directly to a live browser session. A capture adapter should produce a normalized evidence package that can be:

- replayed;
- compared;
- re-scored after rules change;
- inspected by a second model;
- retained for audit.

Minimum evidence envelope:

```yaml
evidence:
  capture_id:
  captured_at:
  source_surface:
  adapter:
  adapter_version:
  viewport:
  input_mode:
  raw_artifacts: []
  normalized_metrics: {}
  observations: []
  provenance: []
```

This architecture is especially important for preserving source authenticity and reducing repeated crawling/rendering work.

---

### DTA-003 — Runtime measurements remain distinct from aesthetic controls

The DevTools Performance surface separates controls, Flutter frames, frame analysis, rebuild statistics, and timeline events.

This proves a strong diagnostic principle:

```text
event
→ measurement
→ classification
→ root-cause inspection
→ evidence
```

It does **not** prove that runtime performance and aesthetic motion magnitude are the same variable.

### ORVIA adoption

Keep the current visual controls intact:

- `MOTION_INTENSITY`
- `MOTION_TEMPO`

Add runtime evidence underneath them rather than redefining them.

Example:

```yaml
control: MOTION_INTENSITY
declared_value: 7

runtime_evidence:
  frame_stability: ...
  long_frame_count: ...
  motion_event_count: ...
  concurrent_motion_regions: ...
  scroll_linked_motion_present: ...

result:
  aesthetic_value: 7
  runtime_validation: PASS | WARN | FAIL
```

A high motion value can be valid if execution remains stable and accessibility guardrails are respected. Performance metrics therefore **constrain** the control; they do not replace it.

---

### DTA-004 — Baseline vs candidate is a first-class analysis mode

The App Size tool explicitly separates:

- `Analysis`
- `Diff`

Its diff workflow accepts old/new artifacts and can inspect combined, increase-only, or decrease-only change views.

### ORVIA adoption

All measurable design refinement should support:

```text
BASELINE ↔ CANDIDATE
```

rather than only a single current-state score.

Recommended comparison envelope:

```yaml
comparison:
  baseline_capture:
  candidate_capture:
  metric_deltas: {}
  parameter_deltas: {}
  guardrail_regressions: []
  improvements: []
  unresolved_changes: []
```

This is architecture reuse only. DevTools does not define ORVIA's design metrics.

---

### DTA-005 — Portable evidence artifact

The Network tool converts request data into HAR 1.2 JSON.

The strategic lesson is that evidence should have a portable representation independent of the visual diagnostic interface.

### ORVIA adoption

Every capture adapter should emit either:

1. an established external format where one exists; or
2. a versioned ORVIA JSON/YAML evidence schema.

The UI, report, and AI interpretation should all consume the same evidence artifact.

---

### DTA-006 — Diagnostic plugins need a minimum metadata contract

DevTools extensions use a small configuration contract with fields such as:

```text
name
issueTracker
version
materialIconCodePoint
requiresConnection
```

The extension runtime exposes managers for DevTools interaction, VM service access, and Dart Tooling Daemon interaction. DevTools also provides a simulated environment so an extension can be developed and tested without repeatedly running inside the full host environment.

### ORVIA adoption

ORVIA diagnostic modules should be plugins with an explicit contract:

```yaml
tool:
  id:
  version:
  domain:
  description:
  applicability:
  required_inputs:
  capture_adapter:
  output_schema:
  parameters_evaluated: []
  guardrails_evaluated: []
  offline_replay: true|false
  comparison_support: true|false
  implementation_targets: []
```

A tool should not be allowed to silently add a parameter. Parameters remain governed by the parameter registry.

---

### DTA-007 — Accessibility inspection and simulation must be separated

The current Flutter DevTools accessibility controller exposes a semantics tree and control state for brightness, text scale, bold text, screen reader, and high contrast.

At the pinned commit:

- semantics-tree loading is implemented;
- brightness override is wired to a service extension;
- text-scale override is still marked TODO;
- bold-text override is still marked TODO;
- screen-reader/semantics-debugger override is still marked TODO;
- high-contrast override is still marked TODO.

### ORVIA adoption

Never equate a visible diagnostic control with a completed validator.

Accessibility tooling records should separately declare:

```yaml
capability:
  inspect: IMPLEMENTED | PARTIAL | UNSUPPORTED
  simulate: IMPLEMENTED | PARTIAL | UNSUPPORTED
  measure: IMPLEMENTED | PARTIAL | UNSUPPORTED
  validate: IMPLEMENTED | PARTIAL | UNSUPPORTED
```

This prevents a UI toggle from being mistaken for verified accessibility coverage.

---

## 4. Mapping to the existing ORVIA controls

The table below distinguishes **measurement support** from **architecture relevance**.

| Existing control | DevTools contribution | Mapping status |
|---|---|---|
| `DESIGN_VARIANCE` | Baseline/candidate diff architecture can compare derived design features, but DevTools supplies no design-variance metric | ARCHITECTURE_ONLY |
| `VISUAL_DENSITY` | Diff/replay architecture can carry density metrics once ORVIA defines a capture adapter | ARCHITECTURE_ONLY |
| `MOTION_INTENSITY` | Performance evidence can constrain implementation stability; does not define aesthetic magnitude | INDIRECT_VALIDATION |
| `MOTION_TEMPO` | Timeline/frame evidence can validate implementation timing/cost; does not define desired tempo | INDIRECT_VALIDATION |
| `RESPONSIVE_RECOMPOSITION` | Applicability and capture architecture are reusable; DevTools does not define cross-viewport recomposition scoring | ARCHITECTURE_ONLY |
| `INTERACTION_EXPLICITNESS` | Semantics labels may provide evidence for discoverability/meaning, but not a complete explicitness metric | PARTIAL_EVIDENCE |
| `ACTION_LABEL_SPECIFICITY` | Semantics-tree labels can become inspectable evidence for label clarity | PARTIAL_EVIDENCE |
| `FOCUS_ESCAPE_RESILIENCE` | No direct complete validator identified in the inspected source | UNSUPPORTED_BY_SOURCE |
| `INPUT_PRECISION_DEMAND` | No direct target-size/precision measurement identified in the inspected source | UNSUPPORTED_BY_SOURCE |
| `DRAG_DEPENDENCY` | No direct interaction-alternative validator identified in the inspected source | UNSUPPORTED_BY_SOURCE |
| `COLOR_INFORMATION_DEPENDENCY` | Accessibility tooling architecture is relevant, but no direct color-information dependency validator was identified | UNSUPPORTED_BY_SOURCE |
| `REDUCED_MOTION_SUPPORT` | Performance architecture is relevant, but the inspected accessibility controller does not implement a reduced-motion simulator/validator | UNSUPPORTED_BY_SOURCE |
| `TASK_RESUMABILITY` | Offline diagnostic replay is not equivalent to end-user task resumability | NON_EQUIVALENT |
| `PREFERENCE_PERSISTENCE` | Diagnostic state persistence is not evidence of product preference persistence | NON_EQUIVALENT |
| `SPATIAL_STABILITY` | No direct navigation-position stability validator identified | UNSUPPORTED_BY_SOURCE |

This mapping is intentionally conservative.

---

## 5. Relationship to the InclusiveTechLab extraction

The InclusiveTechLab source currently contributes **user-exclusion and accessibility mismatch evidence**, including:

- perceivable / operable / understandable categorization;
- permanent / temporary / situational reasoning;
- input-precision demand;
- drag dependency;
- color-information dependency;
- focus escape resilience;
- action-label specificity;
- reduced-motion support;
- task resumability;
- preference persistence;
- spatial stability.

Flutter DevTools contributes a different layer:

```text
InclusiveTechLab
    = WHAT kinds of mismatches and exclusions matter

Flutter DevTools
    = HOW a diagnostic platform can gate, capture, replay,
      compare, serialize, and extend measurement tools
```

They should therefore be combined as:

```text
SOURCE-NATIVE USER NEED / MISMATCH
             ↓
ORVIA PARAMETER
             ↓
APPLICABILITY GATE
             ↓
MEASUREMENT ADAPTER
             ↓
NORMALIZED EVIDENCE
             ↓
RELATIONSHIP RULES
             ↓
CLASSIFICATION
             ↓
DIAGNOSIS
             ↓
IMPLEMENTATION RESOLUTION
             ↓
BASELINE ↔ CANDIDATE VALIDATION
```

This is the main architectural gain from the DevTools study.

---

## 6. Required ORVIA diagnostic record schema

Before adding more dials, diagnostic records should be able to express the following:

```yaml
diagnostic_record:
  id:
  version:
  domain:

  applicability:
    interface_classes: []
    platforms: []
    input_modes: []
    runtime_requirements: []
    activation_conditions: []
    exclusions: []

  inputs:
    required: []
    optional: []

  capture:
    adapter:
    adapter_version:
    live_supported:
    offline_replay_supported:
    baseline_diff_supported:

  evidence:
    raw_artifacts: []
    normalized_metrics: {}
    observations: []
    provenance: []

  evaluation:
    parameters: []
    hard_guardrails: []
    thresholds_or_anchors: {}
    relationship_rules: []

  result:
    classification:
    severity:
    confidence:
    diagnosis:
    remediation: []

  implementation:
    tokens: []
    components: []
    responsive_behavior: []
    interaction_behavior: []

  governance:
    source_ids: []
    source_class:
    status:
    revalidation_trigger: []
```

---

## 7. Validation contract

A diagnostic result should progress through explicit gates:

```text
G0 SOURCE
Is the claim grounded and correctly classified?

G1 APPLICABILITY
Does this diagnostic actually apply?

G2 CAPTURE
Was required evidence captured deterministically?

G3 NORMALIZATION
Were source-native values preserved before normalization?

G4 MEASUREMENT
Can the metric be reproduced without model intuition?

G5 PARAMETER MAP
Does the evidence genuinely support the named ORVIA control?

G6 RELATIONSHIPS
Do dependencies, collisions, caps, or overrides alter the result?

G7 IMPLEMENTATION
Can the result resolve into tokens/components/behavior?

G8 DIFF
Did the candidate improve without introducing regressions?
```

An AI may interpret evidence and propose remediation, but it should not manufacture a deterministic measurement that the adapter did not capture.

---

## 8. What should be adopted now

### Adopt

1. applicability metadata for every diagnostic;
2. capture/normalize/analyze separation;
3. versioned offline evidence packages;
4. baseline-vs-candidate diff as a first-class workflow;
5. portable diagnostic artifacts;
6. plugin-level diagnostic contracts;
7. explicit capability status: inspect / simulate / measure / validate;
8. source authority and provenance on every diagnostic result.

### Do not adopt

1. Flutter-specific VM requirements as general UI rules;
2. Flutter screen IDs as ORVIA taxonomy;
3. any Flutter implementation number as a universal UX threshold;
4. App Size metrics as a proxy for visual density;
5. offline diagnostic replay as a proxy for user task resumability;
6. accessibility UI switches as proof of completed validation;
7. new dials simply because the tool can measure something.

---

## 9. Decision

**No new numerical or categorical design control is promoted from Flutter DevTools in this pass.**

The repository is more valuable as the missing **diagnostic execution architecture** around the existing ORVIA controls than as a source of new design dials.

The immediate ORVIA sequence should therefore be:

```text
1. lock diagnostic schema
2. define capture/evidence package
3. implement applicability resolver
4. implement baseline/candidate diff contract
5. connect existing parameters to measurement adapters
6. connect validated outputs to token/component resolvers
7. only then evaluate whether additional controls are genuinely missing
```

That preserves the current numerical-control framework while making it measurable, auditable, replayable, and extensible.
