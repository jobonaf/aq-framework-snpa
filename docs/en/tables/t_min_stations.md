# T_MIN_STATIONS — Minimum number of sampling points

Source: Directive (EU) 2024/2881, Annex III  
Used by: M_NETWORK, M_EXPOSURE

---

## A. Diffuse sources — Assessment of limit values, target values,
## alert thresholds and information thresholds

### A.1 Pollutants other than ozone

Minimum number of sampling points for fixed-site measurements
**when the concentration exceeds the assessment threshold**.

| Zone population (×1000) | NO₂, SO₂, CO, Benzene | PM10 | PM2.5 | Pb, Cd, As, Ni (in PM10) | Benzo(a)pyrene (in PM10) |
|-------------------------|-------------------------|------|-------|----------------------------|----------------------------|
| 0 – 249 | 2 | 2 | 2 | 1 | 1 |
| 250 – 499 | 2 | 2 | 2 | 1 | 1 |
| 500 – 749 | 2 | 2 | 2 | 1 | 1 |
| 750 – 999 | 3 | 2 | 2 | 2 | 1 |
| 1 000 – 1 499 | 4 | 3 | 3 | 2 | 2 |
| 1 500 – 1 999 | 5 | 3 | 4 | 2 | 2 |
| 2 000 – 2 749 | 6 | 4 | 4 | 2 | 3 |
| 2 750 – 3 749 | 7 | 5 | 5 | 2 | 3 |
| 3 750 – 4 749 | 8 | 5 | 6 | 3 | 4 |
| 4 750 – 5 999 | 9 | 6 | 7 | 4 | 5 |
| ≥ 6 000 | 10 | 7 | 8 | 5 | 5 |

---

### A.2 Ozone (O₃)

Minimum number of sampling points for the assessment of target values,
long-term objectives, and alert and information thresholds.

| Zone population (×1000) | Minimum number |
|-------------------------|----------------|
| < 250 | 1 |
| < 500 | 2 |
| < 1 000 | 2 |
| < 1 500 | 3 |
| < 2 000 | 4 |
| < 2 750 | 5 |
| < 3 750 | 6 |
| ≥ 3 750 | +1 point every 2 million inhabitants |

> Note: at least one sampling point in areas with likely higher exposure;
> in agglomerations at least 50% of points are located in suburban areas.

---

## A.3 Reduction up to 50% of the minimum number of sampling points
### (pollutants other than ozone)

| Zone population (×1000) | NO₂, SO₂, CO, Benzene | PM10 | PM2.5 | Pb, Cd, As, Ni (in PM10) | Benzo(a)pyrene (in PM10) |
|-------------------------|-------------------------|------|-------|----------------------------|----------------------------|
| 0 – 249 | 1 | 1 | 1 | 1 | 1 |
| 250 – 499 | 1 | 1 | 1 | 1 | 1 |
| 500 – 749 | 1 | 1 | 1 | 1 | 1 |
| 750 – 999 | 2 | 1 | 1 | 1 | 1 |
| 1 000 – 1 499 | 2 | 1 | 2 | 1 | 1 |
| 1 500 – 1 999 | 3 | 2 | 2 | 1 | 1 |
| 2 000 – 2 749 | 3 | 2 | 2 | 1 | 2 |
| 2 750 – 3 749 | 4 | 2 | 3 | 1 | 2 |
| 3 750 – 4 749 | 4 | 3 | 3 | 2 | 2 |
| 4 750 – 5 999 | 5 | 3 | 4 | 2 | 3 |
| ≥ 6 000 | 5 | 4 | 4 | 3 | 3 |

---

### A.4 Reduction up to 50% — Ozone

| Zone population (×1000) | Minimum number |
|-------------------------|----------------|
| < 250 | 1 |
| < 500 | 1 |
| < 1 000 | 1 |
| < 1 500 | 2 |
| < 2 000 | 2 |
| < 2 750 | 3 |
| < 3 750 | 3 |
| ≥ 3 750 | +1 point every 4 million inhabitants |

---

## B. Average exposure (PM2.5 and NO₂)

Minimum number of sampling points for the evaluation
of average exposure reduction obligations.

- at least **1 point per average exposure unit**
- at least **1 point per million inhabitants**
  in urban areas > 100,000 inhabitants

Points may coincide with those in section A.

---

## C. Vegetation, ecosystems and rural ozone

### C.1 Critical levels (SO₂ and NOx)

| Condition | Minimum density |
|-----------|------------------|
| Concentration > critical level | 1 point every 20,000 km² |
| Concentration > assessment threshold | 1 point every 40,000 km² |

---

### C.2 Ozone — Long-term objectives

- at least **1 rural background point every 50,000 km²**
- in complex terrain: **1 point every 25,000 km²**

---

## D. Ultrafine particulate matter (UFP)

Minimum number of sampling points in sites
where high concentrations are likely:

- at least **1 point every 5 million inhabitants**
- Member States with < 5 million inhabitants:
  at least **1 point**
- Member States with < 2 million inhabitants:
  supersites are excluded from the count

---

## General notes

- All numbers are prescriptive and derive directly from Annex III.
- Reduction conditions, network composition and station siting
  are handled in module `M_NETWORK`.
- The tables do not contain decision rules:
  their application is delegated to the modules.
