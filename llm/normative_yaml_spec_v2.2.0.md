# Normative YAML Annotation Specification
# Directive (EU) 2024/2881 — Ambient Air Quality

Version: 2.2.0
Status: STABLE


## Purpose

This specification defines a structured YAML representation for normative
units extracted from Directive (EU) 2024/2881.

The objective is to produce:

- machine-readable legal requirements
- traceable normative mappings
- auditable legal interpretations
- interoperable structured data

Annotations must be:

- faithful to the original legal text
- minimally interpretative
- structurally consistent
- deterministic

---

## General Principles

### Atomicity

One YAML unit MUST represent exactly one normative statement that can be
independently interpreted or verified.

A provision MUST be split when it contains:

- multiple obligations with different actors
- multiple obligations with different triggers
- multiple obligations with different actions
- multiple permissions or prohibitions that are independently applicable

Do NOT split when:

- only the objects of the same action differ
- syntactic fragments lack independent normative meaning


### Fidelity

Annotations MUST remain as close as possible to the legal wording.

Do NOT:

- paraphrase aggressively
- introduce inferred obligations
- simplify legal meaning
- merge distinct normative statements


### Explicitness

A unit is valid only if:

- the normative meaning is explicit in the text
- the action is identifiable
- the legal function is identifiable

Do NOT encode:

- purely descriptive statements
- policy aspirations without legal effect (e.g. "This Directive aims to...")
- recitals


### Determinism

All annotation decisions MUST follow the rules in this specification.

The annotator MUST NOT:

- guess missing information
- use fallback categories
- downgrade normative strength


### Traceability

Every unit MUST preserve its source reference.
The source is sufficient to recover the original text.

---

## YAML Structure

```yaml
- id: string

  source:
    document: string
    article: integer
    paragraph: integer        # optional
    point: string or integer  # optional
    annex: string             # optional
    section: string           # optional

  norm_type: obligation | prohibition | permission | discretion |
             definition | recommendation | constitutive_rule

  normative_role: operational | procedural | institutional |
                  definitional | informational

  actor:
    type: string              # mandatory except for constitutive_rule

  action:                     # mandatory except for norm_type: definition
    verb: string
    object: string or list    # optional

  term: string                # mandatory if norm_type: definition
  definition: string          # mandatory if norm_type: definition

  condition:
    text: string              # use for simple or merged conditions
    items: list               # use for multi-limb conditions (see §condition)

  qualifiers: list            # optional — from closed list
  exceptions: list            # optional — explicit exceptions only
  references: list            # optional — article or annex references

  timing:                     # optional
    type: absolute | relative | recurring
    date: ISO 8601 date       # if type: absolute
    offset: ISO 8601 duration # if type: relative
    interval: ISO 8601 duration # if type: recurring
    anchor: string            # if type: relative or recurring

  domain: list                # one or more from closed list
```

---

## Mandatory Fields

The following fields are always mandatory:

- `id`
- `source`
- `norm_type`
- `normative_role`
- `domain`

Additionally:

- `actor` is mandatory except when `norm_type` is `constitutive_rule`
- `action` is mandatory except when `norm_type` is `definition`
- `term` and `definition` are mandatory when `norm_type` is `definition`

---

## Field Specifications

### id

Format: `Art{N}_{descriptor}`

Rules:

- MUST be unique across all YAML files
- MUST reflect source position
- MUST be stable once assigned
- MUST NOT encode semantic meaning beyond position
- If a provision generates multiple units, append `_a`, `_b`, `_c`

Examples:

```
Art4_def12
Art8_p3_a
Art18_p1
Art19_p1_a
Art19_p1_b
```

Revoked units MUST remain in the file with `norm_type: revoked` and a
`revoked_date` field. Their id MUST NOT be reused.


### source

```yaml
source:
  document: directive_2024_2881   # or implementing_decision
  article: 8
  paragraph: 3                    # omit if not applicable
  point: a                        # omit if not applicable
  annex: I                        # omit if not applicable
  section: 2                      # omit if not applicable
```

All provided values MUST correspond to the actual structure of the source
document. Do NOT invent subdivision levels.


### norm_type

Closed set — no other values allowed:

| Value | Trigger in text |
|---|---|
| `obligation` | "shall" |
| `prohibition` | "shall not" |
| `permission` | "may" — when enabling an action |
| `discretion` | "may" — when allowing judgement or non-action |
| `recommendation` | "shall endeavour", "shall consider", "shall aim" |
| `definition` | "means", "refers to", definitional "shall be" |
| `constitutive_rule` | provisions creating legal states without prescribing action |

When ambiguity exists, choose the strongest binding type.
Do NOT downgrade to `recommendation` unless the text explicitly weakens the
obligation (e.g. "where appropriate", "as far as practicable").


### normative_role

Closed set:

| Value | Meaning |
|---|---|
| `operational` | directly prescribes technical or administrative action |
| `procedural` | governs process, timing, or form of action |
| `institutional` | assigns competence, delegation, or governance |
| `definitional` | establishes meaning of terms |
| `informational` | requires disclosure or communication |

When an actor is required to assist another actor in carrying out an
operational function, the `normative_role` is `operational`. Use
`institutional` only when the provision assigns an actor its own competence
or delegates governance authority — not when the actor supports another
actor's exercise of competence.


### actor

```yaml
actor:
  type: member_state
```

MUST be explicitly identifiable in the text.

Allowed values:

- `member_state`
- `competent_authority`
- `commission`
- `european_environment_agency`
- `european_parliament_and_council`
- `operator`
- `natural_or_legal_person`

Forbidden:

- generic abstractions
- inferred actors not named in the provision


### action

```yaml
action:
  verb: establish
  object: air_quality_plan
```

Each unit MUST contain exactly one action.
The verb MUST follow legal wording closely.

Allowed verbs:

- `apply`
- `assess`
- `assign`
- `assist`
- `carry_out`
- `classify`
- `communicate`
- `consider`
- `consult`
- `coordinate`
- `cooperate`
- `designate`
- `determine`
- `ensure`
- `establish`
- `evaluate`
- `inform`
- `install`
- `maintain`
- `make_available`
- `monitor`
- `notify`
- `omit`
- `propose`
- `provide`
- `publish`
- `reduce`
- `relocate`
- `report`
- `review`
- `submit`
- `supplement`
- `update`
- `verify`
- `waive`

Forbidden — use the legal operation, not its effect:

- `effect`
- `outcome`
- `consequence`
- `achieve`

**Guidance on specific verbs:**

`apply` — use when a provision requires the use of a specified method,
standard, or procedure (e.g. "Member States shall apply the reference
measurement methods"). Do not substitute `ensure` or `assess`.

`install` — use when a provision requires the physical or operational
placement of measurement infrastructure (e.g. sampling points, supersites).

`relocate` — use when a provision governs the movement of an existing
sampling point or infrastructure. Required for both obligations and
prohibitions involving relocation.

`waive` — use for permissions structured as exemptions from an existing
obligation (e.g. "may choose not to measure"). Use `omit` when the
permission concerns leaving out a required element in a document or report.

`omit` — use when a permission allows an actor to exclude required content
from a document, plan, or report. Do not use for infrastructure or
measurement decisions (use `waive`).

If a needed verb is not in the list, add it with a comment in the pull
request. Do NOT use a substitute verb.


### term and definition

Used only when `norm_type: definition`.

```yaml
norm_type: definition
normative_role: definitional
term: spatial_representativeness
definition: >
  the approach where the air quality metrics observed at a sampling point
  are representative of an explicitly delineated geographic area
```

Rules:

- `term` MUST be in snake_case
- `definition` SHOULD remain close to the legal wording
- `action` MUST be omitted


### condition

Conditions MUST be explicit in the text. Do NOT infer conditions.
Wording MUST remain close to the legal text.
Do NOT formalise beyond what is stated.

Use `text` for simple conditions or when sub-conditions are not individually
lettered or titled in the source:

```yaml
condition:
  text: where the concentration exceeds the limit value
```

Use `items` when the source provision enumerates distinct, labelled
sub-conditions (e.g. points (a), (b), (c)) that must all be satisfied
for the norm to apply. Each item MUST carry the label as it appears in
the source:

```yaml
condition:
  items:
    - label: a
      text: indicative measurements or modelling applications provide
            sufficient information for the assessment of air quality
    - label: b
      text: the number of sampling points and the spatial resolution are
            sufficient for concentrations to be established in accordance
            with the data quality objectives in Annex V
    - label: c
      text: the number of indicative measurements is at least the same as
            the number of fixed measurements being replaced and is evenly
            distributed over the calendar year
    - label: d
      text: for ozone, nitrogen dioxide is measured at all remaining ozone
            sampling points except at rural background locations
```

`text` and `items` are mutually exclusive within a single `condition` block.
Do NOT split a unit solely because its condition has multiple limbs —
split only if the normative statement itself is independently verifiable.


### qualifiers

Modifiers that alter applicability or normative strength.

Closed list:

- `where_possible`
- `where_appropriate`
- `where_applicable`
- `if_relevant`
- `to_the_extent_possible`
- `at_least`
- `as_far_as_practicable`

```yaml
qualifiers:
  - where_possible
```


### exceptions

MUST be explicitly stated in the text.
Do NOT infer exceptions.

```yaml
exceptions:
  - natural dust contributions as defined in Article 17
```


### references

```yaml
references:
  - article: 17
  - annex: VIII
  - article: 9
    paragraph: 3
  - document: directive_2016_2284
    article: 7
```

No interpretation allowed. List only references explicitly present in
the provision.

The `document` field is optional. It MUST be provided when the reference
points to an instrument other than the document named in `source.document`.
Use the same snake_case naming convention as `source.document`
(e.g. `directive_2016_2284`, `implementing_decision`).
Omit `document` for intra-instrument references.


### timing

```yaml
# Absolute deadline
timing:
  type: absolute
  date: 2030-01-01

# Relative deadline
timing:
  type: relative
  offset: P2Y
  anchor: date_of_first_exceedance

# Recurring
timing:
  type: recurring
  interval: P1Y
  anchor: end_of_reference_year
```

ISO 8601 durations: `P1Y` = 1 year, `P6M` = 6 months, `P20D` = 20 days.

Do NOT invent timing. Omit if not stated.


### domain

Closed list — assign one or more:

| Value | Scope |
|---|---|
| `general` | general provisions, object, scope, definitions |
| `assessment` | air quality assessment, thresholds, methods |
| `monitoring` | measurement networks, sampling points, supersites |
| `modelling` | modelling applications, spatial representativeness, forecasting |
| `planning` | air quality plans, roadmaps, short-term action plans |
| `reporting` | reporting to Commission, EIONET, e-Reporting |
| `public_information` | AQI, public disclosure, access to information |
| `enforcement` | sanctions, remedies, right to compensation |
| `institutional` | competences, delegation, committee procedures |

```yaml
domain:
  - planning
  - reporting
```

---

## Norm Type: constitutive_rule

Used for provisions that create a legal state, right, or category without
prescribing an action to a specific actor.

```yaml
norm_type: constitutive_rule
normative_role: operational
actor:           # omit
action:
  verb: establish
  object: exceedance_period_limit
```

Example: a provision stating that exceedances shall not persist beyond
4 years creates a legal constraint without assigning it to a specific actor.

When the same action applies to multiple objects under the same constitutive
rule, encode them as a list under `action.object` rather than splitting into
separate units.

---

## Granularity Rules

Split a provision into multiple units when it contains:

- different actors performing different actions
- different triggers leading to different obligations
- independently verifiable normative statements

Do NOT split when:

- the same actor performs the same action on multiple objects
- wording varies but normative content is identical
- a condition has multiple limbs but the normative statement is single
  (use `condition.items` instead)

---

## Non-Operational Provisions

Do NOT convert:

- recitals (Whereas...)
- purely descriptive statements
- provisions that only reference other provisions without adding normative content
- entry into force clauses with no substantive content

---

## Validation Rules

A valid unit MUST:

- contain all mandatory fields for its `norm_type`
- use an allowed value for `norm_type`, `normative_role`, `domain`
- contain exactly one `action` (except definitions)
- use an allowed `action.verb`
- be traceable to a specific provision in the source document
- represent exactly one normative meaning
- not use both `condition.text` and `condition.items` in the same block

---

## Examples

### Obligation with timing

```yaml
- id: Art19_p1_a
  source:
    document: directive_2024_2881
    article: 19
    paragraph: 1
  norm_type: obligation
  normative_role: procedural
  actor:
    type: member_state
  action:
    verb: establish
    object: air_quality_plan
  condition:
    text: where limit or target values are exceeded
  timing:
    type: relative
    offset: P2Y
    anchor: end_of_year_of_exceedance
  domain:
    - planning
```

### Constitutive rule

```yaml
- id: Art19_p1_b
  source:
    document: directive_2024_2881
    article: 19
    paragraph: 1
  norm_type: constitutive_rule
  normative_role: operational
  action:
    verb: establish
    object: maximum_exceedance_duration
  condition:
    text: from the end of the first calendar year of exceedance
  timing:
    type: relative
    offset: P4Y
    anchor: end_of_first_exceedance_year
  domain:
    - planning
```

### Constitutive rule with multiple objects

```yaml
- id: Art1_p2_a
  source:
    document: directive_2024_2881
    article: 1
    paragraph: 2
  norm_type: constitutive_rule
  normative_role: operational
  action:
    verb: establish
    object:
      - limit_values
      - target_values
      - alert_thresholds
      - long_term_objectives
  references:
    - annex: I
  domain:
    - general
    - assessment
```

### Definition

```yaml
- id: Art4_def26
  source:
    document: directive_2024_2881
    article: 4
    point: 26
  norm_type: definition
  normative_role: definitional
  term: spatial_representativeness
  definition: >
    the approach where the air quality metrics observed at a sampling point
    are representative of an explicitly delineated geographic area, insofar
    as the concentration at that point is similar to the concentration
    throughout that area
  domain:
    - assessment
    - monitoring
```

### Permission with condition.items

```yaml
- id: Art9_p3
  source:
    document: directive_2024_2881
    article: 9
    paragraph: 3
  norm_type: permission
  normative_role: operational
  actor:
    type: member_state
  action:
    verb: reduce
    object: minimum_number_of_sampling_points_for_fixed_measurements
  condition:
    items:
      - label: precondition
        text: where the level of pollutants exceeds the relevant assessment
              threshold specified in Annex II but not the respective limit
              values, target values and critical levels specified in Annex I
      - label: a
        text: indicative measurements or modelling applications provide
              sufficient information for the assessment of air quality with
              regard to limit values, target values, critical levels, alert
              thresholds and information thresholds, as well as adequate
              information for the public
      - label: b
        text: the number of sampling points and the spatial resolution of
              indicative measurements and modelling applications are sufficient
              for concentrations to be established in accordance with the data
              quality objectives in Annex V, Points A and B, and enable
              assessment results to meet the requirements in Point E of Annex V
      - label: c
        text: the number of indicative measurements, if used, is at least the
              same as the number of fixed measurements being replaced and is
              evenly distributed over the calendar year
      - label: d
        text: for ozone, nitrogen dioxide is measured at all remaining ozone
              sampling points except at rural background locations
  qualifiers:
    - at_least
  references:
    - annex: I
    - annex: II
    - annex: III
    - annex: V
    - annex: IV
  domain:
    - monitoring
    - assessment
```

### Permission with simple condition

```yaml
- id: Art9_p3_simple
  source:
    document: directive_2024_2881
    article: 9
    paragraph: 3
  norm_type: permission
  normative_role: operational
  actor:
    type: member_state
  action:
    verb: reduce
    object: number_of_fixed_measurement_sampling_points
  condition:
    text: where concentrations are below the assessment threshold
  qualifiers:
    - where_appropriate
  references:
    - annex: III
  domain:
    - monitoring
```

### apply obligation

```yaml
- id: Art11_p1_a
  source:
    document: directive_2024_2881
    article: 11
    paragraph: 1
  norm_type: obligation
  normative_role: operational
  actor:
    type: member_state
  action:
    verb: apply
    object: reference_measurement_methods
  references:
    - annex: VI
  domain:
    - assessment
    - monitoring
```

### install obligation

```yaml
- id: Art9_p4
  source:
    document: directive_2024_2881
    article: 9
    paragraph: 4
  norm_type: obligation
  normative_role: operational
  actor:
    type: member_state
  action:
    verb: install
    object: sampling_points_for_ozone_precursor_substances
  references:
    - annex: VII
  domain:
    - monitoring
```

### relocate prohibition

```yaml
- id: Art9_p7_a
  source:
    document: directive_2024_2881
    article: 9
    paragraph: 7
  norm_type: prohibition
  normative_role: operational
  actor:
    type: member_state
  action:
    verb: relocate
    object: sampling_points_with_recorded_exceedances
  condition:
    text: where exceedances of a relevant limit value or target value specified
          in Section 1 of Annex I were recorded within the previous 3 years
  exceptions:
    - relocation necessary due to special circumstances including spatial development
  references:
    - annex: I
  domain:
    - monitoring
```

### waive permission (exemption from obligation)

```yaml
- id: Art10_p6
  source:
    document: directive_2024_2881
    article: 10
    paragraph: 6
  norm_type: permission
  normative_role: operational
  actor:
    type: member_state
  action:
    verb: waive
    object: measurement_of_black_carbon_ultrafine_particles_or_ammonia
  condition:
    text: >
      in half of its monitoring supersites at rural background locations, if the
      number of rural background supersites exceeds the number of urban background
      supersites by at least a ratio of 2:1, as long as the selection is
      representative for those pollutants
  domain:
    - monitoring
```

### Cross-instrument reference

```yaml
- id: Art10_p7
  source:
    document: directive_2024_2881
    article: 10
    paragraph: 7
  norm_type: recommendation
  normative_role: procedural
  actor:
    type: member_state
  action:
    verb: coordinate
    object: monitoring_strategy_and_measurement_programme
  condition:
    text: where appropriate, with the monitoring strategy and measurement programme
          of EMEP, ACTRIS, and the monitoring of air pollution impacts under
          Directive (EU) 2016/2284
  qualifiers:
    - where_appropriate
  references:
    - document: directive_2016_2284
      article: 7
  domain:
    - monitoring
```

### Assist-type obligation

```yaml
- id: Art3_p3
  source:
    document: directive_2024_2881
    article: 3
    paragraph: 3
  norm_type: obligation
  normative_role: operational
  actor:
    type: european_environment_agency
  action:
    verb: assist
    object: commission_in_carrying_out_the_review
  references:
    - article: 3
      paragraph: 1
  domain:
    - general
    - institutional
```

### Carry-out obligation

```yaml
- id: Art6_p1_b
  source:
    document: directive_2024_2881
    article: 6
  norm_type: obligation
  normative_role: operational
  actor:
    type: member_state
  action:
    verb: carry_out
    object:
      - air_quality_assessment
      - air_quality_management
  condition:
    text: in all zones and average exposure territorial units
  domain:
    - assessment
    - general
```

### Coordinate obligation

```yaml
- id: Art5_f
  source:
    document: directive_2024_2881
    article: 5
    point: f
  norm_type: constitutive_rule
  normative_role: institutional
  action:
    verb: coordinate
    object: union_wide_quality_assurance_programmes
  condition:
    text: if Union-wide quality assurance programmes are being organised by the Commission
  domain:
    - monitoring
    - institutional
```

### Supplement permission

```yaml
- id: Art8_p2_b
  source:
    document: directive_2024_2881
    article: 8
    paragraph: 2
  norm_type: permission
  normative_role: operational
  actor:
    type: member_state
  action:
    verb: supplement
    object: fixed_measurements
  condition:
    text: to provide information on spatial distribution of air pollutants
          and spatial representativeness of fixed measurements
  qualifiers:
    - where_appropriate
  domain:
    - monitoring
    - modelling
    - assessment
```