# M_REPR — Spatial representativeness of sampling points

## Normative references

- Art. 4(26) Directive (EU) 2024/2881
- Art. 8 Directive (EU) 2024/2881
- Art. 9 Directive (EU) 2024/2881
- Art. 10 Directive (EU) 2024/2881, where supersite sampling points are used for assessment or minimum-number obligations
- Art. 11 Directive (EU) 2024/2881
- Art. 21 Directive (EU) 2024/2881, for transboundary assessment hooks
- Annex IV (siting criteria and spatial representativeness)
- Annex V (data quality objectives)
- Annex VI (reference methods and modelling conditions)
- Implementing acts under Art. 8(7), where applicable

---

## Description

This module defines and evaluates the **spatial representativeness** of sampling points.

Spatial representativeness is the regulatory relationship between a sampling point and the geographical area for which its measured concentration can be considered representative for a pollutant, metric and assessment period.

The module determines:

- the representativeness area of each sampling point;
- whether a fixed measurement covers a modelled exceedance area;
- whether an exceedance area is uncovered by fixed measurements;
- whether representativeness is sufficient for compliance assessment;
- whether representativeness supports reduction of fixed measurements;
- whether relocation of a sampling point is justified and does not create unacceptable coverage loss;
- whether spatial information is sufficient for downstream planning, reporting and public-information modules.

This module does **not**:

- determine compliance with limit values or target values;
- validate the monitoring network as a whole;
- validate model performance;
- establish new sampling points;
- establish air quality plans or short-term action plans.

Those functions are handled by dedicated modules.

---

## Scope

This module applies to:

- fixed measurements;
- indicative measurements where used for spatial assessment or network reduction;
- model-supported representativeness analysis;
- hybrid measurement–model representativeness determinations;
- modelled exceedance areas;
- hotspot areas;
- supersite sampling points where they are used as sampling points for minimum-number or assessment purposes.

Representativeness shall be assessed by pollutant, metric, station type, assessment period and spatial context.

---

## Definitions

```text
p = pollutant
m = regulatory metric
z = zone
u = average exposure territorial unit or relevant territorial unit
sp = sampling point
x = location
A = geographic area
```

```text
C_sp(p,m,period) =
central concentration value measured at sampling point sp
for pollutant p, metric m and assessment period
```

```text
C_field(p,m,x,period) =
concentration field at location x,
derived from validated modelling, valid measurements or a hybrid method
```

```text
TOL(p,m) =
representativeness tolerance for pollutant p and metric m,
defined in T_REPR_TOLERANCE or applicable methodology
```

```text
Δ(sp,p,m) =
representativeness tolerance interval half-width for sampling point sp,
pollutant p and metric m
```

```text
INTERVAL(sp,p,m) =
[C_sp(p,m) - Δ(sp,p,m), C_sp(p,m) + Δ(sp,p,m)]
```

```text
AREA_REPR(sp,p,m,period) =
geographical area where concentrations are sufficiently similar
to the concentration represented by sampling point sp
for pollutant p, metric m and assessment period
```

```text
MODELLED_EXCEEDANCE_AREA(p,m,period) =
area where a modelling application indicates exceedance of a relevant limit value,
target value, critical level, alert threshold or information threshold
```

```text
COVERED_BY_FIXED_MEASUREMENTS(A,p,m) =
area A is covered by at least one valid fixed-measurement representativeness area
for pollutant p and metric m
```

```text
UNCOVERED_AREA(A,p,m) =
A minus the union of applicable fixed-measurement representativeness areas
```

```text
REPR_VALID(sp,p,m,period) =
representativeness area for sampling point sp is valid for the stated use
```

---

## Normative requirements

---

### REQ-REPR-AREA_DEFINITION

| Field | Value |
|------|-------|
| Source | Art. 4(26); Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_REPR_TOLERANCE; T_SITING |

**Rule**  
A sampling point representativeness area shall be defined as the geographical area for which the concentration measured at the sampling point is representative, taking into account pollutant, metric, station type, emission context, terrain and dispersion conditions.

**Acceptance criterion**

```text
AREA_REPR(sp,p,m,period) =
    { x |
      C_field(p,m,x,period) ∈ INTERVAL(sp,p,m)
      AND spatial_context_compatible(x,sp) = true
      AND station_type_compatible(x,sp) = true
    }
```

---

### REQ-REPR-POLLUTANT_METRIC_SPECIFICITY

| Field | Value |
|------|-------|
| Source | Art. 8; Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_REPR_TOLERANCE; T_LIMIT_VALUES |

**Rule**  
Representativeness shall be determined separately for each pollutant, regulatory metric and assessment period.

**Acceptance criterion**

```text
AREA_REPR(sp,p1,m1,period) is not automatically valid for p2 or m2
unless pollutant_metric_equivalence_documented = true
```

---

### REQ-REPR-STATION_TYPE_AND_CONTEXT

| Field | Value |
|------|-------|
| Source | Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_SITING; M_NETWORK |

**Rule**  
A representativeness area shall be consistent with the station type and the exposure situation represented by the sampling point.

Relevant context includes, where applicable:

- traffic exposure;
- urban background;
- suburban background;
- rural background;
- industrial influence;
- hotspot context;
- ozone rural background context;
- ecosystem or vegetation protection context.

**Acceptance criterion**

```text
if location_context(x) incompatible with station_type(sp):
    x excluded from AREA_REPR(sp,p,m,period)
```

---

### REQ-REPR-GEOGRAPHIC_AND_EMISSION_REFINEMENT

| Field | Value |
|------|-------|
| Source | Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_MOD; T_SITING |

**Rule**  
The preliminary representativeness area shall be refined using geographic, typological, emission-based and dispersion-based criteria.

**Acceptance criterion**

```text
AREA_REPR_refined =
    AREA_REPR_preliminary
    minus locations with incompatible geography
    minus locations with incompatible emission profile
    minus locations with incompatible dispersion conditions
    minus locations outside applicable territorial boundaries
```

---

### REQ-REPR-TERRITORIAL_BOUNDARIES

| Field | Value |
|------|-------|
| Source | Art. 6; Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_ZONE |

**Rule**  
Representativeness areas shall be linked to the relevant zone or territorial unit. Where an area crosses zone or Member State boundaries, the cross-boundary portion shall be explicitly recorded rather than silently discarded.

**Acceptance criterion**

```text
AREA_REPR(sp,p,m) intersects one_or_more territorial_units

if AREA_REPR crosses zone_boundary:
    cross_boundary_representativeness = true
    affected_zones recorded
```

**Note**  
For ordinary zonal compliance aggregation, modules may restrict use to the relevant zone. For transboundary assessment and reporting, the full geometry may be relevant.

---

### REQ-REPR-MODEL_USAGE_FOR_AREA_DEFINITION

| Field | Value |
|------|-------|
| Source | Art. 8; Art. 11; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_MOD; M_MODEL_QA; M_DATA_QUALITY |

**Rule**  
Modelling applications may be used to determine or refine spatial representativeness only if the model satisfies applicable regulatory conditions and the underlying assessment data comply with data quality objectives.

**Acceptance criterion**

```text
if model_used_for_representativeness = true:
    require VALID_MODEL = true
    require AnnexVI_conditions_satisfied = true
    require input_data_quality_valid = true
```

---

### REQ-REPR-HYBRID_METHOD

| Field | Value |
|------|-------|
| Source | Art. 8; Annex IV; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | permitted method |
| Dependencies | M_MOD; M_DATA_QUALITY; M_MODEL_QA |

**Rule**  
Representativeness may be determined using a hybrid method combining measurements, modelling applications, emission information and expert judgement, provided each component is documented and valid for its intended use.

**Acceptance criterion**

```text
if method = hybrid:
    measurement_component_valid = true
    AND modelling_component_valid_if_used = true
    AND emission_context_documented = true
    AND expert_judgement_documented_if_used = true
```

---

### REQ-REPR-COVERAGE_OF_MODELLED_EXCEEDANCE_AREA

| Field | Value |
|------|-------|
| Source | Art. 8(5); Art. 8(6) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_ASSESS; M_LIMITS; M_MOD; M_NETWORK |

**Rule**  
Where modelling applications show an exceedance, this module shall determine whether the modelled exceedance area is covered by fixed measurements and their areas of spatial representativeness.

**Acceptance criterion**

```text
fixed_measurements_cover_modelled_exceedance_area(A,p,m) =
    MODELLED_EXCEEDANCE_AREA(A,p,m)
    ⊆ union{ AREA_REPR(sp,p,m) |
              sp is fixed_measurement
              AND REPR_VALID(sp,p,m) = true }
```

```text
if fixed_measurements_cover_modelled_exceedance_area = true:
    coverage_status = fully_covered_by_fixed_measurements
else:
    coverage_status = not_fully_covered_by_fixed_measurements
    uncovered_area = UNCOVERED_AREA(MODELLED_EXCEEDANCE_AREA,p,m)
```

---

### REQ-REPR-MODELLED_EXCEEDANCE_NON_EXCEEDANCE_SUPPORT

| Field | Value |
|------|-------|
| Source | Art. 8(5) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | conditional support rule |
| Dependencies | M_LIMITS; M_ASSESS |

**Rule**  
A modelled exceedance may be evaluated as a non-exceedance only where fixed measurements with spatial representativeness covering the modelled area of exceedance are available.

This module provides the spatial-coverage determination. The legal compliance consequence is decided by `M_LIMITS`.

**Acceptance criterion**

```text
if modelled_exceedance_area_fully_covered_by_fixed_measurements = true:
    supports_modelled_exceedance_non_exceedance_evaluation = true
else:
    supports_modelled_exceedance_non_exceedance_evaluation = false
```

---

### REQ-REPR-UNCOVERED_MODELLED_EXCEEDANCE_AREA

| Field | Value |
|------|-------|
| Source | Art. 8(6) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_ASSESS; M_NETWORK; M_MOD |

**Rule**  
Where a modelled exceedance is not covered by fixed measurements and their representativeness areas, the uncovered area shall be identified and made available to `M_NETWORK` and `M_ASSESS`.

**Acceptance criterion**

```text
if coverage_status = not_fully_covered_by_fixed_measurements:
    uncovered_modelled_exceedance_area_geometry is not null
    affected_population_or_area_estimate recorded where available
    candidate_monitoring_locations may be generated by M_NETWORK or M_MOD
```

---

### REQ-REPR-ADDITIONAL_MEASUREMENT_TARGETING

| Field | Value |
|------|-------|
| Source | Art. 8(6); Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_NETWORK; M_MOD; M_ASSESS |

**Rule**  
Where additional fixed or indicative measurements are required or permitted after an uncovered modelled exceedance, the uncovered representativeness gap shall be used to support placement of additional measurements.

**Acceptance criterion**

```text
if additional_measurement_required_or_permitted = true:
    measurement_target_area = uncovered_modelled_exceedance_area
    candidate_locations_consistent_with_AnnexIV = true
```

---

### REQ-REPR-NETWORK_REDUCTION_SUPPORT

| Field | Value |
|------|-------|
| Source | Art. 9(3); Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory when reduction is requested |
| Dependencies | M_NETWORK; M_ASSESS; M_MOD; M_DATA_QUALITY |

**Rule**  
Where reduction of fixed measurement sampling points is requested, this module shall assess whether the remaining fixed measurements, indicative measurements and modelling applications provide sufficient spatial coverage for assessment.

**Acceptance criterion**

```text
if Art9_3_reduction_requested = true:
    network_reduction_spatial_coverage_ok =
        remaining_fixed_measurement_coverage_sufficient = true
        AND supplementary_modelling_or_indicative_coverage_sufficient = true
        AND spatial_resolution_sufficient = true
        AND uncovered_high_risk_areas = none
```

---

### REQ-REPR-INDICATIVE_MEASUREMENTS_SUPPORT

| Field | Value |
|------|-------|
| Source | Art. 9(3); Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory when indicative measurements support reduction or assessment |
| Dependencies | M_DATA_QUALITY; M_NETWORK |

**Rule**  
Where indicative measurements are used to support assessment or reduction of fixed measurements, their spatial representativeness shall be evaluated and documented.

**Acceptance criterion**

```text
if indicative_measurements_used_for_assessment_or_reduction = true:
    AREA_REPR(indicative_point,p,m) defined
    AND data_quality_objectives_satisfied = true
    AND temporal_distribution_requirements_satisfied_if_Art9_3 = true
```

---

### REQ-REPR-RELOCATION_IMPACT_ASSESSMENT

| Field | Value |
|------|-------|
| Source | Art. 9(7); Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory when relocation is proposed |
| Dependencies | M_NETWORK; M_MOD; M_LIMITS |

**Rule**  
Where relocation of a sampling point is proposed, the representativeness areas before and after relocation shall be compared.

For sampling points with recorded exceedances of relevant limit values or target values during the previous three years, relocation must be supported by modelling applications or indicative measurements and fully documented.

**Acceptance criterion**

```text
if relocation_proposed(sp) = true:
    old_area = AREA_REPR(sp,p,m,before_relocation)
    new_area = AREA_REPR(sp,p,m,after_relocation)
    coverage_loss_area = old_area - new_area
    coverage_gain_area = new_area - old_area

    if recorded_exceedance_in_previous_3_years(sp) = true:
        relocation_support_valid =
            modelling_or_indicative_support = true
            AND relocation_justification_documented = true
            AND unacceptable_exceedance_coverage_loss = false
```

---

### REQ-REPR-OVERLAP_RESOLUTION

| Field | Value |
|------|-------|
| Source | Annex IV Dir. (EU) 2024/2881; implementing methodology where applicable |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_NETWORK; M_MOD |

**Rule**  
Where a location belongs to multiple representativeness areas, assignment or aggregation shall be resolved using documented criteria.

Relevant criteria include:

- station type;
- emission coherence;
- concentration level;
- distance and dispersion context;
- regulatory metric;
- data quality and method confidence.

**Acceptance criterion**

```text
if x ∈ AREA_REPR(sp1,p,m) AND x ∈ AREA_REPR(sp2,p,m):
    overlap_resolution_method documented
    selected_or_combined_representative_value justified
```

---

### REQ-REPR-CONFLICT_BETWEEN_MEASUREMENT_AND_MODEL

| Field | Value |
|------|-------|
| Source | Art. 8(5); Annex IV; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_MOD; M_LIMITS; M_MODEL_QA |

**Rule**  
Where measurements and modelling applications provide inconsistent results within an area of spatial representativeness, the conflict shall be explicitly flagged and resolved according to the applicable assessment and compliance rules.

A valid fixed measurement covering the modelled exceedance area may support evaluation of the modelled exceedance as a non-exceedance. Outside valid representativeness areas, modelling results remain relevant for assessment.

**Acceptance criterion**

```text
if modelled_exceedance = true
AND fixed_measurement_non_exceedance = true:

    if modelled_exceedance_area fully covered by fixed_measurement_AREA_REPR:
        conflict_resolution = fixed_measurement_supports_non_exceedance_evaluation
    else:
        conflict_resolution = modelled_exceedance_remains_relevant_for_uncovered_area
```

---

### REQ-REPR-VALIDITY_FOR_COMPLIANCE

| Field | Value |
|------|-------|
| Source | Art. 8; Art. 11; Annex V; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_LIMITS; M_DATA_QUALITY; M_MODEL_QA |

**Rule**  
A representativeness area may be used for compliance verification only where the underlying measurement or model data satisfy the applicable method and data quality requirements.

**Acceptance criterion**

```text
valid_for_compliance(sp,p,m) =
    REPR_VALID(sp,p,m) = true
    AND underlying_data_quality_valid = true
    AND method_valid_under_AnnexVI = true
```

---

### REQ-REPR-VALIDITY_FOR_PUBLIC_INFORMATION_AND_REPORTING

| Field | Value |
|------|-------|
| Source | Art. 22; Art. 23 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_PUBLIC_INFORMATION; M_REPORTING; M_ASSESS |

**Rule**  
Representativeness outputs used for public information or reporting shall retain sufficient metadata to explain the spatial basis of assessment results.

**Acceptance criterion**

```text
if representativeness_output_used_for_public_information_or_reporting = true:
    include geometry
    include method
    include pollutant
    include metric
    include assessment_period
    include uncertainty_or_confidence_metadata_where_available
```

---

### REQ-REPR-TRANSBOUNDARY_CONTEXT

| Field | Value |
|------|-------|
| Source | Art. 21 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_TRANSBOUNDARY; M_ZONE; M_MOD |

**Rule**  
Where representativeness areas, modelled exceedance areas or source-contribution areas cross or affect neighbouring Member States, the relevant cross-border geometry and assessment metadata shall be preserved for transboundary modules.

**Acceptance criterion**

```text
if AREA_REPR or MODELLED_EXCEEDANCE_AREA crosses national_boundary
OR transboundary_contribution_relevant = true:
    transboundary_geometry_recorded = true
    affected_member_states_recorded = true
```

---

### REQ-REPR-EXPERT_JUDGEMENT

| Field | Value |
|------|-------|
| Source | Annex IV; implementing methodology where applicable |
| Status | STABLE |
| Type | mandatory when used |
| Dependencies | M_MOD; M_NETWORK |

**Rule**  
Expert judgement may be used to refine or validate representativeness where spatial heterogeneity, complex terrain, non-uniform dispersion or sparse data make purely quantitative determination insufficient.

Expert judgement shall be documented.

**Acceptance criterion**

```text
if expert_judgement_used = true:
    expert_judgement_documented = true
    rationale_recorded = true
    affected_geometry_recorded = true
```

---

### REQ-REPR-UPDATE_FREQUENCY

| Field | Value |
|------|-------|
| Source | Art. 7(2); Art. 8; Art. 9; Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_ASSESS; M_NETWORK; M_MOD |

**Rule**  
Representativeness areas shall be reviewed when assessment classification, network configuration, emissions, activity patterns, terrain/dispersion understanding or modelling evidence changes materially.

At minimum, representativeness should be reviewed in coordination with the five-year classification and assessment review cycle.

**Acceptance criterion**

```text
representativeness_review_due =
    years_since(last_repr_review) >= 5
    OR network_changed = true
    OR sampling_point_relocated = true
    OR significant_emission_change = true
    OR significant_activity_change = true
    OR new_modelled_exceedance_detected = true
    OR updated_methodology_available = true
```

---

## Representativeness-level decision logic

```text
for each sampling point sp, pollutant p and metric m:

    validate method and data quality

    derive preliminary concentration-similarity area

    refine area using:
        station type
        emission context
        geography
        dispersion context
        territorial boundaries
        expert judgement where necessary

    store AREA_REPR(sp,p,m,period)

for each modelled exceedance area A:

    compare A with union of fixed-measurement AREA_REPR geometries

    if A fully covered:
        support possible non-exceedance evaluation under Art. 8(5)

    else:
        identify uncovered area
        expose uncovered area to M_ASSESS and M_NETWORK
```

---

## Interactions with other modules

- `M_ZONE` — provides zones, territorial units and boundary context
- `M_ASSESS` — uses representativeness for modelled exceedance status and assessment method outputs
- `M_NETWORK` — uses representativeness for network coverage, reduction, relocation and additional measurements
- `M_LIMITS` — uses representativeness for compliance verification and modelled exceedance qualification
- `M_MOD` — provides concentration fields and modelled exceedance areas
- `M_MODEL_QA` — validates modelling applications used in representativeness determination
- `M_DATA_QUALITY` — validates measurement and indicative-measurement data
- `M_PUBLIC_INFORMATION` — consumes spatial metadata for public-facing assessment outputs
- `M_REPORTING` — consumes geometries and metadata for reporting
- `M_TRANSBOUNDARY` — consumes cross-boundary representativeness and exceedance information
- `T_REPR_TOLERANCE` — tolerance values and methodology parameters
- `T_SITING` — station type and siting criteria

---

## Output

```text
representativeness_status:
  sampling_point_id
  pollutant
  metric
  assessment_period
  station_type
  zone_id
  territorial_unit_id

  area:
    geometry
    area_size
    method = measurement | modelling | hybrid | expert_judgement
    concentration_interval
    central_value
    tolerance
    confidence_or_uncertainty_metadata

  validity:
    data_quality_valid
    method_valid
    model_valid_if_used
    representativeness_valid
    valid_for_compliance
    valid_for_network_reduction
    valid_for_relocation_support

  coverage:
    covers_modelled_exceedance_area
    modelled_exceedance_area_id
    uncovered_modelled_exceedance_area_geometry
    covering_sampling_points
    overlap_resolution_status

  relocation:
    old_area_geometry
    new_area_geometry
    coverage_loss_area
    coverage_gain_area
    relocation_support_valid

  downstream:
    public_information_metadata_ready
    reporting_metadata_ready
    transboundary_context_available
```

---

## Notes

- Spatial representativeness is not merely a geometric buffer around a station; it is a regulatory determination linking measured concentration, station type, pollutant, metric, territory and assessment use.
- A modelled exceedance cannot be disregarded solely because a fixed measurement exists somewhere in the zone. The fixed measurement must have a valid representativeness area covering the modelled exceedance area.
- Uncovered modelled exceedance areas are not automatically compliance failures, but they are assessment-critical and may trigger additional monitoring consequences.
- Representativeness areas must be versioned because network changes, emission changes and updated modelling can alter their regulatory use.
- Cross-boundary representativeness should be preserved as metadata where relevant for transboundary cooperation and reporting.
