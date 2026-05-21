# Introduction

## Purpose

This document introduces the **AQ Framework — SNPA**, a formal and computable technical specification for implementing Directive (EU) 2024/2881 on ambient air quality.

The repository is available on GitHub:

<https://github.com/jobonaf/aq-framework-snpa>

The purpose of the framework is to translate regulatory requirements into a structure that is:

- explicit;
- modular;
- formal;
- traceable;
- implementable;
- suitable for automation and decision support.

The framework is designed to support both expert interpretation and software implementation of the Directive.

---

## Context

Directive (EU) 2024/2881 introduces a more integrated approach to ambient air quality assessment and management.

Key features requiring formalisation include:

- the integration of fixed measurements, indicative measurements, modelling applications and objective estimation;
- the central role of spatial representativeness;
- stricter and purpose-specific model validation requirements;
- the use of modelling for spatial distribution, hotspots, forecasts and projections;
- revised air quality standards and 2030-oriented obligations;
- average exposure indicators and exposure reduction obligations;
- natural-source and winter sanding/salting attribution rules;
- transboundary pollution cooperation;
- air quality plans, roadmaps and short-term action plans;
- public information and regulatory reporting.

These elements require a framework that can represent not only technical assessment, but also the legal consequences, evidence requirements, planning triggers and reporting flows connected to the assessment process.

---

## Architectural approach

The framework adopts a **layered regulatory architecture**.

It is not a simple linear pipeline. Instead, it separates the Directive into interoperable modules that exchange structured outputs.

The main architectural layers are:

1. territorial context;
2. assessment regime;
3. monitoring and data validity;
4. modelling and spatial representativeness;
5. compliance and exposure assessment;
6. source attribution;
7. planning and attainment-deadline extension;
8. transboundary coordination;
9. public information and reporting.

See also:

- [Framework architecture](architecture.md)

---

## Design principles

### Modular design

Each regulatory function is represented by a dedicated `M_*` module.

This separation avoids mixing assessment, evidence, compliance, planning and reporting logic in a single component.

### Logic/data separation

The framework distinguishes between:

- **modules (`M_*`)**, which contain regulatory logic, conditions, dependencies, legal-effect boundaries and outputs;
- **tables (`T_*`)**, which contain normative parameters, thresholds, values, tolerances and controlled vocabularies.

Only existing table files are linked in the navigation documents. Additional planned tables may be referenced conceptually in module dependencies, but are listed separately until the corresponding files are created.

### Purpose-specific validity

Validity is treated as purpose-specific.

A dataset, model or representativeness area is not simply valid or invalid in absolute terms. It is valid for a defined regulatory purpose.

### Legal-effect boundaries

The framework distinguishes evidence production from final legal or institutional decisions.

Examples:

- `M_SOURCE_ATTRIBUTION` can identify a natural-source case, but it does not itself omit an exceedance for the purposes of the Directive.
- `M_ATTAINMENT_EXTENSION` can determine whether a postponement request is supported, but it does not grant the postponement.
- `M_REPORTING` reports compliance status but does not recalculate compliance.

### Traceability and auditability

All regulatory outputs should be traceable to territorial versions, data versions, model versions, methods, validation status, assessment periods and reporting packages.

---

## Role of modelling

Modelling plays several distinct roles in the framework.

It may be used for:

- assessment support;
- spatial distribution of pollutant concentrations;
- hotspot identification;
- modelled exceedance area delineation;
- spatial representativeness analysis;
- monitoring-network reduction support;
- relocation support;
- alert and information threshold forecasts;
- projections for plans, roadmaps and attainment extensions;
- transboundary contribution assessment;
- source attribution support.

Because these roles have different regulatory consequences, modelling is validated through `M_MODEL_QA` according to its intended purpose and used through `M_MOD`.

---

## Role of spatial representativeness

Spatial representativeness connects point measurements to territory.

It determines:

- where a fixed measurement is spatially valid;
- whether a modelled exceedance area is covered by fixed measurements;
- whether additional monitoring may be required;
- whether network reduction remains spatially adequate;
- whether relocation of a sampling point creates unacceptable coverage loss.

Spatial representativeness is handled by `M_REPR` and interacts closely with `M_NETWORK`, `M_MOD`, `M_LIMITS` and `M_ASSESS`.

---

## Content overview

## Logical modules

The framework currently includes the following modules.

### Territorial and assessment layer

- [M_ZONE — Territorial subdivision and assessment domains](modules/zone.md)
- [M_ASSESS — Assessment regime](modules/assess.md)

### Monitoring, data and validation layer

- [M_NETWORK — Monitoring network](modules/network.md)
- [M_DATA_QUALITY — Data quality and assessment-data validity](modules/data_quality.md)
- [M_MODEL_QA — Validation and quality assurance of modelling applications](modules/model_qa.md)

### Modelling and spatial layer

- [M_MOD — Modelling applications](modules/modelling.md)
- [M_REPR — Spatial representativeness of sampling points](modules/repr.md)

### Compliance and exposure layer

- [M_LIMITS — Compliance and exceedance verification](modules/limits.md)
- [M_EXPOSURE — Average exposure indicator and exposure obligations](modules/exposure.md)

### Source attribution and legal qualification layer

- [M_SOURCE_ATTRIBUTION — Source attribution and contribution qualification](modules/source_attribution.md)

### Planning and attainment layer

- [M_PLANS — Air quality plans, roadmaps and short-term action plans](modules/plans.md)
- [M_ATTAINMENT_EXTENSION — Postponement of attainment deadlines](modules/attainment_extension.md)

### Coordination and output layer

- [M_TRANSBOUNDARY — Transboundary air pollution cooperation and coordination](modules/transboundary.md)
- [M_PUBLIC_INFORMATION — Public information and communication](modules/public_information.md)
- [M_REPORTING — Regulatory reporting and data exchange](modules/reporting.md)

---

## Normative tables

The framework currently contains the following table files:

- [T_ADVANCED_MONITORING — Advanced monitoring obligations](tables/t_advanced_monitoring.md)
- [T_ALERT_THRESHOLDS — Alert and information thresholds](tables/t_alert_thresholds.md)
- [T_ASSESS_THRESHOLDS — Assessment thresholds](tables/t_assess_thresholds.md)
- [T_DATA_QUALITY — Data quality objectives](tables/t_data_quality.md)
- [T_EIONET — EIONET vocabularies and interoperability standards](tables/t_eionet.md)
- [T_EXPOSURE_OBLIGATIONS — Exposure reduction obligations and objectives](tables/t_exposure_obligations.md)
- [T_LIMIT_VALUES — Limit values and target values](tables/t_limit_values.md)
- [T_MIN_STATIONS — Minimum number of sampling points](tables/t_min_stations.md)
- [T_NATURAL_EVENTS — Natural events and natural-source criteria](tables/t_natural_events.md)
- [T_REPR_TOLERANCE — Spatial representativeness tolerances](tables/t_repr_tolerance.md)
- [T_SITING — Siting criteria](tables/t_siting.md)
- [T_SUPERSITES — Supersite parameters](tables/t_supersites.md)

---

## Planned tables — not yet implemented

The following tables are referenced conceptually by the architecture or by module dependencies, but the corresponding files do not yet exist:

- T_MODEL_QA — Model quality assurance parameters
- T_SOURCE_CATEGORIES — Source categories
- T_ATTRIBUTION_METHODS — Source-attribution methods
- T_NUTS — NUTS territorial references
- T_REPORTING_SCHEMA — Reporting schemas

---

## Main outputs

The framework produces structured outputs including:

```text
territorial_context
assessment_status
network_status
data_quality_status
model_validation
model_output
representativeness_status
compliance_result
exposure_status
source_attribution_status
plan_status
attainment_extension
transboundary_status
public_information
reporting_package
```

These outputs can be consumed by software systems, reporting pipelines, public dashboards and regulatory decision-support tools.

---

## Audience

The framework is intended for:

- SNPA entities;
- air quality experts;
- environmental agencies;
- software developers;
- environmental data scientists;
- regulatory analysts;
- public-sector digital transformation teams.

---

## Project status

This version represents an **extended formal technical specification**.

It includes the core technical assessment modules and the downstream modules for source attribution, plans and roadmaps, attainment-deadline extension, transboundary coordination, public information and regulatory reporting.

Further work is required to:

- complete and validate existing normative tables;
- create planned tables where they become necessary;
- align with implementing acts and reporting schemas;
- test the framework against practical cases;
- refine module outputs into machine-readable schemas.

---

## Notes

The framework is designed to be implemented progressively.

A practical implementation can start from territorial data, normative tables, assessment, monitoring and data quality, then progressively add modelling, representativeness, compliance, exposure, source attribution, planning and reporting layers.
