# M_REPORTING — Regulatory reporting and data exchange

## Normative references

- Art. 23 Directive (EU) 2024/2881 — reporting and data exchange
- Art. 22 Directive (EU) 2024/2881 — public information interface
- Art. 19 Directive (EU) 2024/2881 — plans and roadmaps (reporting content)
- Art. 18 Directive (EU) 2024/2881 — postponement (evidence submission)
- Art. 21 Directive (EU) 2024/2881 — transboundary reporting
- Annex I — standards and thresholds
- Annex V — data quality objectives
- Annex VIII — plan and roadmap content
- Implementing acts (data formats, INSPIRE, e-reporting where applicable)

---

## Description

This module defines the **regulatory reporting layer** of the framework.

It transforms validated outputs from all modules into:

```text
official datasets
compliance reporting
EU-level submissions
traceable regulatory evidence packages
```

It ensures that all reported data is:

```text
complete
consistent
traceable
versioned
fit for regulatory use
```

This module does not perform assessment or modelling; it aggregates and standardizes outputs.

---

## Scope

Covers reporting for:

```text
assessment results
limit value compliance
exposure indicators
plans and roadmaps
postponement requests
source attribution cases
transboundary cooperation
data quality metadata
```

---

## Definitions

```text
REPORTING_DATASET =
structured dataset prepared for submission or publication
```

```text
REPORTABLE_ENTITY =
element required for reporting
(e.g. zone, pollutant, metric, year)
```

```text
REPORTING_PACKAGE =
collection of datasets, metadata, and evidence
```

```text
TRACEABILITY =
ability to link reported values to source data and methods
```

---

## Normative requirements

---

### REQ-REP-DATA_VALIDITY

**Rule**  
Only valid data shall be reported.

**Acceptance**

```text
forall dataset:
    DATA_VALID = true OR status = documented_exception
```

---

### REQ-REP-CONSISTENCY

**Rule**  
Reported data must be internally consistent across modules.

**Acceptance**

```text
M_LIMITS_output == reporting_limits
M_EXPOSURE_output == reporting_exposure
```

---

### REQ-REP-COMPLETENESS

**Rule**  
All required entities must be reported.

**Acceptance**

```text
forall required_entities:
    present in REPORTING_DATASET
```

---

### REQ-REP-METADATA

**Rule**  
Each dataset must include metadata.

**Acceptance**

```text
metadata includes:
    source
    method
    quality_status
    version
    timestamp
```

---

### REQ-REP-VERSIONING

**Rule**  
Datasets must be versioned and auditable.

**Acceptance**

```text
report includes:
    dataset_version
    revision_history
```

---

### REQ-REP-TRACEABILITY

**Rule**  
Reported values must be traceable to underlying data.

**Acceptance**

```text
TRACEABILITY = true
link to original dataset exists
```

---

### REQ-REP-PLAN_REPORTING

**Rule**  
Plans and roadmaps must be reported with required content.

**Acceptance**

```text
if plan_exists:
    report includes:
        measures
        timeline
        source_profile
        projections
```

---

### REQ-REP-EXTENSION_REPORTING

**Rule**  
Postponement requests must include full evidence package.

**Acceptance**

```text
if postponement_requested:
    include:
        justification
        modelling_results
        plan
        source_attribution
```

---

### REQ-REP-TRANSBOUNDARY_REPORTING

**Rule**  
Transboundary cases must be reported.

**Acceptance**

```text
if TRANSBOUNDARY_CASE:
    report includes:
        affected_states
        source_states
        contribution_estimates
```

---

### REQ-REP-PUBLIC_ALIGNMENT

**Rule**  
Reported data shall be consistent with public information.

**Acceptance**

```text
public_information == reporting_summary
```

---

### REQ-REP-FORMAT

**Rule**  
Data must follow standardized formats.

**Acceptance**

```text
format complies with implementing acts
```

---

### REQ-REP-LEGAL_BOUNDARY

**Rule**  
This module does not assess compliance; it reports it.

**Acceptance**

```text
compliance_status consumed
not recalculated
```

---

## Decision logic

```text
collect outputs from all modules
validate completeness and consistency
attach metadata and traceability
format dataset
publish/report
```

---

## Output

```text
reporting_package:
  datasets:
    limits
    exposure
    plans
    models

  metadata:
    versions
    sources
    quality

  compliance:
    status

  supporting:
    source_attribution
    transboundary
    extension_requests

  audit:
    traceability_links
```

---

## Notes

- Final aggregation layer of the framework.
- Ensures legal robustness of all outputs.
- Enables interoperability with EU systems.
- Strong dependency on all upstream modules.
