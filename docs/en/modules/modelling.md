# M_MOD — Modelling applications

## Normative references

- Art. 8 Directive (EU) 2024/2881
- Art. 9(3), Art. 9(7) Directive (EU) 2024/2881
- Art. 11 Directive (EU) 2024/2881
- Art. 15 Directive (EU) 2024/2881
- Art. 18 Directive (EU) 2024/2881
- Art. 19 Directive (EU) 2024/2881
- Art. 20 Directive (EU) 2024/2881
- Art. 21 Directive (EU) 2024/2881
- Annex I (limit values, target values, critical levels, alert and information thresholds)
- Annex IV (assessment locations, siting and spatial representativeness context)
- Annex V (data quality objectives)
- Annex VI (reference methods and modelling applications)
- Annex VIII (air quality plans and roadmaps, where projections are used)
- Implementing acts under Art. 8(7), where applicable

---

## Description

This module governs the **regulatory use of modelling applications** in the air quality assessment framework.

Modelling applications may be used to:

- describe the spatial distribution of pollutant concentrations;
- supplement fixed measurements in zones above assessment thresholds;
- provide the primary or sufficient assessment basis in zones below assessment thresholds;
- identify modelled exceedance areas and hotspots;
- determine whether exceedance areas are covered by fixed measurements and their spatial representativeness areas;
- support reduction of fixed measurement sampling points under Art. 9(3);
- support relocation of sampling points under Art. 9(7);
- support alert and information threshold prediction;
- support air quality plans, air quality roadmaps and postponement requests through projections;
- support transboundary source-contribution analysis.

This module distinguishes between different legal roles of modelling:

```text
assessment_modelling
spatial_distribution_modelling
representativeness_modelling
hotspot_modelling
network_reduction_modelling
relocation_support_modelling
forecast_modelling
projection_modelling
transboundary_contribution_modelling
```

The module does **not** itself validate modelling applications in detail. Model validation and quality assurance are handled by `M_MODEL_QA`. This module consumes the result of that validation and determines whether modelling may be used for a given regulatory purpose.

---

## Scope

This module applies to modelling applications used for:

- assessment under Art. 8;
- reduction of fixed measurement points under Art. 9(3);
- relocation support under Art. 9(7);
- representativeness analysis under `M_REPR`;
- compliance verification under `M_LIMITS`;
- public information and alert/information threshold prediction under Art. 15 and Art. 22;
- air quality plans, roadmaps and postponement requests under Art. 18 and Art. 19;
- short-term action plan triggers under Art. 20;
- transboundary cooperation and source-contribution analysis under Art. 21.

---

## Definitions

```text
p = pollutant
z = zone
u = average exposure territorial unit or relevant territorial unit
x = location
t = time
m = regulatory metric
A = geographic area
```

```text
C_model(p,x,t) =
modelled concentration of pollutant p at location x and time t
```

```text
C_meas(p,sp,t) =
measured concentration of pollutant p at sampling point sp and time t
```

```text
MODEL_VALID(model, purpose) =
model satisfies the applicable Annex VI / M_MODEL_QA requirements
for the specified regulatory purpose
```

```text
MODEL_USABLE(model, purpose) =
MODEL_VALID(model, purpose) = true
AND input_data_quality_valid = true
AND purpose_allowed_by_assessment_regime = true
```

```text
MODELLED_EXCEEDANCE_AREA(p,m,period) =
{ x | C_model(p,x,period,m) > STANDARD(p,m) }
```

```text
HOTSPOT_AREA(p,m,period) =
area where modelled or assessed concentrations indicate a localised elevated concentration
or exceedance requiring assessment or monitoring attention
```

```text
FORECAST_VALUE(p,x,t_future) =
modelled or forecast concentration for a future time t_future
```

```text
PROJECTION_SCENARIO(s) =
a modelled future concentration scenario based on assumptions about emissions,
measures, activity levels, meteorology or transboundary contributions
```

---

## Normative requirements

---

### REQ-MOD-PURPOSE_CLASSIFICATION

| Field | Value |
|------|-------|
| Source | Art. 8; Art. 9; Art. 11 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_ASSESS; M_MODEL_QA |

**Rule**  
Every use of modelling shall be classified by regulatory purpose before it is used in assessment, compliance, network, planning, public-information or reporting workflows.

**Acceptance criterion**

```text
if modelling_application_used = true:
    modelling_purpose ∈ [
        assessment_modelling,
        spatial_distribution_modelling,
        representativeness_modelling,
        hotspot_modelling,
        network_reduction_modelling,
        relocation_support_modelling,
        forecast_modelling,
        projection_modelling,
        transboundary_contribution_modelling
    ]
```

---

### REQ-MOD-MODEL_VALIDITY_REQUIRED

| Field | Value |
|------|-------|
| Source | Art. 11(2); Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_MODEL_QA; M_DATA_QUALITY |

**Rule**  
Modelling applications may be used for regulatory assessment only if they satisfy the applicable conditions for their intended purpose.

**Acceptance criterion**

```text
if use_model_for_regulatory_purpose = true:
    MODEL_VALID(model, modelling_purpose) = true
    AND input_data_quality_valid = true
```

---

### REQ-MOD-ASSESSMENT_ABOVE_THRESHOLD

| Field | Value |
|------|-------|
| Source | Art. 8(2) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | permission |
| Dependencies | M_ASSESS; M_REPR; M_NETWORK |

**Rule**  
In zones classified above assessment thresholds, fixed measurements are required. Modelling applications may supplement fixed measurements where appropriate to provide information on spatial distribution and spatial representativeness.

**Acceptance criterion**

```text
if M_ASSESS.threshold_classification(p,z) = above_threshold:
    model_role = supplementary
    fixed_measurements_required = true
    model_may_support = [
        spatial_distribution,
        spatial_representativeness,
        hotspot_identification,
        coverage_gap_detection
    ]
```

---

### REQ-MOD-ASSESSMENT_BELOW_THRESHOLD

| Field | Value |
|------|-------|
| Source | Art. 8(4) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | constitutive rule |
| Dependencies | M_ASSESS; M_MODEL_QA |

**Rule**  
In zones classified below assessment thresholds, modelling applications may be sufficient for assessment, alone or in combination with indicative measurements or objective estimation.

**Acceptance criterion**

```text
if M_ASSESS.threshold_classification(p,z) = below_threshold:
    model_role may be primary_assessment_method
    MODEL_USABLE(model, assessment_modelling) = true
```

---

### REQ-MOD-LIMIT_TARGET_EXCEEDANCE_ASSESSMENT

| Field | Value |
|------|-------|
| Source | Art. 8(3) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where modelling is selected |
| Dependencies | M_ASSESS; M_LIMITS; M_REPR |

**Rule**  
Where levels exceed a relevant limit value or target value, ambient air quality shall be assessed using modelling applications or indicative measurements. Where modelling is used, it shall provide information on spatial distribution and spatial representativeness of fixed measurements.

**Acceptance criterion**

```text
if M_LIMITS.exceeds_limit_or_target_value(p,z) = true
AND modelling_applications_used = true:
    spatial_distribution_output_required = true
    spatial_representativeness_output_required = true
```

---

### REQ-MOD-MODELLING_FREQUENCY_ART8_3

| Field | Value |
|------|-------|
| Source | Art. 8(3) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory with practicability qualifier |
| Dependencies | M_ASSESS |

**Rule**  
Where modelling applications are used under Art. 8(3), they shall be carried out at least every five years, as far as practicable.

**Acceptance criterion**

```text
if modelling_used_under_Art8_3 = true:
    modelling_frequency_ok =
        years_since(previous_modelling_run) <= 5
        OR practicability_exception_documented = true
```

---

### REQ-MOD-SPATIAL_DISTRIBUTION_OUTPUT

| Field | Value |
|------|-------|
| Source | Art. 8(2), Art. 8(3), Art. 8(4) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where modelling is used for spatial assessment |
| Dependencies | M_ASSESS; M_REPR |

**Rule**  
Where modelling is used for assessment or supplementation, it shall provide a spatial concentration field sufficient for the stated assessment purpose.

**Acceptance criterion**

```text
if model_role includes spatial_distribution:
    model_output includes concentration_field
    concentration_field has documented spatial_resolution
    concentration_field covers relevant assessment_domain
```

---

### REQ-MOD-REPRESENTATIVENESS_SUPPORT

| Field | Value |
|------|-------|
| Source | Art. 8(3); Art. 8(5); Annex IV; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where modelling supports representativeness |
| Dependencies | M_REPR; M_MODEL_QA |

**Rule**  
Where modelling is used to support spatial representativeness, it shall provide the concentration field and spatial evidence needed by `M_REPR` to determine representativeness areas and coverage of modelled exceedance areas.

**Acceptance criterion**

```text
if model_role includes representativeness_modelling:
    provide C_field(p,m,x,period)
    provide spatial_resolution
    provide uncertainty_or_confidence_metadata_where_available
    provide model_validity_status
```

---

### REQ-MOD-MODELLED_EXCEEDANCE_AREA_DELINEATION

| Field | Value |
|------|-------|
| Source | Art. 8(5), Art. 8(6) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where model indicates exceedance |
| Dependencies | M_LIMITS; M_REPR; M_ASSESS |

**Rule**  
Where modelling applications indicate an exceedance, the modelled exceedance area shall be delineated and passed to `M_REPR` and `M_ASSESS`.

**Acceptance criterion**

```text
if any x in assessment_domain satisfies C_model(p,x,m,period) > STANDARD(p,m):
    MODELLED_EXCEEDANCE_AREA(p,m,period) defined
    modelled_exceedance_area_geometry is not null
    modelled_exceedance_area_metadata recorded
```

---

### REQ-MOD-MODELLED_EXCEEDANCE_STATUS_LINK

| Field | Value |
|------|-------|
| Source | Art. 8(5) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_REPR; M_LIMITS; M_ASSESS |

**Rule**  
A modelled exceedance may be evaluated as a non-exceedance only where `M_REPR` determines that fixed measurements with valid spatial representativeness cover the modelled exceedance area.

This module produces the modelled exceedance area. It does not by itself decide the legal compliance status.

**Acceptance criterion**

```text
if modelled_exceedance_area_defined = true:
    send MODELLED_EXCEEDANCE_AREA to M_REPR
    receive fixed_measurements_cover_modelled_exceedance_area
    send modelled_exceedance_package to M_LIMITS
```

---

### REQ-MOD-HOTSPOT_IDENTIFICATION

| Field | Value |
|------|-------|
| Source | Art. 8(6); Art. 9(3); Annex IV Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where modelling used for hotspot analysis |
| Dependencies | M_ASSESS; M_REPR; M_NETWORK |

**Rule**  
Modelling applications used for spatial assessment shall identify localised exceedances or hotspots relevant to network coverage and additional measurement decisions.

**Acceptance criterion**

```text
if model_role includes hotspot_modelling:
    HOTSPOT_AREA(p,m,period) defined where applicable
    hotspot_covered_by_fixed_measurements = M_REPR.coverage_status(HOTSPOT_AREA)
```

---

### REQ-MOD-ADDITIONAL_MEASUREMENT_TRIGGER_LINK

| Field | Value |
|------|-------|
| Source | Art. 8(6); Art. 9(3) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_ASSESS; M_REPR; M_NETWORK |

**Rule**  
Where modelling shows an exceedance not covered by fixed measurements and their representativeness areas, the uncovered area shall be provided to `M_NETWORK` to support additional fixed or indicative measurements.

**Acceptance criterion**

```text
if M_REPR.coverage_status(MODELLED_EXCEEDANCE_AREA) = not_fully_covered_by_fixed_measurements:
    provide uncovered_modelled_exceedance_area to M_NETWORK
    provide modelled_exceedance_date
    provide pollutant, metric, period and source model metadata
```

---

### REQ-MOD-NETWORK_REDUCTION_SUPPORT

| Field | Value |
|------|-------|
| Source | Art. 9(3); Annex V; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory when modelling supports reduction |
| Dependencies | M_NETWORK; M_ASSESS; M_REPR; M_DATA_QUALITY; M_MODEL_QA |

**Rule**  
Where modelling applications are used to support reduction of fixed measurement sampling points, they shall provide sufficient information for assessment with regard to limit values, target values, critical levels, alert thresholds and information thresholds, as well as adequate information for the public.

They shall have sufficient spatial resolution and satisfy applicable data quality and modelling conditions.

**Acceptance criterion**

```text
if model_role = network_reduction_modelling:
    MODEL_USABLE(model, network_reduction_modelling) = true
    AND assessment_information_sufficient_for_regulatory_values = true
    AND assessment_information_sufficient_for_alert_information_thresholds = true
    AND spatial_resolution_sufficient = true
    AND public_information_sufficient = true
    AND M_REPR.network_reduction_spatial_coverage_ok = true
```

---

### REQ-MOD-RELOCATION_SUPPORT

| Field | Value |
|------|-------|
| Source | Art. 9(7); Annex IV; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where modelling supports relocation |
| Dependencies | M_NETWORK; M_REPR; M_LIMITS; M_MODEL_QA |

**Rule**  
Where a sampling point with recorded exceedances is relocated, modelling applications may support the relocation only if they demonstrate the spatial effect of relocation and support a documented justification.

**Acceptance criterion**

```text
if relocation_support_modelling = true:
    provide old_area_coverage_evidence
    provide new_area_coverage_evidence
    provide coverage_loss_or_gain_estimate
    provide exceedance_area_coverage_effect
    MODEL_VALID(model, relocation_support_modelling) = true
```

---

### REQ-MOD-MEASUREMENT_MODEL_CONFLICT

| Field | Value |
|------|-------|
| Source | Art. 8(5); Annex IV; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_REPR; M_LIMITS |

**Rule**  
Where modelled concentrations and measured concentrations lead to different exceedance conclusions, the conflict shall be handled through representativeness and compliance rules.

A fixed measurement does not automatically override a modelled exceedance for the whole zone. It may support non-exceedance evaluation only for the modelled exceedance area covered by its valid representativeness area.

**Acceptance criterion**

```text
if modelled_exceedance = true
AND fixed_measurement_non_exceedance = true:

    if M_REPR.fixed_measurements_cover_modelled_exceedance_area = true:
        conflict_status = fixed_measurement_may_support_non_exceedance_evaluation
    else:
        conflict_status = modelled_exceedance_remains_relevant_for_uncovered_area
```

---

### REQ-MOD-MODEL_ONLY_USE

| Field | Value |
|------|-------|
| Source | Art. 8(4); Art. 11; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | permission |
| Dependencies | M_ASSESS; M_MODEL_QA; M_DATA_QUALITY |

**Rule**  
Modelling may be used as the sole or primary assessment method where the applicable assessment regime permits it and the modelling application is valid for that use.

**Acceptance criterion**

```text
if no_valid_fixed_measurement_coverage = true
AND M_ASSESS.regime_allows_model_only_assessment = true:
    model_only_assessment_allowed = MODEL_USABLE(model, assessment_modelling)
```

---

### REQ-MOD-FORECAST_ALERT_INFORMATION_THRESHOLDS

| Field | Value |
|------|-------|
| Source | Art. 15; Art. 20 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where forecasting is used |
| Dependencies | M_LIMITS; M_SHORT_TERM_ACTION; M_PUBLIC_INFORMATION |

**Rule**  
Where modelling or forecasting tools predict exceedance of alert thresholds or information thresholds, the prediction shall be exposed to modules responsible for public information and short-term action plans.

**Acceptance criterion**

```text
if FORECAST_VALUE(p,x,t_future) > ALERT_THRESHOLD(p,m):
    alert_threshold_predicted = true
    public_information_trigger_input = true
    short_term_action_trigger_input = true

if FORECAST_VALUE(p,x,t_future) > INFORMATION_THRESHOLD(p,m):
    information_threshold_predicted = true
    public_information_trigger_input = true
```

---

### REQ-MOD-PROJECTIONS_FOR_ROADMAPS_AND_PLANS

| Field | Value |
|------|-------|
| Source | Art. 18; Art. 19; Annex VIII Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where projections are used |
| Dependencies | M_PLANS; M_ATTAINMENT_EXTENSION; M_LIMITS |

**Rule**  
Where modelling projections are used to support air quality roadmaps, air quality plans or postponement requests, the projections shall document scenarios, assumptions, measures, expected attainment dates and uncertainty or confidence metadata where available.

**Acceptance criterion**

```text
if model_role = projection_modelling:
    projection_scenarios_defined = true
    assumptions_documented = true
    measures_considered_documented = true
    projected_attainment_date_available = true
    methods_and_data_used_for_projections_justified = true
```

---

### REQ-MOD-POSTPONEMENT_SUPPORT

| Field | Value |
|------|-------|
| Source | Art. 18(1), Art. 18(2), Art. 18(4) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_ATTAINMENT_EXTENSION; M_PLANS |

**Rule**  
Where projections are provided as a reason for postponement of attainment deadlines, this module shall provide the projection package and documentation needed by `M_ATTAINMENT_EXTENSION`.

**Acceptance criterion**

```text
if postponement_request_uses_projections = true:
    provide projection_package:
        baseline_scenario
        measures_scenario
        expected_impact_of_measures
        projected_attainment_date
        methods_used
        data_used
        assumptions
        uncertainty_or_confidence_metadata_where_available
```

---

### REQ-MOD-TRANSBOUNDARY_CONTRIBUTION_SUPPORT

| Field | Value |
|------|-------|
| Source | Art. 21 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_TRANSBOUNDARY; M_SOURCE_ATTRIBUTION; M_REPR |

**Rule**  
Where transboundary transport of air pollution contributes or may contribute significantly to exceedances, modelling may be used to estimate source contributions, affected areas and cross-border concentration impacts.

**Acceptance criterion**

```text
if model_role = transboundary_contribution_modelling:
    source_contribution_estimates_available = true
    affected_zones_or_territorial_units_identified = true
    cross_boundary_geometry_available = true
    contribution_uncertainty_metadata_recorded_where_available = true
```

---

### REQ-MOD-SOURCE_ATTRIBUTION_SUPPORT

| Field | Value |
|------|-------|
| Source | Art. 16; Art. 17; Art. 21 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_SOURCE_ATTRIBUTION; M_LIMITS |

**Rule**  
Modelling may support source attribution for natural sources, winter sanding or salting, domestic heating, transboundary contributions or other source categories, but this module does not itself decide legal attribution status.

**Acceptance criterion**

```text
if source_attribution_modelling_used = true:
    provide source_contribution_evidence_package
    provide concentration_contribution_estimates
    provide spatial_and_temporal_scope
    provide model_validity_status
```

---

### REQ-MOD-PUBLIC_INFORMATION_SUPPORT

| Field | Value |
|------|-------|
| Source | Art. 22 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_PUBLIC_INFORMATION; M_LIMITS |

**Rule**  
Model outputs used for public information shall preserve sufficient metadata to communicate the spatial and temporal basis of assessment, forecasts or air quality index inputs.

**Acceptance criterion**

```text
if model_output_used_for_public_information = true:
    public_information_metadata_ready = true
    include pollutant
    include metric
    include spatial_domain
    include time_period
    include model_purpose
    include confidence_or_uncertainty_metadata_where_available
```

---

### REQ-MOD-REPORTING_SUPPORT

| Field | Value |
|------|-------|
| Source | Art. 23 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_REPORTING; M_ASSESS; M_LIMITS |

**Rule**  
Model outputs used in reported assessments shall retain metadata sufficient for traceability, including model purpose, domain, resolution, period, input data, validation status and relevant exceedance areas.

**Acceptance criterion**

```text
if model_output_used_for_reporting = true:
    reporting_metadata_ready = true
    model_traceability_package_complete = true
```

---

### REQ-MOD-VERSIONING_AND_REPRODUCIBILITY

| Field | Value |
|------|-------|
| Source | Art. 11; Annex VI; framework traceability rule |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_MODEL_QA; M_REPORTING |

**Rule**  
Regulatory modelling applications shall be versioned and reproducible for the assessment or projection period to which they apply.

**Acceptance criterion**

```text
model_run_record includes:
    model_identifier
    model_version
    run_date
    assessment_period
    spatial_domain
    spatial_resolution
    input_data_versions
    scenario_identifier_if_applicable
    validation_status
    purpose
```

---

### REQ-MOD-UNCERTAINTY_AND_CONFIDENCE_METADATA

| Field | Value |
|------|-------|
| Source | Annex V; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where available / required |
| Dependencies | M_MODEL_QA; M_DATA_QUALITY |

**Rule**  
Model outputs shall include uncertainty, confidence or quality metadata where required by Annex V, Annex VI or the applicable modelling methodology.

**Acceptance criterion**

```text
if model_uncertainty_required_for_purpose = true:
    model_uncertainty_metadata_available = true
```

---

## Modelling-level decision logic

```text
for each proposed model use:

    classify modelling_purpose

    verify MODEL_VALID(model, modelling_purpose)
    verify input_data_quality_valid
    verify purpose_allowed_by_assessment_regime

    if purpose = assessment_modelling:
        provide concentration_field
        provide exceedance areas where applicable

    if purpose = representativeness_modelling:
        provide C_field and metadata to M_REPR

    if purpose = hotspot_modelling:
        identify HOTSPOT_AREA and coverage status

    if purpose = network_reduction_modelling:
        verify Art9_3 information sufficiency and spatial resolution

    if purpose = relocation_support_modelling:
        provide old/new coverage evidence and relocation support package

    if purpose = forecast_modelling:
        provide predicted alert/information threshold exceedance triggers

    if purpose = projection_modelling:
        provide scenario and projected attainment package

    if purpose = transboundary_contribution_modelling:
        provide source contribution and cross-boundary impact package
```

---

## Interactions with other modules

- `M_ASSESS` — determines when modelling is required, permitted or sufficient
- `M_REPR` — consumes model concentration fields and exceedance areas for representativeness and coverage decisions
- `M_NETWORK` — consumes hotspot, coverage-gap, reduction and relocation support outputs
- `M_LIMITS` — consumes modelled exceedance areas and model outputs for compliance qualification
- `M_MODEL_QA` — validates model suitability and performance for the intended purpose
- `M_DATA_QUALITY` — validates input data and model-validation data
- `M_SOURCE_ATTRIBUTION` — consumes model source-contribution evidence
- `M_ATTAINMENT_EXTENSION` — consumes projection packages for Art. 18 postponement assessment
- `M_PLANS` — consumes projections, spatial distributions and measure scenarios for air quality plans and roadmaps
- `M_SHORT_TERM_ACTION` — consumes forecast exceedance triggers
- `M_TRANSBOUNDARY` — consumes transboundary contribution and cross-boundary impact outputs
- `M_PUBLIC_INFORMATION` — consumes forecasts, model outputs and metadata for public communication
- `M_REPORTING` — consumes traceable modelling packages for Commission reporting

---

## Output

```text
model_output:
  model_id
  model_version
  model_purpose
  pollutant
  metric
  assessment_period
  spatial_domain
  spatial_resolution

  validity:
    model_valid
    validation_reference
    input_data_quality_valid
    AnnexVI_conditions_satisfied
    uncertainty_or_confidence_metadata

  concentration:
    concentration_field
    time_resolution
    averaging_period

  exceedance:
    modelled_exceedance_detected
    modelled_exceedance_area_geometry
    hotspot_area_geometry
    exceedance_standard_type
    exceedance_value

  representativeness_support:
    C_field_for_M_REPR
    coverage_gap_candidate_areas
    spatial_representativeness_metadata

  network_support:
    network_reduction_support
    relocation_support
    additional_measurement_target_area

  forecast:
    alert_threshold_predicted
    information_threshold_predicted
    forecast_period
    public_information_trigger_input
    short_term_action_trigger_input

  projection:
    scenario_id
    baseline_scenario
    measures_scenario
    projected_attainment_date
    methods_and_data_justification
    assumptions

  transboundary:
    source_contribution_estimates
    affected_member_states
    cross_boundary_geometry

  traceability:
    run_date
    input_data_versions
    scenario_identifier
    reproducibility_package_available
    reporting_metadata_ready
```

---

## Notes

- Modelling does not automatically replace fixed measurements in zones where fixed measurements are required.
- A modelled exceedance is not automatically disregarded when a fixed measurement exists in the zone. Coverage by the fixed measurement's valid representativeness area is required.
- Modelling may be primary, supplementary, predictive or projective depending on the legal purpose.
- Network reduction under Art. 9(3) requires more than a valid model: it requires sufficient regulatory information, spatial resolution, data-quality compliance and public-information adequacy.
- Forecasts and projections are distinct: forecasts support alert/information threshold and short-term action triggers; projections support roadmaps, plans and postponement requests.
- This module provides evidence packages and model outputs; final legal status is determined by downstream modules.
