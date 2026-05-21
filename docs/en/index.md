# AQ Framework — SNPA

## Formal Compliance Framework for Ambient Air Quality

Directive (EU) 2024/2881

---

## Overview

This project defines a **formal and computable specification** for implementing Directive (EU) 2024/2881 on ambient air quality.

The framework translates regulatory requirements into:

- logical modules (`M_*`);
- normative tables (`T_*`);
- structured outputs;
- dependencies between assessment, monitoring, modelling, compliance, planning, public information and reporting components.

The framework is intended to support:

- automatic or semi-automatic compliance verification;
- consistent interpretation of the Directive;
- software implementation and validation;
- auditability and traceability of regulatory decisions;
- interoperability with air-quality reporting and data-exchange systems.

---

## Architectural model

The framework is organised as a **layered regulatory decision system** rather than a single linear pipeline.

Main layers:

1. territorial context;
2. assessment regime;
3. monitoring and data quality;
4. modelling and spatial representativeness;
5. compliance and exposure assessment;
6. source attribution;
7. planning and attainment-deadline extension;
8. transboundary coordination;
9. public information and reporting.

See:

- [Framework architecture](architecture.md)

---

## Framework structure

The system is organised into:

### Modules

Modules define operational and regulatory logic:

- conditions;
- dependencies;
- decision flows;
- validation gates;
- legal-effect boundaries;
- structured outputs.

### Tables

Tables define regulatory parameters:

- thresholds;
- values;
- minimum numbers;
- data-quality objectives;
- tolerances;
- obligations;
- source categories;
- reporting schemas.

The index below links only to table files that currently exist in the repository. Planned tables are listed separately without links.

---

## Navigation

## Core architecture and documentation

- [Introduction](introduction.md)
- [Framework architecture](architecture.md)
- [Changelog](CHANGELOG.md)
- [TODO](TODO.md)

---

## Modules

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

## Existing tables

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

The following tables are referenced by the logical architecture or by module dependencies, but the files have not yet been created. They are intentionally listed without links to avoid broken navigation.

- T_MODEL_QA — Model quality assurance parameters
- T_SOURCE_CATEGORIES — Source categories
- T_ATTRIBUTION_METHODS — Source-attribution methods
- T_NUTS — NUTS territorial references
- T_REPORTING_SCHEMA — Reporting schemas

---

## Main module outputs

The framework produces a set of structured outputs, including:

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

---

## Current project status

Status: **v0.3 — extended formal technical specification**

This version includes:

- core assessment and monitoring modules;
- modelling, model QA and spatial representativeness;
- compliance and exposure assessment;
- source attribution;
- plans, roadmaps and attainment-deadline extension;
- transboundary coordination;
- public information;
- regulatory reporting.

The framework is still subject to:

- verification against the consolidated Official Journal text;
- refinement of existing normative tables (`T_*`);
- creation of planned tables where needed;
- alignment with implementing acts and data-exchange formats;
- validation through practical use cases.

---

## Intended audience

- air quality experts;
- SNPA entities;
- environmental agencies;
- software developers;
- environmental data analysts;
- regulatory analysts;
- decision-support system designers.

---

## Implementation note

The recommended implementation order is:

1. territorial and table foundations;
2. assessment, monitoring and data-quality modules;
3. modelling, model QA and representativeness;
4. compliance and exposure;
5. source attribution;
6. plans and attainment extensions;
7. transboundary coordination;
8. public information and reporting.

---

## Notes

The framework is designed to be:

- explicit;
- formal;
- modular;
- auditable;
- automation-friendly;
- compatible with traditional rule engines;
- compatible with LLM-assisted workflows;
- interoperable with official air-quality data systems.
