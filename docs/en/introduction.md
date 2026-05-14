# Introduction

## Purpose

This document defines a **formal technical specification** for Directive (EU) 2024/2881 on ambient air quality.

The repository is available on GitHub: https://github.com/jobonaf/aq-framework-snpa

The goal is to translate regulatory requirements into a structure that is:

- explicit
- formal
- implementable

---

## Context

Directive 2024/2881 introduces:

- greater integration between measurements and modelling
- centrality of spatial representativeness
- stricter model validation requirements
- new limit values (2030 horizon)

These elements require formalization to:

- ensure consistent application
- support automation and verification

---

## Approach

The framework adopts an architecture that is:

### Modular

Each component is represented by an `M_*` module:

- `M_NETWORK` → network
- `M_MOD` → modelling
- `M_REPR` → representativeness
- `M_LIMITS` → compliance

---

### Logical/data separation

- modules → rules and logic
- tables → normative parameters

---

### Formalization

Rules are expressed through:

- pseudocode
- logical conditions
- relationships between variables

---

## Role of modelling

Modelling plays a central role:

- supports spatial distribution
- integrates measurements
- enables representativeness
- contributes to compliance assessment

---

## Role of spatial representativeness

Spatial representativeness:

- connects measurements and territory
- determines validity of exceedances
- guides network design

---

## Content overview

### Logical modules
The 9 `M_*` modules describe the decision flow:

1. `M_ZONE` — Territorial classification (urban, rural, background)
2. `M_ASSESS` — Selection of the assessment regime (fixed, model, hybrid)
3. `M_NETWORK` — Monitoring network design
4. `M_MOD` — Configuration of modelling applications
5. `M_MODEL_QA` — Model validation and quality verification
6. `M_REPR` — Spatial representativeness calculation
7. `M_LIMITS` — Check compliance with limit values
8. `M_EXPOSURE` — Compute the average exposure indicator
9. `M_DATA_QUALITY` (cross-cutting) — Data quality and completeness

### Normative tables
The 12 `T_*` tables contain directive-prescribed parameters:

- `T_ASSESS_THRESHOLDS` — Assessment thresholds for regime
- `T_ALERT_THRESHOLDS` — Alert and information thresholds
- `T_LIMIT_VALUES` — Limit values for pollutants
- `T_MIN_STATIONS` — Minimum station counts by zone
- `T_SITING` — Station siting criteria
- `T_ADVANCED_MONITORING` — Enhanced monitoring obligations
- `T_DATA_QUALITY` — Completeness and validity requirements
- `T_REPR_TOLERANCE` — Spatial representativeness tolerances
- `T_EXPOSURE_OBLIGATIONS` — Exposure reduction obligations
- `T_NATURAL_EVENTS` — Types and criteria for natural exceptional events
- `T_SUPERSITES` — Supersite-specific parameters
- `T_EIONET` — Vocabularies and interoperability standards

This version:

- incorporates elements from implementing acts (draft)
- is subject to revision

---

## Audience

- SNPA entities
- developers
- environmental data scientists
