# M_PUBLIC_INFORMATION — Public information and communication

## Normative references

- Art. 22 Directive (EU) 2024/2881 — public information
- Art. 15 Directive (EU) 2024/2881 — alert and information thresholds
- Art. 19 Directive (EU) 2024/2881 — plans (public availability)
- Art. 21 Directive (EU) 2024/2881 — transboundary context (communication)
- Art. 23 Directive (EU) 2024/2881 — reporting interfaces
- Annex I — thresholds and standards

---

## Description

This module defines the **requirements for public communication of air quality information**, ensuring that information provided to the public is:

```text
accurate
timely
clear
accessible
traceable
```

It transforms technical outputs into **public-facing information** for:

- current air quality conditions
- exceedances of thresholds
- forecasts and short-term risks
- health implications
- source context and transboundary influence

This module does not generate measurements or validate data — it consumes validated outputs.

---

## Scope

Applies to information concerning:

```text
limit values
target values
alert thresholds
information thresholds
average exposure
forecasts and alerts
```

---

## Definitions

```text
PUBLIC_OUTPUT =
information communicated to the public
```

```text
ALERT_EVENT =
exceedance or predicted exceedance of alert threshold
```

```text
INFO_EVENT =
exceedance or predicted exceedance of information threshold
```

```text
PUBLIC_DATA_READY =
true when data is validated and suitable for release
```

---

## Normative requirements

---

### REQ-PUB-DATA_VALIDITY

**Rule**  
Only validated data may be communicated.

**Acceptance**

```text
PUBLIC_DATA_READY = M_DATA_QUALITY.DATA_VALID = true
```

---

### REQ-PUB-TIMELINESS

**Rule**  
Information must be provided without delay.

**Acceptance**

```text
publication_time - detection_time <= acceptable_delay
```

---

### REQ-PUB-CONTENT

**Rule**  
Public information shall include key elements.

**Acceptance**

```text
PUBLIC_OUTPUT includes:
    pollutant
    value
    threshold
    location
    time
```

---

### REQ-PUB-ALERT

**Rule**  
Alert threshold exceedances must be communicated immediately.

**Acceptance**

```text
if ALERT_EVENT = true:
    alert_message_published = true
```

---

### REQ-PUB-INFORMATION_THRESHOLD

**Rule**  
Information threshold exceedances must be communicated.

**Acceptance**

```text
if INFO_EVENT = true:
    information_message_published = true
```

---

### REQ-PUB-FORECAST

**Rule**  
Forecasts must be communicated when relevant.

**Acceptance**

```text
if forecast_available = true:
    forecast_published = true
```

---

### REQ-PUB-HEALTH_INFO

**Rule**  
Health-related advice must be included.

**Acceptance**

```text
health_guidance_included = true
```

---

### REQ-PUB-SENSITIVE_GROUPS

**Rule**  
Information must address sensitive populations.

**Acceptance**

```text
if vulnerable_groups_present:
    tailored_health_advice = true
```

---

### REQ-PUB-TRANSPARENCY

**Rule**  
Methods and data sources must be transparent.

**Acceptance**

```text
data_source_available = true
method_explained = true
```

---

### REQ-PUB-TRANSBOUNDARY_CONTEXT

**Rule**  
Where relevant, cross-border pollution must be communicated.

**Acceptance**

```text
if TRANSBOUNDARY_CASE = true:
    source_origin_disclosed = true
```

---

### REQ-PUB-PLAN_ACCESS

**Rule**  
Air quality plans and roadmaps must be publicly accessible.

**Acceptance**

```text
if plan_exists:
    plan_publicly_available = true
```

---

### REQ-PUB-UPDATE

**Rule**  
Information must be updated as conditions change.

**Acceptance**

```text
if data_updated:
    public_output_updated = true
```

---

## Decision logic

```text
if data validated:
    publish current levels

if alert or forecast:
    publish alerts immediately

if plans exist:
    publish access

include health info and transparency
```

---

## Output

```text
public_information:
  pollutant
  value
  threshold
  location
  timestamp

  alerts:
    alert_active
    information_active

  forecast:
    available
    summary

  health:
    guidance
    vulnerable_groups

  context:
    source
    transboundary

  metadata:
    data_source
    method
```

---

## Notes

- Focus is on clarity and accessibility.
- Interprets technical results for the public.
- Must be synchronized with real-time data and forecasts.
