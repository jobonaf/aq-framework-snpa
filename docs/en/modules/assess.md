# M_ASSESS — Assessment regime

## Normative references

- Art. 6 Directive (EU) 2024/2881
- Art. 7 Directive (EU) 2024/2881
- Art. 8 Directive (EU) 2024/2881
- Art. 9(3) Directive (EU) 2024/2881
- Art. 11 Directive (EU) 2024/2881
- Art. 12–18 Directive (EU) 2024/2881, for downstream compliance and qualification hooks
- Art. 19–23 Directive (EU) 2024/2881, for downstream planning, public-information and reporting hooks
- Annex I (limit values, target values, critical levels, alert and information thresholds, exposure obligations)
- Annex II (assessment thresholds)
- Annex IV (sampling-point location, spatial representativeness and assessment context)
- Annex V (data quality objectives)
- Annex VI (reference methods and modelling conditions)

---

## Description

This module determines the **assessment regime** for ambient air quality for each pollutant, zone and, where relevant, average exposure territorial unit.

It defines:

- classification of zones in relation to assessment thresholds;
- review of that classification;
- assessment methods required or sufficient for each pollutant and zone;
- when fixed measurements are mandatory;
- when modelling, indicative measurements or objective estimation may or shall be used;
- how modelled exceedances are treated for assessment purposes;
- when additional measurements are required or permitted after uncovered modelled exceedances;
- outputs needed by `M_NETWORK`, `M_LIMITS`, `M_DATA_QUALITY`, `M_MOD`, `M_REPR`, `M_PLANS`, `M_PUBLIC_INFORMATION` and `M_REPORTING`.

This module does **not**:

- calculate compliance with limit values or target values;
- define the monitoring network in detail;
- validate data quality;
- validate modelling applications;
- establish air quality plans, roadmaps or short-term action plans;
- perform public-information or reporting obligations.

Those functions are handled by dedicated modules.

---

## Scope

The module applies to assessment of pollutants and metrics governed by Annex I and Annex II, including:

- pollutants subject to limit values;
- pollutants subject to target values;
- pollutants subject to critical levels;
- pollutants subject to alert thresholds and information thresholds;
- PM2.5 and NO2 average exposure indicators and average exposure reduction obligations;
- ozone-specific assessment contexts;
- pollutants assessed through fixed measurements, indicative measurements, modelling applications or objective estimation.

The spatial unit is generally the **zone**. For average exposure indicators and ozone territorial assessment contexts, the module also supports **average exposure territorial units** or other territorial units supplied by `M_ZONE`.

---

## Definitions

```text
p = pollutant
z = zone
u = average exposure territorial unit or relevant territorial unit
y = calendar year
m = regulatory metric
```

```text
TH(p) = assessment threshold for pollutant p,
as defined in Annex II / T_ASSESS_THRESHOLDS
```

```text
C(p,z,y,m) = concentration value for pollutant p,
zone z, year y and metric m,
calculated using the applicable averaging period and assessment method
```

```text
PREVIOUS_5_YEARS(y0) =
the five calendar years preceding the relevant classification year y0
```

```text
ABOVE_THRESHOLD(p,z) =
COUNT{ y ∈ PREVIOUS_5_YEARS | C(p,z,y,m_threshold) > TH(p) } >= 3
```

```text
ASSESSMENT_REGIME(p,z) =
above_threshold_fixed_measurements
| below_threshold_simplified_assessment
| exceedance_requires_modelling_or_indicative
| reduced_fixed_measurement_regime
| not_classifiable_due_to_insufficient_data
```

```text
ASSESSMENT_METHOD =
fixed_measurements
| indicative_measurements
| modelling_applications
| objective_estimation
| combined_assessment
```

```text
MODELLED_EXCEEDANCE_STATUS =
not_applicable
| may_be_evaluated_as_non_exceedance
| evaluated_with_additional_measurements
| used_for_assessment
```

---

## Normative requirements

---

### REQ-ASSESS-ZONE_AND_UNIT_CONTEXT

| Field | Value |
|------|-------|
| Source | Art. 6 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_ZONE |

**Rule**  
Assessment shall be carried out in all zones and, where relevant, in average exposure territorial units established for air quality assessment and management.

**Acceptance criterion**

```text
for each pollutant p:
    assessment_domain = zones supplied by M_ZONE

if metric = average_exposure_indicator:
    assessment_domain = average_exposure_territorial_units supplied by M_ZONE
```

---

### REQ-ASSESS-THRESHOLD_APPLICABILITY

| Field | Value |
|------|-------|
| Source | Art. 7(1); Annex II Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | constitutive rule |
| Dependencies | T_ASSESS_THRESHOLDS |

**Rule**  
Assessment thresholds laid down in Annex II apply for classification of zones.

**Acceptance criterion**

```text
TH(p) = T_ASSESS_THRESHOLDS[p]
```

---

### REQ-ASSESS-ZONE_CLASSIFICATION

| Field | Value |
|------|-------|
| Source | Art. 7(1), Art. 7(3); Annex II Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | T_ASSESS_THRESHOLDS; M_DATA_QUALITY |

**Rule**  
Each zone shall be classified in relation to assessment thresholds.

Where sufficient data are available, an assessment threshold is considered exceeded where it has been exceeded during at least **three separate calendar years** out of the previous **five calendar years**.

**Acceptance criterion**

```text
ABOVE_THRESHOLD(p,z) =
    COUNT{ y ∈ previous_5_calendar_years |
           C(p,z,y,m_threshold) > TH(p) } >= 3
```

```text
if ABOVE_THRESHOLD(p,z) = true:
    threshold_classification = above_threshold
else:
    threshold_classification = below_threshold
```

**Note**  
The relevant years are the previous five calendar years. The three exceedance years must be separate years but need not be consecutive.

---

### REQ-ASSESS-INSUFFICIENT_DATA_FOR_CLASSIFICATION

| Field | Value |
|------|-------|
| Source | Art. 7(3) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | permission |
| Dependencies | M_DATA_QUALITY; M_MOD; M_NETWORK |

**Rule**  
Where data are available for less than five years, exceedances of assessment thresholds may be determined using available data combined with other appropriate assessment information.

**Acceptance criterion**

```text
if available_valid_years(p,z) < 5:
    threshold_determination_method = alternative_combined_method
    classification_basis = insufficient_data_alternative_method
```

**Documentation requirement**

```text
if classification_basis = insufficient_data_alternative_method:
    require evidence_sources_documented = true
```

---

### REQ-ASSESS-CLASSIFICATION_REVIEW

| Field | Value |
|------|-------|
| Source | Art. 7(2) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | REQ-ASSESS-ZONE_CLASSIFICATION |

**Rule**  
Zone classification in relation to assessment thresholds shall be reviewed at least every five years and more frequently in the event of significant changes in activities impacting ambient concentrations.

**Acceptance criterion**

```text
classification_review_due =
    years_since(last_classification_review) >= 5
    OR significant_activity_change_affecting_concentrations = true
```

---

### REQ-ASSESS-REGIME_ABOVE_THRESHOLD

| Field | Value |
|------|-------|
| Source | Art. 8(2) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | REQ-ASSESS-ZONE_CLASSIFICATION; M_NETWORK |

**Rule**  
In all zones classified as above the assessment thresholds, ambient air quality shall be monitored using fixed measurements.

Fixed measurements may be supplemented by modelling applications or indicative measurements where appropriate to provide information on spatial distribution of air pollutants and spatial representativeness of fixed measurements.

**Acceptance criterion**

```text
if ABOVE_THRESHOLD(p,z) = true:
    required_methods include fixed_measurements
    supplementary_methods_allowed = [modelling_applications, indicative_measurements]
```

---

### REQ-ASSESS-REGIME_BELOW_THRESHOLD

| Field | Value |
|------|-------|
| Source | Art. 8(4) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | constitutive rule |
| Dependencies | REQ-ASSESS-ZONE_CLASSIFICATION |

**Rule**  
In all zones classified as below the assessment thresholds, assessment using modelling applications, indicative measurements, objective estimation, or a combination thereof is sufficient.

**Acceptance criterion**

```text
if ABOVE_THRESHOLD(p,z) = false:
    sufficient_methods = [
        modelling_applications,
        indicative_measurements,
        objective_estimation,
        combined_assessment
    ]
```

---

### REQ-ASSESS-EXCEEDANCE_REQUIRES_MODELLING_OR_INDICATIVE

| Field | Value |
|------|-------|
| Source | Art. 8(3) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_LIMITS; M_MOD; M_REPR; M_NETWORK |

**Rule**  
Where pollutant levels exceed a relevant limit value or target value set out in Annex I, ambient air quality shall be assessed using modelling applications or indicative measurements, within the timing linked to implementing acts referred to in Art. 8(7).

Where those methods are used, they shall provide information on the spatial distribution of pollutants. Where modelling applications are used, information on the spatial representativeness of fixed measurements shall also be provided.

**Acceptance criterion**

```text
if M_LIMITS.exceeds_limit_or_target_value(p,z) = true:
    required_methods include one_of([
        modelling_applications,
        indicative_measurements
    ])
    require spatial_distribution_information = true

    if modelling_applications used:
        require spatial_representativeness_information = true
```

---

### REQ-ASSESS-MODELLING_FREQUENCY

| Field | Value |
|------|-------|
| Source | Art. 8(3) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory with practicability qualifier |
| Dependencies | M_MOD |

**Rule**  
Where modelling applications are used under Art. 8(3), they shall be carried out at least every five years, as far as practicable.

**Acceptance criterion**

```text
if modelling_applications_used_under_Art8_3 = true:
    modelling_frequency_ok =
        years_since(previous_modelling_run) <= 5
        OR practicability_exception_documented = true
```

---

### REQ-ASSESS-MODELLED_EXCEEDANCE_STATUS

| Field | Value |
|------|-------|
| Source | Art. 8(5) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory / permission |
| Dependencies | M_LIMITS; M_MOD; M_REPR; M_NETWORK |

**Rule**  
Assessment with respect to limit values and target values shall take into account results of modelling applications or indicative measurements used under the relevant assessment provisions.

A modelled exceedance may be evaluated as a non-exceedance only where fixed measurements with spatial representativeness covering the modelled area of exceedance are available.

**Acceptance criterion**

```text
if modelled_exceedance(p,z,area) = true:

    if fixed_measurements_cover_modelled_exceedance_area(area) = true:
        MODELLED_EXCEEDANCE_STATUS = may_be_evaluated_as_non_exceedance

    else:
        MODELLED_EXCEEDANCE_STATUS = used_for_assessment
```

---

### REQ-ASSESS-ADDITIONAL_MEASUREMENTS_AFTER_MODELLED_EXCEEDANCE

| Field | Value |
|------|-------|
| Source | Art. 8(6); Art. 9(3) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mixed |
| Dependencies | M_NETWORK; M_MOD; M_REPR; M_DATA_QUALITY |

**Rule**  
Where modelling applications used under Art. 8(3) or Art. 8(4) show an exceedance not covered by fixed measurements and their area of spatial representativeness, at least one additional fixed or indicative measurement may be used.

Where modelling applications used under Art. 9(3) show such an exceedance, at least one additional fixed or indicative measurement shall be used.

Where additional fixed measurements are used, they shall be established within two years from the date of the modelled exceedance. Where additional indicative measurements are used, they shall be established within one year from that date. Measurements shall cover at least one calendar year and meet Annex V minimum data coverage requirements.

If no additional fixed or indicative measurements are conducted in a case where they are optional, the modelled exceedance shall be used for air quality assessment.

**Acceptance criterion**

```text
if modelled_exceedance_not_covered_by_fixed_measurements = true:

    if modelling_trigger in [Art8_3, Art8_4]:
        additional_measurement_permitted = true

    if modelling_trigger = Art9_3:
        additional_measurement_required = true

    if additional_fixed_measurement_used = true:
        establishment_deadline = modelled_exceedance_date + 2 years

    if additional_indicative_measurement_used = true:
        establishment_deadline = modelled_exceedance_date + 1 year

    require measurement_duration >= 1 calendar_year
    require AnnexV_minimum_coverage_satisfied = true

    if additional_measurement_used = false:
        MODELLED_EXCEEDANCE_STATUS = used_for_assessment
```

---

### REQ-ASSESS-NETWORK_REDUCTION_ELIGIBILITY_LINK

| Field | Value |
|------|-------|
| Source | Art. 9(3) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | permission / gating rule |
| Dependencies | M_NETWORK; M_LIMITS; M_DATA_QUALITY; M_MOD; M_REPR |

**Rule**  
The assessment regime may support reduction of the minimum number of fixed measurement points only where Art. 9(3) conditions are met.

This module determines and exposes the assessment-side prerequisites. `M_NETWORK` determines network adequacy and applies the reduction quantitatively.

**Acceptance criterion**

```text
network_reduction_assessment_eligible =
    ABOVE_THRESHOLD(p,z) = true
    AND M_LIMITS.exceeds_limit_target_or_critical_level(p,z) = false
    AND supplementary_assessment_sufficient_for_regulatory_values = true
    AND supplementary_assessment_sufficient_for_alert_and_information_thresholds = true
    AND public_information_sufficient = true
    AND spatial_resolution_sufficient = true
    AND AnnexV_requirements_satisfied = true
```

---

### REQ-ASSESS-METHOD_VALIDITY_LINK

| Field | Value |
|------|-------|
| Source | Art. 11; Annex V; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_DATA_QUALITY; M_MODEL_QA; M_NETWORK |

**Rule**  
Assessment methods shall rely on reference measurement methods or other methods satisfying Annex VI conditions. Modelling applications used for assessment shall satisfy Annex VI conditions. Assessment data shall satisfy Annex V data quality objectives.

**Acceptance criterion**

```text
if fixed_measurements or indicative_measurements used:
    require measurement_method_valid_under_AnnexVI = true
    require data_quality_valid_under_AnnexV = true

if modelling_applications used:
    require model_valid_under_AnnexVI = true
```

---

### REQ-ASSESS-BIO_INDICATORS

| Field | Value |
|------|-------|
| Source | Art. 8(8) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | recommendation |
| Dependencies | M_ECOSYSTEMS; M_NETWORK |

**Rule**  
Where regional patterns of the impact on ecosystems are to be assessed, the use of bio-indicators shall be considered where applicable.

**Acceptance criterion**

```text
if ecosystem_impact_regional_patterns_assessed = true:
    bio_indicator_use_considered = true
```

---

### REQ-ASSESS-DOWNSTREAM_COMPLIANCE_HOOKS

| Field | Value |
|------|-------|
| Source | Art. 12–18 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_LIMITS; M_SOURCE_ATTRIBUTION; M_ATTAINMENT_EXTENSION |

**Rule**  
Assessment outputs shall provide the concentration fields, method basis, threshold classification and modelled exceedance status needed by downstream compliance modules.

The module does not decide final compliance status, natural-source omission, winter sanding or salting effects, or valid postponements.

**Acceptance criterion**

```text
assessment_output includes:
    assessment_methods_used
    concentration_basis
    threshold_classification
    modelled_exceedance_status
    spatial_distribution_information
    spatial_representativeness_information
```

---

### REQ-ASSESS-DOWNSTREAM_PLANNING_HOOKS

| Field | Value |
|------|-------|
| Source | Art. 19; Art. 20 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_LIMITS; M_PLANS; M_SHORT_TERM_ACTION; M_PUBLIC_INFORMATION |

**Rule**  
Assessment results shall expose triggers needed for air quality plans, air quality roadmaps and short-term action plans.

This module does not establish those plans. It only provides assessment-side inputs, including exceedance detection context and forecast/model inputs where relevant.

**Acceptance criterion**

```text
if assessment identifies or supports exceedance of limit_value_or_target_value:
    expose planning_trigger_input = true

if assessment or forecast identifies alert_threshold_exceeded_or_predicted:
    expose short_term_action_trigger_input = true
```

---

### REQ-ASSESS-DOWNSTREAM_TRANSBOUNDARY_PUBLIC_REPORTING_HOOKS

| Field | Value |
|------|-------|
| Source | Art. 21; Art. 22; Art. 23 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_TRANSBOUNDARY; M_PUBLIC_INFORMATION; M_REPORTING |

**Rule**  
Assessment outputs shall support downstream transboundary cooperation, public information and reporting obligations.

This includes information on zones, territorial units, assessment methods, exceedance context, spatial distribution and source/transport indicators where available.

**Acceptance criterion**

```text
assessment_output supports:
    transboundary_exceedance_context
    public_information_assessment_summary
    reporting_to_commission_payload
```

---

### REQ-ASSESS-STRICTEST_PREVAILS

| Field | Value |
|------|-------|
| Source | Framework rule derived from Art. 7–9 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | REQ-ASSESS-REGIME_ABOVE_THRESHOLD; REQ-ASSESS-EXCEEDANCE_REQUIRES_MODELLING_OR_INDICATIVE; REQ-ASSESS-NETWORK_REDUCTION_ELIGIBILITY_LINK |

**Rule**  
Where more than one assessment obligation or permission applies to the same pollutant and zone, the most protective or restrictive applicable assessment regime shall prevail.

**Precedence order**

```text
1. mandatory additional measurement after Art9_3 modelled exceedance
2. fixed measurements required above assessment threshold
3. modelling or indicative assessment required after limit/target exceedance
4. below-threshold simplified assessment
5. objective estimation only where sufficient under the applicable regime
```

**Acceptance criterion**

```text
ASSESSMENT_REGIME(p,z) = highest_precedence_applicable_regime(p,z)
```

---

## Assessment-level decision logic

```text
for each pollutant p and zone z:

    determine threshold_classification under Art. 7

    if ABOVE_THRESHOLD(p,z):
        required_methods include fixed_measurements

    else:
        sufficient_methods include modelling_applications
        sufficient_methods include indicative_measurements
        sufficient_methods include objective_estimation
        sufficient_methods include combined_assessment

    if M_LIMITS.exceeds_limit_or_target_value(p,z):
        required_methods include one_of([modelling_applications, indicative_measurements])
        require spatial_distribution_information

    if modelling_applications used:
        require model_valid_under_AnnexVI
        require spatial_representativeness_information where Art. 8(3) applies

    if modelled_exceedance_not_covered_by_fixed_measurements:
        determine additional_measurement_required_or_permitted
        determine MODELLED_EXCEEDANCE_STATUS

    if Art9_3 network reduction requested:
        determine network_reduction_assessment_eligible
```

---

## Interactions with other modules

- `M_ZONE` — provides zones and average exposure territorial units
- `M_NETWORK` — applies sampling-point and supersite requirements based on assessment regime
- `M_DATA_QUALITY` — validates measurement data and Annex V requirements
- `M_MOD` — provides modelling applications, modelled concentration fields and modelled exceedances
- `M_MODEL_QA` — validates modelling applications under Annex VI
- `M_REPR` — defines spatial representativeness of fixed measurements
- `M_LIMITS` — determines compliance and exceedance status under Annex I
- `M_SOURCE_ATTRIBUTION` — handles natural sources, winter sanding/salting and transboundary contributions
- `M_ATTAINMENT_EXTENSION` — handles Art. 18 postponement status
- `M_PLANS` — handles air quality plans and roadmaps under Art. 19
- `M_SHORT_TERM_ACTION` — handles short-term action plans under Art. 20
- `M_TRANSBOUNDARY` — handles Art. 21 cooperation and source contribution interfaces
- `M_PUBLIC_INFORMATION` — handles Art. 22 public information outputs
- `M_REPORTING` — handles Art. 23 reporting to the Commission
- `T_ASSESS_THRESHOLDS` — Annex II thresholds
- `T_LIMIT_VALUES` — Annex I limit values and target values
- `T_SITING` — Annex IV siting and representativeness criteria

---

## Output

```text
assessment_status:
  zone_id
  territorial_unit_id
  pollutant
  metric
  assessment_year

  classification:
    threshold_classification = above_threshold | below_threshold | insufficient_data_alternative_method
    classification_basis
    years_evaluated
    exceedance_year_count
    review_due

  methods:
    required_methods
    sufficient_methods
    supplementary_methods_allowed
    methods_used
    method_validity_status
    data_quality_status
    model_validity_status

  spatial_information:
    spatial_distribution_information_required
    spatial_distribution_information_available
    spatial_representativeness_information_required
    spatial_representativeness_information_available

  modelled_exceedance:
    modelled_exceedance_detected
    fixed_measurements_cover_modelled_area
    additional_measurement_required
    additional_measurement_permitted
    additional_measurement_deadline
    modelled_exceedance_status

  network_reduction:
    Art9_3_reduction_requested
    network_reduction_assessment_eligible
    reduction_blocking_reasons

  downstream_triggers:
    compliance_assessment_input_ready
    planning_trigger_input
    short_term_action_trigger_input
    transboundary_context_available
    public_information_input_ready
    reporting_input_ready
```

---

## Notes

- Above-threshold classification requires fixed measurements; it does not exclude supplementary modelling or indicative measurements.
- Below-threshold classification permits simplified assessment methods but does not remove the need for valid assessment where downstream obligations are triggered.
- Modelled exceedances are not automatically disregarded. They may be evaluated as non-exceedances only where fixed measurements with adequate spatial representativeness cover the modelled exceedance area.
- Reduction of fixed measurement points is not an assessment default; it is a conditional permission requiring the Art. 9(3) criteria and network validation in `M_NETWORK`.
