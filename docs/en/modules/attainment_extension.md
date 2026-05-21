# M_ATTAINMENT_EXTENSION — Postponement of attainment deadlines (Art. 18)

## Naming note

The module is named **M_ATTAINMENT_EXTENSION** (not M_DEROGATION) because:

- Art. 18 uses the legal concept of *postponement of attainment deadline*;
- it is conditional and evidence-based, not a broad derogation from obligations.

---

## Normative references

- Art. 18 Directive (EU) 2024/2881 — postponement of attainment deadlines
- Art. 19 Directive (EU) 2024/2881 — air quality plans and roadmaps
- Art. 21 Directive (EU) 2024/2881 — transboundary contributions
- Art. 16 Directive (EU) 2024/2881 — natural sources
- Art. 11 Directive (EU) 2024/2881 — data quality and modelling
- Annex I — limit values and attainment deadlines
- Annex VIII — air quality roadmaps

---

## Description

This module determines whether a **postponement of attainment deadlines** is:

```text
requested
supported by evidence
potentially acceptable under Art. 18
```

It does **not grant** the postponement. It produces a structured evaluation package used by:

- `M_LIMITS`
- `M_PLANS`
- `M_REPORTING`

The final legal decision remains outside this module.

---

## Scope

Applies to cases where:

```text
limit value exceedance persists beyond attainment deadline
```

and a Member State claims that attainment cannot be achieved in time due to:

```text
unfavourable dispersion conditions
transboundary contributions
site-specific conditions
or combinations thereof
```

---

## Definitions

```text
p = pollutant
z = zone
m = metric
period = assessment period
```

```text
ATTAINMENT_DEADLINE(p,m) =
legal deadline defined in Annex I
```

```text
POSTPONEMENT_REQUEST(p,z,m) =
formal claim to extend attainment deadline
```

```text
POSTPONEMENT_SUPPORTED =
true if all Art. 18 conditions are satisfied
```

---

## Normative requirements

---

### REQ-EXT-TRIGGER

**Rule**  
A postponement assessment is triggered where exceedance persists beyond the attainment deadline.

**Acceptance**

```text
if exceedance = true AND current_year > ATTAINMENT_DEADLINE:
    postponement_assessment_required = true
```

---

### REQ-EXT-PRECONDITION_PLAN

**Rule**  
A valid air quality plan or roadmap must exist.

**Acceptance**

```text
air_quality_plan_exists = true
AND plan_contains_measures = true
```

---

### REQ-EXT-MEASURE_SUFFICIENCY

**Rule**  
All appropriate measures must be taken to keep exceedance period as short as possible.

**Acceptance**

```text
all_reasonable_measures_implemented = true
AND no_less_restrictive_alternative_available = true
```

---

### REQ-EXT-JUSTIFICATION

**Rule**  
Postponement must be justified by eligible causes.

**Acceptance**

```text
justification in [
    unfavourable_dispersion_conditions,
    transboundary_contribution,
    site_specific_conditions
]
```

---

### REQ-EXT-TRANSBOUNDARY_SUPPORT

**Rule**  
Where transboundary contribution is invoked, evidence from `M_SOURCE_ATTRIBUTION` is required.

**Acceptance**

```text
if justification = transboundary_contribution:
    source_attribution_evidence_valid = true
```

---

### REQ-EXT-MODELLING_SUPPORT

**Rule**  
Projection modelling must demonstrate future attainment.

**Acceptance**

```text
MODEL_VALID = true
AND projected_attainment_date available
```

---

### REQ-EXT-TIME_LIMIT

**Rule**  
Postponement must not exceed allowed extension period.

**Acceptance**

```text
extended_deadline <= legal_max_extension_year
```

---

### REQ-EXT-REPORTING_PACKAGE

**Rule**  
A full evidence package must be prepared.

**Acceptance**

```text
package includes:
    exceedance_data
    plan_measures
    modelling_projections
    source_attribution_if_applicable
    justification
```

---

### REQ-EXT-LEGAL_BOUNDARY

**Rule**  
This module does not decide approval.

**Acceptance**

```text
POSTPONEMENT_SUPPORTED = evaluation output only
decision_by = external_authority
```

---

## Decision logic

```text
if REQ-EXT-TRIGGER satisfied:
    check plan
    check measures
    check justification
    check modelling
    check time limit

    if all satisfied:
        POSTPONEMENT_SUPPORTED = true
    else:
        POSTPONEMENT_SUPPORTED = false
```

---

## Output

```text
attainment_extension:
  pollutant
  zone
  metric

  trigger:
    exceedance_persisting
    deadline_passed

  justification:
    type
    evidence_reference

  measures:
    plan_exists
    measures_implemented

  modelling:
    model_valid
    projected_attainment_date

  transboundary:
    contribution_present
    evidence_available

  result:
    postponement_supported
    blocking_conditions

  reporting:
    package_ready
```

---

## Notes

- This is not a derogation regime; it is a conditional extension.
- Strong dependency on modelling and source attribution.
- Always linked to planning obligations.
