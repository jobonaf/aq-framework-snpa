# Directive to YAML Conversion Specification
This document defines the YAML data model and conversion rules for transforming EU legal acts into structured, machine‑readable normative units.
It is intended to be used as CONTEXT for an LLM performing the conversion.

## 1. YAML DATA MODEL — Normative Unit

The output MUST be a YAML list of normative units.
Each normative unit MUST be a top‑level YAML object.
Each normative unit MUST conform to the following structure.

```yaml
- id: string                        # REQUIRED, unique
  source:                           # REQUIRED
    article: int                    # REQUIRED
    paragraph: int                  # OPTIONAL
    subpart: string | int           # OPTIONAL
    sentence: int                   # OPTIONAL

  norm_type:                        # REQUIRED
    enum: [obligation, prohibition, permission, discretion]
  normative_role: definitional | operational | procedural # OPTIONAL but RECOMMENDED

  actor:                            # REQUIRED
    type: string                    # e.g. member_state, commission
    level: string                   # e.g. national, zone, territorial_unit

  conditions:                       # OPTIONAL (may be {})
    free_form_object: true          # keys and values are not restricted
                                    # logical operators allowed: all_of, any_of

  action:                           # REQUIRED
    verb: string                    # normalized action token
    object: string | list | object  # OPTIONAL
    means: list                     # OPTIONAL
    purpose: list                   # OPTIONAL
    requirements: list              # OPTIONAL

  timing:                           # OPTIONAL
    type: enum [relative, absolute, before]
    reference_event: string
    deadline: string                # ISO‑8601 duration (e.g. P2Y)
    date: string                    # ISO date

  exceptions:                       # OPTIONAL
    - type: string
      condition: object
      consequence: object

  references:                       # OPTIONAL
    internal: list                  # Articles, Annexes
    external: list                  # Other EU acts
```

## 2. Semantic Interpretation Rules

These rules MUST be applied when converting legal text into YAML units.

### Normative Scope
Normative units MUST include:

- obligations, prohibitions, permissions and discretions;
- binding definitional provisions that determine legal parameters to be applied (e.g. thresholds, limit values, reference standards).

### Granularity

- The default granularity is ONE unit per PARAGRAPH.
- Split a paragraph into multiple units ONLY IF:
  - it contains multiple independent normative effects (e.g. different deadlines, actors, or norm types).
- Do NOT split merely for stylistic reasons.

### Norm Types

- "shall" → norm_type: obligation
- "shall not" → norm_type: prohibition
- "may" →
    - use permission IF it enables an operational option
    - use discretion IF it allows non-action or derogation
- Soft formulations ("shall endeavour", "shall consider") →
    - norm_type: obligation
    - add: strength: soft

### Actors

- Default actor is member_state unless explicitly stated otherwise.
- Always specify the administrative level (zone, territorial_unit, etc.)
- Do NOT encode specific geographic instances.

### Conditions

- Conditions MUST be explicit if the legal text implies applicability limits.
- Use structured but free-form keys.
- Prefer clarity over normalization.
- Use "all_of" / "any_of" ONLY when the logic is explicit.

Example:

```yaml
conditions:
  all_of:
    - pollutant: ozone
    - target_value_exceeded: true
```

### Actions

- Each action MUST have a verb.
- Action verbs MUST be normalized and descriptive.
- Generic verbs such as "do", "implement", "inform", "apply" MUST NOT be used alone.
- The verb SHOULD encode the legal function.
- Do NOT embed legal prose inside action fields.

### Timing

- If the legal text specifies a deadline, it MUST be encoded.
- Prefer relative timing where possible.
- Use ISO-8601 durations (P1Y, P2M, etc.).

### Exceptions

- Model exceptions explicitly.
- An exception MUST NOT remove the original obligation.
- If an exception applies, encode residual obligations (e.g. duty to justify).
-  When a "may" clause is followed by a mandatory consequence: model the permission and the obligation as SEPARATE normative units.

## Conversion Instructions for the LLM

You are converting a legal act into YAML normative units.
Follow these steps strictly:

1. Read the article in full before producing any output.
2. Identify all paragraphs and sentences with normative force.
3. For each normative effect:
   - determine actor
   - determine norm_type
   - extract conditions
   - define the action
   - extract timing and exceptions
4. Produce YAML ONLY.
5. Do NOT include explanations, comments, or legal text.
6. Preserve traceability through the "source" field.
7. If uncertain, prefer fewer units over over-fragmentation.

## Output Constraints

- Output MUST be valid YAML.
- Output MUST be deterministic given the same input.
- Do NOT invent obligations.
- Do NOT paraphrase legal meaning.
- If information is missing, omit the field.
