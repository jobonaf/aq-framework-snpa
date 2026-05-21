# AQ Framework SNPA

Formal technical compliance framework for ambient air quality under Directive (EU) 2024/2881.

## Purpose

This repository contains a **formal technical specification** for implementing the ambient air quality framework introduced by Directive (EU) 2024/2881.

The project translates regulatory requirements into:

- logical modules (`M_*`);
- normative and parameter tables (`T_*`);
- explicit dependencies between modules, tables and outputs;
- machine-readable output concepts suitable for implementation and testing.

The goal is to support:

- consistent technical interpretation of the Directive;
- automated or semi-automated compliance checks;
- software implementation, including rule engines and LLM-assisted systems;
- harmonisation of technical work within the SNPA context;
- interoperability with AQD / EIONET reporting workflows.

## Repository scope

The repository is a **technical specification**, not a legal document.

It is intended to describe how regulatory concepts can be represented, connected and evaluated in a structured implementation framework. Legal interpretation, formal decisions, enforcement and official reporting obligations remain with the competent authorities.

## Documentation structure

The main documentation lives under `docs/` and is intended for MkDocs / Read the Docs publication.

Root-level files such as `README.md` and `CONTRIBUTING.md` are repository-facing files. They are meant for users browsing the repository directly and are not part of the published documentation content.

### Logical modules

The module layer defines the operational logic and decision interfaces of the framework.

Current module families include:

- `M_ZONE` — territorial subdivision and assessment domains;
- `M_ASSESS` — assessment regime and assessment-method selection;
- `M_NETWORK` — monitoring-network adequacy;
- `M_MOD` — regulatory use of modelling applications;
- `M_MODEL_QA` — validation and quality assurance of modelling applications;
- `M_REPR` — spatial representativeness of sampling points;
- `M_LIMITS` — compliance and exceedance verification;
- `M_EXPOSURE` — average exposure indicators and exposure obligations;
- `M_DATA_QUALITY` — data quality and assessment-data validity;
- `M_SOURCE_ATTRIBUTION` — source attribution and contribution qualification;
- `M_ATTAINMENT_EXTENSION` — postponement of attainment deadlines;
- `M_PLANS` — air quality plans, roadmaps and short-term action plans;
- `M_PUBLIC_INFORMATION` — public information and communication;
- `M_REPORTING` — regulatory reporting and data exchange;
- `M_TRANSBOUNDARY` — transboundary air pollution cooperation and coordination.

### Normative tables

The table layer stores parameters, thresholds, vocabularies and applicability rules used by the modules.

Typical table groups include:

- assessment thresholds;
- limit values, target values, critical levels and alert/information thresholds;
- minimum monitoring requirements;
- siting and location criteria;
- data quality objectives;
- representativeness tolerances;
- exposure obligations;
- supersite requirements;
- natural-event and source-attribution metadata;
- EIONET / reporting vocabularies.

Some planned tables may remain placeholders until the relevant implementing acts, reporting schemas or methodology details are finalised.

### Contextual documentation

The documentation also includes:

- introduction and architecture pages;
- glossary and terminology alignment;
- TODO / roadmap material;
- implementation notes and consistency checks where relevant.

## Project status

- Current specification line: **v0.3 — extended formal technical specification**
- Scope: SNPA-oriented technical framework for Directive (EU) 2024/2881
- Status: evolving technical specification
- Implementing acts: to be checked and integrated as they are published or stabilised

The framework currently covers the main technical areas required for:

- territorial domains and assessment units;
- assessment classification and assessment methods;
- monitoring-network adequacy;
- integration of measurements and modelling;
- spatial representativeness;
- compliance and exceedance qualification;
- average exposure obligations;
- data quality validation;
- source attribution and transboundary contribution handling;
- planning, postponement, public information and reporting interfaces.

## Intended users

This repository is intended for:

- air-quality experts and analysts;
- SNPA technical working groups;
- software developers implementing compliance logic;
- environmental data scientists;
- documentation maintainers;
- LLM-assisted review and implementation workflows.

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for contribution rules, issue reporting guidance and the expected structure of requirements.

## Disclaimer

This repository is **not a legal act, legal opinion or official guidance document**.

It is a technical specification designed to make the regulatory framework implementable, reviewable and testable in a consistent way.
