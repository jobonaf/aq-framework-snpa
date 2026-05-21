# Normative YAML Annotation Specification

# Directive (EU) 2024/2881 — Ambient Air Quality

Version: 2.3.1
Status: STABLE

## Purpose

This specification defines a structured YAML representation for normative
units extracted from Directive (EU) 2024/2881.

The objective is to produce:

* machine-readable legal requirements
* traceable normative mappings
* auditable legal interpretations
* interoperable structured data

Annotations must be:

* faithful to the original legal text
* minimally interpretative
* structurally consistent
* deterministic

---

## General Principles

### Atomicity

One YAML unit MUST represent exactly one normative statement that can be
independently interpreted or verified.

A provision MUST be split when it contains:

* multiple obligations with different actors
* multiple obligations with different triggers
* multiple obligations with different actions
* multiple permissions or prohibitions that are independently applicable

Do NOT split when:

* only the objects of the same action differ
* syntactic fragments lack independent normative meaning
* multiple conditions apply to the same normative statement

### Fidelity

Annotations MUST remain as close as possible to the legal wording.

Do NOT:

* paraphrase aggressively
* introduce inferred obligations
* simplify legal meaning
* merge distinct normative statements

### Explicitness

A unit is valid only if:

* the normative meaning is explicit in the text
* the action is identifiable
* the legal function is identifiable

Do NOT encode:

* purely descriptive statements
* policy aspirations without legal effect (e.g. "This Directive aims to...")
* recitals

### Determinism

All annotation decisions MUST follow the rules in this specification.

The annotator MUST NOT:

* guess missing information
* use fallback categories
* downgrade normative strength

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
                  definitional | informational | reporting

  actor:
    type: string              # mandatory except for constitutive_rule

  action:                     # mandatory except for norm_type: definition
    verb: string
    object: string or list    # optional

  term: string                # mandatory if norm_type: definition
  definition: string          # mandatory if norm_type: definition

  condition:
    text: string              # use for simple or merged conditions
    operator: and | or        # optional
    items: list               # use for structured conditions

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

* `id`
* `source`
* `norm_type`
* `normative_role`
* `domain`

Additionally:

* `actor` is mandatory except when `norm_type` is `constitutive_rule`
* `action` is mandatory except when `norm_type` is `definition`
* `term` and `definition` are mandatory when `norm_type` is `definition`

---

## Field Specifications

### id

Format: `Art{N}_{descriptor}`

Rules:

* MUST be unique across all YAML files
* MUST reflect source position
* MUST be stable once assigned
* MUST NOT encode semantic meaning beyond position
* If a provision generates multiple units, append `_a`, `_b`, `_c`

Examples:

```text
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
  document: directive_2024_2881
  article: 8
  paragraph: 3
  point: a
  annex: I
  section: 2
```

All provided values MUST correspond to the actual structure of the source
document. Do NOT invent subdivision levels.

### norm_type

Closed set — no other values allowed:

| Value               | Trigger in text                                             |
| ------------------- | ----------------------------------------------------------- |
| `obligation`        | "shall"                                                     |
| `prohibition`       | "shall not"                                                 |
| `permission`        | "may" — when enabling an action                             |
| `discretion`        | "may" — when allowing judgement or non-action               |
| `recommendation`    | "shall endeavour", "shall consider", "shall aim"            |
| `definition`        | "means", "refers to", definitional "shall be"               |
| `constitutive_rule` | provisions creating legal states without prescribing action |

When ambiguity exists, choose the strongest binding type.
Do NOT downgrade to `recommendation` unless the text explicitly weakens the
obligation (e.g. "where appropriate", "as far as practicable").

Use `permission` when the provision authorises a concrete operational or
procedural action that may be exercised by the actor.

Use `discretion` only when the provision primarily grants evaluative freedom,
judgement, or optional non-action without establishing a concrete operative
permission.

Statements of the form:

* "shall be those laid down in ..."
* "shall be assessed in accordance with ..."

SHOULD normally be encoded as `constitutive_rule` unless they define the
meaning of a legal term.

### normative_role

Closed set:

| Value           | Meaning                                                                                             |
| --------------- | --------------------------------------------------------------------------------------------------- |
| `operational`   | directly prescribes technical or administrative action                                              |
| `procedural`    | governs process, timing, or form of action                                                          |
| `institutional` | assigns competence, delegation, or governance                                                       |
| `definitional`  | establishes meaning of terms                                                                        |
| `informational` | requires disclosure or communication                                                                |
| `reporting`     | requires transmission of information, notifications, reports or data to another institutional actor |

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

* `member_state`
* `competent_authority`
* `commission`
* `european_environment_agency`
* `european_parliament_and_council`
* `operator`
* `natural_or_legal_person`

Forbidden:

* generic abstractions
* inferred actors not named in the provision

For `constitutive_rule`, the `actor` field MAY be omitted when the provision
creates a legal state without assigning an operational duty to a specific
actor.

### action

```yaml
action:
  verb: establish
  object: air_quality_plan
```

Each unit MUST contain exactly one action.
The verb MUST follow legal wording closely.

Allowed verbs:

* `adopt`
* `apply`
* `assess`
* `assign`
* `assist`
* `carry_out`
* `classify`
* `communicate`
* `consider`
* `consult`
* `coordinate`
* `cooperate`
* `designate`
* `determine`
* `encourage`
* `ensure`
* `establish`
* `evaluate`
* `identify`
* `implement`
* `inform`
* `install`
* `invite`
* `justify`
* `maintain`
* `make_available`
* `monitor`
* `notify`
* `omit`
* `postpone`
* `prepare`
* `propose`
* `provide`
* `publish`
* `reduce`
* `relocate`
* `report`
* `require`
* `respond`
* `review`
* `specify`
* `submit`
* `supplement`
* `update`
* `verify`
* `waive`

Forbidden — use the legal operation, not its effect:

* `effect`
* `outcome`
* `consequence`
* `achieve`

If a necessary verb is not listed:

* add the verb explicitly to the controlled vocabulary;
* preserve fidelity to the legal wording;
* do NOT substitute a semantically broader or weaker verb.

**Guidance on specific verbs:**

`apply` — use when a provision requires the use of a specified method,
standard, or procedure (e.g. "Member States shall apply the reference
measurement methods"). Do not substitute `ensure` or `assess`.

`identify` — use when a provision permits or requires formal recognition,
designation, or determination of legally relevant entities, zones, or states.

`implement` — use when a provision requires operational execution of measures,
plans, or programmes already established.

`install` — use when a provision requires the physical or operational
placement of measurement infrastructure (e.g. sampling points, supersites).

`justify` — use when a provision requires supporting reasons, methods,
evidence, or explanatory material to substantiate a legal position,
projection, or request.

`maintain` — use when a provision requires preserving an existing legal,
technical, environmental, or operational state.

`postpone` — use when a provision permits or requires deferral of a legal
deadline or obligation.

`relocate` — use when a provision governs the movement of an existing
sampling point or infrastructure. Required for both obligations and
prohibitions involving relocation.

`require` — use when an institutional actor is empowered to demand corrective
action, additional information, or replacement documentation from another
actor.

`specify` — use when a provision requires establishing the content or form of
required information, procedures, or technical elements.

`waive` — use for permissions structured as exemptions from an existing
obligation (e.g. "may choose not to measure"). Use `omit` when the
permission concerns leaving out a required element in a document or report.

`omit` — use when a permission allows an actor to exclude required content
from a document, plan, or report. Do not use for infrastructure or
measurement decisions (use `waive`).

#### action.object

The object SHOULD remain close to the legal wording while preserving
structural consistency.

Objects:

* SHOULD use snake_case;
* MAY contain legally meaningful detail;
* SHOULD avoid unnecessary abstraction;
* SHOULD NOT paraphrase legal concepts into generic labels.

Prefer:

```yaml
object: air_quality_roadmap
```

over:

```yaml
object: roadmap
```

Long composite objects are permitted when necessary to preserve legal meaning.

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

* `term` MUST be in snake_case
* `definition` SHOULD remain close to the legal wording
* `action` MUST be omitted

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
sub-conditions (e.g. points (a), (b), (c)):

```yaml
condition:
  operator: and
  items:
    - label: a
      text: indicative measurements or modelling applications provide
            sufficient information for the assessment of air quality
    - label: b
      text: the number of sampling points and the spatial resolution are
            sufficient for concentrations to be established in accordance
            with the data quality objectives in Annex V
```

`operator` indicates whether all conditions must apply cumulatively (`and`)
or alternatively (`or`).

If `operator` is omitted:

* `and` MUST be assumed only where the legal text clearly establishes
  cumulative applicability;
* otherwise, conditions SHOULD be represented using separate units or
  explicit `operator: or`.

`text` and `items` are mutually exclusive within a single `condition` block.
Do NOT split a unit solely because its condition has multiple limbs —
split only if the normative statement itself is independently verifiable.

### qualifiers

Modifiers that alter applicability or normative strength.

Closed list:

* `where_possible`
* `where_appropriate`
* `where_applicable`
* `if_relevant`
* `to_the_extent_possible`
* `at_least`
* `as_far_as_practicable`

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

Procedural formulations such as:

* "by means of implementing acts"
* "in accordance with the examination procedure"

SHOULD be represented in `references` when they explicitly invoke another
legal procedure or provision.

### timing

```yaml
timing:
  type: absolute
  date: 2030-01-01

timing:
  type: relative
  offset: P2Y
  anchor: date_of_first_exceedance

timing:
  type: recurring
  interval: P1Y
  anchor: end_of_reference_year
```

ISO 8601 durations: `P1Y` = 1 year, `P6M` = 6 months, `P20D` = 20 days.

Do NOT invent timing. Omit if not stated.

### domain

Closed list — assign one or more:

| Value                | Scope                                                           |
| -------------------- | --------------------------------------------------------------- |
| `general`            | general provisions, object, scope, definitions                  |
| `assessment`         | air quality assessment, thresholds, methods                     |
| `monitoring`         | measurement networks, sampling points, supersites               |
| `modelling`          | modelling applications, spatial representativeness, forecasting |
| `planning`           | air quality plans, roadmaps, short-term action plans            |
| `reporting`          | reporting to Commission, EIONET, e-Reporting                    |
| `public_information` | AQI, public disclosure, access to information                   |
| `enforcement`        | sanctions, remedies, right to compensation                      |
| `institutional`      | competences, delegation, committee procedures                   |

```yaml
domain:
  - planning
  - reporting
```

---

## Norm Type: constitutive_rule

Used for provisions that create a legal state, right, category, assessment
framework, or legal applicability condition without directly prescribing an
operational action to a specific actor.

```yaml
norm_type: constitutive_rule
normative_role: operational
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

* different actors performing different actions
* different triggers leading to different obligations
* independently verifiable normative statements
* distinct permissions with different legal effects

Do NOT split when:

* the same actor performs the same action on multiple objects
* wording varies but normative content is identical
* a condition has multiple limbs but the normative statement is single
  (use `condition.items` instead)

---

## Non-Operational Provisions

Do NOT convert:

* recitals (Whereas...)
* purely descriptive statements
* provisions that only reference other provisions without adding normative content
* entry into force clauses with no substantive content

---

## Validation Rules

A valid unit MUST:

* contain all mandatory fields for its `norm_type`
* use an allowed value for `norm_type`, `normative_role`, `domain`
* contain exactly one `action` (except definitions)
* use an allowed `action.verb`
* be traceable to a specific provision in the source document
* represent exactly one normative meaning
* not use both `condition.text` and `condition.items` in the same block
* not use `condition.operator` without `condition.items`
