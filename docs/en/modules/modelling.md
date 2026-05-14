# M_MOD — Modelling applications

## Normative references

- Art. 8 Directive (EU) 2024/2881
- Annex IV (combined use of methods)
- Annex V (uncertainty and quality)
- Implementing acts (2026 draft — modelling requirements)

---

## Description

This module governs the **regulatory use of modelling**
in the air quality assessment system.

Modelling is used to:

- support the spatial distribution of concentrations
- identify hotspots
- delimit exceedance areas
- integrate point measurements
- support spatial representativeness (`M_REPR`)

The module **does not validate models**
(see `M_MODEL_QA`).

---

## Definitions

```
C_model(x, t) =
modelled concentration
at location x and time t
```
```
C_meas(sp, t) =
measured concentration
at sampling point sp
```
```
AREA_REPR(sp) =
representativeness area
defined in M_REPR
```
```
MODEL_USABLE =
model validated
according to M_MODEL_QA
```

---

## Normative requirements

### REQ-MOD-MODEL_VALIDATION_REQUIRED

| Field | Value |
|------|-------|
| Source | Art. 8 Dir. (EU) 2024/2881; Annex V |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_MODEL_QA |

**Rule**  
Model results may be used only if the model is validated.

**Acceptance criterion**

```
if use_model = true:
    MODEL_USABLE = true
```

---

### REQ-MOD-COMBINED_USE

| Field | Value |
|------|-------|
| Source | Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_REPR |

**Rule**  
When valid measurements are available, modelling is used in combination with measurements.

**Acceptance criterion**

```
if x ∈ AREA_REPR:
    C(x) = C_meas
else:
    C(x) = C_model
```

---

### REQ-MOD-USE_ABOVE_THRESHOLD

| Field | Value |
|------|-------|
| Source | Art. 8 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_ASSESS |

**Rule**  
In zones classified above threshold, modelling may be used only in integration with measurements.

**Acceptance criterion**

```
if zone ABOVE_THRESHOLD:
    model used only with measurements
```

---

### REQ-MOD-USE_BELOW_THRESHOLD

| Field | Value |
|------|-------|
| Source | Art. 8 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_ASSESS |

**Rule**  
In zones classified below threshold, modelling may be the primary assessment method.

**Acceptance criterion**

```
if zone BELOW_THRESHOLD:
    model may be primary method
```

---

### REQ-MOD-MEASUREMENT_MODEL_CONFLICT

| Field | Value |
|------|-------|
| Source | Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_REPR |

**Rule**  
In case of conflict between measurements and modelling within a representativeness area, measurements prevail.

**Acceptance criterion**

```
if x ∈ AREA_REPR
AND C_meas indicates exceedance
AND C_model does not:
    model not used for assessment
```

---

### REQ-MOD-MODEL_ONLY_USE

| Field | Value |
|------|-------|
| Source | Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_MODEL_QA |

**Rule**  
In the absence of valid measurement coverage, modelling may be used as the sole assessment method if validated.

**Acceptance criterion**

```
if no valid measurements
AND MODEL_USABLE = true:
    model may be used exclusively
```

---

## Operational role of modelling

The model must provide:

- a continuous spatial concentration field
- hotspot identification
- exceedance area delineation
- support for network coverage

---

## Interactions with other modules

- `M_MODEL_QA` — validates models
- `M_REPR` — defines spatial representativeness
- `M_LIMITS` — uses modelled results for compliance
- `M_NETWORK` — uses the model to identify coverage gaps

---

## Output

```
model_output:
concentration_field
exceedance_areas
hotspots
```

---

## Notes

- Modelling does not replace measurements in represented areas.
- Model use is always subject to regulatory validation.
