# M_EXPOSURE — Average exposure indicator (AEI / IEM)

## Normative references

- Directive (EU) 2024/2881, Annex I — Section 5
- Average exposure reduction obligation
- Average exposure concentration objectives

---

## Description

This module defines the average exposure indicator
(**AEI / IEM — Average Exposure Indicator**), used to assess
population exposure to air pollution over time.

The AEI is an **exposure assessment tool**, distinct from:
- limit values,
- information/alert thresholds,
- air quality plans.

---

## Definitions

```
C_ann(sp, y) = annual mean concentration
at sampling point sp
for year y

stations = sampling points at
urban background locations (and suburban sites)

AEI(y) = average exposure indicator
for year y
```

---

## Calculation logic

The AEI for a given year is defined as the
**mean of annual concentrations for the last three calendar years**,
calculated over all urban background sampling points
within the average exposure territorial units.

```
AEI(y) =
mean(
    C_ann(sp, y),
    C_ann(sp, y-1),
    C_ann(sp, y-2)
    for sp ∈ stations
)
```

---

## Operational rules

### Station selection

- only sampling points at **urban background** locations
- suburban background may be included if provided at national level
- points must be deployed in accordance with Annex III

---

### Treatment of natural sources

If exceedances attributable to natural sources are identified,
the related contributions are **deducted** before AEI calculation,
provided the exclusion is documented.

---

### Temporal averaging

- moving average over **three consecutive calendar years**
- for 2030–2032 it is possible to exclude the year 2020 from the calculation

---

## Use of the indicator

The AEI is used to:

- verify compliance with the **average exposure reduction obligation**
- compare exposure over time
- assess achievement of the **average exposure concentration objectives**

The AEI is **not used directly** for limit value compliance verification.

---

## Obligations and objectives (reference)

The percentage reduction obligations and concentration objectives
are defined in:

- `T_EXPOSURE_OBLIGATIONS`

This module is limited to **indicator calculation and assessment**;
corrective action planning is addressed in separate modules.

---

## Related modules and tables

- `M_NETWORK` — defines urban background sampling points
- `M_DATA_QUALITY` — ensures validity of the data used
- `T_EXPOSURE_OBLIGATIONS` — reduction obligations and objectives

---

## Output

```
AEI:
value
year
averaging_period = 3 years
```
