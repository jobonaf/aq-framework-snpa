# M_ZONE — Territorial subdivision and assessment domains

## Normative references

- Art. 4 Directive (EU) 2024/2881 — definitions of zone, average exposure territorial unit, agglomeration, assessment and related territorial concepts
- Art. 6 Directive (EU) 2024/2881 — establishment of zones and average exposure territorial units
- Art. 7 Directive (EU) 2024/2881 — classification of zones in relation to assessment thresholds
- Art. 8 Directive (EU) 2024/2881 — assessment in all zones
- Art. 9 Directive (EU) 2024/2881 — monitoring network requirements by zone and average exposure territorial unit
- Art. 10 Directive (EU) 2024/2881 — monitoring supersites and territorial requirements
- Art. 12–15 Directive (EU) 2024/2881 — maintenance, compliance, critical levels, alert and information thresholds
- Art. 16–18 Directive (EU) 2024/2881 — natural sources, winter sanding/salting and attainment-deadline postponement interfaces
- Art. 19–21 Directive (EU) 2024/2881 — air quality plans, short-term action plans and transboundary cooperation interfaces
- Art. 22–23 Directive (EU) 2024/2881 — public information and reporting interfaces
- Annex I — air quality standards and exposure obligations
- Annex II — assessment thresholds
- Annex III — minimum number of sampling points
- Annex IV — siting and spatial representativeness
- Annex VIII — air quality plans and roadmaps

---

## Description

This module defines the **territorial assessment domains** used by the air quality framework.

It governs:

- zones;
- agglomerations;
- average exposure territorial units;
- relevant territorial units used for ozone, plans, roadmaps and short-term action plans;
- cross-boundary territorial context for transboundary pollution;
- versioning and reporting of territorial changes.

The module provides the territorial context for:

- assessment classification;
- monitoring-network requirements;
- spatial representativeness;
- compliance verification;
- exposure indicators;
- air quality plans and roadmaps;
- public information;
- reporting to the Commission;
- transboundary cooperation.

This module does **not**:

- classify zones above or below assessment thresholds;
- determine compliance with limit values or target values;
- calculate average exposure indicators;
- design the monitoring network;
- establish air quality plans or short-term action plans.

Those functions are handled by dedicated modules.

---

## Definitions

```text
territory(ms) =
territory of a Member State relevant for ambient air quality assessment
```

```text
zone =
part of the territory of a Member State, delimited by that Member State
for the purposes of air quality assessment and management
```

```text
agglomeration =
a conurbation with population > 250,000 inhabitants or,
where population <= 250,000 inhabitants,
with a population density per km² established by the Member State
```

```text
average_exposure_territorial_unit =
part of the territory of a Member State designated for determining the average exposure indicator,
corresponding to a NUTS 1 or NUTS 2 region, or a combination of adjacent NUTS 1/NUTS 2 regions,
subject to the size constraints defined by the Directive
```

```text
assessment_domain =
territorial unit over which an assessment, monitoring obligation,
compliance check, plan, roadmap, public-information output or reporting obligation is applied
```

```text
zone_version =
time-stamped version of zone geometry, attributes and legal status
```

```text
ZONE_COVERAGE_VALID =
all relevant territory is assigned to exactly one active zone,
except where specific legal or reporting rules require additional overlapping territorial units
```

---

## Territorial object types

```text
territorial_object_type =
    zone
    agglomeration
    average_exposure_territorial_unit
    ozone_territorial_unit
    plan_area
    roadmap_area
    short_term_action_area
    transboundary_affected_area
    reporting_area
```

---

## Normative requirements

---

### REQ-ZONE-TERRITORIAL_COVERAGE

| Field | Value |
|------|-------|
| Source | Art. 6 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | — |

**Rule**  
Member State territory shall be subdivided into zones for the purposes of air quality assessment and management.

**Acceptance criterion**

```text
ZONE_COVERAGE_VALID =
    for all x in territory(ms):
        exists exactly one active zone z such that x ∈ z
```

**Note**  
Additional territorial units, such as average exposure territorial units or plan areas, may overlap zones. The non-overlap requirement applies to the primary zone layer unless a specific legal function requires another layer.

---

### REQ-ZONE-ZONE_DEFINITION

| Field | Value |
|------|-------|
| Source | Art. 4(17) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | definitional |
| Dependencies | — |

**Rule**  
A zone is a part of the territory of a Member State delimited for air quality assessment and management.

**Acceptance criterion**

```text
zone:
    id is not null
    geometry is not null
    member_state is not null
    purpose includes [air_quality_assessment, air_quality_management]
```

---

### REQ-ZONE-AGGLOMERATION_DEFINITION

| Field | Value |
|------|-------|
| Source | Art. 4(19) Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | definitional |
| Dependencies | population_data; density_data |

**Rule**  
Agglomerations shall be identified as conurbations with population above 250,000 inhabitants, or with population of 250,000 inhabitants or fewer where the Member State establishes the relevant population-density criterion.

**Acceptance criterion**

```text
if conurbation_population > 250000:
    is_agglomeration = true
else if conurbation_population <= 250000
AND member_state_density_criterion_satisfied = true:
    is_agglomeration = true
else:
    is_agglomeration = false
```

---

### REQ-ZONE-AVERAGE_EXPOSURE_TERRITORIAL_UNITS

| Field | Value |
|------|-------|
| Source | Art. 4(18); Art. 6 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory where applicable |
| Dependencies | NUTS_REGIONS; M_LIMITS |

**Rule**  
Average exposure territorial units shall be designated for determining average exposure indicators. They shall correspond to NUTS 1 or NUTS 2 regions, or combinations of adjacent NUTS 1/NUTS 2 regions, subject to the Directive's size and territorial constraints.

**Acceptance criterion**

```text
AETU_VALID(u) =
    u.geometry is not null
    AND u is based_on NUTS_1_or_NUTS_2_or_valid_adjacent_combination
    AND u.total_area <= 85000 km2
    AND u.total_area < territory(ms).area
```

**Note**  
The entire territory of a Member State shall not be collapsed into a single average exposure territorial unit where the Directive's size constraint prevents it.

---

### REQ-ZONE-ASSESSMENT_DOMAIN_ASSIGNMENT

| Field | Value |
|------|-------|
| Source | Art. 6; Art. 8 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_ASSESS |

**Rule**  
Each assessment shall be assigned to the correct territorial domain.

**Acceptance criterion**

```text
if metric = average_exposure_indicator:
    assessment_domain = average_exposure_territorial_unit
else:
    assessment_domain = zone
```

```text
if pollutant_or_obligation_requires_specific_territorial_unit:
    assessment_domain = applicable_territorial_unit
```

---

### REQ-ZONE-ZONE_CLASSIFICATION_INTERFACE

| Field | Value |
|------|-------|
| Source | Art. 7 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_ASSESS |

**Rule**  
This module provides the zone geometry and attributes needed by `M_ASSESS` to classify zones in relation to assessment thresholds.

This module does not perform threshold classification.

**Acceptance criterion**

```text
for each active zone z:
    provide z.geometry
    provide z.population
    provide z.type
    provide z.version
    provide assessment_year
```

---

### REQ-ZONE-NETWORK_REQUIREMENT_INTERFACE

| Field | Value |
|------|-------|
| Source | Art. 9; Art. 10; Annex III Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_NETWORK |

**Rule**  
This module provides the territorial attributes required to calculate monitoring-network obligations, including population, area, agglomeration status, exposure context and supersite-relevant territorial attributes.

**Acceptance criterion**

```text
network_context(z) includes:
    geometry
    population
    area
    zone_type
    agglomeration_status
    urban_rural_context
    relevant_territorial_units
```

---

### REQ-ZONE-REPRESENTATIVENESS_BOUNDARY_CONTEXT

| Field | Value |
|------|-------|
| Source | Annex IV; Art. 21 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_REPR |

**Rule**  
This module provides boundaries used by `M_REPR` to link representativeness areas to zones, territorial units and cross-boundary contexts.

Representativeness areas may cross zone or national boundaries; such crossings shall be preserved as metadata where relevant.

**Acceptance criterion**

```text
if AREA_REPR intersects multiple zones or Member States:
    affected_territorial_objects recorded
    cross_boundary_context_available = true
```

---

### REQ-ZONE-COMPLIANCE_DOMAIN_INTERFACE

| Field | Value |
|------|-------|
| Source | Art. 12; Art. 13; Art. 14; Annex I Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_LIMITS |

**Rule**  
This module shall provide the applicable territorial domain for compliance verification, maintenance obligations, critical levels and average exposure obligations.

**Acceptance criterion**

```text
if standard_type in [limit_value, target_value]:
    compliance_domain = zone

if standard_type = critical_level:
    compliance_domain = applicable_ecosystem_or_vegetation_context

if standard_type in [average_exposure_indicator,
                     average_exposure_reduction_obligation,
                     average_exposure_concentration_objective]:
    compliance_domain = average_exposure_territorial_unit
```

---

### REQ-ZONE-ALERT_INFORMATION_DOMAIN_INTERFACE

| Field | Value |
|------|-------|
| Source | Art. 15; Art. 20; Art. 22 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_LIMITS; M_SHORT_TERM_ACTION; M_PUBLIC_INFORMATION |

**Rule**  
This module shall provide the territorial domain for alert-threshold and information-threshold exceedance or prediction outputs, including the affected population and sensitive or vulnerable groups where available.

**Acceptance criterion**

```text
if alert_or_information_threshold_exceeded_or_predicted = true:
    affected_area_geometry is not null
    affected_zone_or_units identified
    affected_population_metadata available where available
```

---

### REQ-ZONE-PLANNING_DOMAIN_INTERFACE

| Field | Value |
|------|-------|
| Source | Art. 19; Art. 20; Annex VIII Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_PLANS; M_SHORT_TERM_ACTION; M_LIMITS |

**Rule**  
This module shall provide the territorial domains for air quality plans, air quality roadmaps and short-term action plans.

Plans and roadmaps may be zone-based, territorial-unit-based or cover multiple territorial objects where integrated planning is required.

**Acceptance criterion**

```text
if air_quality_plan_trigger = true:
    plan_area = affected_zone_or_territorial_unit_or_integrated_area

if air_quality_roadmap_trigger = true:
    roadmap_area = affected_zone_or_territorial_unit_or_integrated_area

if short_term_action_plan_trigger = true:
    short_term_action_area = affected_area_or_neighbouring_zones_as_applicable
```

---

### REQ-ZONE-OZONE_TERRITORIAL_CONTEXT

| Field | Value |
|------|-------|
| Source | Art. 19(2); Art. 12; Art. 13; Annex I Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_LIMITS; M_PLANS |

**Rule**  
Where ozone target values or long-term objectives are assessed or where ozone-related plans are required, the relevant territorial unit shall be identified and linked to the zones it covers.

**Acceptance criterion**

```text
ozone_territorial_unit:
    id is not null
    geometry is not null
    covered_zones not empty
    ozone_assessment_context recorded
```

---

### REQ-ZONE-SOURCE_ATTRIBUTION_CONTEXT

| Field | Value |
|------|-------|
| Source | Art. 16; Art. 17; Art. 21 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_SOURCE_ATTRIBUTION; M_LIMITS; M_TRANSBOUNDARY |

**Rule**  
This module shall provide territorial objects required to identify zones or average exposure territorial units affected by natural sources, winter sanding/salting or transboundary contributions.

**Acceptance criterion**

```text
if source_attribution_case = true:
    affected_zones_or_AETUs identified
    source_contribution_area identified where available
    territorial_metadata_available = true
```

---

### REQ-ZONE-TRANSBOUNDARY_CONTEXT

| Field | Value |
|------|-------|
| Source | Art. 21 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_TRANSBOUNDARY; M_REPR; M_MOD |

**Rule**  
Where significant transboundary pollution contributes to exceedances, or where neighbouring zones in other Member States may be affected, this module shall preserve cross-border territorial context.

**Acceptance criterion**

```text
if transboundary_contribution_relevant = true:
    affected_member_states recorded
    affected_zones_or_territorial_units recorded
    cross_boundary_geometry available
```

---

### REQ-ZONE-PUBLIC_INFORMATION_CONTEXT

| Field | Value |
|------|-------|
| Source | Art. 22 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | interface rule |
| Dependencies | M_PUBLIC_INFORMATION |

**Rule**  
Territorial outputs used for public information shall be suitable for communicating air quality status and risks to the public.

**Acceptance criterion**

```text
if public_information_output_required = true:
    public_area_label available
    public_geometry_or_map_area available
    affected_population_metadata available where available
    sensitive_population_context available where available
```

---

### REQ-ZONE-REPORTING_CHANGES

| Field | Value |
|------|-------|
| Source | Art. 23 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory interface rule |
| Dependencies | M_REPORTING |

**Rule**  
Changes to the list and delimitation of zones or average exposure territorial units shall be made available for reporting purposes.

**Acceptance criterion**

```text
if zone_or_AETU_changed = true:
    change_record created
    old_version retained
    new_version retained
    change_effective_date recorded
    reporting_payload_ready = true
```

---

### REQ-ZONE-VERSIONING

| Field | Value |
|------|-------|
| Source | Art. 23; framework traceability rule |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_REPORTING; M_ASSESS; M_LIMITS |

**Rule**  
Territorial objects shall be versioned so that assessment, compliance, monitoring, planning and reporting outputs can be traced to the territorial boundaries in force for the relevant period.

**Acceptance criterion**

```text
territorial_object_version includes:
    object_id
    object_type
    geometry
    valid_from
    valid_to
    legal_status
    change_reason
    previous_version_id
```

---

### REQ-ZONE-UPDATE_REVIEW

| Field | Value |
|------|-------|
| Source | Art. 6; Art. 7(2); Art. 23 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_ASSESS; M_REPORTING |

**Rule**  
Territorial subdivisions shall be reviewed when required for assessment and management, including when significant changes affect population distribution, emission patterns, administrative delimitation, monitoring objectives or assessment classification.

**Acceptance criterion**

```text
zone_review_due =
    significant_population_change = true
    OR significant_emission_change = true
    OR administrative_boundary_change = true
    OR assessment_classification_review_due = true
    OR monitoring_network_change_requires_zone_review = true
```

**Note**  
The review of zone classification under Art. 7 is handled by `M_ASSESS`; this module provides and versions the territorial layer used for that review.

---

### REQ-ZONE-DATA_INTEGRITY

| Field | Value |
|------|-------|
| Source | Framework rule derived from Art. 6, Art. 23 Dir. (EU) 2024/2881 |
| Status | STABLE |
| Type | mandatory |
| Dependencies | M_REPORTING |

**Rule**  
Territorial data shall be topologically valid and traceable.

**Acceptance criterion**

```text
for each active territorial object:
    geometry_valid = true
    geometry_not_empty = true
    member_state_assigned = true
    valid_from is not null
```

```text
for zone layer:
    no_unintended_overlap = true
    no_unintended_gaps = true
```

---

## Territorial decision logic

```text
for each Member State:

    define active zone layer
    verify full territorial coverage
    verify no unintended overlap between zones

    identify agglomerations

    define average exposure territorial units
    verify NUTS basis and size constraints

    provide territorial context to:
        M_ASSESS
        M_NETWORK
        M_REPR
        M_LIMITS
        M_PLANS
        M_SHORT_TERM_ACTION
        M_TRANSBOUNDARY
        M_PUBLIC_INFORMATION
        M_REPORTING

    version all territorial objects
    report changes where required
```

---

## Interactions with other modules

- `M_ASSESS` — classifies zones and applies assessment regimes
- `M_NETWORK` — calculates minimum monitoring requirements and supersite context
- `M_REPR` — links representativeness areas to zones and cross-boundary contexts
- `M_MOD` — provides modelled areas and transboundary geometries requiring territorial attribution
- `M_LIMITS` — verifies compliance by zone or average exposure territorial unit
- `M_SOURCE_ATTRIBUTION` — uses affected territorial units for natural sources, winter sanding/salting and transboundary cases
- `M_ATTAINMENT_EXTENSION` — uses affected zones for postponement requests
- `M_PLANS` — uses zones, territorial units and integrated areas for plans and roadmaps
- `M_SHORT_TERM_ACTION` — uses affected and neighbouring areas for short-term action plans
- `M_TRANSBOUNDARY` — uses cross-border territorial context
- `M_PUBLIC_INFORMATION` — uses public-facing territorial labels and affected-area metadata
- `M_REPORTING` — reports zone and AETU lists, delimitation and changes
- `T_NUTS` — NUTS 1 and NUTS 2 geometries
- `T_ZONE_TYPES` — zone and exposure classifications

---

## Output

```text
territorial_context:
  member_state
  reference_year

  zones:
    - id
      version
      geometry
      valid_from
      valid_to
      population
      area
      zone_type
      agglomeration_status
      urban_rural_context
      legal_status

  agglomerations:
    - id
      geometry
      population
      density
      linked_zones

  average_exposure_territorial_units:
    - id
      version
      geometry
      valid_from
      valid_to
      NUTS_basis
      area
      linked_zones
      valid_for_AEI

  other_territorial_units:
    - id
      type = ozone_territorial_unit | plan_area | roadmap_area | short_term_action_area | transboundary_affected_area | reporting_area
      geometry
      linked_zones
      legal_or_operational_basis

  changes:
    - change_id
      object_id
      object_type
      previous_version_id
      new_version_id
      effective_date
      change_reason
      reporting_payload_ready
```

---

## Notes

- `M_ZONE` provides territorial domains; it does not classify zones as above or below assessment thresholds.
- Zone boundaries and assessment results must be versioned together to preserve traceability.
- Average exposure territorial units are not interchangeable with zones; they exist specifically for average exposure indicators and related obligations.
- Planning and reporting may require territorial units that aggregate or cut across zones; these should be represented explicitly rather than overloading the primary zone object.
- Cross-border geometries should be preserved where relevant for transboundary cooperation and public/reporting outputs.
