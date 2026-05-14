# M_LIMITS — Compliance verification with limit values

## Normative references

- Directive (EU) 2024/2881, Annex I (limit values and target values)
- Art. 8 (assessment)
- Art. 16 (natural sources)
- Art. 17 (exceedances)
- Art. 18 (time extensions)
- Annex V (data quality)

---

## Description

This module verifies the **regulatory compliance** of air quality
with limit values (LV) and target values (TV) defined in Annex I.

Verification is performed:

- by pollutant
- by regulatory metric
- on a zonal basis

The module does **not** define air quality plans
and does **not** plan corrective measures.

---

## Definitions

```
C(p, x, t) =
concentration of pollutant p
at location x
and time t
```
```
METRIC(p) =
applicable regulatory metric
for pollutant p,
defined in T_LIMIT_VALUES
```
```
LV(p) =
regulatory value (LV or TV)
for pollutant p
and the metric METRIC(p),
defined in T_LIMIT_VALUES
```
```
EXCEEDANCE(p, t) =
C(p, x, t) > LV(p)
```

---

## Normative requirements

### REQ-LIMITS-SPATIAL_INTEGRATION

| Field | Value |
|------|-------|
| Source | Art. 8 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_REPR, M_MODEL_QA |

**Rule**  
The concentration used for compliance verification is determined by integrating measurements and modelling according to spatial representativeness.

**Acceptance criterion**

```
if x ∈ AREA_REPR:
    C(p, x, t) = measurement
else if VALID_MODEL = true:
    C(p, x, t) = model
else:
    concentration undefined
```

---

### REQ-LIMITS-DATA_VALIDITY

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_DATA_QUALITY |

**Rule**  
Only data that satisfy quality requirements may be used for compliance verification.

**Acceptance criterion**

```
use only data where DATA_VALID = true
```

---

### REQ-LIMITS-MEAN_COMPLIANCE

| Field | Value |
|------|-------|
| Source | Annex I Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_LIMIT_VALUES |

**Rule**  
For metrics assessed as a mean (e.g. annual mean), compliance is verified by comparing the mean value with the applicable regulatory value.

**Acceptance criterion**

```
COMPLIANT_MEAN(p) =
mean(C(p)) ≤ LV(p)
```

---

### REQ-LIMITS-EXCEEDANCE_COMPLIANCE

| Field | Value |
|------|-------|
| Source | Annex I Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_LIMIT_VALUES |

**Rule**  
For metrics that allow a maximum number of exceedances, compliance is verified by comparing the number of exceedances with the permitted maximum.

**Acceptance criterion**

```
COMPLIANT_EXCEEDANCE(p) =
COUNT{ EXCEEDANCE(p, t) } ≤ MAX_EXCEED(p)
```

where `MAX_EXCEED(p)` is defined in `T_LIMIT_VALUES`.

---

### REQ-LIMITS-OVERALL_COMPLIANCE

| Field | Value |
|------|-------|
| Source | Annex I Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | REQ-LIMITS-MEAN_COMPLIANCE, REQ-LIMITS-EXCEEDANCE_COMPLIANCE |

**Rule**  
Overall compliance for a pollutant is verified only if all applicable conditions are satisfied.

**Acceptance criterion**

```
COMPLIANT(p) =
COMPLIANT_MEAN(p)
AND COMPLIANT_EXCEEDANCE(p)
```

---

### REQ-LIMITS-NATURAL_SOURCES

| Field | Value |
|------|-------|
| Source | Art. 16 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | — |

**Rule**  
Exceedances attributable to natural sources may be excluded from compliance verification if adequately documented and accepted.

**Acceptance criterion**

```
if exceedance attributable to natural sources:
    EXCLUDED_FROM_COMPLIANCE = true
```

---

### REQ-LIMITS-EXCEPTIONAL_EVENTS

| Field | Value |
|------|-------|
| Source | Art. 16 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | — |

**Rule**  
Exceptional events may be excluded from exceedance counting if permitted by regulation and documented.

**Acceptance criterion**  
The event is identified and justified with regulatory reference.

---

### REQ-LIMITS-DEROGATION_RECORD

| Field | Value |
|------|-------|
| Source | Art. 18 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | — |

**Rule**  
When a time extension is granted, the non-compliance is recorded as temporarily allowed.

**Acceptance criterion**

```
if extension granted:
    TEMPORARY_NON_COMPLIANCE_ALLOWED = true
```

The module records the derogation but **does not evaluate** the associated plan.

---

## Interactions with other modules

- `M_REPR` — defines spatial validity of measurements
- `M_MOD` — provides the concentration field
- `M_MODEL_QA` — enables the use of modelling
- `M_DATA_QUALITY` — validates the data
- `M_NETWORK` — may be activated in case of coverage gaps

---

## Output

```
compliance_result:
zone_id
pollutant
metric
compliant (boolean)

statistics:
mean_value
exceedance_count
allowed_exceedances

adjustments:
natural_sources (bool)
exceptional_events (bool)
derogation (bool)
```

---

## Notes

- The module does not artificially reduce exceedances using modelling.
- Compliance is determined on a zonal basis.
- Final verification is subject to Commission validation.
