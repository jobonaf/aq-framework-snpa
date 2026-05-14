# M_MODEL_QA — Validation of modelling applications

## Normative references

- Directive (EU) 2024/2881, Annex V
- Art. 8 Directive (EU) 2024/2881
- Implementing acts (model validation methodology)

---

## Description

This module defines the **regulatory validation requirements**
for air quality modelling applications.

A validated model is a **necessary condition**
for the use of results in the following modules:

- `M_MOD` — use of modelling
- `M_REPR` — spatial representativeness
- `M_LIMITS` — compliance verification
- `M_NETWORK` — monitoring network decisions

The module **does not cover** institutional governance aspects
(accreditation, JRC, EU intercomparisons).

---

## Definitions

```
OBS_VALID(p, m) =
observations that satisfy the data quality requirements defined in M_DATA_QUALITY
```
```
validation_data =
set of independent observations
not used as model input
```
```
U_meas(sp) =
measurement uncertainty at point sp,
defined in T_DATA_QUALITY
```
```
U_model(sp) =
modelling uncertainty at point sp,
determined according to Annex V
```
```
N_val =
number of sampling points
used for validation
```

---

## Modelling quality indicator (MQI)

```
MQI(sp) =
RMSE(sp)
-----------------------------
sqrt( U_model(sp)^2 + U_meas(sp)^2 )
```

where:

- `RMSE(sp)` is the root mean square error between modelled and observed values at point `sp`, calculated over the full evaluation period;
- `U_model(sp)` is the modelling uncertainty;
- `U_meas(sp)` is the measurement uncertainty.

The MQI is calculated:

- using **only valid observations**
- on data **independent** from the model input
- for the applicable regulatory metric (long or short term)

---

## Normative requirements

### REQ-MODELQA-MQI_DEFINITION

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_DATA_QUALITY |

**Rule**  
Model quality is assessed using the MQI, defined as the ratio of modelling error to total uncertainty.

**Acceptance criterion**  
The MQI is calculated according to the regulatory definition.

---

### REQ-MODELQA-MQI_THRESHOLD

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | REQ-MODELQA-MQI_DEFINITION |

**Rule**  
A model meets the quality objective if the MQI does not exceed one.

**Acceptance criterion**

```
MQI(sp) ≤ 1
```

---

### REQ-MODELQA-COVERAGE_90_PERCENT

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | REQ-MODELQA-MQI_THRESHOLD |

**Rule**  
The `MQI ≤ 1` criterion must be satisfied in at least **90% of available sampling points** in the evaluation area and period.

**Acceptance criterion**

```
COUNT{ sp | MQI(sp) ≤ 1 } / N_val ≥ 0.9
```

---

### REQ-MODELQA-LOW_STATION_COUNT

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | REQ-MODELQA-MQI_THRESHOLD |

**Rule**  
If the number of validation points is less than 10, the model is considered valid only if `MQI ≤ 1` is met at **all** available points.

**Acceptance criterion**

```
if N_val < 10:
    ∀ sp : MQI(sp) ≤ 1
```

---

### REQ-MODELQA-DATA_INDEPENDENCE

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | — |

**Rule**  
Data used for validation must be independent from data used as model input.

**Acceptance criterion**

```
validation_data ∩ model_input_data = ∅
```

---

### REQ-MODELQA-DATA_VALIDITY

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_DATA_QUALITY |

**Rule**  
Only observations that satisfy data quality requirements may be used for validation.

**Acceptance criterion**

```
∀ obs ∈ validation_data :
    OBS_VALID = true
```

---

### REQ-MODELQA-VALIDATION_METHOD

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | recommended |
| Dependencies | — |

**Rule**  
Model validation uses methods that allow comparison between model results and independent observations.

The recommended method is Leave-One-Out Cross-Validation (LOOCV).

**Acceptance criterion**  
The validation method is documented.

---

### REQ-MODELQA-USAGE_CONSTRAINT

| Field | Value |
|------|-------|
| Source | Art. 8 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | REQ-MODELQA-COVERAGE_90_PERCENT |

**Rule**  
Results from an unvalidated model may not be used for regulatory purposes.

**Acceptance criterion**

```
if VALID_MODEL = false:
    model_output NOT usable
```

---

## Interactions with other modules

- `M_DATA_QUALITY` — defines data validity
- `M_MOD` — produces modelling results
- `M_REPR` — uses the model only if validated
- `M_LIMITS` — accepts modelled exceedances only if validated
- `M_NETWORK` — allows network reduction only if validated

---

## Output

```
model_validation:
valid (boolean)
MQI_summary
N_val
validation_method
```

---

## Notes

- The `MQI ≤ 1` criterion and the 90% threshold derive directly from Annex V.
- The module defines minimum regulatory requirements and does not replace expert judgement.
