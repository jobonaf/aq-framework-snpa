# Changelog

This project follows semantic versioning oriented to the regulatory maturity of the framework, not to executable software.

## [Unreleased]

_No unreleased changes at this time._

## [v0.3] — Extended formal technical specification

_Date: 2026-05-21_

### 🔵 Added

- Released version **v0.3 — extended formal technical specification**.
- Extended the modular coverage beyond the v0.2 regulatory core, including modules for:
  - `M_SOURCE_ATTRIBUTION` — source attribution and contribution qualification;
  - `M_ATTAINMENT_EXTENSION` — postponement of attainment deadlines;
  - `M_PLANS` — air quality plans, roadmaps and short-term action plans;
  - `M_PUBLIC_INFORMATION` — public information and communication;
  - `M_REPORTING` — regulatory reporting and data exchange;
  - `M_TRANSBOUNDARY` — transboundary air pollution cooperation and coordination;
  - `M_EXPOSURE` — average exposure indicators and exposure obligations;
  - `M_ZONE` — territorial subdivision and assessment domains.
- Added structured outputs for the new functional domains, including:
  - `source_attribution_status`;
  - `attainment_extension`;
  - `plan_status`;
  - `public_information`;
  - `reporting_package`;
  - `transboundary_status`;
  - `exposure_status`;
  - `territorial_context`.
- Added the Italian version of modules and main documentation under `docs/`, with terminology alignment through the glossary.
- Added English versions of repository-facing root files, especially `README.md` and `CONTRIBUTING.md`.
- Clarified that `README.md` and `CONTRIBUTING.md` are intended for users accessing the repository directly and are not primary ReadTheDocs content.

### 🔵 Changed

- Updated the framework from a regulatory core to an **extended formal technical specification**, including planning, postponements, source attribution, public information, reporting and transboundary cooperation.
- Reorganised Italian/English terminology according to the repository’s bilingual approach.
- Updated `CONTRIBUTING.md` to reflect that descriptive text may be in **Italian or English**, while module names, variables, functions, REQ identifiers and EIONET vocabularies remain stable and in English where required.
- Updated `README.md` to describe the current repository status, extended module coverage and the distinction between documentation under `docs/` and repository root files.
- Strengthened the distinction between:
  - technical assessment;
  - legal compliance;
  - downstream legal or procedural effects such as plans, postponements, natural-source omissions, reporting and public information.
- Aligned modules with the Italian glossary terminology while preserving technical identifiers, machine-readable outputs and `M_*` / `T_*` names.

### 🔵 Fixed

- Fixed terminology inconsistencies between Italian and English module versions.
- Clarified that postponement of attainment deadlines is not a general derogation, but a conditional and evidence-based assessment.
- Clarified that source attribution produces evidence and attribution status, but does not directly determine final legal effects.
- Clarified that reporting aggregates and transmits results already produced by upstream modules and does not recalculate compliance.
- Clarified that spatial representativeness is not a simple geometric buffer, but a regulatory determination depending on pollutant, metric, period, station type and territorial context.

### ⚠️ Known limitations / Out of scope

- Some planned tables remain to be created or stabilised, including:
  - `T_MODEL_QA`;
  - `T_SOURCE_CATEGORIES`;
  - `T_ATTRIBUTION_METHODS`;
  - `T_NUTS`, if NUTS references are managed internally;
  - `T_REPORTING_SCHEMA`, once reporting payloads are defined.
- Official implementing acts must be checked and integrated when published or stabilised.
- Complete machine-readable schemas for all module outputs still need to be formalised.
- Consistency tests, test cases and end-to-end examples remain to be completed.

### ✅ Release notes

Version **v0.3** extends the framework from the technical-regulatory core to a broader specification including the procedural and information domains needed for planning, postponements, source attribution, public information, reporting and transboundary cooperation.

## [v0.2] — Formal technical specification of the regulatory core

_Date: 2026-05-13_

### 🔵 Added

#### Requirement formalisation (REQ)

- Introduced systematic formalisation of regulatory requirements (`REQ`) in all core modules, following the syntax defined in `CONTRIBUTING.md`.
- Adopted stable, non-ordinal **slot-based** identifiers (`REQ-{MODULE}-{SLOT}`).

#### Full coverage of Directive annexes

- Structured and complete coverage of the following annexes:
  - Annex I — limit values and target values;
  - Annex II — assessment thresholds;
  - Annex III — minimum number of sampling points;
  - Annex IV — siting criteria and combined-use context;
  - Annex V — data quality and modelling;
  - Annex VII — monitoring supersites.

#### New normative tables

- `T_ASSESS_THRESHOLDS` revised and aligned with Annex II.
- `T_LIMIT_VALUES` fully rewritten on the basis of Annex I.
- `T_MIN_STATIONS` unified into a single file, covering Annex III.
- `T_DATA_QUALITY` revised with distinction between:
  - long-term / short-term;
  - fixed measurements / indicative measurements / modelling.
- `T_SUPERSITES` revised and fully aligned with Annex VII.

### 🔵 Changed

#### Framework architecture

- Clear and systematic separation between:
  - **normative data** in tables;
  - **normative logic** in modules.
- Removed hard-coded numerical values from modules.
- Explicitly clarified responsibilities between the core modules (`ASSESS`, `NETWORK`, `REPR`, `MOD`, `LIMITS`).

#### Modules fully refactored with REQ

- `M_ASSESS`
  - formalisation of above/below-threshold classification rules;
  - explicit definition of the assessment regime.
- `M_NETWORK`
  - formalisation of Annex III obligations;
  - minimum network composition, reduction, UFP and rural ozone.
- `M_LIMITS`
  - formalisation of compliance verification;
  - handling of natural sources, exceptional events and special exceedance qualifications.
- `M_DATA_QUALITY`
  - formalisation of data-validity criteria;
  - explicit exclusions and special rules.
- `M_MODEL_QA`
  - formal definition of the MQI indicator;
  - `MQI <= 1` criterion;
  - 90% sampling-point criterion.
- `M_REPR`
  - formalisation of spatial representativeness rules;
  - handling of critical cases and expert judgement.
- `M_MOD`
  - formalisation of the regulatory use of modelling;
  - handling of measurement–model conflicts.

### 🔵 Fixed

- Removed conceptual ambiguities between:
  - assessment thresholds and limit values;
  - assessment and compliance;
  - representativeness and compliance.
- Removed implicit or non-traceable regulatory references.
- Clarified the role of modelling throughout the framework.

### ⚠️ Known limitations / Out of scope

- Not included in this version:
  - air quality plans;
  - roadmaps and reduction trajectories;
  - institutional governance and laboratory QA/QC;
  - official reporting to the Commission.
- These aspects were explicitly deferred to later versions, starting with version v0.3.

### ✅ Release notes

Version **v0.2** represents the completion of the **technical-regulatory core** of the framework, with a complete and verifiable formalisation of the main requirements of Directive (EU) 2024/2881.

It is the first version that is:

- fully REQ-based;
- audit-ready;
- consistent for SNPA use and LLM-assisted support.

## [v0.1] — Initial framework setup

_Date: before v0.2_

### 🔵 Added

- Initial setup of the repository structure.
- First definition of core modules and normative tables.
- First organisation of documentation into modules, tables and introductory content.
