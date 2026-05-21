# M_MODEL_QA — Validation and quality assurance of modelling applications

## Normative references

- Art. 4(24) Directive (EU) 2024/2881 — definition of modelling application
- Art. 8 Directive (EU) 2024/2881 — use of modelling applications for assessment, spatial distribution, representativeness and modelled exceedances
- Art. 9(3), Art. 9(7) Directive (EU) 2024/2881 — modelling support for reduction of fixed measurements and relocation of sampling points
- Art. 11(2), Art. 11(3) Directive (EU) 2024/2881 — modelling applications and data quality objectives
- Art. 15 Directive (EU) 2024/2881 — prediction of alert and information threshold exceedances
- Art. 18 Directive (EU) 2024/2881 — projections used for postponement requests
- Art. 19 Directive (EU) 2024/2881 — projections and scenarios used for air quality plans and roadmaps
- Art. 21 Directive (EU) 2024/2881 — transboundary contribution analysis
- Art. 22–23 Directive (EU) 2024/2881 — public information and reporting interfaces
- Annex V — data quality objectives
- Annex VI — reference methods and modelling conditions
- Annex VIII — air quality plans and roadmaps, where projections are used
- Implementing acts under Art. 8(7), where applicable

---

## Description

This module defines the **regulatory validation and quality assurance requirements** for modelling applications used in the air quality framework.

A modelling application is treated as a chain of models and sub-models, including all necessary input data and post-processing. Validation is therefore purpose-specific and must cover the modelling system, the input data, the evaluation data, the assessment period and the intended regulatory use.

This module determines whether model outputs may be used for:

- ambient air quality assessment;
- spatial distribution of concentrations;
- spatial representativeness;
- modelled exceedance area delineation;
- hotspot identification;
- reduction of fixed measurement points;
- relocation support;
- compliance verification;
- forecasts of alert or information threshold exceedances;
- projections for air quality plans, roadmaps and postponement requests;
- source attribution and transboundary contribution analysis;
- public information and reporting.

This module does **not** decide the legal consequence of model outputs. It only determines whether the modelling application is valid and fit for the intended regulatory purpose. The final legal use is handled by `M_MOD`, `M_REPR`, `M_ASSESS`, `M_NETWORK`, `M_LIMITS`, `M_PLANS`, `M_ATTAINMENT_EXTENSION`, `M_PUBLIC_INFORMATION` and `M_REPORTING`.

---

## Scope

The module applies to modelling applications used for the following purposes:

```text
assessment_modelling
spatial_distribution_modelling
representativeness_modelling
modelled_exceedance_delineation
hotspot_modelling
network_reduction_modelling
relocation_support_modelling
compliance_support_modelling
forecast_modelling
projection_modelling
source_attribution_modelling
transboundary_contribution_modelling
public_information_modelling
reporting_modelling
```

Validation shall be performed separately for the intended purpose, pollutant, regulatory metric, assessment period and spatial domain.

---

## Definitions

```text
model_application =
modelling system chain, including models, sub-models, input data and post-processing
```

```text
MODEL_PURPOSE =
regulatory purpose for which model output is used
```

```text
OBS_VALID(p,m,period) =
observations that satisfy data quality requirements in M_DATA_QUALITY
for pollutant p, metric m and assessment period
```

```text
validation_data =
set of independent observations or datasets used to evaluate model performance
```

```text
model_input_data =
data used to initialise, constrain, train, calibrate, assimilate or otherwise drive the modelling application
```

```text
U_meas(sp,p,m) =
measurement uncertainty at sampling point sp for pollutant p and metric m
```

```text
U_model(sp,p,m) =
modelling uncertainty at sampling point sp for pollutant p and metric m
```

```text
RMSE(sp,p,m) =
root mean square error between modelled and observed values at sampling point sp
for pollutant p and metric m over the evaluation period
```

```text
MQI(sp,p,m) =
RMSE(sp,p,m) / sqrt(U_model(sp,p,m)^2 + U_meas(sp,p,m)^2)
```

```text
N_val =
number of independent validation sampling points or validation locations
available for the model-purpose, pollutant, metric, period and domain
```

```text
MODEL_VALID(model_application, MODEL_PURPOSE, p, m, domain, period) =
model satisfies the applicable validation, data-quality, traceability and purpose-specific criteria
```

---

## Normative requirements

---

### REQ-MODELQA-PURPOSE_SPECIFIC_VALIDATION

| Field | Value |
|------|-------|
| Source | Art. 8; Art. 11; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_MOD |

**Rule**  
Validation shall be performed for the specific regulatory purpose for which the modelling application is used.

A model valid for one purpose is not automatically valid for another purpose.

**Acceptance criterion**

```text
if model_output_used_for_regulatory_purpose = true:
    MODEL_PURPOSE assigned
    MODEL_VALID(model_application, MODEL_PURPOSE, p, m, domain, period) evaluated
```

---

### REQ-MODELQA-MODEL_CHAIN_DEFINITION

| Field | Value |
|------|-------|
| Source | Art. 4(24); Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_MOD; M_DATA_QUALITY |

**Rule**  
The modelling application shall be documented as a full modelling chain, including models, sub-models, input data and post-processing.

**Acceptance criterion**

```text
model_application_record includes:
    model_id
    model_version
    sub_models
    input_data_sources
    post_processing_steps
    spatial_domain
    temporal_domain
    pollutant
    metric
    purpose
```

---

### REQ-MODELQA-DATA_VALIDITY

| Field | Value |
|------|-------|
| Source | Art. 11(3); Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_DATA_QUALITY |

**Rule**  
Only observations and datasets that satisfy applicable data quality requirements may be used for model validation.

**Acceptance criterion**

```text
for each obs in validation_data:
    OBS_VALID(obs,p,m,period) = true
```

---

### REQ-MODELQA-DATA_INDEPENDENCE

| Field | Value |
|------|-------|
| Source | Annex V; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_DATA_QUALITY; M_MOD |

**Rule**  
Validation data shall be independent from model input data, unless the applicable validation method explicitly documents and controls the dependency.

**Acceptance criterion**

```text
validation_data ∩ model_input_data = ∅
```

**Qualification**  
Where full independence is not possible, the dependency shall be documented and the validation result shall be flagged as `limited_independence`.

```text
if validation_data_not_fully_independent = true:
    independence_status = limited_independence
    limitation_documented = true
```

---

### REQ-MODELQA-MQI_DEFINITION

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where MQI applies |
| Dependencies | T_DATA_QUALITY; M_DATA_QUALITY |

**Rule**  
Where the modelling quality indicator applies, model quality shall be assessed using the ratio of model error to total uncertainty.

**Acceptance criterion**

```text
MQI(sp,p,m) =
    RMSE(sp,p,m) / sqrt(U_model(sp,p,m)^2 + U_meas(sp,p,m)^2)
```

---

### REQ-MODELQA-MQI_THRESHOLD

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where MQI applies |
| Dependencies | REQ-MODELQA-MQI_DEFINITION |

**Rule**  
A model meets the MQI-based quality objective at a validation point if the MQI does not exceed one.

**Acceptance criterion**

```text
MQI_pass(sp,p,m) = MQI(sp,p,m) <= 1
```

---

### REQ-MODELQA-MQI_COVERAGE_90_PERCENT

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where MQI applies and N_val >= 10 |
| Dependencies | REQ-MODELQA-MQI_THRESHOLD |

**Rule**  
Where ten or more independent validation points are available, the MQI criterion shall be satisfied at least at 90% of available validation points in the evaluation area and period.

**Acceptance criterion**

```text
if N_val >= 10:
    COUNT{ sp | MQI(sp,p,m) <= 1 } / N_val >= 0.9
```

---

### REQ-MODELQA-LOW_VALIDATION_POINT_COUNT

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where MQI applies and N_val < 10 |
| Dependencies | REQ-MODELQA-MQI_THRESHOLD |

**Rule**  
Where fewer than ten independent validation points are available, the model shall meet the MQI criterion at all available validation points.

**Acceptance criterion**

```text
if N_val < 10:
    for all sp in validation_points:
        MQI(sp,p,m) <= 1
```

---

### REQ-MODELQA-METRIC_SPECIFIC_VALIDATION

| Field | Value |
|------|-------|
| Source | Annex V; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_LIMIT_VALUES; M_DATA_QUALITY |

**Rule**  
Validation shall be performed for the applicable regulatory metric and averaging period.

Validation for annual means is not automatically valid for short-term metrics, alert thresholds or information thresholds.

**Acceptance criterion**

```text
if model_output_used_for_metric m:
    validation_metric = m
    validation_period matches regulatory_averaging_period(m)
```

---

### REQ-MODELQA-SPATIAL_DOMAIN_VALIDATION

| Field | Value |
|------|-------|
| Source | Art. 8; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_ZONE; M_REPR; M_MOD |

**Rule**  
Validation shall be appropriate to the spatial domain for which the model is used.

**Acceptance criterion**

```text
MODEL_VALID_FOR_DOMAIN =
    validation_domain covers intended_model_use_domain
    OR domain_transfer_justification_documented = true
```

---

### REQ-MODELQA-SPATIAL_RESOLUTION_FITNESS

| Field | Value |
|------|-------|
| Source | Art. 8; Art. 9(3); Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory for spatial, hotspot and network-reduction uses |
| Dependencies | M_MOD; M_REPR; M_NETWORK |

**Rule**  
The spatial resolution of the modelling application shall be sufficient for the intended purpose.

**Acceptance criterion**

```text
if MODEL_PURPOSE in [
    spatial_distribution_modelling,
    representativeness_modelling,
    modelled_exceedance_delineation,
    hotspot_modelling,
    network_reduction_modelling,
    relocation_support_modelling
]:
    spatial_resolution_sufficient = true
```

---

### REQ-MODELQA-HOTSPOT_FITNESS

| Field | Value |
|------|-------|
| Source | Art. 4(27); Art. 8(6); Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where model is used for hotspot analysis |
| Dependencies | M_MOD; M_REPR; M_NETWORK |

**Rule**  
A model used to identify air pollution hotspots shall be fit to represent localised high concentrations and relevant source contexts.

**Acceptance criterion**

```text
if MODEL_PURPOSE = hotspot_modelling:
    hotspot_scale_represented = true
    AND relevant_source_context_represented = true
    AND spatial_resolution_sufficient = true
```

---

### REQ-MODELQA-NETWORK_REDUCTION_FITNESS

| Field | Value |
|------|-------|
| Source | Art. 9(3); Annex V; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where model supports fixed-measurement reduction |
| Dependencies | M_NETWORK; M_REPR; M_MOD; M_DATA_QUALITY |

**Rule**  
A model used to support reduction of fixed measurement sampling points shall be valid for the regulatory values and thresholds relevant to Art. 9(3), and shall provide adequate spatial information and public-information support.

**Acceptance criterion**

```text
if MODEL_PURPOSE = network_reduction_modelling:
    MODEL_VALID(model, network_reduction_modelling, p, m, domain, period) = true
    AND spatial_resolution_sufficient = true
    AND assessment_information_sufficient_for_regulatory_values = true
    AND assessment_information_sufficient_for_alert_information_thresholds = true
    AND public_information_sufficient = true
```

---

### REQ-MODELQA-REPRESENTATIVENESS_FITNESS

| Field | Value |
|------|-------|
| Source | Art. 8(3); Art. 8(5); Annex IV; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where model supports representativeness |
| Dependencies | M_REPR; M_MOD |

**Rule**  
A model used for spatial representativeness shall be valid for determining concentration similarity, modelled exceedance areas and coverage by fixed measurements.

**Acceptance criterion**

```text
if MODEL_PURPOSE = representativeness_modelling:
    concentration_field_valid_for_repr = true
    AND spatial_resolution_sufficient = true
    AND validation_metric_matches_repr_metric = true
```

---

### REQ-MODELQA-MODELLED_EXCEEDANCE_DELINEATION_FITNESS

| Field | Value |
|------|-------|
| Source | Art. 8(5), Art. 8(6); Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where modelled exceedance areas are used |
| Dependencies | M_MOD; M_REPR; M_LIMITS |

**Rule**  
A model used to delineate modelled exceedance areas shall be valid for the relevant pollutant, metric, standard and spatial domain.

**Acceptance criterion**

```text
if MODEL_PURPOSE = modelled_exceedance_delineation:
    validation_metric matches exceedance_standard_metric
    AND spatial_domain_valid = true
    AND spatial_resolution_sufficient = true
```

---

### REQ-MODELQA-RELOCATION_SUPPORT_FITNESS

| Field | Value |
|------|-------|
| Source | Art. 9(7); Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where model supports relocation |
| Dependencies | M_NETWORK; M_REPR |

**Rule**  
A model used to support relocation of sampling points shall be valid for comparing coverage before and after relocation and for assessing exceedance-area coverage loss.

**Acceptance criterion**

```text
if MODEL_PURPOSE = relocation_support_modelling:
    old_location_context_represented = true
    new_location_context_represented = true
    coverage_change_assessment_supported = true
```

---

### REQ-MODELQA-FORECAST_FITNESS

| Field | Value |
|------|-------|
| Source | Art. 15; Art. 20; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where model is used for forecast triggers |
| Dependencies | M_MOD; M_PUBLIC_INFORMATION; M_SHORT_TERM_ACTION |

**Rule**  
Forecast models used to predict exceedance of alert or information thresholds shall be validated or quality-controlled for the relevant forecast horizon, pollutant, threshold and area.

**Acceptance criterion**

```text
if MODEL_PURPOSE = forecast_modelling:
    forecast_horizon_documented = true
    threshold_metric_supported = true
    forecast_quality_metadata_available = true
```

---

### REQ-MODELQA-PROJECTION_FITNESS

| Field | Value |
|------|-------|
| Source | Art. 18; Art. 19; Annex VIII Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where model is used for projections |
| Dependencies | M_MOD; M_PLANS; M_ATTAINMENT_EXTENSION |

**Rule**  
Projection models used for plans, roadmaps or postponement requests shall document scenarios, assumptions, input data, measures, projected attainment dates and uncertainty or confidence metadata where available.

**Acceptance criterion**

```text
if MODEL_PURPOSE = projection_modelling:
    scenarios_documented = true
    assumptions_documented = true
    measures_documented = true
    input_data_versions_recorded = true
    projected_attainment_date_available = true
    methods_and_data_used_justified = true
```

---

### REQ-MODELQA-SOURCE_ATTRIBUTION_AND_TRANSBOUNDARY_FITNESS

| Field | Value |
|------|-------|
| Source | Art. 16; Art. 17; Art. 21; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where model supports attribution or transboundary analysis |
| Dependencies | M_SOURCE_ATTRIBUTION; M_TRANSBOUNDARY; M_MOD |

**Rule**  
Models used for source attribution or transboundary contribution analysis shall be fit for estimating source contributions, affected areas and cross-boundary impacts.

**Acceptance criterion**

```text
if MODEL_PURPOSE in [source_attribution_modelling, transboundary_contribution_modelling]:
    source_categories_documented = true
    contribution_method_documented = true
    affected_area_geometry_available = true
    contribution_uncertainty_metadata_recorded_where_available = true
```

---

### REQ-MODELQA-VALIDATION_METHOD_DOCUMENTATION

| Field | Value |
|------|-------|
| Source | Annex V; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_DATA_QUALITY |

**Rule**  
The validation method shall be documented. Leave-One-Out Cross-Validation or another appropriate method may be used where suitable for the model purpose and data context.

**Acceptance criterion**

```text
validation_method documented
validation_period documented
validation_locations documented
validation_metric documented
```

---

### REQ-MODELQA-USAGE_CONSTRAINT

| Field | Value |
|------|-------|
| Source | Art. 8; Art. 11; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | REQ-MODELQA-PURPOSE_SPECIFIC_VALIDATION |

**Rule**  
Results from a model that is not valid for the intended regulatory purpose shall not be used for that purpose.

**Acceptance criterion**

```text
if MODEL_VALID(model_application, MODEL_PURPOSE, p, m, domain, period) = false:
    model_output not usable for MODEL_PURPOSE
```

---

### REQ-MODELQA-VERSIONING_AND_REPRODUCIBILITY

| Field | Value |
|------|-------|
| Source | Art. 11; Art. 23; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_MOD; M_REPORTING |

**Rule**  
Model runs used for regulatory purposes shall be versioned, traceable and reproducible.

**Acceptance criterion**

```text
model_run_record includes:
    model_id
    model_version
    model_chain_version
    input_data_versions
    post_processing_version
    run_date
    assessment_period
    spatial_domain
    spatial_resolution
    MODEL_PURPOSE
    validation_status
```

---

### REQ-MODELQA-QUALITY_METADATA_FOR_PUBLIC_INFORMATION_AND_REPORTING

| Field | Value |
|------|-------|
| Source | Art. 22; Art. 23 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_PUBLIC_INFORMATION; M_REPORTING |

**Rule**  
Where model outputs are used for public information or reporting, the validation status and quality metadata shall be available for traceability and explanation.

**Acceptance criterion**

```text
if model_output_used_for_public_information_or_reporting = true:
    validation_status_available = true
    model_quality_metadata_available = true
    model_traceability_package_complete = true
```

---

## Model-validation decision logic

```text
for each model_application and intended MODEL_PURPOSE:

    document full model chain

    identify pollutant, metric, period and spatial domain

    verify data validity of validation data

    verify independence of validation data

    select validation method

    if MQI applies:
        calculate MQI at validation points

        if N_val >= 10:
            require MQI <= 1 at at least 90% of validation points

        if N_val < 10:
            require MQI <= 1 at all validation points

    verify purpose-specific fitness:
        spatial resolution
        metric match
        domain match
        hotspot, reduction, relocation, forecast, projection or attribution requirements where relevant

    verify versioning and traceability

    if all applicable checks pass:
        MODEL_VALID = true
    else:
        MODEL_VALID = false

    expose validation status and blocking reasons to downstream modules
```

---

## Interactions with other modules

- `M_DATA_QUALITY` — validates observations, validation datasets and model input data
- `M_MOD` — consumes `MODEL_VALID` and purpose-specific validation status
- `M_REPR` — uses only models valid for representativeness, concentration-field and exceedance-area purposes
- `M_LIMITS` — accepts modelled exceedance evidence only where the model is valid for the relevant compliance-support purpose
- `M_NETWORK` — uses models valid for reduction, relocation and additional measurement support
- `M_ASSESS` — consumes model validity for assessment-method decisions
- `M_PLANS` — consumes models valid for projections and scenario analysis
- `M_ATTAINMENT_EXTENSION` — consumes projection-validation status for postponement requests
- `M_SHORT_TERM_ACTION` — consumes forecast-validation status
- `M_TRANSBOUNDARY` — consumes transboundary contribution validation status
- `M_SOURCE_ATTRIBUTION` — consumes source-attribution validation status
- `M_PUBLIC_INFORMATION` — consumes forecast/model quality metadata
- `M_REPORTING` — consumes validation and traceability metadata
- `T_DATA_QUALITY` — stores measurement and modelling uncertainty objectives
- `T_MODEL_QA` — stores purpose-specific validation thresholds, if separated from `T_DATA_QUALITY`

---

## Output

```text
model_validation:
  model_id
  model_version
  model_chain_version
  model_purpose
  pollutant
  metric
  assessment_period
  spatial_domain

  validation_data:
    N_val
    validation_points
    validation_period
    validation_data_quality_valid
    independence_status = independent | limited_independence | not_independent

  method:
    validation_method
    MQI_applies
    RMSE_summary
    U_model_summary
    U_meas_summary
    MQI_summary
    MQI_pass_rate

  purpose_fitness:
    metric_specific_validation_ok
    spatial_domain_validation_ok
    spatial_resolution_sufficient
    hotspot_fitness
    network_reduction_fitness
    representativeness_fitness
    exceedance_delineation_fitness
    relocation_support_fitness
    forecast_fitness
    projection_fitness
    source_attribution_fitness
    transboundary_fitness

  validity:
    valid
    valid_for_purposes
    invalid_for_purposes
    blocking_reasons

  traceability:
    input_data_versions
    post_processing_version
    run_date
    reproducibility_package_available
    public_information_quality_metadata_ready
    reporting_quality_metadata_ready
```

---

## Notes

- Model validation is purpose-specific. A model valid for annual spatial assessment is not automatically valid for short-term forecasts, hotspot detection or projections.
- The MQI rule is retained as a core regulatory quality test where applicable, but additional purpose-specific fitness checks are required for the uses introduced by Art. 8, Art. 9, Art. 15, Art. 18, Art. 19 and Art. 21.
- Validation requires valid and independent observations unless a documented limitation is explicitly accepted for the relevant purpose.
- This module provides `MODEL_VALID`; it does not decide whether a modelled exceedance is a compliance breach or whether a plan, roadmap or postponement is valid.
