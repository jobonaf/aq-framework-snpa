# M_DATA_QUALITY — Data quality and assessment-data validity

## Normative references

- Art. 4 Directive (EU) 2024/2881 — definitions of fixed measurements, indicative measurements, modelling applications and assessment
- Art. 8(6) Directive (EU) 2024/2881 — additional measurements after modelled exceedance and minimum coverage link
- Art. 9(3) Directive (EU) 2024/2881 — reduction of fixed measurement points using modelling or indicative measurements
- Art. 11 Directive (EU) 2024/2881 — reference methods, modelling applications and data quality objectives
- Art. 15 Directive (EU) 2024/2881 — alert and information thresholds, including predicted exceedances
- Art. 18 Directive (EU) 2024/2881 — projections used for postponement requests
- Art. 19 Directive (EU) 2024/2881 — air quality plans and roadmaps
- Art. 20 Directive (EU) 2024/2881 — short-term action plans
- Art. 22–23 Directive (EU) 2024/2881 — public information and reporting interfaces
- Annex V — data quality objectives
- Annex VI — reference methods and modelling conditions
- Annex VIII — air quality plans and roadmaps, where projections are used

---

## Description

This module defines the **regulatory validity conditions for data and assessment evidence** used in the air quality framework.

It determines whether measurement, indicative-measurement, modelling-input, model-validation, forecast and projection data may be used for:

- assessment classification;
- compliance and exceedance verification;
- spatial representativeness;
- modelling applications;
- reduction of fixed measurement points;
- additional measurements after modelled exceedances;
- public information;
- reporting;
- air quality plans, roadmaps and postponement requests.

The module does **not**:

- determine the assessment regime;
- design the monitoring network;
- validate models as models;
- determine spatial representativeness geometries;
- decide legal compliance status;
- establish plans, roadmaps or short-term action plans.

Those functions are handled by dedicated modules.

---

## Scope

This module applies to:

- fixed measurements;
- indicative measurements;
- additional fixed measurements;
- additional indicative measurements;
- measurements used for average exposure indicators;
- measurements used at supersites;
- data used for model validation;
- input data used in modelling applications;
- forecast data used for alert or information thresholds;
- projection data used for air quality roadmaps, plans or postponement requests;
- data packages used for reporting or public information.

---

## Definitions

```text
p = pollutant
m = regulatory metric
z = zone
u = average exposure territorial unit
sp = sampling point
period = assessment period
```

```text
DATASET =
a coherent set of observations, measurements, model inputs,
model-validation data, forecasts or projections used for a regulatory purpose
```

```text
DATA_PURPOSE =
    assessment_classification
    compliance_verification
    spatial_representativeness
    model_validation
    modelling_input
    network_reduction
    additional_measurement_after_modelled_exceedance
    average_exposure_indicator
    alert_or_information_threshold
    public_information
    reporting
    projection_for_plan_or_roadmap
    projection_for_postponement
```

```text
coverage(dataset,p,m,period) =
percentage of valid data available for pollutant p,
metric m and assessment period
```

```text
uncertainty(dataset,p,m) =
uncertainty of the dataset for pollutant p and metric m,
assessed according to the applicable data quality objective
```

```text
MIN_COVERAGE(p,m,DATA_PURPOSE) =
minimum data coverage required by Annex V or applicable rule
```

```text
MAX_UNCERTAINTY(p,m,DATA_PURPOSE) =
maximum uncertainty allowed by Annex V or applicable rule
```

```text
DATA_VALID(dataset,p,m,DATA_PURPOSE) =
coverage >= MIN_COVERAGE
AND uncertainty <= MAX_UNCERTAINTY
AND method_valid = true
AND traceability_complete = true
```

---

## Normative requirements

---

### REQ-DATA-PURPOSE_CLASSIFICATION

| Field | Value |
|------|-------|
| Source | Art. 11; Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_ASSESS; M_LIMITS; M_MOD; M_NETWORK |

**Rule**  
Every dataset shall be classified by regulatory purpose before data quality requirements are applied.

**Acceptance criterion**

```text
if dataset_used = true:
    DATA_PURPOSE is assigned
    applicable_quality_objectives are selected from DATA_PURPOSE, pollutant and metric
```

---

### REQ-DATA-METHOD_VALIDITY

| Field | Value |
|------|-------|
| Source | Art. 11(1); Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory / permission |
| Dependencies | M_NETWORK; M_MODEL_QA; T_METHODS |

**Rule**  
Reference measurement methods shall be valid under Annex VI. Other measurement methods may be used only where the applicable Annex VI conditions are satisfied.

**Acceptance criterion**

```text
if measurement_method = reference_method:
    method_valid = true
else:
    method_valid = AnnexVI_conditions_or_equivalence_satisfied
```

---

### REQ-DATA-VALIDITY_CRITERIA

| Field | Value |
|------|-------|
| Source | Art. 11(3); Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_DATA_QUALITY |

**Rule**  
Air quality assessment data shall comply with the applicable data quality objectives. Data may be used only where minimum coverage, uncertainty, method validity and traceability conditions are satisfied for the intended purpose.

**Acceptance criterion**

```text
DATA_VALID(dataset,p,m,DATA_PURPOSE) =
    coverage(dataset,p,m,period) >= MIN_COVERAGE(p,m,DATA_PURPOSE)
    AND uncertainty(dataset,p,m) <= MAX_UNCERTAINTY(p,m,DATA_PURPOSE)
    AND method_valid = true
    AND traceability_complete = true
```

---

### REQ-DATA-EXCLUSION_IF_INVALID

| Field | Value |
|------|-------|
| Source | Annex V; Art. 11 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | REQ-DATA-VALIDITY_CRITERIA |

**Rule**  
Invalid data shall not be used as valid assessment data for the regulatory purpose for which they fail the applicable requirements.

**Acceptance criterion**

```text
if DATA_VALID(dataset,p,m,DATA_PURPOSE) = false:
    dataset excluded for DATA_PURPOSE
```

**Qualification**  
Exclusion of invalid measurement data does not by itself invalidate a modelled exceedance. Where Art. 8(6) applies and no additional valid fixed or indicative measurement is available, the modelled exceedance status is determined by `M_ASSESS` and `M_LIMITS`.

---

### REQ-DATA-INCOMPLETE_BUT_CONCLUSIVE_DATA

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | conditional |
| Dependencies | M_LIMITS; M_ASSESS |

**Rule**  
Where the minimum data coverage is not met, assessment may still be flagged as conclusive where the available valid data are sufficient to support the regulatory conclusion under the applicable rule.

**Acceptance criterion**

```text
if coverage < MIN_COVERAGE
AND conclusive_evaluation_possible = true:
    data_quality_status = conclusive_with_incomplete_coverage
else if coverage < MIN_COVERAGE:
    data_quality_status = insufficient_coverage
```

---

### REQ-DATA-FIXED_MEASUREMENTS

| Field | Value |
|------|-------|
| Source | Art. 4(22); Art. 11; Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_NETWORK; T_DATA_QUALITY |

**Rule**  
Fixed measurements shall be taken at sampling points, continuously or by random sampling, at constant locations for at least one calendar year and in accordance with the relevant data quality objectives.

**Acceptance criterion**

```text
if data_type = fixed_measurement:
    location_constant = true
    measurement_duration >= 1 calendar_year
    DATA_VALID(dataset,p,m,fixed_measurement_purpose) = true
```

---

### REQ-DATA-INDICATIVE_MEASUREMENTS

| Field | Value |
|------|-------|
| Source | Art. 4(23); Art. 9(3); Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory when used |
| Dependencies | M_NETWORK; M_ASSESS |

**Rule**  
Indicative measurements shall satisfy the data quality objectives applicable to indicative measurements and their intended regulatory use.

Where indicative measurements are used to replace fixed measurements under Art. 9(3), their number shall be at least equal to the number of fixed measurements replaced and they shall be evenly distributed over the calendar year.

**Acceptance criterion**

```text
if data_type = indicative_measurement:
    DATA_VALID(dataset,p,m,indicative_measurement_purpose) = true
```

```text
if indicative_measurements_used_for_Art9_3_reduction = true:
    N_indicative >= N_fixed_replaced
    AND indicative_measurements_evenly_distributed_over_year = true
```

---

### REQ-DATA-ADDITIONAL_MEASUREMENTS_ART8_6

| Field | Value |
|------|-------|
| Source | Art. 8(6); Annex V, Point B Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory when additional measurements are used |
| Dependencies | M_ASSESS; M_NETWORK; M_REPR |

**Rule**  
Additional fixed or indicative measurements following a modelled exceedance shall cover at least one calendar year and comply with minimum data coverage requirements.

**Acceptance criterion**

```text
if DATA_PURPOSE = additional_measurement_after_modelled_exceedance:
    measurement_duration >= 1_calendar_year
    AND coverage >= MIN_COVERAGE_AnnexV_B
    AND method_valid = true
```

---

### REQ-DATA-MODEL_INPUT_DATA

| Field | Value |
|------|-------|
| Source | Art. 11(2); Annex V; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory when modelling is used |
| Dependencies | M_MOD; M_MODEL_QA |

**Rule**  
Input data used in modelling applications for regulatory purposes shall be traceable and suitable for the modelling purpose.

**Acceptance criterion**

```text
if DATA_PURPOSE = modelling_input:
    input_data_source_documented = true
    input_data_version_recorded = true
    input_data_quality_valid = true
```

---

### REQ-DATA-MODEL_VALIDATION_DATA

| Field | Value |
|------|-------|
| Source | Art. 11(2); Annex V; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory when used for model QA |
| Dependencies | M_MODEL_QA; M_MOD |

**Rule**  
Data used to validate modelling applications shall satisfy the applicable quality and traceability requirements for model validation.

**Acceptance criterion**

```text
if DATA_PURPOSE = model_validation:
    validation_dataset_traceable = true
    validation_dataset_quality_valid = true
    validation_period_documented = true
```

---

### REQ-DATA-NETWORK_REDUCTION_ART9_3

| Field | Value |
|------|-------|
| Source | Art. 9(3); Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory when reduction is requested |
| Dependencies | M_NETWORK; M_MOD; M_REPR; M_ASSESS |

**Rule**  
Where fixed measurement points are reduced under Art. 9(3), the modelling applications or indicative measurements supporting reduction shall satisfy the applicable Annex V data quality objectives and allow assessment results to meet the required quality and reporting conditions.

**Acceptance criterion**

```text
if Art9_3_reduction_requested = true:
    supporting_data_quality_valid = true
    AND spatial_resolution_sufficient = true
    AND assessment_results_quality_requirements_satisfied = true
    AND public_information_sufficient = true
```

---

### REQ-DATA-SPATIAL_REPRESENTATIVENESS_DATA

| Field | Value |
|------|-------|
| Source | Art. 8; Annex IV; Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory when used by M_REPR |
| Dependencies | M_REPR; M_MOD |

**Rule**  
Data used to determine spatial representativeness shall be valid for the relevant pollutant, metric, period, station type and method.

**Acceptance criterion**

```text
if DATA_PURPOSE = spatial_representativeness:
    pollutant_metric_period_consistent = true
    AND underlying_measurement_or_model_data_valid = true
    AND method_metadata_available = true
```

---

### REQ-DATA-AVERAGE_EXPOSURE_INDICATOR

| Field | Value |
|------|-------|
| Source | Art. 4(33); Art. 9(6); Art. 13(5); Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_ZONE; M_NETWORK; M_LIMITS |

**Rule**  
Data used to calculate average exposure indicators shall come from the relevant urban background locations throughout the average exposure territorial unit, or rural background locations where no urban area is located in that territorial unit, and shall satisfy applicable data quality requirements.

**Acceptance criterion**

```text
if DATA_PURPOSE = average_exposure_indicator:
    sampling_point_context_valid_for_AEI = true
    AND DATA_VALID(dataset,p,m,average_exposure_indicator) = true
```

---

### REQ-DATA-ALERT_INFORMATION_FORECASTS

| Field | Value |
|------|-------|
| Source | Art. 15; Art. 20; Art. 22 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where forecasts are used |
| Dependencies | M_MOD; M_LIMITS; M_PUBLIC_INFORMATION; M_SHORT_TERM_ACTION |

**Rule**  
Data and forecasts used to predict exceedance of alert or information thresholds shall be traceable and suitable for the forecast purpose. The forecast quality metadata shall be preserved for downstream modules.

**Acceptance criterion**

```text
if DATA_PURPOSE = alert_or_information_threshold:
    forecast_timestamp recorded
    forecast_domain recorded
    forecast_method recorded
    forecast_quality_metadata_available where required
```

---

### REQ-DATA-PROJECTIONS_FOR_PLANS_AND_POSTPONEMENT

| Field | Value |
|------|-------|
| Source | Art. 18; Art. 19; Annex VIII Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where projections are used |
| Dependencies | M_MOD; M_PLANS; M_ATTAINMENT_EXTENSION |

**Rule**  
Projection data used for air quality plans, air quality roadmaps or postponement requests shall be documented, traceable and linked to the relevant scenario, assumptions, measures and projected attainment date.

**Acceptance criterion**

```text
if DATA_PURPOSE in [projection_for_plan_or_roadmap, projection_for_postponement]:
    scenario_id recorded
    assumptions_documented = true
    methods_used_documented = true
    input_data_versions_recorded = true
    projected_attainment_date_recorded = true
```

---

### REQ-DATA-REPORTING_TRACEABILITY

| Field | Value |
|------|-------|
| Source | Art. 23 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory when used for reporting |
| Dependencies | M_REPORTING; M_ZONE; M_ASSESS; M_LIMITS |

**Rule**  
Data used for reporting shall retain sufficient metadata to identify the pollutant, metric, assessment period, territorial domain, method, data quality status and version.

**Acceptance criterion**

```text
if DATA_PURPOSE = reporting:
    reporting_metadata_complete = true
```

---

### REQ-DATA-PUBLIC_INFORMATION_TRACEABILITY

| Field | Value |
|------|-------|
| Source | Art. 22 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory when used for public information |
| Dependencies | M_PUBLIC_INFORMATION; M_LIMITS; M_MOD |

**Rule**  
Data used for public information shall retain sufficient metadata to support clear public communication, including pollutant, area, period, standard or threshold, data status and forecast status where relevant.

**Acceptance criterion**

```text
if DATA_PURPOSE = public_information:
    public_information_metadata_complete = true
    AND public_information_area_identified = true
    AND pollutant_metric_period_identified = true
```

---

### REQ-DATA-RANDOM_AND_NON_CONTINUOUS_SAMPLING

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881; Art. 4(22), Art. 4(23) |
| Status | STABLE |
| Type | conditional permission |
| Dependencies | T_DATA_QUALITY |

**Rule**  
Random or non-continuous sampling may be used where allowed by the relevant measurement type and provided that the overall uncertainty and coverage requirements for the intended purpose remain satisfied.

**Acceptance criterion**

```text
if sampling_mode in [random_sampling, non_continuous_sampling]:
    sampling_mode_allowed_for_data_type = true
    AND overall_uncertainty <= MAX_UNCERTAINTY
    AND coverage >= MIN_COVERAGE
```

---

### REQ-DATA-EXCLUDED_OR_SPECIAL_METRICS

| Field | Value |
|------|-------|
| Source | Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | special rule |
| Dependencies | T_DATA_QUALITY |

**Rule**  
Some metrics may have special or excluded data quality requirements under Annex V. These shall be handled through table-driven applicability rules rather than by applying a generic coverage/uncertainty test to all metrics.

**Acceptance criterion**

```text
if metric in T_DATA_QUALITY.special_or_excluded_metrics:
    apply metric_specific_data_quality_rule
else:
    apply general DATA_VALID rule
```

---

### REQ-DATA-VERSIONING_AND_AUDIT_TRAIL

| Field | Value |
|------|-------|
| Source | Art. 11; Art. 23; Annex V; framework traceability rule |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_REPORTING |

**Rule**  
Datasets used for regulatory purposes shall be versioned and auditable.

**Acceptance criterion**

```text
dataset_record includes:
    dataset_id
    dataset_version
    source
    acquisition_period
    processing_version
    validation_status
    DATA_PURPOSE
    pollutant
    metric
    territorial_domain
    quality_flags
```

---

## Data-quality decision logic

```text
for each dataset:

    classify DATA_PURPOSE

    identify pollutant, metric, period and territorial domain

    select applicable quality objectives

    verify method validity

    calculate coverage

    calculate or import uncertainty

    verify traceability and versioning

    if all applicable checks pass:
        DATA_VALID = true
    else if incomplete but conclusive assessment is allowed and justified:
        data_quality_status = conclusive_with_incomplete_coverage
    else:
        DATA_VALID = false

    expose data_quality_status to downstream modules
```

---

## Interactions with other modules

- `M_ZONE` — provides territorial domain for data validity and reporting context
- `M_ASSESS` — consumes valid data for threshold classification and assessment regime outputs
- `M_NETWORK` — consumes data validity for fixed, indicative and additional measurements
- `M_REPR` — consumes valid data for spatial representativeness
- `M_MOD` — consumes valid input and validation data for modelling applications
- `M_MODEL_QA` — consumes model-validation datasets and quality metadata
- `M_LIMITS` — consumes valid data for compliance and exceedance verification
- `M_PLANS` — consumes projection and assessment data for plans and roadmaps
- `M_ATTAINMENT_EXTENSION` — consumes projection data for postponement requests
- `M_SHORT_TERM_ACTION` — consumes forecast data for short-term triggers
- `M_PUBLIC_INFORMATION` — consumes public-information-ready data and metadata
- `M_REPORTING` — consumes reporting-ready datasets and metadata
- `T_DATA_QUALITY` — stores Annex V quality objectives and applicability rules
- `T_METHODS` — stores reference methods and equivalence/validity rules

---

## Output

```text
data_quality_status:
  dataset_id
  dataset_version
  pollutant
  metric
  assessment_period
  territorial_domain

  data_type:
    fixed_measurement
    indicative_measurement
    additional_fixed_measurement
    additional_indicative_measurement
    model_input
    model_validation
    forecast
    projection
    reporting_dataset
    public_information_dataset

  purpose:
    assessment_classification
    compliance_verification
    spatial_representativeness
    model_validation
    modelling_input
    network_reduction
    additional_measurement_after_modelled_exceedance
    average_exposure_indicator
    alert_or_information_threshold
    public_information
    reporting
    projection_for_plan_or_roadmap
    projection_for_postponement

  method:
    method_id
    method_valid
    reference_method
    AnnexVI_conditions_satisfied

  quality:
    coverage
    minimum_coverage_required
    uncertainty
    maximum_uncertainty_allowed
    traceability_complete
    valid
    data_quality_status = valid | invalid | insufficient_coverage | conclusive_with_incomplete_coverage | special_metric_rule_applied

  metadata:
    source
    processing_version
    validation_date
    quality_flags
    reporting_metadata_complete
    public_information_metadata_complete
```

---

## Notes

- Data validity is purpose-specific: a dataset may be valid for one regulatory use and invalid for another.
- Invalid measurement data do not automatically invalidate modelled exceedances; modelled exceedance status is handled through `M_ASSESS`, `M_REPR`, `M_MOD` and `M_LIMITS`.
- Art. 9(3) network reduction requires stronger data-quality evidence than ordinary supplementary modelling.
- Forecasts and projections require traceability even where they are not treated like ordinary measurement datasets.
- The module is table-driven: pollutant-specific and metric-specific Annex V values belong in `T_DATA_QUALITY`.
