# M_SOURCE_ATTRIBUTION — Source attribution and contribution qualification

## Normative references

- Art. 4(40) Directive (EU) 2024/2881 — contributions from natural sources
- Art. 8 Directive (EU) 2024/2881 — assessment methods and modelling applications
- Art. 16 Directive (EU) 2024/2881 — exceedances attributable to natural sources
- Art. 17 Directive (EU) 2024/2881 — PM10 exceedances due to re-suspension following winter sanding or winter salting of roads
- Art. 18 Directive (EU) 2024/2881 — postponement of attainment deadlines, including transboundary contributions as a possible ground
- Art. 19 Directive (EU) 2024/2881 — air quality plans and roadmaps
- Art. 21 Directive (EU) 2024/2881 — transboundary air pollution
- Art. 22–23 Directive (EU) 2024/2881 — public information and reporting interfaces
- Annex I — limit values, target values, critical levels and exposure obligations
- Annex V — data quality objectives
- Annex VI — modelling applications and method conditions
- Annex VIII — air quality plans and roadmaps
- Implementing acts under Art. 16(4), Art. 17(4), Art. 18(5), where applicable

---

## Description

This module determines whether an exceedance, concentration level, exposure indicator or contribution can be attributed, wholly or partly, to specific source categories relevant under the Directive.

It supports legal qualification of:

- exceedances attributable to natural sources;
- PM10 exceedances attributable to re-suspension from winter sanding or winter salting of roads;
- transboundary contributions to exceedances;
- other source contributions relevant for plans, roadmaps, projections or postponement requests;
- source-contribution evidence used in reporting, public information and planning.

This module produces **evidence and attribution status**. It does not itself decide final compliance status, omit exceedances for the purposes of the Directive, grant postponements, or waive planning obligations. Those legal consequences are handled by downstream modules.

---

## Scope

This module applies to source-attribution assessments used for:

```text
natural_source_attribution
winter_sanding_or_salting_attribution
transboundary_contribution_attribution
plan_source_contribution_analysis
roadmap_source_contribution_analysis
postponement_ground_support
average_exposure_adjustment_support
public_information_source_context
reporting_source_context
```

It may be applied to:

- limit value exceedances;
- target value exceedances;
- average exposure indicators and exposure obligations;
- modelled exceedance areas;
- zones;
- average exposure territorial units;
- transboundary affected areas;
- plan or roadmap areas.

---

## Definitions

```text
p = pollutant
z = zone
u = average exposure territorial unit
A = geographic area
y = calendar year
m = regulatory metric
```

```text
SOURCE_CATEGORY =
    natural_source
    winter_sanding_or_salting
    transboundary_contribution
    local_traffic
    industrial_source
    domestic_heating
    agriculture
    shipping_or_port
    aviation_or_airport
    other_anthropogenic_source
```

```text
SOURCE_CONTRIBUTION(p,A,m,period,source) =
estimated contribution of source category source to the assessed level
of pollutant p in area A, metric m and period
```

```text
ATTRIBUTABLE_EXCEEDANCE(p,A,m,period,source) =
exceedance for which the evidence package supports attribution to source
```

```text
NATURAL_SOURCE_CASE =
exceedance or exposure-related level attributable to contributions from natural sources
under Art. 16
```

```text
WINTER_SANDING_SALTING_CASE =
PM10 limit-value exceedance attributable to re-suspension of particulates
following winter sanding or winter salting of roads under Art. 17
```

```text
TRANSBOUNDARY_CASE =
exceedance or level to which transboundary transport of air pollution contributes significantly
under Art. 21 or Art. 18
```

```text
ATTRIBUTION_EVIDENCE_PACKAGE =
traceable dataset, method, modelling, measurement, event and source-contribution evidence
supporting an attribution conclusion
```

---

## Normative requirements

---

### REQ-SOURCE-PURPOSE_CLASSIFICATION

| Field | Value |
|------|-------|
| Source | Art. 16; Art. 17; Art. 18; Art. 21 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_LIMITS; M_EXPOSURE; M_MOD; M_ZONE |

**Rule**  
Every attribution assessment shall be classified by regulatory purpose before attribution consequences are applied.

**Acceptance criterion**

```text
if source_attribution_assessment_used = true:
    attribution_purpose in [
        natural_source_attribution,
        winter_sanding_or_salting_attribution,
        transboundary_contribution_attribution,
        plan_source_contribution_analysis,
        roadmap_source_contribution_analysis,
        postponement_ground_support,
        average_exposure_adjustment_support,
        public_information_source_context,
        reporting_source_context
    ]
```

---

### REQ-SOURCE-EVIDENCE_PACKAGE

| Field | Value |
|------|-------|
| Source | Art. 16(2); Art. 17(2); Art. 21; Annex V; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_DATA_QUALITY; M_MOD; M_MODEL_QA; M_REPORTING |

**Rule**  
A source attribution conclusion shall be supported by a documented evidence package.

The evidence package shall identify:

- pollutant;
- metric;
- area or territorial unit;
- assessment period;
- source category;
- concentration contribution or qualitative contribution evidence;
- methods used;
- datasets used;
- data quality status;
- modelling validation status where modelling is used;
- uncertainty or confidence metadata where available.

**Acceptance criterion**

```text
ATTRIBUTION_EVIDENCE_PACKAGE_COMPLETE =
    pollutant identified
    AND metric identified
    AND territorial_domain identified
    AND period identified
    AND source_category identified
    AND method_documented = true
    AND data_quality_status_available = true
    AND modelling_validation_status_available_if_model_used = true
```

---

### REQ-SOURCE-NATURAL_SOURCE_CLASSIFICATION

| Field | Value |
|------|-------|
| Source | Art. 4(40); Art. 16(1); Art. 16(2) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | permission / mandatory evidence where used |
| Dependencies | M_LIMITS; M_EXPOSURE; M_REPORTING |

**Rule**  
Zones and average exposure territorial units may be classified as affected by exceedances attributable to natural sources.

Where such classification is used, the required lists, information on concentrations and sources, and evidence of natural-source attribution shall be provided.

**Acceptance criterion**

```text
if source_category = natural_source
AND attribution_evidence_sufficient = true:
    natural_source_case = true
    affected_zones_or_AETUs recorded
    concentration_and_source_information_available = true
    evidence_package_complete = true
else:
    natural_source_case = false
```

---

### REQ-SOURCE-NATURAL_SOURCE_CONTRIBUTION_TYPES

| Field | Value |
|------|-------|
| Source | Art. 4(40) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | definitional / classification |
| Dependencies | T_SOURCE_CATEGORIES |

**Rule**  
Natural-source contributions include emissions not caused directly or indirectly by human activities, including natural events such as volcanic eruptions, seismic activities, geothermal activities, wild-land fires, high-wind events, sea sprays, or atmospheric re-suspension or transport of natural particles from dry regions.

**Acceptance criterion**

```text
if event_or_contribution_type in T_SOURCE_CATEGORIES.natural_sources:
    natural_source_candidate = true
else:
    natural_source_candidate = false
```

---

### REQ-SOURCE-NATURAL_SOURCE_ADJUSTMENT_OUTPUT

| Field | Value |
|------|-------|
| Source | Art. 16(3); Art. 16(4) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_LIMITS; M_EXPOSURE; M_REPORTING |

**Rule**  
Where a natural-source case is supported and communicated as required, this module shall provide the contribution estimate and evidence needed by downstream modules to determine whether the exceedance or contribution may be omitted or adjusted for the purposes of the Directive.

This module does not itself perform the legal omission.

**Acceptance criterion**

```text
if natural_source_case = true:
    provide natural_source_contribution_estimate
    provide adjusted_concentration_candidate
    provide evidence_package_reference
    provide reporting_payload
```

---

### REQ-SOURCE-COMMISSION_EVIDENCE_STATUS_INTERFACE

| Field | Value |
|------|-------|
| Source | Art. 16(3) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_REPORTING; M_LIMITS |

**Rule**  
Where the Commission considers evidence insufficient, the downstream legal effect of omission shall not apply. This module shall record the evidence-status interface where available.

**Acceptance criterion**

```text
if commission_evidence_insufficient_notification = true:
    evidence_status = insufficient
    natural_source_omission_supported = false
else if evidence_package_submitted = true:
    evidence_status = submitted_no_insufficiency_recorded
```

---

### REQ-SOURCE-WINTER_SANDING_SALTING_CLASSIFICATION

| Field | Value |
|------|-------|
| Source | Art. 17(1); Art. 17(2) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | permission / mandatory evidence where used |
| Dependencies | M_LIMITS; M_REPORTING |

**Rule**  
For a given year, zones may be identified where PM10 limit values are exceeded due to re-suspension of particulates following winter sanding or winter salting of roads.

Where such identification is used, the required list of zones, information on PM10 concentrations and sources, and evidence that the exceedances are due to re-suspended particulates and that reasonable measures have been taken shall be provided.

**Acceptance criterion**

```text
if pollutant = PM10
AND limit_value_exceedance = true
AND source_category = winter_sanding_or_salting
AND attribution_evidence_sufficient = true
AND reasonable_measures_to_lower_concentrations_documented = true:
    winter_sanding_salting_case = true
else:
    winter_sanding_salting_case = false
```

---

### REQ-SOURCE-WINTER_SANDING_SALTING_EFFECT_INTERFACE

| Field | Value |
|------|-------|
| Source | Art. 17(3) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_LIMITS; M_PLANS |

**Rule**  
Where PM10 exceedances are attributable to winter sanding or winter salting, the establishment of an air quality plan may be omitted for those exceedances, except where exceedances are attributable to PM10 sources other than winter sanding or winter salting.

This module shall identify the attribution and any non-winter PM10 source contribution. The planning consequence is determined by `M_PLANS` or `M_LIMITS`.

**Acceptance criterion**

```text
if winter_sanding_salting_case = true:
    provide winter_sanding_salting_component
    provide other_PM10_source_contribution_status

if other_PM10_sources_cause_exceedance = true:
    plan_omission_supported_for_winter_component_only = true
```

---

### REQ-SOURCE-TRANSBOUNDARY_IDENTIFICATION

| Field | Value |
|------|-------|
| Source | Art. 21(1); Art. 21(6); Art. 18(1) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory / permission depending on trigger |
| Dependencies | M_TRANSBOUNDARY; M_MOD; M_MODEL_QA; M_ZONE |

**Rule**  
Where transboundary transport of air pollution contributes or may contribute significantly to exceedances, the affected zones or average exposure territorial units and source-contribution context shall be identified.

Transboundary contributions may also support postponement grounds under Art. 18 where the relevant conditions are satisfied.

**Acceptance criterion**

```text
if transboundary_contribution_relevant = true:
    affected_member_states recorded
    affected_zones_or_AETUs recorded
    source_member_state_or_third_country recorded where available
    contribution_estimate_available = true
    cross_boundary_geometry_available = true
```

---

### REQ-SOURCE-TRANSBOUNDARY_CONTRIBUTION_PACKAGE

| Field | Value |
|------|-------|
| Source | Art. 21(2); Art. 21(6) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_TRANSBOUNDARY; M_MOD; M_REPORTING |

**Rule**  
Where transboundary contribution is identified, the module shall provide a contribution package suitable for cooperation, reporting and downstream assessment.

**Acceptance criterion**

```text
if transboundary_case = true:
    provide source_contribution_estimates
    provide affected_area_geometry
    provide affected_zones_or_AETUs
    provide source_categories
    provide method_and_model_metadata
    provide uncertainty_or_confidence_metadata_where_available
```

---

### REQ-SOURCE-POSTPONEMENT_GROUND_SUPPORT

| Field | Value |
|------|-------|
| Source | Art. 18(1); Art. 18(4) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_ATTAINMENT_EXTENSION; M_MOD; M_PLANS |

**Rule**  
Where transboundary contributions, or other source-context grounds, are used to justify postponement of attainment deadlines, the module shall provide evidence and contribution context to `M_ATTAINMENT_EXTENSION`.

**Acceptance criterion**

```text
if postponement_request_uses_source_context = true:
    provide source_contribution_evidence_package
    provide affected_pollutant_and_zone
    provide projected_effect_on_attainment
    provide methods_and_data_used
```

---

### REQ-SOURCE-PLAN_AND_ROADMAP_SOURCE_ANALYSIS

| Field | Value |
|------|-------|
| Source | Art. 19; Annex VIII Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_PLANS; M_MOD; M_LIMITS |

**Rule**  
Air quality plans and roadmaps require source-contribution context sufficient to identify relevant measures and demonstrate how exceedance periods are kept as short as possible.

This module shall provide source categories and contribution estimates where available.

**Acceptance criterion**

```text
if plan_or_roadmap_trigger = true:
    provide source_contribution_profile
    provide major_source_categories
    provide spatial_distribution_of_contributions where available
    provide temporal_pattern_of_contributions where available
```

---

### REQ-SOURCE-AVERAGE_EXPOSURE_ADJUSTMENT_SUPPORT

| Field | Value |
|------|-------|
| Source | Art. 16; Annex I Section 5 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_EXPOSURE; M_DATA_QUALITY |

**Rule**  
Where natural-source contributions affect annual mean concentrations used for AEI calculation, the module shall provide adjusted and unadjusted contribution information to `M_EXPOSURE`.

**Acceptance criterion**

```text
if natural_source_contribution_affects_AEI_input = true:
    provide station_year_contribution_estimate
    provide adjusted_annual_mean_candidate
    provide unadjusted_annual_mean_reference
    provide evidence_package_reference
```

---

### REQ-SOURCE-MODELLING_EVIDENCE_VALIDITY

| Field | Value |
|------|-------|
| Source | Art. 8; Art. 11; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where modelling is used |
| Dependencies | M_MOD; M_MODEL_QA; M_DATA_QUALITY |

**Rule**  
Where modelling is used for source attribution, the model shall be valid for source-attribution or transboundary-contribution purposes, and input data shall be valid for that purpose.

**Acceptance criterion**

```text
if source_attribution_uses_modelling = true:
    MODEL_VALID(model, source_attribution_or_transboundary_purpose) = true
    AND input_data_quality_valid = true
```

---

### REQ-SOURCE-MEASUREMENT_EVIDENCE_VALIDITY

| Field | Value |
|------|-------|
| Source | Art. 11; Annex V Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where measurements are used |
| Dependencies | M_DATA_QUALITY; M_NETWORK |

**Rule**  
Measurements used as source-attribution evidence shall be valid for the source-attribution purpose and traceable to sampling point, pollutant, metric and period.

**Acceptance criterion**

```text
if source_attribution_uses_measurements = true:
    measurement_data_valid_for_source_attribution = true
    AND sampling_point_metadata_available = true
    AND pollutant_metric_period_identified = true
```

---

### REQ-SOURCE-UNCERTAINTY_AND_CONFIDENCE

| Field | Value |
|------|-------|
| Source | Annex V; Annex VI Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where available / required |
| Dependencies | M_DATA_QUALITY; M_MODEL_QA |

**Rule**  
Source-contribution estimates shall include uncertainty or confidence metadata where required or available.

**Acceptance criterion**

```text
if source_contribution_estimate_used_for_regulatory_purpose = true:
    uncertainty_or_confidence_metadata_recorded_where_available = true
```

---

### REQ-SOURCE-PUBLIC_INFORMATION_INTERFACE

| Field | Value |
|------|-------|
| Source | Art. 22 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_PUBLIC_INFORMATION |

**Rule**  
Where source-attribution information is used for public information, it shall be traceable and suitable for clear communication, without obscuring the underlying exceedance or assessment result.

**Acceptance criterion**

```text
if source_attribution_used_for_public_information = true:
    public_information_metadata_ready = true
    source_category_label_available = true
    affected_area_available = true
    pollutant_metric_period_identified = true
```

---

### REQ-SOURCE-REPORTING_INTERFACE

| Field | Value |
|------|-------|
| Source | Art. 16(2); Art. 17(2); Art. 21(6); Art. 23 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory interface rule |
| Dependencies | M_REPORTING |

**Rule**  
Where source attribution is used for natural sources, winter sanding/salting, transboundary contribution or reporting of concentration/source information, the module shall provide a reporting-ready evidence package.

**Acceptance criterion**

```text
if attribution_case_reportable = true:
    reporting_payload_ready = true
    affected_zones_or_AETUs included
    concentration_information included
    source_information included
    evidence_reference included
```

---

### REQ-SOURCE-LEGAL_EFFECT_BOUNDARY

| Field | Value |
|------|-------|
| Source | Art. 16; Art. 17; Art. 18; Art. 19; Art. 21 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | framework boundary rule |
| Dependencies | M_LIMITS; M_PLANS; M_ATTAINMENT_EXTENSION; M_TRANSBOUNDARY |

**Rule**  
This module identifies and documents source attribution. It does not itself:

- omit an exceedance for the purposes of the Directive;
- determine final compliance status;
- waive an air quality plan;
- grant or validate a postponement;
- complete transboundary cooperation.

**Acceptance criterion**

```text
source_attribution_status produced
legal_consequence delegated_to_downstream_module = true
```

---

## Source-attribution decision logic

```text
for each exceedance, exposure input, modelled exceedance area or planning case:

    identify pollutant, metric, period and territorial domain

    classify attribution_purpose

    identify candidate source categories

    collect evidence:
        measurements
        modelling outputs
        event data
        emission/source data
        meteorological or transport data
        activity data
        documentation of measures where required

    validate evidence:
        data quality
        model validity if modelling is used
        traceability
        temporal and spatial consistency

    estimate or document source contribution

    determine attribution status:
        natural_source_case
        winter_sanding_salting_case
        transboundary_case
        other_source_contribution_profile

    prepare outputs for:
        M_LIMITS
        M_EXPOSURE
        M_PLANS
        M_ATTAINMENT_EXTENSION
        M_TRANSBOUNDARY
        M_PUBLIC_INFORMATION
        M_REPORTING
```

---

## Interactions with other modules

- `M_ZONE` — provides affected zones, average exposure territorial units and transboundary areas
- `M_LIMITS` — consumes attribution status for compliance qualification and legal status of exceedances
- `M_EXPOSURE` — consumes natural-source adjustment support for AEI inputs
- `M_MOD` — provides modelled contribution, transport and source-apportionment outputs
- `M_MODEL_QA` — validates modelling used for attribution
- `M_DATA_QUALITY` — validates measurements and datasets used as evidence
- `M_REPR` — links source contributions and modelled exceedance areas to spatial representativeness
- `M_PLANS` — consumes source profiles for plans and roadmaps
- `M_ATTAINMENT_EXTENSION` — consumes transboundary or source-context evidence for postponement requests
- `M_TRANSBOUNDARY` — consumes and coordinates transboundary contribution cases
- `M_PUBLIC_INFORMATION` — consumes source-context summaries for public information
- `M_REPORTING` — consumes reporting-ready lists, concentration/source information and evidence packages
- `T_SOURCE_CATEGORIES` — source category definitions and classification rules
- `T_ATTRIBUTION_METHODS` — accepted or documented attribution methods

---

## Output

```text
source_attribution_status:
  attribution_case_id
  pollutant
  metric
  assessment_period
  territorial_domain
  affected_zones
  affected_average_exposure_territorial_units
  affected_area_geometry

  source:
    source_category
    source_subcategory
    source_location
    source_member_state_or_third_country
    source_contribution_estimate
    contribution_units
    contribution_uncertainty_or_confidence

  case_status:
    natural_source_case
    winter_sanding_salting_case
    transboundary_case
    other_source_contribution_case
    attribution_evidence_sufficient
    evidence_status

  evidence:
    evidence_package_complete
    measurement_evidence_used
    modelling_evidence_used
    model_validity_status
    data_quality_status
    method_reference
    event_or_activity_documentation
    reasonable_measures_documented_for_winter_case

  legal_effect_interfaces:
    natural_source_omission_candidate
    winter_sanding_salting_plan_omission_candidate
    postponement_ground_support
    transboundary_cooperation_trigger
    planning_source_profile_ready

  downstream:
    limits_input_ready
    exposure_input_ready
    plans_input_ready
    attainment_extension_input_ready
    transboundary_input_ready
    public_information_metadata_ready
    reporting_payload_ready
```

---

## Notes

- Source attribution is evidence-based and purpose-specific.
- Natural-source attribution and winter sanding/salting attribution have different legal effects and shall not be merged.
- Winter sanding/salting applies specifically to PM10 exceedances due to re-suspension following winter sanding or salting of roads.
- Transboundary contribution can support cooperation obligations and may also support postponement grounds, but does not itself grant postponement.
- Adjusted and unadjusted concentrations shall be preserved where source contributions are subtracted or omitted for downstream purposes.
