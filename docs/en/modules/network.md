# M_NETWORK — Monitoring network

## Normative references

- Art. 9 Directive (EU) 2024/2881
- Annex III (minimum number of sampling points)
- Annex IV (siting criteria)

---

## Description

This module checks the **structural adequacy** of the monitoring network
for each pollutant and zone.

The network must ensure:

- a minimum number of sampling points
- adequate spatial coverage
- representation of relevant concentration levels
- consistency with the assessment regime defined in `M_ASSESS`

---

## Definitions

```
N_active(p, z) =
number of active sampling points
for pollutant p in zone z
```
```
N_min(p, z) =
minimum number of sampling points required,
defined in T_MIN_STATIONS
```
```
VALID(st) =
sampling point compliant
with T_SITING siting criteria
```
```
ASSESSMENT_TYPE(p, z) =
assessment regime defined in M_ASSESS
```

---

## Normative requirements

### REQ-NETWORK-MINIMUM_ADEQUACY

| Field | Value |
|------|-------|
| Source | Art. 9 Dir. (EU) 2024/2881; Annex III |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_MIN_STATIONS, T_SITING |

**Rule**  
A monitoring network is adequate if the number of active sampling points
is at least the minimum required and all points satisfy the siting criteria.

**Acceptance criterion**

```
NETWORK_OK =
(N_active ≥ N_min)
AND ∀ st : VALID(st)
```

---

### REQ-NETWORK-MINIMUM_COMPOSITION

| Field | Value |
|------|-------|
| Source | Annex III, letter A Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_MIN_STATIONS |

**Rule**  
For each zone, the minimum number of sampling points must include at least:
- one background point,
- one traffic-related sampling point,
in accordance with Annex IV, provided this does not increase the total number of points.

**Acceptance criterion**  
At least one background point and at least one traffic point are present.

---

### REQ-NETWORK-TRAFFIC_REPRESENTATION

| Field | Value |
|------|-------|
| Source | Annex III, letter A Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | — |

**Rule**  
For NO₂, PM, benzene and CO, the network includes at least one sampling point
intended to capture the contribution of traffic emissions.

**Acceptance criterion**  
For each applicable pollutant, at least one traffic-type sampling point is present.

---

### REQ-NETWORK-BACKGROUND_TRAFFIC_BALANCE

| Field | Value |
|------|-------|
| Source | Annex III, letter A Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | — |

**Rule**  
For NO₂, PM, benzene and CO, the number of urban background points
and the number of traffic points must not differ by a factor greater than 2.

**Acceptance criterion**

```
max(
N_background / N_traffic,
N_traffic / N_background
) ≤ 2
```

---

### REQ-NETWORK-REDUCTION_ALLOWED

| Field | Value |
|------|-------|
| Source | Annex III Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_MODEL_QA, M_ASSESS |

**Rule**  
A reduction of up to 50% of the minimum number of sampling points
is allowed only if:
- the assessment regime permits it;
- validated modelling is available.

**Acceptance criterion**

```
if ASSESSMENT_TYPE = fixedMeasurements
AND VALID_MODEL = true:
    reduction_allowed = true
```

---

### REQ-NETWORK-REDUCTION_LIMIT

| Field | Value |
|------|-------|
| Source | Annex III Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_MIN_STATIONS |

**Rule**  
The reduction of the minimum number of sampling points
must not exceed 50%.

**Acceptance criterion**

```
N_min_eff ≥ ceil(0.5 × N_min)
```

---

### REQ-NETWORK-NEW_STATION_REQUIRED

| Field | Value |
|------|-------|
| Source | Art. 9 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_REPR, M_MOD |

**Rule**  
If areas with relevant exceedances are identified
that are not covered by any representativeness area,
new sampling points must be introduced.

**Acceptance criterion**

```
if exceedance detected
AND outside all AREA_REPR:
    ADDITIONAL_STATION_REQUIRED = true
```

---

### REQ-NETWORK-RELOCATION_FORBIDDEN

| Field | Value |
|------|-------|
| Source | Art. 9 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_REPR |

**Rule**  
Relocation of a sampling point is forbidden if it has recorded relevant exceedances
during the last three calendar years.

**Acceptance criterion**

```
RELOCATION_FORBIDDEN =
∃ y ∈ last_3_years :
    exceedance detected
```
```

---

### REQ-NETWORK-OZONE_RURAL

| Field | Value |
|------|-------|
| Source | Annex III, letter C.2 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | — |

**Rule**  
For long-term ozone assessment, the network includes rural background points with an average density of:
- at least one point every 50,000 km²,
- at least one point every 25,000 km² in complex terrain.

**Acceptance criterion**  
The rural point distribution satisfies the minimum density requirements.

---

### REQ-NETWORK-UFP

| Field | Value |
|------|-------|
| Source | Annex III, letter D Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | — |

**Rule**  
Ultrafine particulate matter (UFP) is measured in sites
where high concentrations are likely, with at least:
- one point every 5 million inhabitants,
- at least one point in Member States with smaller populations.

For Member States with fewer than 2 million inhabitants,
supersites are not counted for this obligation.

**Acceptance criterion**  
The minimum number of UFP points is present
in areas with the highest likelihood of elevated concentrations.

---

## Related modules and tables

- `M_ASSESS` — defines the assessment regime
- `M_REPR` — defines representativeness areas
- `M_MOD` — supports the identification of uncovered areas
- `M_MODEL_QA` — enables network reduction
- `T_MIN_STATIONS` — quantitative requirements
- `T_SITING` — siting criteria
