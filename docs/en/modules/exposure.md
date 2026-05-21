# M_EXPOSURE — Average exposure indicator and exposure obligations

## Normative references

- Art. 4(18) Directive (EU) 2024/2881 — average exposure territorial unit
- Art. 4(28) Directive (EU) 2024/2881 — urban background locations
- Art. 4(29) Directive (EU) 2024/2881 — rural background locations
- Art. 4(33) Directive (EU) 2024/2881 — average exposure indicator
- Art. 4(34) Directive (EU) 2024/2881 — average exposure reduction obligation
- Art. 4(35) Directive (EU) 2024/2881 — average exposure concentration objective
- Art. 4(40) Directive (EU) 2024/2881 — contributions from natural sources
- Art. 6 Directive (EU) 2024/2881 — establishment of average exposure territorial units
- Art. 9(6) Directive (EU) 2024/2881 — sampling points for average exposure indicators
- Art. 12(3) Directive (EU) 2024/2881 — maintenance below average exposure concentration objectives
- Art. 13(3), Art. 13(5) Directive (EU) 2024/2881 — average exposure reduction obligations and assessment of average exposure indicators
- Art. 16 Directive (EU) 2024/2881 — natural source attribution interface
- Art. 19(3) Directive (EU) 2024/2881 — air quality plans where average exposure reduction obligation is not achieved
- Art. 23 Directive (EU) 2024/2881 — reporting interface
- Annex I, Section 5 — average exposure indicators, reduction obligations and concentration objectives
- Annex III — minimum number and distribution of sampling points
- Annex IV — siting and location criteria
- Annex V — data quality objectives

---

## Description

This module calculates and assesses the **average exposure indicator** (`AEI`) for pollutants subject to population-exposure obligations, primarily PM2.5 and NO2.

The AEI is a population-exposure assessment tool used to determine whether an average exposure territorial unit satisfies:

- the average exposure concentration objective;
- the average exposure reduction obligation;
- maintenance obligations where levels are already below the applicable exposure objective;
- planning triggers where exposure reduction obligations are not achieved.

This module is distinct from ordinary limit-value compliance. It does not determine compliance with limit values, target values, alert thresholds or information thresholds.

This module does **not**:

- define zones or average exposure territorial units;
- determine the monitoring network;
- validate data quality;
- attribute natural sources;
- establish air quality plans;
- perform public information or reporting.

Those functions are handled by dedicated modules.

---

## Scope

The module applies to:

```text
pollutants = [PM2_5, NO2]
```

where Annex I, Section 5 defines:

- average exposure indicators;
- average exposure concentration objectives;
- average exposure reduction obligations;
- baseline or reference periods;
- target years or compliance periods;
- transitional or exceptional calculation rules.

The territorial domain is the **average exposure territorial unit** (`AETU`) supplied by `M_ZONE`.

---

## Definitions

```text
p = pollutant
u = average exposure territorial unit
y = calendar year
sp = sampling point
```

```text
AETU(u) =
average exposure territorial unit defined by M_ZONE
```

```text
AEI_STATIONS(p,u,y) =
valid sampling points used for AEI calculation for pollutant p,
territorial unit u and year y
```

```text
C_ann(sp,p,y) =
annual mean concentration for pollutant p at sampling point sp in year y
```

```text
C_ann_adj(sp,p,y) =
annual mean concentration after any valid regulatory adjustment,
including natural-source adjustment where applicable
```

```text
AEI(p,u,y) =
average exposure indicator for pollutant p,
territorial unit u and reference year y
```

```text
AECO(p,u) =
average exposure concentration objective applicable to pollutant p and unit u
```

```text
AERO(p,u) =
average exposure reduction obligation applicable to pollutant p and unit u
```

```text
BASELINE_AEI(p,u) =
baseline or reference average exposure indicator defined by Annex I / T_EXPOSURE_OBLIGATIONS
```

```text
AEI_REDUCTION(p,u,y) =
percentage reduction of AEI relative to the applicable baseline
```

---

## Normative requirements

---

### REQ-EXPOSURE-TERRITORIAL_DOMAIN

| Field | Value |
|------|-------|
| Source | Art. 4(18); Art. 6 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_ZONE |

**Rule**  
The AEI shall be calculated for each applicable average exposure territorial unit.

**Acceptance criterion**

```text
for each AETU u supplied by M_ZONE:
    AEI domain = u.geometry
    AETU_VALID(u) = true
```

---

### REQ-EXPOSURE-POLLUTANT_SCOPE

| Field | Value |
|------|-------|
| Source | Art. 12(3); Art. 13(3); Art. 13(5); Annex I Section 5 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_EXPOSURE_OBLIGATIONS |

**Rule**  
AEI assessment applies to pollutants and exposure obligations defined in Annex I, Section 5.

**Acceptance criterion**

```text
if p in T_EXPOSURE_OBLIGATIONS.pollutants:
    AEI assessment required
else:
    AEI assessment not applicable
```

---

### REQ-EXPOSURE-STATION_SELECTION

| Field | Value |
|------|-------|
| Source | Art. 4(33); Art. 9(6); Annex III; Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_NETWORK; M_ZONE; M_DATA_QUALITY |

**Rule**  
The AEI shall be determined on the basis of measurements at urban background locations throughout the average exposure territorial unit.

Where no urban area is located in the territorial unit, rural background locations may be used in accordance with the definition of AEI and the applicable monitoring rules.

Sampling points shall be adequately distributed to reflect general population exposure and shall satisfy Annex III and Annex IV requirements.

**Acceptance criterion**

```text
if urban_area_exists(u) = true:
    AEI_STATIONS(p,u,y) = valid urban_background sampling points in u
else:
    AEI_STATIONS(p,u,y) = valid rural_background sampling points in u
```

```text
AEI_station_selection_valid =
    N_AEI_points(p,u) >= N_min_AEI(p,u)
    AND AEI_spatial_distribution_adequate = true
    AND all selected stations satisfy M_NETWORK and AnnexIV siting rules
```

---

### REQ-EXPOSURE-DATA_VALIDITY

| Field | Value |
|------|-------|
| Source | Art. 11(3); Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_DATA_QUALITY |

**Rule**  
Only data valid for the purpose of average exposure indicator calculation may be used.

**Acceptance criterion**

```text
for each sp in AEI_STATIONS(p,u,y):
    M_DATA_QUALITY.DATA_VALID(dataset(sp,p,y), p, annual_mean, average_exposure_indicator) = true
```

---

### REQ-EXPOSURE-ANNUAL_MEAN_INPUT

| Field | Value |
|------|-------|
| Source | Art. 4(33); Annex I Section 5; Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_DATA_QUALITY |

**Rule**  
The AEI shall use annual mean concentrations calculated from valid data for the relevant pollutant, station and calendar year.

**Acceptance criterion**

```text
if annual_mean_valid(sp,p,y) = true:
    C_ann(sp,p,y) usable_for_AEI = true
else:
    C_ann(sp,p,y) excluded_from_AEI
```

---

### REQ-EXPOSURE-AEI_CALCULATION

| Field | Value |
|------|-------|
| Source | Art. 4(33); Annex I Section 5 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | REQ-EXPOSURE-STATION_SELECTION; REQ-EXPOSURE-DATA_VALIDITY |

**Rule**  
For a reference year, the AEI is calculated as the mean of annual mean concentrations over the applicable three-calendar-year averaging period and the valid AEI sampling points in the average exposure territorial unit.

**Acceptance criterion**

```text
AEI(p,u,y) =
    mean{
        C_ann_adj(sp,p,yy)
        | yy in AEI_AVERAGING_YEARS(y)
        | sp in AEI_STATIONS(p,u,yy)
    }
```

```text
AEI_AVERAGING_YEARS(y) = [y-2, y-1, y]
```

---

### REQ-EXPOSURE-THREE_YEAR_MOVING_AVERAGE

| Field | Value |
|------|-------|
| Source | Annex I Section 5 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_EXPOSURE_OBLIGATIONS |

**Rule**  
The AEI is based on a three-calendar-year moving average unless a specific transitional or exceptional rule applies.

**Acceptance criterion**

```text
if no_special_AEI_rule_applies(y):
    AEI_AVERAGING_YEARS(y) = [y-2, y-1, y]
```

---

### REQ-EXPOSURE-TRANSITIONAL_2020_EXCLUSION

| Field | Value |
|------|-------|
| Source | Annex I Section 5 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | conditional |
| Dependencies | T_EXPOSURE_OBLIGATIONS |

**Rule**  
Where the Directive or Annex I allows exclusion of the year 2020 for specified AEI calculations or periods, the calculation shall apply the table-defined rule.

**Acceptance criterion**

```text
if T_EXPOSURE_OBLIGATIONS.allows_2020_exclusion(p,u,y) = true:
    AEI_AVERAGING_YEARS(y) = averaging_years_excluding_2020_as_defined_in_table
else:
    AEI_AVERAGING_YEARS(y) = [y-2, y-1, y]
```

**Note**  
The exact years and conditions are table-driven in `T_EXPOSURE_OBLIGATIONS`.

---

### REQ-EXPOSURE-NATURAL_SOURCE_ADJUSTMENT

| Field | Value |
|------|-------|
| Source | Art. 16; Art. 4(40) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | conditional |
| Dependencies | M_SOURCE_ATTRIBUTION; M_LIMITS; M_DATA_QUALITY |

**Rule**  
Where contributions from natural sources are validly identified and may be omitted or adjusted for the relevant regulatory purpose, the adjusted annual mean shall be used for AEI calculation.

**Acceptance criterion**

```text
if natural_source_adjustment_valid(sp,p,y) = true:
    C_ann_adj(sp,p,y) = C_ann(sp,p,y) - natural_source_contribution(sp,p,y)
else:
    C_ann_adj(sp,p,y) = C_ann(sp,p,y)
```

**Traceability requirement**

```text
if natural_source_adjustment_valid = true:
    adjustment_evidence_reference recorded
    adjusted_and_unadjusted_values retained
```

---

### REQ-EXPOSURE-BASELINE_AEI

| Field | Value |
|------|-------|
| Source | Annex I Section 5 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where reduction obligation applies |
| Dependencies | T_EXPOSURE_OBLIGATIONS |

**Rule**  
Where an average exposure reduction obligation applies, the baseline AEI shall be determined according to Annex I and `T_EXPOSURE_OBLIGATIONS`.

**Acceptance criterion**

```text
BASELINE_AEI(p,u) =
    AEI calculated over baseline_years defined in T_EXPOSURE_OBLIGATIONS
```

---

### REQ-EXPOSURE-REDUCTION_CALCULATION

| Field | Value |
|------|-------|
| Source | Art. 4(34); Art. 13(3); Annex I Section 5 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where reduction obligation applies |
| Dependencies | REQ-EXPOSURE-AEI_CALCULATION; REQ-EXPOSURE-BASELINE_AEI; T_EXPOSURE_OBLIGATIONS |

**Rule**  
The average exposure reduction achieved shall be calculated relative to the applicable baseline AEI.

**Acceptance criterion**

```text
AEI_REDUCTION(p,u,y) =
    100 * (BASELINE_AEI(p,u) - AEI(p,u,y)) / BASELINE_AEI(p,u)
```

```text
if AEI_REDUCTION(p,u,y) >= REQUIRED_REDUCTION(p,u,y):
    exposure_reduction_obligation_status = achieved
else:
    exposure_reduction_obligation_status = not_achieved
```

---

### REQ-EXPOSURE-CONCENTRATION_OBJECTIVE

| Field | Value |
|------|-------|
| Source | Art. 4(35); Art. 12(3); Annex I Section 5 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_EXPOSURE_OBLIGATIONS |

**Rule**  
The AEI shall be compared with the applicable average exposure concentration objective.

Where the AEI is below the objective, a maintenance obligation applies.

**Acceptance criterion**

```text
if AEI(p,u,y) <= AECO(p,u,y):
    exposure_concentration_objective_status = attained
    maintenance_obligation = true
else:
    exposure_concentration_objective_status = not_attained
```

---

### REQ-EXPOSURE-OVERALL_EXPOSURE_STATUS

| Field | Value |
|------|-------|
| Source | Art. 12(3); Art. 13(3); Annex I Section 5 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | REQ-EXPOSURE-REDUCTION_CALCULATION; REQ-EXPOSURE-CONCENTRATION_OBJECTIVE |

**Rule**  
The exposure status for an average exposure territorial unit shall reflect both the concentration objective and the reduction obligation where applicable.

**Acceptance criterion**

```text
if exposure_concentration_objective_status = attained
AND exposure_reduction_obligation_status in [achieved, not_applicable]:
    exposure_status = compliant_or_attained
else:
    exposure_status = not_attained_or_not_achieved
```

---

### REQ-EXPOSURE-PLAN_TRIGGER

| Field | Value |
|------|-------|
| Source | Art. 19(3) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_PLANS; M_LIMITS |

**Rule**  
Where the average exposure reduction obligation is not achieved in a given average exposure territorial unit, the result shall expose a planning trigger to `M_PLANS`.

**Acceptance criterion**

```text
if exposure_reduction_obligation_status = not_achieved:
    air_quality_plan_trigger = true
    planning_domain = average_exposure_territorial_unit
```

---

### REQ-EXPOSURE-NOT_LIMIT_VALUE_COMPLIANCE

| Field | Value |
|------|-------|
| Source | Art. 4(33); Art. 13; Annex I Section 5 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | framework boundary rule |
| Dependencies | M_LIMITS |

**Rule**  
AEI assessment is not a substitute for limit value or target value compliance verification.

**Acceptance criterion**

```text
AEI_result not used as limit_value_compliance_result
```

---

### REQ-EXPOSURE-REPRESENTATIVENESS_INTERFACE

| Field | Value |
|------|-------|
| Source | Art. 4(33); Art. 9(6); Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_REPR; M_NETWORK |

**Rule**  
Sampling points used for AEI shall represent general population exposure within the average exposure territorial unit.

This module consumes station context and representativeness outputs; it does not calculate representativeness geometries.

**Acceptance criterion**

```text
for each sp in AEI_STATIONS:
    station_context_valid_for_population_exposure = true
    representativeness_status_available = true
```

---

### REQ-EXPOSURE-DATA_GAPS_AND_STATION_CHANGES

| Field | Value |
|------|-------|
| Source | Annex V; Art. 9(6) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_DATA_QUALITY; M_NETWORK; M_REPR |

**Rule**  
Where AEI station availability, station location, representativeness or data validity changes during the averaging period, the calculation shall document the change and determine whether the AEI remains valid.

**Acceptance criterion**

```text
if AEI_station_set_changes_during_averaging_period = true:
    station_change_documented = true
    AEI_continuity_assessed = true
    AEI_validity_status determined
```

---

### REQ-EXPOSURE-REPORTING_TRACEABILITY

| Field | Value |
|------|-------|
| Source | Art. 23 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_REPORTING; M_ZONE; M_DATA_QUALITY |

**Rule**  
AEI results shall preserve traceability to territorial units, station sets, data quality status, adjustment status and calculation periods for reporting.

**Acceptance criterion**

```text
if AEI_result_reported = true:
    reporting_metadata_complete = true
    AETU_version recorded
    station_set_version recorded
    data_quality_status recorded
    calculation_years recorded
```

---

### REQ-EXPOSURE-PUBLIC_INFORMATION_INTERFACE

| Field | Value |
|------|-------|
| Source | Art. 22 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_PUBLIC_INFORMATION |

**Rule**  
Where AEI information is used for public information, it shall be communicated with the relevant territorial context, pollutant, period and attainment/reduction status.

**Acceptance criterion**

```text
if AEI_result_used_for_public_information = true:
    public_information_metadata_ready = true
    AETU_label available
    pollutant and period identified
    exposure_status identified
```

---

## Exposure calculation logic

```text
for each pollutant p subject to AEI obligations:

    for each average exposure territorial unit u:

        validate AETU(u)

        for each year y:

            determine AEI_AVERAGING_YEARS(y)

            select AEI_STATIONS(p,u,yy) for each yy

            verify station selection and representativeness

            verify data validity for annual means

            apply valid natural-source adjustments where applicable

            calculate AEI(p,u,y)

            compare AEI with AECO

            if reduction obligation applies:
                calculate BASELINE_AEI
                calculate AEI_REDUCTION
                compare with REQUIRED_REDUCTION

            determine exposure_status

            expose planning, reporting and public-information outputs
```

---

## Interactions with other modules

- `M_ZONE` — provides average exposure territorial units and versions
- `M_NETWORK` — provides AEI sampling points, siting context and distribution adequacy
- `M_DATA_QUALITY` — validates annual mean datasets for AEI use
- `M_REPR` — provides representativeness and population-exposure context
- `M_LIMITS` — consumes exposure status for broader compliance and obligation reporting
- `M_SOURCE_ATTRIBUTION` — provides natural-source contribution evidence where adjustment is allowed
- `M_PLANS` — consumes planning triggers where exposure reduction obligations are not achieved
- `M_PUBLIC_INFORMATION` — consumes public-facing AEI metadata
- `M_REPORTING` — consumes AEI calculation, station-set and territorial metadata
- `T_EXPOSURE_OBLIGATIONS` — stores objectives, obligations, baseline periods, target years and transitional rules
- `T_DATA_QUALITY` — stores Annex V data quality requirements

---

## Output

```text
exposure_status:
  pollutant
  average_exposure_territorial_unit_id
  AETU_version
  reference_year

  calculation:
    averaging_years
    station_set
    station_set_version
    annual_mean_values
    adjusted_annual_mean_values
    natural_source_adjustment_applied
    AEI_value
    AEI_validity_status

  objectives:
    average_exposure_concentration_objective
    exposure_concentration_objective_status = attained | not_attained | not_applicable
    maintenance_obligation

  reduction:
    baseline_years
    baseline_AEI
    required_reduction
    achieved_reduction
    exposure_reduction_obligation_status = achieved | not_achieved | not_applicable

  downstream:
    air_quality_plan_trigger
    reporting_metadata_complete
    public_information_metadata_ready

  data_quality:
    data_quality_status
    incomplete_but_conclusive
    excluded_data_summary
```

---

## Notes

- `M_EXPOSURE` is retained as a separate module because AEI assessment is neither ordinary limit-value compliance nor generic data-quality validation.
- The AEI is calculated over average exposure territorial units, not ordinary zones, unless a downstream module needs to link the result back to zones.
- Station selection is as important as arithmetic averaging: AEI points must reflect general population exposure.
- Natural-source adjustments must preserve both adjusted and unadjusted values for traceability.
- Planning consequences are not implemented here; this module emits the trigger consumed by `M_PLANS`.
