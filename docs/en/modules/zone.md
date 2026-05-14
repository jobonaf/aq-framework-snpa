# M_ZONE — Territorial subdivision

## Normative references

- Art. 3 Directive (EU) 2024/2881
- Annex II (classification of zones)

---

## Description

This module defines the subdivision of territory into zones
and agglomerations for air quality assessment.

---

## Definitions

```
zone = administrative or functional unit
```
```
population(zone)
```
```
area(zone)
```

---

## Logic

```
ZONE_COVERAGE_VALID =
∀ x ∈ territory :
    ∃ zone z : x ∈ z
```
```

---

## Operational rules

### Territorial coverage

- the entire territory must be subdivided into zones
- zones must not overlap

---

### Update

```
update zoning:
at least every 5 years
OR when significant changes occur
```

---

### Classification

Zones can be:

- urban
- suburban
- rural
- agglomerations

---

### Consistency

```
zones must be consistent with:
    emission patterns
    population distribution
    monitoring objectives
```

---

## Related modules and tables

This module provides the territorial context for the whole framework.

- `M_ASSESS` — the assessment regime is determined for each zone
- `M_NETWORK` — the minimum number and type of stations depend on zone characteristics
- `M_REPR` — representativeness areas are always limited by zone boundaries

---

## Output

```
zone:
    id
    geometry
    population
    type
```
