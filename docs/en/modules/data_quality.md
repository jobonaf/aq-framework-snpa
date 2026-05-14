# M_DATA_QUALITY — Data quality

## Normative references

- Directive (EU) 2024/2881, Annex V
- Art. 8 Directive (EU) 2024/2881

---

## Description

This module defines the **regulatory conditions for data validity**
used in air quality assessment.

Data quality conditions affect:

- compliance verification (`M_LIMITS`)
- spatial representativeness (`M_REPR`)
- validation of modelling applications (`M_MODEL_QA`)

The module **does not cover** institutional governance aspects
(laboratories, accreditation, JRC).

---

## Definitions

```
coverage(p, m) =
percentage of valid data
for pollutant p and metric m
```
```
uncertainty(p, m) =
data uncertainty
(95% confidence level)
```
```
MIN_coverage(p, m)
MAX_uncertainty(p, m)
= regulatory values defined in T_DATA_QUALITY
```
```
DATA_VALID(p, m) =
(coverage ≥ MIN_coverage)
AND (uncertainty ≤ MAX_uncertainty)
```

---

## Normative requirements

### REQ-DATA-VALIDITY_CRITERIA

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_DATA_QUALITY |

**Rule**  
Data may be used for air quality assessment only if it meets the minimum coverage and uncertainty requirements defined by the regulation.

**Acceptance criterion**

```
DATA_VALID(p, m) =
(coverage ≥ MIN_coverage)
AND (uncertainty ≤ MAX_uncertainty)
```

---

### REQ-DATA-EXCLUSION_IF_INVALID

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | REQ-DATA-VALIDITY_CRITERIA |

**Rule**  
Data that do not satisfy quality requirements are excluded from subsequent assessments.

**Acceptance criterion**

```
if DATA_VALID = false:
    data excluded from assessment
```

---

### REQ-DATA-SCOPE_OF_APPLICATION

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | — |

**Rule**  
Data quality requirements apply to:

- fixed measurements
- indicative measurements
- data used for model validation

**Acceptance criterion**  
The data type is correctly classified before quality verification.

---

### REQ-DATA-EXCLUDED_METRICS

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | — |

**Rule**  
Uncertainty and coverage requirements do **not apply** to:

- AOT40
- AEI / average exposure indicator
- information and alert thresholds
- critical levels for vegetation and ecosystems

**Acceptance criterion**  
Excluded metrics are not subject to data quality verification.

---

### REQ-DATA-LONG_SHORT_TERM

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_DATA_QUALITY |

**Rule**  
Data quality is checked separately for:

- long-term concentrations (annual means)
- short-term concentrations (hourly, 8-hour, 24-hour)

**Acceptance criterion**  
The applicable metric is the one required by regulation for each pollutant.

---

### REQ-DATA-TEMPORAL_TRANSITION

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_DATA_QUALITY |

**Rule**  
Uncertainty requirements take transitional provisions into account before and after 2030.

**Acceptance criterion**  
Uncertainty comparisons use the regulatory values valid for the considered year.

---

### REQ-DATA-INCOMPLETE_COVERAGE

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | — |

**Rule**  
Compliance assessment may be performed even with incomplete data coverage, provided the available valid data allow a conclusive evaluation.

**Acceptance criterion**  
A non-compliance may be reported even if minimum coverage is not reached, provided the valid data are sufficient.

---

### REQ-DATA-RANDOM_SAMPLING

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | — |

**Rule**  
For pollutants with minimum coverage below 80%, non-continuous or random sampling is allowed, provided the overall uncertainty meets data quality objectives.

**Acceptance criterion**  
The use of random sampling is documented and justified.

---

## Interactions with other modules

- `M_LIMITS` — uses only valid data for compliance
- `M_REPR` — uses only valid data for representativeness
- `M_MODEL_QA` — uses only valid data for model validation

---

## Output

```
data_quality_status:
pollutant
metric
data_type = fixed | indicative | model_input
coverage
uncertainty
valid (boolean)
```

---

## Notes

- The module defines **minimum regulatory requirements**.
- Data validity does not replace expert judgement in borderline cases.
