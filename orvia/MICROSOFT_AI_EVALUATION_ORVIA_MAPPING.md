# Microsoft Design — Human-Centered AI Evaluation → ORVIA

The article's central contribution is methodological: **evaluation dimensions should begin with evidence about what users actually value, then machine evaluation can scale those dimensions.**

## 1. Human evaluation defines the construct

The source describes a recurring gap between what teams believe makes an AI response good and what users actually prefer.

That produces a useful ORVIA rule:

```text
DO NOT
builder assumptions
    → metric
    → automated judge

PREFER
user evidence
    → quality construct
    → shared definition
    → golden examples
    → scalable judge
```

This is especially important for subjective constructs such as relevance, usefulness, trust, perceived intelligence, and preservation of the user's point of view.

## 2. Machine judge vs user judge

The article does not argue that machine evaluation is useless. It argues for a division of labor.

A machine may be able to test a criterion such as factual correctness when that criterion is well specified. But the source emphasizes that whether an answer actually matters for the user's company, strategy, goal, or lived context may require human judgment.

For ORVIA, this suggests every evaluation dimension should declare:

```yaml
judgeability:
  deterministic_machine:
  learned_machine:
  expert_human:
  target_user:
```

and possibly more than one evaluator type.

## 3. Golden datasets

Human evaluation does not need to remain a purely qualitative endpoint.

The article describes converting human judgments into **golden datasets**: concrete examples of good and bad responses that can seed much larger automated evaluation sets.

ORVIA consequence:

```text
human study
   ↓
quality dimensions
   ↓
gold examples / loss examples
   ↓
machine-readable labels
   ↓
scaled evaluation
   ↓
human recalibration
```

This creates a feedback loop rather than a permanent handoff from people to machines.

## 4. Loss-pattern taxonomy

The article emphasizes a shared vocabulary for recurring response failures.

Terms such as "generic" or "too shallow" become operationally valuable only when different teams mean the same thing by them.

A future ORVIA loss record should therefore contain:

```yaml
loss_pattern:
  id:
  name:
  definition:
  positive_examples:
  negative_examples:
  boundary_cases:
  evaluator_guidance:
  related_dimensions:
  severity:
  taxonomy_version:
```

The source does not provide Microsoft's full taxonomy, so ORVIA should not invent it.

## 5. Human-authored prompts

One of the strongest findings for our framework is that evaluation prompts should preserve the reality of human interaction.

Real users may be:

- tired;
- distracted;
- incomplete;
- ambiguous;
- poor at prompting;
- holding relevant context in their head rather than in the prompt.

That means:

```text
perfect synthetic prompt distribution
          ≠
real usage distribution
```

ORVIA's future evaluation datasets should therefore distinguish at least:

```yaml
prompt_origin:
  USER_AUTHORED
  SYNTHETIC
  CURATED
  ADVERSARIAL
```

without assuming one source is sufficient.

## 6. Multi-turn evaluation

The article proposes treating the **conversation itself as the stimulus**.

This matters because response quality can depend on:

- callbacks to earlier turns;
- accumulated context;
- conversational repair;
- tone progression;
- whether the interaction moves the user toward their goal.

So evaluation mode should become explicit:

```text
SINGLE_TURN
vs
MULTI_TURN
```

A single-turn score should never silently stand in for conversation quality.

## 7. Evaluation shelf life

The practitioners describe evaluation frameworks as rapidly aging artifacts.

The exact three- and six-month examples in the article should not become ORVIA expiration constants. The underlying rule is stronger:

```text
evaluation taxonomy
      ↓
version
      ↓
date
      ↓
model generation
      ↓
user expectation state
      ↓
scheduled revalidation
```

ORVIA evaluation assets should be versioned and carry revalidation triggers.

## 8. Model vs experience

A critical separation in the source is:

```text
MODEL RESPONSE EVALUATION
           ≠
PRODUCT EXPERIENCE EVALUATION
           ≠
AGENTIC WORKFLOW EVALUATION
```

Whole-experience evaluation introduces confounds such as interface, onboarding, other features, multi-stage workflows, and hidden agent stages.

This maps directly onto the ORVIA applicability architecture already extracted from Flutter DevTools: before measuring quality, define **what object is being evaluated**.

## 9. Proposed ORVIA evaluation pipeline

```text
REAL USER / REAL TASK
        ↓
USER-AUTHORED OR CONTROLLED INPUT
        ↓
MODEL / PRODUCT / AGENT EXPERIENCE
        ↓
QUALITATIVE USER EVIDENCE
        ↓
USER-DERIVED QUALITY DIMENSIONS
        ↓
LOSS-PATTERN TAXONOMY
        ↓
GOLDEN DATASET
        ↓
MACHINE-SCALE EVALUATION
        ↓
BASELINE ↔ CANDIDATE COMPARISON
        ↓
PERIODIC HUMAN RECALIBRATION
```

This fits the existing ORVIA diagnostic architecture without requiring a new generic "AI quality score."

## 10. Current decision

Do not create one `AI_QUALITY` dial.

Instead, build an evaluation layer containing:

- user-derived quality dimensions;
- evaluator/judgeability metadata;
- loss-pattern taxonomies;
- golden examples;
- prompt-origin metadata;
- single-turn vs multi-turn mode;
- evaluation target scope;
- taxonomy/model/dataset versioning;
- revalidation triggers.

The exact dimensions should be learned from target-user evidence rather than frozen globally.
