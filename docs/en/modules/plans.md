# M_PLANS — Air quality plans, roadmaps and short-term action plans

## Normative references

- Art. 19 Directive (EU) 2024/2881 — air quality plans and roadmaps
- Art. 20 Directive (EU) 2024/2881 — short-term action plans
- Art. 12 Directive (EU) 2024/2881 — maintenance of air quality
- Art. 13 Directive (EU) 2024/2881 — limit values and exposure obligations
- Art. 15 Directive (EU) 2024/2881 — alert thresholds
- Art. 18 Directive (EU) 2024/2881 — postponement of attainment deadlines
- Annex VIII — requirements for plans and roadmaps

---

## Description

This module defines the **planning obligations and instruments** required under the Directive, including:

```text
air quality plans
air quality roadmaps
short-term action plans
```

It determines when planning is required, what must be included, and how plans interact with:

- compliance status (`M_LIMITS`)
- exposure obligations (`M_EXPOSURE`)
- source attribution (`M_SOURCE_ATTRIBUTION`)
- modelling and projections (`M_MOD`)
- postponement requests (`M_ATTAINMENT_EXTENSION`)

This module does not assess compliance or calculate exceedances; it consumes those outputs.

---

## Scope

Applies where:

```text
limit value exceedance
exposure obligation not achieved
alert threshold exceeded or risked
```

---

## Definitions

```text
PLAN_TYPE =
    air_quality_plan
    air_quality_roadmap
    short_term_action_plan
```

```text
PLAN_AREA =
zone | AETU | multi-zone | transboundary area
```

```text
PLAN_TRIGGER =
condition requiring plan or update
```

---

## Normative requirements

---

### REQ-PLAN-TRIGGER-LIMIT

**Rule**  
An air quality plan is required where limit values are exceeded.

**Acceptance**

```text
if limit_value_exceedance = true:
    PLAN_TRIGGER = air_quality_plan
```

---

### REQ-PLAN-TRIGGER-EXPOSURE

**Rule**  
A plan is required where exposure reduction obligation is not achieved.

**Acceptance**

```text
if exposure_reduction_obligation_status = not_achieved:
    PLAN_TRIGGER = air_quality_plan
```

---

### REQ-PLAN-TRIGGER-ROADMAP

**Rule**  
Roadmaps are required where future compliance must be demonstrated.

**Acceptance**

```text
if future_non_compliance_risk = true:
    PLAN_TRIGGER = air_quality_roadmap
```

---

### REQ-PLAN-TRIGGER-SHORT_TERM

**Rule**  
Short-term action plans are required where alert thresholds are exceeded or risked.

**Acceptance**

```text
if alert_exceedance_or_risk = true:
    PLAN_TRIGGER = short_term_action_plan
```

---

### REQ-PLAN-CONTENT

**Rule**  
Plans must include measures to keep exceedance period as short as possible.

**Acceptance**

```text
plan_contains:
    measures
    timeline
    responsible_authorities
    implementation_schedule
```

---

### REQ-PLAN-SOURCE_LINK

**Rule**  
Plans must include source contribution analysis.

**Acceptance**

```text
if plan_exists:
    source_profile_available = true
```

---

### REQ-PLAN-MODELLING

**Rule**  
Plans and roadmaps must include modelling projections.

**Acceptance**

```text
MODEL_VALID = true
AND projection_available = true
```

---

### REQ-PLAN-TIMEFRAME

**Rule**  
Measures must target attainment deadlines.

**Acceptance**

```text
plan_timeline aligned_with ATTAINMENT_DEADLINE
```

---

### REQ-PLAN-EXCEPTION-WINTER

**Rule**  
Plans may be omitted for PM10 exceedances caused solely by winter sanding/salting.

**Acceptance**

```text
if winter_sanding_salting_only = true:
    plan_not_required = true
```

---

### REQ-PLAN-INTERACTION-EXTENSION

**Rule**  
Plans must support postponement requests.

**Acceptance**

```text
if postponement_requested = true:
    plan_supports_extension = true
```

---

### REQ-PLAN-UPDATE

**Rule**  
Plans must be updated when conditions change.

**Acceptance**

```text
if exceedance_changes OR measures_fail:
    plan_update_required = true
```

---

## Decision logic

```text
if trigger_detected:
    identify PLAN_TYPE
    define PLAN_AREA
    include source analysis
    include modelling
    include measures
```

---

## Output

```text
plan_status:
  plan_type
  plan_area

  trigger:
    type
    source

  content:
    measures
    timeline
    authorities

  modelling:
    projections
    validity

  source:
    main_sources

  result:
    plan_required
    plan_exists
    plan_valid
```

---

## Notes

- Central module linking technical outputs to policy actions.
- Always interacts with all core modules.
- Essential for both compliance and extension mechanisms.
