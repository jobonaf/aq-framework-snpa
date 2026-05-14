# M_ASSESS — Assessment regime

## Normative references

- Art. 8 Directive (EU) 2024/2881
- Annex II (assessment thresholds)
- Annex IV (assessment methods)

---

## Description

This module determines the **assessment regime** for air quality for each pollutant and zone.

The assessment regime establishes which methods must or may be used:

- fixed measurements
- indicative measurements
- modelling
- objective estimation

---

## Definitions

```
TH(p) = assessment threshold for pollutant p,
defined in T_ASSESS_THRESHOLDS
```
```
C(p, y) = concentration value of pollutant p
in calendar year y,
calculated according to the required averaging period
```
```
ASSESSMENT_TYPE(p, z) =
assessment regime for pollutant p
in zone z
```

---

## Normative requirements

### REQ-ASSESS-CLASSIFICATION

| Field | Value |
|------|-------|
| Source | Art. 8 Dir. (EU) 2024/2881; Annex II |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_ASSESS_THRESHOLDS |

**Rule**  
For each pollutant, a zone is classified as **above threshold** if the concentration value exceeds the assessment threshold in at least **three of the five preceding calendar years**.

**Acceptance criterion**  
Given a pollutant `p` and a zone `z`, the system returns `ABOVE_THRESHOLD = true` if:

```
COUNT{ y ∈ last_5_years | C(p, y) > TH(p) } ≥ 3
```

**Pseudo-code**  
Descriptive, not executable.

---

### REQ-ASSESS-TIME_WINDOW

| Field | Value |
|------|-------|
| Source | Art. 8 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | — |

**Rule**  
The above/below threshold classification is performed using a rolling window of **five calendar years**.

The years do not need to be consecutive.

**Acceptance criterion**  
Given a set of five calendar years, the system evaluates the exceedance condition independently of temporal order.

**Pseudo-code**  
Descriptive, not executable.

---

### REQ-ASSESS-REGIME_DEFINITION

| Field | Value |
|------|-------|
| Source | Art. 8 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | REQ-ASSESS-CLASSIFICATION |

**Rule**  
If a zone is classified above threshold for a pollutant, the assessment regime is based on **fixed measurements**.

If a zone is classified below threshold, the assessment regime may be based on modelling or objective estimation.

**Acceptance criterion**

```
if ABOVE_THRESHOLD:
    ASSESSMENT_TYPE = fixedMeasurements
else:
    ASSESSMENT_TYPE = modelOrObjectiveEstimation
```

**Pseudo-code**  
Descriptive, not executable.

---

### REQ-ASSESS-STRICTEST_PREVAILS

| Field | Value |
|------|-------|
| Source | Art. 8 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | REQ-ASSESS-REGIME_DEFINITION |

**Rule**  
If more than one assessment regime is applicable for the same pollutant, the **most restrictive regime prevails**.

**Acceptance criterion**  
Given a set of potentially applicable regimes, the system selects the one with the highest level of obligation.

**Pseudo-code**  
Descriptive, not executable.

---

## Related modules and tables

- `T_ASSESS_THRESHOLDS` — defines regulatory thresholds
- `M_NETWORK` — uses the assessment regime
- `M_MOD` — enables or limits the use of modelling
- `M_LIMITS` — uses the regime as evaluation context
