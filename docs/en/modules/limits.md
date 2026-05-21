# M_LIMITS — Compliance and exceedance verification

## Normative references

- Directive (EU) 2024/2881, Annex I
- Art. 8 Directive (EU) 2024/2881
- Art. 12 Directive (EU) 2024/2881
- Art. 13 Directive (EU) 2024/2881
- Art. 14 Directive (EU) 2024/2881
- Art. 15 Directive (EU) 2024/2881
- Art. 16 Directive (EU) 2024/2881
- Art. 17 Directive (EU) 2024/2881
- Art. 18 Directive (EU) 2024/2881
- Annex V, data quality objectives

---

## Description

This module verifies the **regulatory status of pollutant levels**
against the values and thresholds defined in Annex I.

It determines, for each pollutant, metric, zone or territorial unit:

- compliance with limit values
- compliance with target values
- compliance with critical levels
- attainment or exceedance of alert and information thresholds
- attainment of average exposure indicators and reduction obligations
- maintenance obligations where air quality is already below regulatory values
- legal qualification of exceedances
- whether an exceedance is omitted for the purposes of the Directive
- whether an exceedance is covered by a valid postponement of the attainment deadline

The module does **not**:

- define the monitoring network
- validate raw data quality
- attribute sources in detail
- establish air quality plans
- evaluate the full procedural validity of time extensions

Those functions belong to dedicated modules.

---

## Scope

This module applies to:

- limit values
- target values
- critical levels
- average exposure indicators
- average exposure reduction obligations
- average exposure concentration objectives
- alert thresholds
- information thresholds

The module distinguishes between:

```text
technical_exceedance
regulatory_exceedance
exceedance_omitted_for_directive_purposes
exceedance_relevant_for_planning
non_compliance_under_valid_postponement
````

***

## Definitions

```text
p = pollutant
z = zone
u = average exposure territorial unit
x = location
t = time
m = regulatory metric
```

```text
C(p, x, t) =
concentration of pollutant p
at location x and time t
```

```text
VALUE(p, z, m, period) =
regulatory value calculated for pollutant p,
zone z, metric m and assessment period
```

```text
STANDARD(p, m) =
applicable regulatory value from T_LIMIT_VALUES
or T_TARGET_VALUES
or T_CRITICAL_LEVELS
or T_ALERT_THRESHOLDS
```

```text
LV(p, m) =
limit value for pollutant p and metric m
```

```text
TV(p, m) =
target value for pollutant p and metric m
```

```text
CL(p, m) =
critical level for pollutant p and metric m
```

```text
AEI(p, u, y) =
average exposure indicator for pollutant p
in territorial unit u and year y
```

```text
EXCEEDANCE(p, z, m, period) =
VALUE(p, z, m, period) > STANDARD(p, m)
```

```text
COMPLIANCE_STATUS =
compliant
| non_compliant
| non_compliant_under_valid_postponement
| exceedance_omitted_natural_sources
| exceedance_attributable_to_winter_sanding_or_salting
| not_assessable
```

***

## Normative requirements

***

### REQ-LIMITS-DATA_VALIDITY

| Field        | Value                       |
| ------------ | --------------------------- |
| Source       | Annex V Dir. (EU) 2024/2881 |
| Status       | STABLE                      |
| Type         | mandatory                   |
| Dependencies | M_DATA_QUALITY              |

**Rule**  
Only data satisfying applicable data quality requirements may be used for compliance and exceedance verification.

**Acceptance criterion**

```text
use only data where DATA_VALID = true
```

**Exception / qualification**  
Where regulatory rules allow assessment with incomplete coverage, the result shall be marked as based on incomplete but conclusive data.

```text
if DATA_VALID = false
AND incomplete_data_conclusive = true:
    assessment_status = conclusive_with_incomplete_coverage
```

***

### REQ-LIMITS-SPATIAL_INTEGRATION

| Field        | Value                                        |
| ------------ | -------------------------------------------- |
| Source       | Art. 8 Dir. (EU) 2024/2881                   |
| Status       | STABLE                                       |
| Type         | mandatory                                    |
| Dependencies | M_ASSESS; M_REPR; M_MOD; M_DATA_QUALITY |

**Rule**  
Compliance verification shall take into account fixed measurements, indicative measurements and modelling applications according to the assessment regime and spatial representativeness.

Where modelling applications or indicative measurements are used under the applicable assessment regime, their results shall be considered in the evaluation of pollutant levels.

**Acceptance criterion**

```text
if valid_fixed_measurement_available
AND x ∈ AREA_REPR(fixed_measurement):
    C_assessment(p,x,t) = fixed_measurement_value

else if valid_indicative_measurement_available:
    C_assessment(p,x,t) = indicative_measurement_value

else if VALID_MODEL = true:
    C_assessment(p,x,t) = modelled_value

else:
    C_assessment(p,x,t) = undefined
```

***

### REQ-LIMITS-MODELLED_EXCEEDANCE

| Field        | Value                                    |
| ------------ | ---------------------------------------- |
| Source       | Art. 8(5), Art. 8(6) Dir. (EU) 2024/2881 |
| Status       | STABLE                                   |
| Type         | mandatory                                |
| Dependencies | M_ASSESS; M_REPR; M_MOD; M_NETWORK   |

**Rule**  
A modelled exceedance is relevant for assessment unless it can be evaluated as a non-exceedance because fixed measurements with spatial representativeness covering the modelled exceedance area are available.

Where no additional fixed or indicative measurements are conducted after a modelled exceedance not covered by existing fixed measurements, the modelled exceedance shall be used for air quality assessment.

**Acceptance criterion**

```text
if modelled_exceedance = true:

    if fixed_measurements_cover_modelled_exceedance_area = true:
        modelled_exceedance_status =
            may_be_evaluated_as_non_exceedance

    else if additional_valid_measurements_available = true:
        modelled_exceedance_status =
            evaluated_with_additional_measurements

    else:
        modelled_exceedance_status =
            used_for_assessment
```

***

### REQ-LIMITS-MEAN_COMPLIANCE

| Field        | Value                                |
| ------------ | ------------------------------------ |
| Source       | Art. 13; Annex I Dir. (EU) 2024/2881 |
| Status       | STABLE                               |
| Type         | mandatory                            |
| Dependencies | T_LIMIT_VALUES; T_TARGET_VALUES  |

**Rule**  
For regulatory metrics assessed as a mean, compliance is verified by comparing the calculated mean with the applicable regulatory value.

**Acceptance criterion**

```text
COMPLIANT_MEAN(p,z,m) =
MEAN_VALUE(p,z,m) ≤ STANDARD(p,m)
```

***

### REQ-LIMITS-EXCEEDANCE_COUNT_COMPLIANCE

| Field        | Value                                |
| ------------ | ------------------------------------ |
| Source       | Art. 13; Annex I Dir. (EU) 2024/2881 |
| Status       | STABLE                               |
| Type         | mandatory                            |
| Dependencies | T_LIMIT_VALUES; T_TARGET_VALUES      |

**Rule**  
For metrics allowing a maximum number of exceedances, compliance is verified by comparing the number of exceedances with the permitted maximum.

**Acceptance criterion**

```text
COMPLIANT_EXCEEDANCE_COUNT(p,z,m) =
COUNT{ t | VALUE(p,z,m,t) > STANDARD(p,m) }
≤ MAX_ALLOWED_EXCEEDANCES(p,m)
```

***

### REQ-LIMITS-OVERALL_COMPLIANCE

| Field        | Value                                                                 |
| ------------ | --------------------------------------------------------------------- |
| Source       | Art. 13; Annex I Dir. (EU) 2024/2881                                  |
| Status       | STABLE                                                                |
| Type         | mandatory                                                             |
| Dependencies | REQ-LIMITS-MEAN_COMPLIANCE; REQ-LIMITS-EXCEEDANCE_COUNT_COMPLIANCE |

**Rule**  
Overall compliance for a pollutant and metric is achieved only if all applicable regulatory conditions are satisfied.

**Acceptance criterion**

```text
COMPLIANT(p,z,m) =
all applicable compliance tests are true
```

```text
if COMPLIANT(p,z,m) = true:
    compliance_status = compliant
else:
    compliance_status = non_compliant
```

***

### REQ-LIMITS-MAINTENANCE_OBLIGATION

| Field        | Value                                          |
| ------------ | ---------------------------------------------- |
| Source       | Art. 12 Dir. (EU) 2024/2881                    |
| Status       | STABLE                                         |
| Type         | mandatory                                      |
| Dependencies | T_LIMIT_VALUES; T_TARGET_VALUES; M_ASSESS |

**Rule**  
Where pollutant levels are below applicable limit values or target values, the system records a maintenance obligation.

The obligation is not merely to detect future exceedances but to maintain levels below the applicable values.

**Acceptance criterion**

```text
if VALUE(p,z,m,period) < STANDARD(p,m):
    maintenance_obligation = true
```

***

### REQ-LIMITS-LIMIT_VALUES

| Field        | Value                          |
| ------------ | ------------------------------ |
| Source       | Art. 13(1); Annex I, Section 1 |
| Status       | STABLE                         |
| Type         | mandatory                      |
| Dependencies | T_LIMIT_VALUES               |

**Rule**  
Pollutant levels shall not exceed the applicable limit values.

**Acceptance criterion**

```text
if VALUE(p,z,m,period) > LV(p,m):
    limit_value_exceedance = true
    compliance_status = non_compliant
else:
    limit_value_exceedance = false
```

***

### REQ-LIMITS-TARGET_VALUES

| Field        | Value               |
| ------------ | ------------------- |
| Source       | Art. 13(2); Annex I |
| Status       | STABLE              |
| Type         | mandatory           |
| Dependencies | T_TARGET_VALUES   |

**Rule**  
Pollutant levels shall not exceed the applicable target values.

**Acceptance criterion**

```text
if VALUE(p,z,m,period) > TV(p,m):
    target_value_exceedance = true
    compliance_status = non_compliant
else:
    target_value_exceedance = false
```

***

### REQ-LIMITS-AVERAGE_EXPOSURE_INDICATORS

| Field        | Value                                                  |
| ------------ | ------------------------------------------------------ |
| Source       | Art. 12(3), Art. 13(3), Art. 13(5); Annex I, Section 5 |
| Status       | STABLE                                                 |
| Type         | mandatory                                              |
| Dependencies | M_ZONE; T_AVERAGE_EXPOSURE; M_DATA_QUALITY        |

**Rule**  
Average exposure indicators for PM2.5 and NO2 shall be assessed in average exposure territorial units.

Where applicable, average exposure reduction obligations and average exposure concentration objectives shall be verified.

**Acceptance criterion**

```text
if AEI(p,u,y) ≤ AEI_OBJECTIVE(p):
    exposure_objective_status = attained
else:
    exposure_objective_status = not_attained
```

```text
if AEI_REDUCTION(p,u,period) ≥ REQUIRED_REDUCTION(p,u):
    exposure_reduction_status = compliant
else:
    exposure_reduction_status = non_compliant
```

***

### REQ-LIMITS-CRITICAL_LEVELS

| Field        | Value               |
| ------------ | ------------------- |
| Source       | Art. 14; Annex I    |
| Status       | STABLE              |
| Type         | mandatory           |
| Dependencies | T_CRITICAL_LEVELS |

**Rule**  
Critical levels for the protection of vegetation and natural ecosystems shall be complied with where applicable.

**Acceptance criterion**

```text
if VALUE(p,z,m,period) > CL(p,m):
    critical_level_exceedance = true
else:
    critical_level_exceedance = false
```

***

### REQ-LIMITS-ALERT_INFORMATION_THRESHOLDS

| Field        | Value                                                     |
| ------------ | --------------------------------------------------------- |
| Source       | Art. 15; Annex I, Section 4                               |
| Status       | STABLE                                                    |
| Type         | mandatory                                                 |
| Dependencies | T_ALERT_THRESHOLDS; M_FORECAST; M_PUBLIC_INFORMATION |

**Rule**  
Where alert or information thresholds are exceeded or predicted to be exceeded, the system records the exceedance and triggers the relevant downstream obligations.

Alert threshold exceedance or predicted exceedance may trigger emergency measures under short-term action plans.

Alert or information threshold exceedance or predicted exceedance triggers public information obligations.

**Acceptance criterion**

```text
if VALUE(p,z,m,t) > ALERT_THRESHOLD(p,m)
OR predicted_value(p,z,m,t) > ALERT_THRESHOLD(p,m):
    alert_threshold_status = exceeded_or_predicted
    short_term_action_plan_trigger = true
    public_information_trigger = true
```

```text
if VALUE(p,z,m,t) > INFORMATION_THRESHOLD(p,m)
OR predicted_value(p,z,m,t) > INFORMATION_THRESHOLD(p,m):
    information_threshold_status = exceeded_or_predicted
    public_information_trigger = true
```

**Note**  
This module records the trigger. The content of public information and emergency measures is handled by dedicated modules.

***

### REQ-LIMITS-NATURAL_SOURCES_CLASSIFICATION

| Field        | Value                                      |
| ------------ | ------------------------------------------ |
| Source       | Art. 16(1), Art. 16(2) Dir. (EU) 2024/2881 |
| Status       | STABLE                                     |
| Type         | conditional                                |
| Dependencies | M_SOURCE_ATTRIBUTION; M_REPORTING       |

**Rule**  
Zones and average exposure territorial units may be classified as affected by exceedances attributable to natural sources.

The Member State shall provide:

* lists of affected zones or territorial units
* information on concentrations
* information on sources
* evidence of attribution to natural sources

**Acceptance criterion**

```text
if natural_source_attribution = true
AND required_information_submitted = true
AND evidence_submitted = true:
    natural_source_case = true
else:
    natural_source_case = false
```

***

### REQ-LIMITS-NATURAL_SOURCES_OMISSION

| Field        | Value                                |
| ------------ | ------------------------------------ |
| Source       | Art. 16(3) Dir. (EU) 2024/2881       |
| Status       | STABLE                               |
| Type         | conditional                          |
| Dependencies | M_SOURCE_ATTRIBUTION; M_REPORTING |

**Rule**  
Where the Commission has been informed of an exceedance attributable to natural sources, the exceedance is omitted for the purposes of the Directive unless the Commission considers the evidence insufficient.

**Acceptance criterion**

```text
if natural_source_case = true
AND commission_evidence_insufficient != true:
    exceedance_status = exceedance_omitted_natural_sources
    omitted_for_directive_purposes = true
else:
    omitted_for_directive_purposes = false
```

**Note**  
This rule applies to the legal effect of the exceedance. It does not delete the underlying measured or modelled concentration.

***

### REQ-LIMITS-WINTER_SANDING_SALTING_IDENTIFICATION

| Field        | Value                                      |
| ------------ | ------------------------------------------ |
| Source       | Art. 17(1), Art. 17(2) Dir. (EU) 2024/2881 |
| Status       | STABLE                                     |
| Type         | conditional                                |
| Dependencies | M_SOURCE_ATTRIBUTION; M_REPORTING       |

**Rule**  
For a given year, zones may be identified where PM10 limit values are exceeded due to re-suspension of particulates following winter sanding or winter salting of roads.

For such zones, the required information and evidence shall be provided.

**Acceptance criterion**

```text
if pollutant = PM10
AND limit_value_exceedance = true
AND attribution = winter_sanding_or_salting
AND required_information_submitted = true
AND evidence_submitted = true:
    winter_sanding_salting_case = true
else:
    winter_sanding_salting_case = false
```

***

### REQ-LIMITS-WINTER_SANDING_SALTING_EFFECT

| Field        | Value                            |
| ------------ | -------------------------------- |
| Source       | Art. 17(3) Dir. (EU) 2024/2881   |
| Status       | STABLE                           |
| Type         | conditional                      |
| Dependencies | M_SOURCE_ATTRIBUTION; M_PLANS |

**Rule**  
Where PM10 exceedances are attributable to winter sanding or winter salting of roads, the establishment of an air quality plan may be omitted for those exceedances.

This does not apply to exceedances attributable to PM10 sources other than winter sanding or winter salting.

**Acceptance criterion**

```text
if winter_sanding_salting_case = true
AND other_PM10_sources_cause_exceedance != true:
    air_quality_plan_required_for_this_exceedance = false
    compliance_status =
        exceedance_attributable_to_winter_sanding_or_salting
else if limit_value_exceedance = true:
    air_quality_plan_required_for_this_exceedance = true
```

**Note**  
This rule affects the planning consequence of the exceedance. It does not automatically make the zone compliant.

***

### REQ-LIMITS-POSTPONED_DEADLINE_STATUS

| Field        | Value                       |
| ------------ | --------------------------- |
| Source       | Art. 18 Dir. (EU) 2024/2881 |
| Status       | STABLE                      |
| Type         | conditional                 |
| Dependencies | M_ATTAINMENT_EXTENSION    |

**Rule**  
Where an attainment deadline for a limit value has been validly postponed, an exceedance during the valid postponement period shall be recorded as non-compliance under valid postponement rather than ordinary non-compliance.

**Acceptance criterion**

```text
if limit_value_exceedance = true
AND valid_postponement = true
AND assessment_date ≤ postponed_deadline:
    compliance_status = non_compliant_under_valid_postponement
else if limit_value_exceedance = true:
    compliance_status = non_compliant
```

**Note**  
The full evaluation of postponement eligibility, roadmap conditions, projections, notification deadlines and Commission objection status is handled by `M_ATTAINMENT_EXTENSION`.

***

### REQ-LIMITS-POSTPONEMENT_BOUNDARIES

| Field        | Value                                                  |
| ------------ | ------------------------------------------------------ |
| Source       | Art. 18(1), Art. 18(2), Art. 18(4) Dir. (EU) 2024/2881 |
| Status       | STABLE                                                 |
| Type         | reference                                              |
| Dependencies | M_ATTAINMENT_EXTENSION                               |

**Rule**  
The module shall not itself grant or validate a postponement.

It shall only consume the result of `M_ATTAINMENT_EXTENSION`.

**Acceptance criterion**

```text
valid_postponement =
M_ATTAINMENT_EXTENSION.status = valid
```

```text
postponement_status ∈
    pending
    valid
    objected
    invalid
    expired
```

***

### REQ-LIMITS-STATUS_PRECEDENCE

| Field        | Value                                                                                                                      |
| ------------ | -------------------------------------------------------------------------------------------------------------------------- |
| Source       | System rule derived from Art. 13, Art. 16, Art. 17, Art. 18                                                                |
| Status       | STABLE                                                                                                                     |
| Type         | mandatory                                                                                                                  |
| Dependencies | REQ-LIMITS-NATURAL_SOURCES_OMISSION; REQ-LIMITS-WINTER_SANDING_SALTING_EFFECT; REQ-LIMITS-POSTPONED_DEADLINE_STATUS |

**Rule**  
Where multiple legal qualifications apply, the compliance status shall be assigned according to the following precedence.

**Precedence order**

```text
1. not_assessable
2. exceedance_omitted_natural_sources
3. non_compliant_under_valid_postponement
4. exceedance_attributable_to_winter_sanding_or_salting
5. non_compliant
6. compliant
```

**Acceptance criterion**

```text
if assessment_data_sufficient = false:
    compliance_status = not_assessable

else if omitted_for_directive_purposes = true:
    compliance_status = exceedance_omitted_natural_sources

else if limit_value_exceedance = true
AND valid_postponement = true:
    compliance_status = non_compliant_under_valid_postponement

else if winter_sanding_salting_case = true:
    compliance_status = exceedance_attributable_to_winter_sanding_or_salting

else if any_applicable_exceedance = true:
    compliance_status = non_compliant

else:
    compliance_status = compliant
```

**Note**  
The precedence rule is a framework rule. It does not alter the substantive legal conditions of the underlying provisions.

***

## Interactions with other modules

* `M_ZONE` — provides zones and average exposure territorial units
* `M_ASSESS` — defines the assessment regime and applicable assessment methods
* `M_NETWORK` — provides monitoring network context
* `M_DATA_QUALITY` — validates data used in compliance verification
* `M_REPR` — defines spatial representativeness of measurements
* `M_MOD` — provides modelled concentration fields and modelled exceedances
* `M_SOURCE_ATTRIBUTION` — determines natural source and winter sanding/salting attribution
* `M_ATTAINMENT_EXTENSION` — evaluates validity of Art. 18 postponements
* `M_PLANS` — determines planning consequences under Art. 19 and Art. 20
* `M_REPORTING` — handles reporting to the Commission and public information flows

***

## Output

```text
compliance_result:
  zone_id
  territorial_unit_id
  pollutant
  metric
  assessment_period

  value:
    calculated_value
    standard_value
    standard_type =
      limit_value
      target_value
      critical_level
      average_exposure_objective
      alert_threshold
      information_threshold

  assessment_basis:
    fixed_measurements_used
    indicative_measurements_used
    modelling_used
    spatial_representativeness_applied
    data_quality_status

  exceedance:
    technical_exceedance
    regulatory_exceedance
    exceedance_count
    allowed_exceedances
    modelled_exceedance_status

  adjustments:
    natural_sources_case
    omitted_for_directive_purposes
    winter_sanding_salting_case
    valid_postponement
    postponed_deadline

  obligations:
    maintenance_obligation
    public_information_trigger
    short_term_action_plan_trigger
    air_quality_plan_required

  compliance_status:
    compliant
    non_compliant
    non_compliant_under_valid_postponement
    exceedance_omitted_natural_sources
    exceedance_attributable_to_winter_sanding_or_salting
    not_assessable
```

***

## Notes

* The module verifies compliance and qualifies exceedances.
* It does not establish air quality plans.
* It does not validate Art. 18 postponements procedurally.
* It does not itself attribute pollutant sources.
* Natural-source omission and winter sanding/salting cases must preserve the original measured or modelled concentration.
* A valid postponement does not make the pollutant level compliant; it changes the legal status of non-compliance during the postponement period.
