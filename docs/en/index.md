# AQ Framework — SNPA

## Formal Compliance Framework for Ambient Air Quality  
Directive (EU) 2024/2881

---

## Overview

This project defines a **formal and computable specification** for implementing Directive (EU) 2024/2881 on air quality.

The framework translates regulatory requirements into:

- logical modules (`M_*`)
- normative tables (`T_*`)
- relationships between components

---

## Objectives

- support automatic compliance verification
- simplify software development (LLM + traditional systems)
- ensure consistency and interoperability with AQD / EIONET
- provide a shared SNPA technical foundation

---

## Framework structure

The system is organized into:

- **Modules** → operational logic (network, modelling, limits)
- **Tables** → normative parameters (limit values, thresholds, requirements)
- **Annexes** → regulatory and methodological references

---

## Navigation

### Modules
- [M_ZONE — Territorial subdivision](modules/zone.md)
- [M_ASSESS — Assessment regime](modules/assess.md)
- [M_NETWORK — Monitoring network](modules/network.md)
- [M_MOD — Modelling applications](modules/modelling.md)
- [M_MODEL_QA — Model quality assurance](modules/model_qa.md)
- [M_REPR — Spatial representativeness](modules/repr.md)
- [M_LIMITS — Compliance with limit values](modules/limits.md)
- [M_EXPOSURE — Average exposure indicator](modules/exposure.md)
- [M_DATA_QUALITY — Data quality](modules/data_quality.md)

### Tables
- [T_ASSESS_THRESHOLDS — Assessment thresholds](tables/t_assess_thresholds.md)
- [T_ALERT_THRESHOLDS — Alert thresholds](tables/t_alert_thresholds.md)
- [T_LIMIT_VALUES — Limit values](tables/t_limit_values.md)
- [T_MIN_STATIONS — Minimum station numbers](tables/t_min_stations.md)
- [T_SITING — Siting criteria](tables/t_siting.md)
- [T_ADVANCED_MONITORING — Advanced monitoring](tables/t_advanced_monitoring.md)
- [T_DATA_QUALITY — Data quality](tables/t_data_quality.md)
- [T_REPR_TOLERANCE — Representativeness tolerances](tables/t_repr_tolerance.md)
- [T_EXPOSURE_OBLIGATIONS — Exposure reduction obligations](tables/t_exposure_obligations.md)
- [T_NATURAL_EVENTS — Natural events](tables/t_natural_events.md)
- [T_SUPERSITES — Supersite parameters](tables/t_supersites.md)
- [T_EIONET — EIONET vocabularies](tables/t_eionet.md)

---

## Project status

✅ Version: **v0.2 — formal technical specification (core normative)**  
⚠ Includes references to implementing acts (2026 draft)  
⚠ Requires final verification against the consolidated Official Journal text

---

## Intended audience

- air quality experts (SNPA)
- software developers
- environmental data analysts