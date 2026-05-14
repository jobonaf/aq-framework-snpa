# M_REPR — Spatial representativeness of sampling points

## Normative references

- Art. 4(26) Directive (EU) 2024/2881
- Art. 8 Directive (EU) 2024/2881
- Art. 9 Directive (EU) 2024/2881
- Implementing acts (2026 draft — spatial representativeness methodology)
- Annex IV (siting criteria)

---

## Description

This module defines the **spatial representativeness**
of sampling points, i.e. the geographic area
in which concentrations observed or modelled at a point
are representative within a regulatory tolerance.

The module:

- links point measurements and territory
- supports network design
- integrates measurements and modelling

The module does not:
- verify compliance with limit values
- assess overall network adequacy

---

## Definitions

```
C_sp =
central concentration value
at sampling point sp,
calculated according to the applicable regulatory metric
```
```
T_min(p) =
minimum tolerance for pollutant p,
defined in T_REPR_TOLERANCE
```
```
Δ(sp, p) =
max(0.15 × C_sp, T_min(p))
```
```
interval(sp, p) =
[C_sp − Δ, C_sp + Δ]
```
```
AREA_REPR(sp, p) =
{ x ∈ zones | C(x, p) ∈ interval(sp, p) }
```

---

## Normative requirements

### REQ-REPR-AREA_DEFINITION

| Field | Value |
|------|-------|
| Source | Art. 4(26) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_REPR_TOLERANCE |

**Rule**  
A sampling point representativeness area is defined as the set of locations
where the concentration falls within the tolerance interval
around the measured central value.

**Acceptance criterion**

```
AREA_REPR(sp, p) =
{ x | C(x, p) ∈ interval(sp, p) }
```

---

### REQ-REPR-MODEL_USAGE

| Field | Value |
|------|-------|
| Source | Art. 8–9 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_MODEL_QA |

**Rule**  
Modelling may be used to determine a representativeness area
only if the model is validated.

**Acceptance criterion**

```
if use_model = true:
    VALID_MODEL = true
```

---

### REQ-REPR-AREA_REFINEMENT

| Field | Value |
|------|-------|
| Source | Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | — |

**Rule**  
The preliminary representativeness area
must be refined using criteria that are:
- geographic
- typological
- emission-based

**Acceptance criterion**  
Locations inconsistent with the station type
or the emission profile are excluded from the area.

---

### REQ-REPR-EXPERT_JUDGEMENT

| Field | Value |
|------|-------|
| Source | Implementing acts (methodology) |
| Status | STABLE |
| Type | mandatory |
| Dependencies | — |

**Rule**  
Expert judgement is required in cases of:
- strong spatial heterogeneity
- complex terrain
- non-uniform dispersion conditions

**Acceptance criterion**  
Expert judgement is documented
and associated with the area definition.

---

### REQ-REPR-OVERLAP_RESOLUTION

| Field | Value |
|------|-------|
| Source | Implementing acts (methodology) |
| Status | STABLE |
| Type | mandatory |
| Dependencies | — |

**Rule**  
If a location belongs to multiple representativeness areas,
assignment is based on qualitative criteria.

**Acceptance criterion**  
Assignment considers at least:
- station type
- emission coherence
- concentration level

---

### REQ-REPR-CRITICAL_CASE

| Field | Value |
|------|-------|
| Source | Art. 8–9 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_MODEL_QA, M_NETWORK |

**Rule**  
If a modelled concentration is not consistent with any representativeness area,
the case requires review of the model or the network.

**Acceptance criterion**

```
if no AREA_REPR applicable:
    MODEL_REVIEW_REQUIRED = true
```

The case does not automatically imply regulatory non-compliance.

---

## Downstream module use

- `M_NETWORK` uses representativeness areas
to assess network coverage
- `M_LIMITS` uses representativeness
to determine the spatial validity of measurements

---

## Related modules and tables

- `M_ASSESS` — assessment regime
- `M_NETWORK` — network adequacy
- `M_MODEL_QA` — model validation
- `T_REPR_TOLERANCE` — regulatory tolerances

---

## Output

```
representativeness:
    station_id
    pollutant
    geometry (polygon / multipolygon)
    central_value
    tolerance
    method = measurement | modelling | hybrid
```

---

## Update frequency

Spatial representativeness is updated:

- at least every 5 years
- when the network changes
- when emission patterns change significantly
- when dispersion conditions change substantially
