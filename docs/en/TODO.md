## TODO

### General repository alignment

- [ ] Verify all rewritten `M_*` modules against the consolidated Official Journal text
- [ ] Align module filenames and navigation links with the final repository structure
- [ ] Replace draft module filenames with canonical filenames where needed
- [ ] Remove or avoid links to planned tables until the corresponding files exist
- [ ] Check all internal links after repository file renaming or relocation
- [ ] Check all Markdown diagrams and Mermaid syntax after file integration
- [ ] Decide whether the Italian version or English version is the authoritative repository version
- [ ] Update `CHANGELOG.md` with the transition to v0.3 extended formal technical specification

### Module consolidation

- [ ] Review and consolidate `M_LIMITS`
- [ ] Review and consolidate `M_PLANS`
- [ ] Review and consolidate `M_ATTAINMENT_EXTENSION`
- [ ] Review and consolidate `M_PUBLIC_INFORMATION`
- [ ] Review and consolidate `M_REPORTING`
- [ ] Review and consolidate `M_TRANSBOUNDARY`
- [ ] Review and consolidate `M_SOURCE_ATTRIBUTION`
- [ ] Review and consolidate `M_EXPOSURE`
- [ ] Review and consolidate `M_MODEL_QA`

### Existing tables to review

- [ ] Review existing tables and align them with the extended module architecture
- [ ] Update `T_LIMIT_VALUES`
- [ ] Update `T_ALERT_THRESHOLDS`
- [ ] Update `T_ASSESS_THRESHOLDS`
- [ ] Update `T_MIN_STATIONS`
- [ ] Update `T_SITING`
- [ ] Update `T_DATA_QUALITY`
- [ ] Update `T_REPR_TOLERANCE`
- [ ] Update `T_EXPOSURE_OBLIGATIONS`
- [ ] Update `T_SUPERSITES`
- [ ] Update `T_NATURAL_EVENTS`
- [ ] Update `T_ADVANCED_MONITORING`
- [ ] Update `T_EIONET`

### Planned tables to create

- [ ] Create `T_MODEL_QA` once the model-validation table schema is stable
- [ ] Create `T_SOURCE_CATEGORIES` once source-attribution categories are finalized
- [ ] Create `T_ATTRIBUTION_METHODS` once accepted attribution methods are formalized
- [ ] Create `T_NUTS` if NUTS references are managed internally
- [ ] Create `T_REPORTING_SCHEMA` once reporting payloads are defined

### Output schemas and implementation artifacts

- [ ] Define canonical output schemas for all module outputs
- [ ] Define machine-readable schema for `assessment_status`
- [ ] Define machine-readable schema for `compliance_result`
- [ ] Define machine-readable schema for `exposure_status`
- [ ] Define machine-readable schema for `reporting_package`

### Test cases

- [ ] Add test case for ordinary exceedance
- [ ] Add test case for natural-source omission
- [ ] Add test case for winter sanding/salting
- [ ] Add test case for valid postponement
- [ ] Add test case for AEI failure
- [ ] Add test case for transboundary contribution

### Consistency tests

- [ ] Test consistency across `M_ASSESS`, `M_NETWORK`, `M_REPR`, `M_MOD` and `M_LIMITS`
- [ ] Test consistency across `M_LIMITS`, `M_SOURCE_ATTRIBUTION`, `M_PLANS` and `M_ATTAINMENT_EXTENSION`
- [ ] Test consistency between `M_PUBLIC_INFORMATION` and `M_REPORTING`

### End-to-end examples

- [ ] Prepare workflow for one pollutant, one zone and one assessment year
- [ ] Prepare workflow with modelling and representativeness
- [ ] Prepare workflow with modelled exceedance area
- [ ] Prepare workflow with AEI calculation
- [ ] Prepare workflow with exposure reduction obligation
- [ ] Prepare workflow with Art. 18 postponement request
- [ ] Prepare workflow with Art. 21 transboundary contribution

### After publication of official implementing acts

Review and update all references to official implementing acts across modules and tables.

#### Assessment and modelling

- [ ] Verify Art. 8(7) modelling requirements
- [ ] Verify Art. 8(7) assessment requirements
- [ ] Verify `M_ASSESS` against official implementing acts
- [ ] Verify `M_MOD` against official implementing acts
- [ ] Verify `M_REPR` against official implementing acts
- [ ] Verify `M_MODEL_QA` against official implementing acts

#### Model validation

- [ ] Verify model-validation criteria
- [ ] Verify MQI rules
- [ ] Verify spatial resolution requirements
- [ ] Verify uncertainty metadata requirements
- [ ] Update `M_MODEL_QA` where needed
- [ ] Update planned `T_MODEL_QA` where needed

#### Spatial representativeness

- [ ] Verify spatial representativeness methodology
- [ ] Verify representativeness tolerance rules
- [ ] Verify coverage logic for modelled exceedance areas
- [ ] Update `M_REPR` where needed
- [ ] Update `T_REPR_TOLERANCE` where needed

#### Monitoring network and additional measurements

- [ ] Verify additional measurement rules after modelled exceedances
- [ ] Verify network-reduction conditions
- [ ] Verify supplementary assessment requirements
- [ ] Verify siting requirements
- [ ] Verify station classification requirements
- [ ] Update `M_ASSESS` where needed
- [ ] Update `M_NETWORK` where needed
- [ ] Update `T_MIN_STATIONS` where needed
- [ ] Update `T_SITING` where needed

#### Data quality

- [ ] Verify data quality objectives
- [ ] Verify minimum data coverage rules
- [ ] Verify uncertainty rules
- [ ] Verify purpose-specific applicability rules
- [ ] Update `M_DATA_QUALITY` where needed
- [ ] Update `T_DATA_QUALITY` where needed

#### Source attribution

- [ ] Verify natural-source attribution methodology
- [ ] Verify natural-source evidence requirements
- [ ] Verify winter sanding/salting attribution methodology
- [ ] Verify winter sanding/salting evidence requirements
- [ ] Update `M_SOURCE_ATTRIBUTION` where needed
- [ ] Update `T_NATURAL_EVENTS` where needed

#### Attainment extension and roadmaps

- [ ] Verify Art. 18 roadmap rules
- [ ] Verify Art. 18 projection rules
- [ ] Verify Art. 18 notification rules
- [ ] Verify Art. 18 assessment rules
- [ ] Verify Commission objection rules
- [ ] Update `M_ATTAINMENT_EXTENSION` where needed
- [ ] Update `M_PLANS` where needed
- [ ] Update `M_MOD` where needed

#### Reporting and public information

- [ ] Verify reporting data formats
- [ ] Verify reporting vocabularies
- [ ] Verify reporting schemas
- [ ] Verify submission flows
- [ ] Verify public information requirements
- [ ] Verify air quality index requirements
- [ ] Update `M_REPORTING` where needed
- [ ] Update `M_PUBLIC_INFORMATION` where needed
- [ ] Update `T_EIONET` where needed
- [ ] Update planned `T_REPORTING_SCHEMA` where needed

#### Repository-wide follow-up

- [ ] Update planned tables list
- [ ] Create any newly required `T_*` tables
- [ ] Run consistency check on all module dependencies
- [ ] Run consistency check on all table references
- [ ] Run consistency check on all internal links
- [ ] Run consistency check on all Mermaid diagrams
