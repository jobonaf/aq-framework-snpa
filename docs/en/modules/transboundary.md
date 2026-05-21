# M_TRANSBOUNDARY — Transboundary air pollution cooperation and coordination

## Normative references

- Art. 21 Directive (EU) 2024/2881 — transboundary air pollution
- Art. 18 Directive (EU) 2024/2881 — postponement of attainment deadlines (transboundary grounds)
- Art. 19 Directive (EU) 2024/2881 — air quality plans and roadmaps
- Art. 8 Directive (EU) 2024/2881 — modelling applications
- Art. 22–23 Directive (EU) 2024/2881 — public information and reporting

---

## Description

This module governs **transboundary air pollution cases** where pollution originating in one Member State or third country affects air quality in another.

It defines:

- identification of transboundary cases
- cooperation and coordination triggers between Member States
- data-sharing and modelling requirements
- interaction with planning, compliance and postponement

This module does not calculate concentrations or validate models — it coordinates how transboundary evidence is used.

---

## Scope

Applies when:

```text
pollution in one territory contributes significantly to exceedance in another
```

Covers:

```text
cross-border zones
regional pollution transport
multi-country exceedance areas
```

---

## Definitions

```text
TRANSBOUNDARY_CASE =
case where external sources significantly influence air quality
```

```text
AFFECTED_STATE =
Member State experiencing exceedance
```

```text
SOURCE_STATE =
Member State or third country contributing pollution
```

```text
COOPERATION_REQUIRED =
true when cross-border contribution is significant
```

---

## Normative requirements

---

### REQ-TRANS-IDENTIFICATION

**Rule**  
Transboundary cases shall be identified where significant contribution exists.

**Acceptance**

```text
if source_contribution_from_external_origin > threshold:
    TRANSBOUNDARY_CASE = true
```

---

### REQ-TRANS-COOPERATION

**Rule**  
Member States shall cooperate where transboundary pollution occurs.

**Acceptance**

```text
if TRANSBOUNDARY_CASE = true:
    COOPERATION_REQUIRED = true
```

---

### REQ-TRANS-DATA_SHARING

**Rule**  
Relevant data and modelling results must be shared.

**Acceptance**

```text
share:
    monitoring data
    modelling outputs
    source attribution
```

---

### REQ-TRANS-MODELLING

**Rule**  
Validated models shall be used to assess transport and contribution.

**Acceptance**

```text
MODEL_VALID = true
AND cross_border_modelling_available = true
```

---

### REQ-TRANS-PLANNING_LINK

**Rule**  
Transboundary contributions must be considered in plans.

**Acceptance**

```text
if TRANSBOUNDARY_CASE = true:
    plan_includes_transboundary_context = true
```

---

### REQ-TRANS-EXTENSION_LINK

**Rule**  
Transboundary contribution may justify postponement.

**Acceptance**

```text
if postponement_requested:
    transboundary_evidence_required = true
```

---

### REQ-TRANS-PUBLIC_INFORMATION

**Rule**  
Public information must include cross-border context where relevant.

**Acceptance**

```text
if TRANSBOUNDARY_CASE = true:
    public_information_includes_source = true
```

---

### REQ-TRANS-REPORTING

**Rule**  
Transboundary cases must be reported with evidence.

**Acceptance**

```text
report includes:
    affected_area
    source_country
    contribution
```

---

### REQ-TRANS-LIMIT_BOUNDARY

**Rule**  
This module does not assign responsibility or enforce measures.

**Acceptance**

```text
responsibility_assignment NOT performed
```

---

## Decision logic

```text
if transboundary contribution detected:
    flag TRANSBOUNDARY_CASE
    trigger cooperation
    share data
    integrate into plans
    support extension if requested
```

---

## Output

```text
transboundary_status:
  affected_state
  source_state

  contribution:
    estimated_value
    uncertainty

  cooperation:
    required
    initiated

  modelling:
    model_used
    valid

  downstream:
    plan_integration
    extension_support
    reporting_ready
```

---

## Notes

- Focus on coordination, not attribution (handled by M_SOURCE_ATTRIBUTION).
- Critical for completeness of plans and postponement justification.
- Relies heavily on modelling and international cooperation.
