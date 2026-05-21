# M_NETWORK — Monitoring network

## Normative references

- Art. 8 Directive (EU) 2024/2881
- Art. 9 Directive (EU) 2024/2881
- Art. 10 Directive (EU) 2024/2881
- Art. 11 Directive (EU) 2024/2881
- Annex II (assessment thresholds)
- Annex III (minimum number of sampling points)
- Annex IV (location and siting criteria)
- Annex V (data quality objectives)
- Annex VI (reference methods and modelling conditions)
- Annex VII (measurements at supersites and ozone precursor substances)

---

## Description

This module verifies the **structural and legal adequacy of the monitoring network** for each pollutant and zone.

It determines whether the network:

- contains the required minimum number of sampling points;
- satisfies siting and location criteria;
- is consistent with the assessment regime defined in `M_ASSESS`;
- may lawfully reduce fixed measurement points using modelling or indicative measurements;
- includes required ozone, PAH, UFP and average-exposure monitoring components;
- complies with relocation restrictions;
- implements additional measurements required after uncovered modelled exceedances;
- includes monitoring supersites where required.

This module does **not** determine compliance with limit values. Compliance and exceedance qualification are handled by `M_LIMITS`.

---

## Definitions

```text
p = pollutant
z = zone
u = average exposure territorial unit
st = sampling point
ms = Member State
```

```text
N_active(p,z) =
number of active sampling points for pollutant p in zone z
```

```text
N_min(p,z) =
minimum number of sampling points required for pollutant p in zone z,
as defined in T_MIN_STATIONS / Annex III
```

```text
N_min_eff(p,z) =
effective minimum number of fixed measurement points after any lawful reduction
```

```text
VALID_SITING(st) =
sampling point st complies with Annex IV siting and location criteria
```

```text
ABOVE_THRESHOLD(p,z) =
zone z is above the assessment threshold for pollutant p,
as determined by M_ASSESS under Art. 7 and Annex II
```

```text
EXCEEDS_STANDARD(p,z) =
pollutant p exceeds a relevant limit value, target value or critical level
as determined by M_LIMITS under Annex I
```

```text
AREA_REPR(st) =
area of spatial representativeness of sampling point st
```

```text
NETWORK_OK(p,z) =
network for pollutant p in zone z satisfies applicable requirements
```

---

## Normative requirements

---

### REQ-NETWORK-SAMPLING_POINT_LOCATION

| Field | Value |
|------|-------|
| Source | Art. 9(1); Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_SITING; M_ZONE |

**Rule**  
The location of sampling points shall be determined in accordance with Annex IV.

**Acceptance criterion**

```text
for each sampling_point st:
    VALID_SITING(st) = true
```

---

### REQ-NETWORK-MINIMUM_ADEQUACY_ABOVE_THRESHOLD

| Field | Value |
|------|-------|
| Source | Art. 9(2); Annex II; Annex III Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_ASSESS; T_MIN_STATIONS; T_SITING |

**Rule**  
In each zone where pollutant levels exceed the assessment threshold specified in Annex II, the minimum number of sampling points for fixed measurements shall comply with Annex III.

**Acceptance criterion**

```text
if ABOVE_THRESHOLD(p,z) = true:
    NETWORK_MINIMUM_OK(p,z) =
        N_active_fixed(p,z) >= N_min_eff(p,z)
        AND all active sampling points satisfy VALID_SITING
```

---

### REQ-NETWORK-MINIMUM_COMPOSITION

| Field | Value |
|------|-------|
| Source | Annex III; Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_MIN_STATIONS; T_SITING |

**Rule**  
Where required by Annex III and Annex IV, the monitoring network shall include sampling points representing relevant exposure situations, including urban background and traffic-oriented locations.

**Acceptance criterion**

```text
if pollutant_requires_background_and_traffic_points(p):
    N_background(p,z) >= 1
    AND N_traffic(p,z) >= 1
```

**Note**  
The exact quantitative composition is table-driven and shall be read from `T_MIN_STATIONS` and `T_SITING`.

---

### REQ-NETWORK-TRAFFIC_REPRESENTATION

| Field | Value |
|------|-------|
| Source | Annex III; Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_SITING |

**Rule**  
For pollutants for which traffic exposure is relevant, the network shall include sampling points intended to capture traffic emission contributions.

**Acceptance criterion**

```text
if p in [NO2, PM10, PM2_5, benzene, CO]:
    N_traffic(p,z) >= 1
```

---

### REQ-NETWORK-BACKGROUND_TRAFFIC_BALANCE

| Field | Value |
|------|-------|
| Source | Annex III Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_MIN_STATIONS |

**Rule**  
For applicable pollutants, the number of urban background points and traffic points shall remain balanced according to Annex III.

**Acceptance criterion**

```text
if p in [NO2, PM10, PM2_5, benzene, CO]
AND N_background(p,z) > 0
AND N_traffic(p,z) > 0:
    max(
        N_background(p,z) / N_traffic(p,z),
        N_traffic(p,z) / N_background(p,z)
    ) <= 2
```

---

### REQ-NETWORK-REDUCTION_ALLOWED

| Field | Value |
|------|-------|
| Source | Art. 9(3); Annex I; Annex II; Annex III; Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | permission |
| Dependencies | M_ASSESS; M_LIMITS; M_DATA_QUALITY; M_MOD; M_REPR |

**Rule**  
The minimum number of sampling points for fixed measurements may be reduced only where all Art. 9(3) conditions are satisfied.

Reduction is possible only where pollutant levels exceed the relevant assessment threshold but do not exceed the respective limit values, target values or critical levels.

Indicative measurements or modelling applications must provide sufficient information for:

- assessment with regard to limit values;
- assessment with regard to target values;
- assessment with regard to critical levels;
- alert thresholds;
- information thresholds;
- adequate information to the public.

The number of sampling points and the spatial resolution of indicative measurements or modelling applications shall satisfy Annex V data quality objectives and enable assessment results to meet Annex V requirements.

**Acceptance criterion**

```text
reduction_allowed(p,z) =
    ABOVE_THRESHOLD(p,z) = true
    AND EXCEEDS_STANDARD(p,z) = false
    AND supplementary_assessment_sufficient = true
    AND spatial_resolution_sufficient = true
    AND AnnexV_A_B_E_requirements_satisfied = true
    AND public_information_sufficient = true
    AND ozone_specific_conditions_satisfied = true
```

---

### REQ-NETWORK-REDUCTION_INDICATIVE_MEASUREMENTS

| Field | Value |
|------|-------|
| Source | Art. 9(3)(c) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory when reduction uses indicative measurements |
| Dependencies | M_DATA_QUALITY |

**Rule**  
Where indicative measurements are used to justify reduction of fixed measurements, the number of indicative measurements shall be at least equal to the number of fixed measurements being replaced and shall be evenly distributed over the calendar year.

**Acceptance criterion**

```text
if reduction_allowed = true
AND indicative_measurements_used = true:
    N_indicative(p,z) >= N_fixed_replaced(p,z)
    AND indicative_measurements_evenly_distributed_over_year = true
```

---

### REQ-NETWORK-REDUCTION_OZONE_NO2

| Field | Value |
|------|-------|
| Source | Art. 9(3)(d); Art. 9(5); Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory when ozone fixed points are reduced or operated |
| Dependencies | T_SITING |

**Rule**  
For ozone, nitrogen dioxide shall be measured at all remaining ozone sampling points, except at rural background locations where other measurement methods may be used.

**Acceptance criterion**

```text
if p = ozone:
    for each remaining_ozone_sampling_point st:
        if st.location_type != rural_background:
            NO2_measured_continuously(st) = true
```

---

### REQ-NETWORK-REDUCTION_LIMIT

| Field | Value |
|------|-------|
| Source | Art. 9(3); Annex III Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_MIN_STATIONS |

**Rule**  
The reduction of fixed measurement sampling points shall not reduce the network below the minimum allowed by Annex III and Art. 9(3).

**Acceptance criterion**

```text
if reduction_allowed = true:
    N_active_fixed(p,z) >= N_min_eff(p,z)
else:
    N_active_fixed(p,z) >= N_min(p,z)
```

**Note**  
Where the quantitative reduction ceiling is table-specific, `N_min_eff` shall be calculated from `T_MIN_STATIONS`.

---

### REQ-NETWORK-ADDITIONAL_MEASUREMENTS_AFTER_MODELLED_EXCEEDANCE

| Field | Value |
|------|-------|
| Source | Art. 8(6); Art. 9(3) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mixed |
| Dependencies | M_ASSESS; M_LIMITS; M_REPR; M_MOD; M_DATA_QUALITY |

**Rule**  
Where modelling applications show an exceedance not covered by fixed measurements and their area of spatial representativeness, additional fixed or indicative measurements may or shall be used depending on the trigger.

- For Art. 8(3) or Art. 8(4) assessment cases, additional measurements are permitted.
- For Art. 9(3) reduction cases, additional measurements are mandatory.

Where additional fixed measurements are used, they shall be established within two years from the date of the modelled exceedance.  
Where additional indicative measurements are used, they shall be established within one year from the date of the modelled exceedance.

Additional measurements shall cover at least one calendar year and meet Annex V minimum data coverage requirements.

**Acceptance criterion**

```text
if modelled_exceedance_not_covered_by_fixed_measurements = true:

    if trigger = Art9_3_reduction:
        additional_measurement_required = true

    if trigger in [Art8_3, Art8_4]:
        additional_measurement_permitted = true

    if additional_fixed_measurement_used = true:
        establishment_deadline = modelled_exceedance_date + 2 years

    if additional_indicative_measurement_used = true:
        establishment_deadline = modelled_exceedance_date + 1 year

    measurement_duration >= 1 calendar_year
    AND AnnexV_minimum_coverage_satisfied = true
```

---

### REQ-NETWORK-RELOCATION_RESTRICTION

| Field | Value |
|------|-------|
| Source | Art. 9(7); Annex I; Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | prohibition / conditional permission |
| Dependencies | M_LIMITS; M_MOD; M_REPR; T_SITING |

**Rule**  
Sampling points that recorded exceedances of a relevant limit value or target value during the previous three years shall not be relocated unless relocation is supported by modelling applications or indicative measurements and fully documented in accordance with Annex IV.

**Acceptance criterion**

```text
if recorded_exceedance_in_previous_3_years(st) = true:
    relocation_allowed(st) =
        modelling_or_indicative_support = true
        AND relocation_justification_documented = true
        AND new_location_satisfies_AnnexIV = true
else:
    relocation_allowed(st) = evaluate_under_general_siting_rules
```

---

### REQ-NETWORK-AVERAGE_EXPOSURE_INDICATOR_POINTS

| Field | Value |
|------|-------|
| Source | Art. 9(6); Annex III; Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_ZONE; M_LIMITS; T_MIN_STATIONS; T_SITING |

**Rule**  
Sampling points used for calculating average exposure indicators for PM2.5 and NO2 shall be adequately distributed to reflect general population exposure and shall not be fewer than the number determined by Annex III.

**Acceptance criterion**

```text
if p in [PM2_5, NO2]
AND metric = average_exposure_indicator:
    N_AEI_points(p,u) >= N_min_AEI(p,u)
    AND AEI_spatial_distribution_adequate = true
    AND all AEI sampling points satisfy AnnexIV = true
```

---

### REQ-NETWORK-OZONE_PRECURSOR_SUBSTANCES

| Field | Value |
|------|-------|
| Source | Art. 9(4); Annex VII Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_OZONE_PRECURSORS; T_SITING |

**Rule**  
Sampling points for ozone precursor substances shall be ensured in accordance with Annex VII.

**Acceptance criterion**

```text
ozone_precursor_sampling_points_present =
    required_ozone_precursor_points(p,z) satisfied
```

---

### REQ-NETWORK-OZONE_RURAL_BACKGROUND

| Field | Value |
|------|-------|
| Source | Annex III; Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_MIN_STATIONS; T_SITING |

**Rule**  
For long-term ozone assessment, the network shall include rural background points according to the density and terrain requirements defined in Annex III and Annex IV.

**Acceptance criterion**

```text
rural_ozone_background_density_ok = true
```

**Note**  
Exact density thresholds are table-driven and shall be defined in `T_MIN_STATIONS`.

---

### REQ-NETWORK-PAH_MONITORING

| Field | Value |
|------|-------|
| Source | Art. 9(8) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_PAH; T_SITING |

**Rule**  
At a limited number of sampling points, the contribution of benzo(a)pyrene in ambient air shall be assessed by monitoring relevant polycyclic aromatic hydrocarbons.

PAH sampling points shall, where appropriate, be co-located with benzo(a)pyrene sampling points and selected so that geographical variation and long-term trends can be identified.

**Acceptance criterion**

```text
PAH_monitoring_ok =
    required_PAH_species_monitored = true
    AND PAH_points_colocated_with_BaP_where_required = true
    AND PAH_points_support_geographical_variation_and_trend_analysis = true
```

---

### REQ-NETWORK-UFP_MONITORING

| Field | Value |
|------|-------|
| Source | Art. 9(9); Art. 10; Annex III; Annex VII Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_MIN_STATIONS; T_SUPERSITES; T_SITING |

**Rule**  
Ultrafine particle levels shall be monitored in accordance with Art. 9, Art. 10, Annex III and Annex VII, including sites where high concentrations are likely and applicable supersite requirements.

**Acceptance criterion**

```text
UFP_monitoring_ok =
    N_UFP_points >= N_min_UFP
    AND high_concentration_locations_covered = true
    AND applicable_supersite_UFP_requirements_satisfied = true
```

---

### REQ-NETWORK-SUPERSITES_URBAN_BACKGROUND

| Field | Value |
|------|-------|
| Source | Art. 10(1) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_SUPERSITES; T_SITING |

**Rule**  
Monitoring supersites shall be established at urban background locations at least according to the population-based requirements in Art. 10.

**Acceptance criterion**

```text
N_urban_background_supersites(ms) >= N_min_urban_background_supersites(ms)
```

---

### REQ-NETWORK-SUPERSITES_RURAL_BACKGROUND

| Field | Value |
|------|-------|
| Source | Art. 10(1) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_SUPERSITES; T_SITING |

**Rule**  
Monitoring supersites shall be established at rural background locations according to the territory-size requirements in Art. 10.

**Acceptance criterion**

```text
N_rural_background_supersites(ms) >= N_min_rural_background_supersites(ms)
```

---

### REQ-NETWORK-SUPERSITE_SITING

| Field | Value |
|------|-------|
| Source | Art. 10(2); Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_SITING |

**Rule**  
The siting of monitoring supersites shall be determined in accordance with Annex IV.

**Acceptance criterion**

```text
for each supersite ss:
    supersite_siting_valid(ss) = true
```

---

### REQ-NETWORK-SUPERSITE_COUNTING_TOWARDS_MINIMUM

| Field | Value |
|------|-------|
| Source | Art. 10(3); Annex III; Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | constitutive rule |
| Dependencies | T_MIN_STATIONS; T_SITING |

**Rule**  
Sampling points installed at monitoring supersites may count towards the minimum number of sampling points required for the relevant pollutants only where they meet the applicable requirements of Annex IV and Annex III.

**Acceptance criterion**

```text
if sampling_point_at_supersite = true
AND pollutant_specific_requirements_satisfied = true
AND AnnexIV_requirements_satisfied = true:
    count_towards_minimum_number = true
else:
    count_towards_minimum_number = false
```

---

### REQ-NETWORK-JOINT_SUPERSITES

| Field | Value |
|------|-------|
| Source | Art. 10(4) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | permission / constitutive rule |
| Dependencies | T_SUPERSITES |

**Rule**  
Neighbouring Member States may establish joint monitoring supersites. Joint supersites do not remove each Member State's individual obligation to meet the supersite requirements applicable to it.

**Acceptance criterion**

```text
if joint_supersite_used = true:
    individual_member_state_obligations_preserved = true
```

---

### REQ-NETWORK-SUPERSITE_POLLUTANTS

| Field | Value |
|------|-------|
| Source | Art. 10(5); Annex VII Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_SUPERSITE_POLLUTANTS |

**Rule**  
At monitoring supersites at urban background and rural background locations, pollutants listed in Annex VII Section 1 Tables 1 and 2 shall be monitored.

**Acceptance criterion**

```text
for each supersite ss:
    monitored_pollutants(ss) includes required_AnnexVII_pollutants(ss.type)
```

---

### REQ-NETWORK-RURAL_SUPERSITE_OPTIONAL_NON_MEASUREMENT

| Field | Value |
|------|-------|
| Source | Art. 10(6) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | permission |
| Dependencies | T_SUPERSITE_POLLUTANTS |

**Rule**  
Where the number of rural background supersites exceeds the number of urban background supersites by at least a 2:1 ratio, black carbon, ultrafine particles or ammonia may be omitted at half of the rural background supersites, provided the selection is representative for those pollutants.

**Acceptance criterion**

```text
if N_rural_background_supersites >= 2 * N_urban_background_supersites:
    optional_non_measurement_allowed_for_selected_pollutants = true
    AND representative_selection = true
```

---

### REQ-NETWORK-MEASUREMENT_METHODS

| Field | Value |
|------|-------|
| Source | Art. 11(1), Art. 11(3); Annex V; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory / permission |
| Dependencies | M_DATA_QUALITY; T_METHODS |

**Rule**  
Reference measurement methods shall be used in accordance with Annex VI. Other measurement methods may be used only under the conditions set out in Annex VI.

All air quality assessment data shall comply with Annex V data quality objectives.

**Acceptance criterion**

```text
if method = reference_method:
    method_valid = true
else:
    method_valid = AnnexVI_equivalence_or_conditions_satisfied

assessment_data_quality_ok = AnnexV_data_quality_objectives_satisfied
```

---

### REQ-NETWORK-MODELLING_METHODS_LINK

| Field | Value |
|------|-------|
| Source | Art. 11(2); Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_MOD; M_MODEL_QA |

**Rule**  
Where modelling applications are used in the assessment or to support network reduction, they shall satisfy the conditions set out in Annex VI.

**Acceptance criterion**

```text
if modelling_used_for_network_or_assessment = true:
    VALID_MODEL = AnnexVI_PointE_conditions_satisfied
```

---

## Network-level adequacy decision

```text
NETWORK_OK(p,z) =
    sampling_point_location_ok
    AND minimum_adequacy_ok
    AND composition_ok
    AND reduction_conditions_ok_if_reduction_applied
    AND relocation_conditions_ok
    AND additional_measurements_ok_if_triggered
    AND pollutant_specific_requirements_ok
    AND method_requirements_ok
```

---

## Interactions with other modules

- `M_ZONE` — provides zones and average exposure territorial units
- `M_ASSESS` — determines above/below assessment threshold and assessment regime
- `M_LIMITS` — determines exceedances of limit values, target values and critical levels
- `M_DATA_QUALITY` — validates measurement data and Annex V requirements
- `M_REPR` — defines spatial representativeness areas
- `M_MOD` — provides modelled concentration fields and supports relocation/reduction decisions
- `M_MODEL_QA` — validates modelling applications under Annex VI
- `M_PUBLIC_INFORMATION` — consumes network/assessment outputs where public information is triggered
- `T_MIN_STATIONS` — quantitative requirements from Annex III
- `T_SITING` — siting and location requirements from Annex IV
- `T_SUPERSITES` — supersite population/area thresholds and pollutant obligations

---

## Output

```text
network_status:
  zone_id
  pollutant
  assessment_threshold_status

  sampling_points:
    active_fixed_count
    effective_minimum_required
    minimum_number_satisfied
    siting_valid
    background_points
    traffic_points
    rural_background_points

  reduction:
    reduction_applied
    reduction_allowed
    fixed_points_replaced
    indicative_measurements_used
    modelling_used
    reduction_conditions_satisfied

  relocation:
    relocation_requested
    relocation_allowed
    relocation_documented

  additional_measurements:
    modelled_exceedance_not_covered
    additional_measurement_required
    additional_measurement_permitted
    additional_measurement_type
    establishment_deadline
    minimum_duration_satisfied
    AnnexV_coverage_satisfied

  pollutant_specific:
    ozone_precursors_ok
    ozone_NO2_monitoring_ok
    PAH_monitoring_ok
    UFP_monitoring_ok
    AEI_points_ok

  supersites:
    urban_background_supersites_required
    urban_background_supersites_present
    rural_background_supersites_required
    rural_background_supersites_present
    supersite_pollutants_ok

  method_status:
    reference_or_equivalent_methods_ok
    modelling_conditions_ok
    data_quality_objectives_ok

  network_ok
```

---

## Notes

- Network adequacy is not equivalent to compliance with limit or target values.
- Lawful reduction of fixed measurements requires a stricter condition set than the mere availability of a valid model.
- A modelled exceedance outside the spatial representativeness of fixed measurements can trigger additional monitoring consequences.
- Supersites may support minimum-number obligations only when pollutant-specific and siting requirements are satisfied.
- Relocation of exceedance-recording sampling points is exceptional and requires modelling or indicative support plus full documentation.
