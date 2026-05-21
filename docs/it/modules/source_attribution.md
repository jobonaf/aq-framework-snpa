# M_SOURCE_ATTRIBUTION — Attribuzione delle fonti e qualificazione dei contributi

## Riferimenti normativi

- Art. 4(40) Direttiva (UE) 2024/2881 — contributi da fonti naturali
- Art. 8 Direttiva (UE) 2024/2881 — metodi di valutazione e applicazioni di modellizzazione
- Art. 16 Direttiva (UE) 2024/2881 — superamenti attribuibili a fonti naturali
- Art. 17 Direttiva (UE) 2024/2881 — superamenti di PM10 dovuti a risospensione in seguito a sabbiatura o salatura invernale delle strade
- Art. 18 Direttiva (UE) 2024/2881 — proroga dei termini di conseguimento, inclusi i contributi transfrontalieri come possibile motivo
- Art. 19 Direttiva (UE) 2024/2881 — piani per la qualità dell’aria e tabelle di marcia
- Art. 21 Direttiva (UE) 2024/2881 — inquinamento atmosferico transfrontaliero
- Art. 22–23 Direttiva (UE) 2024/2881 — interfacce di informazione al pubblico e rendicontazione
- Allegato I — valori limite, valori-obiettivo, livelli critici e obblighi di esposizione
- Allegato V — obiettivi di qualità dei dati
- Allegato VI — applicazioni di modellizzazione e condizioni sui metodi
- Allegato VIII — piani per la qualità dell’aria e tabelle di marcia
- Atti implementativi ai sensi degli Art. 16(4), 17(4), 18(5), ove applicabili

## Descrizione

Questo modulo determina se un superamento, un livello di concentrazione, un indicatore di esposizione o un contributo possa essere attribuito, in tutto o in parte, a categorie di fonti specifiche rilevanti ai sensi della Direttiva.

Supporta la qualificazione giuridica di:

- superamenti attribuibili a fonti naturali;
- superamenti di PM10 attribuibili alla risospensione da sabbiatura o salatura invernale delle strade;
- contributi transfrontalieri ai superamenti;
- altri contributi di fonte rilevanti per piani, tabelle di marcia, proiezioni o richieste di proroga;
- evidenze di contributo di fonte usate per rendicontazione, informazione al pubblico e pianificazione.

Questo modulo produce **evidenze e stato di attribuzione**. Non decide direttamente lo stato finale di conformità, non omette superamenti ai fini della Direttiva, non concede proroghe e non esonera dagli obblighi di pianificazione. Tali conseguenze giuridiche sono gestite dai moduli downstream.

## Ambito

Il modulo si applica a valutazioni di attribuzione delle fonti usate per:

```text
natural_source_attribution
winter_sanding_or_salting_attribution
transboundary_contribution_attribution
plan_source_contribution_analysis
roadmap_source_contribution_analysis
postponement_ground_support
average_exposure_adjustment_support
public_information_source_context
reporting_source_context
```

Può essere applicato a:

- superamenti di valori limite;
- superamenti di valori-obiettivo;
- indicatori di esposizione media e obblighi di esposizione;
- aree modellate di superamento;
- zone;
- unità territoriali di esposizione media;
- aree interessate transfrontaliere;
- aree di piano o di tabella di marcia.

## Definizioni

```text
p = inquinante
z = zona
u = unità territoriale di esposizione media
A = area geografica
y = anno civile
m = metrica regolatoria
```

```text
SOURCE_CATEGORY =
  natural_source
  winter_sanding_or_salting
  transboundary_contribution
  local_traffic
  industrial_source
  domestic_heating
  agriculture
  shipping_or_port
  aviation_or_airport
  other_anthropogenic_source
```

```text
SOURCE_CONTRIBUTION(p,A,m,period,source) =
contributo stimato della categoria di fonte source al livello valutato
dell’inquinante p nell’area A, metrica m e periodo
```

```text
ATTRIBUTABLE_EXCEEDANCE(p,A,m,period,source) =
superamento per cui il pacchetto di evidenze supporta l’attribuzione alla fonte
```

```text
NATURAL_SOURCE_CASE =
superamento o livello collegato all’esposizione attribuibile a contributi da fonti naturali
ai sensi dell’Art. 16
```

```text
WINTER_SANDING_SALTING_CASE =
superamento di valore limite PM10 attribuibile a risospensione di particolato
in seguito a sabbiatura o salatura invernale delle strade ai sensi dell’Art. 17
```

```text
TRANSBOUNDARY_CASE =
superamento o livello cui contribuisce significativamente il trasporto transfrontaliero
di inquinamento atmosferico ai sensi dell’Art. 21 o dell’Art. 18
```

```text
ATTRIBUTION_EVIDENCE_PACKAGE =
dataset tracciabile, metodo, modellizzazione, misurazioni, eventi ed evidenze
di contributo di fonte a supporto di una conclusione di attribuzione
```

## Requisiti normativi

### REQ-SOURCE-PURPOSE_CLASSIFICATION

**Regola**  
Ogni valutazione di attribuzione deve essere classificata per scopo regolatorio prima che siano applicate le conseguenze dell’attribuzione.

```text
if source_attribution_assessment_used = true:
    attribution_purpose in [
        natural_source_attribution,
        winter_sanding_or_salting_attribution,
        transboundary_contribution_attribution,
        plan_source_contribution_analysis,
        roadmap_source_contribution_analysis,
        postponement_ground_support,
        average_exposure_adjustment_support,
        public_information_source_context,
        reporting_source_context
    ]
```

### REQ-SOURCE-EVIDENCE_PACKAGE

**Regola**  
Una conclusione di attribuzione delle fonti deve essere supportata da un pacchetto di evidenze documentato.

Il pacchetto deve identificare inquinante, metrica, area o unità territoriale, periodo di valutazione, categoria di fonte, contributo di concentrazione o evidenza qualitativa, metodi, dataset, stato di qualità dei dati, stato di validazione dei modelli ove usati e metadati di incertezza o confidenza ove disponibili.

```text
ATTRIBUTION_EVIDENCE_PACKAGE_COMPLETE =
    pollutant identified
    AND metric identified
    AND territorial_domain identified
    AND period identified
    AND source_category identified
    AND method_documented = true
    AND data_quality_status_available = true
    AND modelling_validation_status_available_if_model_used = true
```

### REQ-SOURCE-NATURAL_SOURCE_CLASSIFICATION

**Regola**  
Zone e unità territoriali di esposizione media possono essere classificate come interessate da superamenti attribuibili a fonti naturali. Quando tale classificazione è usata, devono essere forniti gli elenchi richiesti, le informazioni su concentrazioni e fonti e le evidenze di attribuzione a fonte naturale.

```text
if source_category = natural_source
AND attribution_evidence_sufficient = true:
    natural_source_case = true
    affected_zones_or_AETUs recorded
    concentration_and_source_information_available = true
    evidence_package_complete = true
else:
    natural_source_case = false
```

### REQ-SOURCE-NATURAL_SOURCE_CONTRIBUTION_TYPES

**Regola**  
I contributi da fonti naturali includono emissioni non causate direttamente o indirettamente da attività umane, inclusi eventi naturali quali eruzioni vulcaniche, attività sismiche, attività geotermiche, incendi in aree naturali, eventi di vento forte, spray marino, oppure risospensione o trasporto atmosferico di particelle naturali da regioni secche.

```text
if event_or_contribution_type in T_SOURCE_CATEGORIES.natural_sources:
    natural_source_candidate = true
else:
    natural_source_candidate = false
```

### REQ-SOURCE-NATURAL_SOURCE_ADJUSTMENT_OUTPUT

**Regola**  
Quando un caso di fonte naturale è supportato e comunicato come richiesto, il modulo deve fornire la stima del contributo e le evidenze necessarie ai moduli downstream per determinare se il superamento o il contributo possa essere omesso o aggiustato ai fini della Direttiva.

Questo modulo non effettua direttamente l’omissione giuridica.

```text
if natural_source_case = true:
    provide natural_source_contribution_estimate
    provide adjusted_concentration_candidate
    provide evidence_package_reference
    provide reporting_payload
```

### REQ-SOURCE-COMMISSION_EVIDENCE_STATUS_INTERFACE

**Regola**  
Quando la Commissione ritiene insufficienti le evidenze, l’effetto giuridico downstream dell’omissione non si applica. Questo modulo deve registrare l’interfaccia sullo stato delle evidenze ove disponibile.

```text
if commission_evidence_insufficient_notification = true:
    evidence_status = insufficient
    natural_source_omission_supported = false
else if evidence_package_submitted = true:
    evidence_status = submitted_no_insufficiency_recorded
```

### REQ-SOURCE-WINTER_SANDING_SALTING_CLASSIFICATION

**Regola**  
Per un dato anno possono essere identificate zone in cui i valori limite di PM10 sono superati a causa della risospensione di particolato in seguito alla sabbiatura o salatura invernale delle strade.

Quando tale identificazione è usata, devono essere forniti l’elenco richiesto delle zone, informazioni sulle concentrazioni e sulle fonti di PM10, evidenze che i superamenti sono dovuti a particolato risospeso e documentazione delle misure ragionevoli adottate.

```text
if pollutant = PM10
AND limit_value_exceedance = true
AND source_category = winter_sanding_or_salting
AND attribution_evidence_sufficient = true
AND reasonable_measures_to_lower_concentrations_documented = true:
    winter_sanding_salting_case = true
else:
    winter_sanding_salting_case = false
```

### REQ-SOURCE-WINTER_SANDING_SALTING_EFFECT_INTERFACE

**Regola**  
Quando i superamenti di PM10 sono attribuibili a sabbiatura o salatura invernale, l’istituzione di un piano per la qualità dell’aria può essere omessa per tali superamenti, salvo quando i superamenti siano attribuibili a fonti di PM10 diverse dalla sabbiatura o salatura invernale.

Questo modulo identifica l’attribuzione e l’eventuale contributo di fonti PM10 non invernali. La conseguenza di pianificazione è determinata da `M_PLANS` o `M_LIMITS`.

```text
if winter_sanding_salting_case = true:
    provide winter_sanding_salting_component
    provide other_PM10_source_contribution_status

if other_PM10_sources_cause_exceedance = true:
    plan_omission_supported_for_winter_component_only = true
```

### REQ-SOURCE-TRANSBOUNDARY_IDENTIFICATION

**Regola**  
Quando il trasporto transfrontaliero di inquinamento atmosferico contribuisce o può contribuire in modo significativo ai superamenti, devono essere identificati le zone o unità territoriali di esposizione media interessate e il contesto del contributo di fonte.

I contributi transfrontalieri possono inoltre supportare motivi di proroga ai sensi dell’Art. 18 quando le condizioni pertinenti sono soddisfatte.

```text
if transboundary_contribution_relevant = true:
    affected_member_states recorded
    affected_zones_or_AETUs recorded
    source_member_state_or_third_country recorded where available
    contribution_estimate_available = true
    cross_boundary_geometry_available = true
```

### REQ-SOURCE-TRANSBOUNDARY_CONTRIBUTION_PACKAGE

**Regola**  
Quando è identificato un contributo transfrontaliero, il modulo deve fornire un pacchetto di contributo idoneo per cooperazione, rendicontazione e valutazione downstream.

```text
if transboundary_case = true:
    provide source_contribution_estimates
    provide affected_area_geometry
    provide affected_zones_or_AETUs
    provide source_categories
    provide method_and_model_metadata
    provide uncertainty_or_confidence_metadata_where_available
```

### REQ-SOURCE-POSTPONEMENT_GROUND_SUPPORT

**Regola**  
Quando contributi transfrontalieri o altri motivi legati al contesto delle fonti sono usati per giustificare una proroga dei termini di conseguimento, il modulo deve fornire evidenze e contesto del contributo a `M_ATTAINMENT_EXTENSION`.

```text
if postponement_request_uses_source_context = true:
    provide source_contribution_evidence_package
    provide affected_pollutant_and_zone
    provide projected_effect_on_attainment
    provide methods_and_data_used
```

### REQ-SOURCE-PLAN_AND_ROADMAP_SOURCE_ANALYSIS

**Regola**  
I piani per la qualità dell’aria e le tabelle di marcia richiedono un contesto dei contributi di fonte sufficiente a individuare misure pertinenti e a dimostrare come i periodi di superamento siano mantenuti il più brevi possibile.

Questo modulo deve fornire categorie di fonti e stime di contributo ove disponibili.

```text
if plan_or_roadmap_trigger = true:
    provide source_contribution_profile
    provide major_source_categories
    provide spatial_distribution_of_contributions where available
    provide temporal_pattern_of_contributions where available
```

### REQ-SOURCE-AVERAGE_EXPOSURE_ADJUSTMENT_SUPPORT

**Regola**  
Quando contributi da fonti naturali incidono sulle concentrazioni medie annue usate per il calcolo dell’IEM/AEI, il modulo deve fornire a `M_EXPOSURE` informazioni sui contributi aggiustati e non aggiustati.

```text
if natural_source_contribution_affects_AEI_input = true:
    provide station_year_contribution_estimate
    provide adjusted_annual_mean_candidate
    provide unadjusted_annual_mean_reference
    provide evidence_package_reference
```

### REQ-SOURCE-MODELLING_EVIDENCE_VALIDITY

**Regola**  
Quando la modellizzazione è usata per l’attribuzione delle fonti, il modello deve essere valido per scopi di attribuzione delle fonti o contributo transfrontaliero e i dati di input devono essere validi per tale scopo.

```text
if source_attribution_uses_modelling = true:
    MODEL_VALID(model, source_attribution_or_transboundary_purpose) = true
    AND input_data_quality_valid = true
```

### REQ-SOURCE-MEASUREMENT_EVIDENCE_VALIDITY

**Regola**  
Le misurazioni usate come evidenza di attribuzione delle fonti devono essere valide per tale scopo e tracciabili a punto di campionamento, inquinante, metrica e periodo.

```text
if source_attribution_uses_measurements = true:
    measurement_data_valid_for_source_attribution = true
    AND sampling_point_metadata_available = true
    AND pollutant_metric_period_identified = true
```

### REQ-SOURCE-UNCERTAINTY_AND_CONFIDENCE

**Regola**  
Le stime dei contributi di fonte devono includere metadati di incertezza o confidenza ove richiesti o disponibili.

```text
if source_contribution_estimate_used_for_regulatory-purpose = true:
    uncertainty_or_confidence_metadata_recorded_where_available = true
```

### REQ-SOURCE-PUBLIC_INFORMATION_INTERFACE

**Regola**  
Quando le informazioni di attribuzione delle fonti sono usate per l’informazione al pubblico, devono essere tracciabili e adatte a una comunicazione chiara, senza oscurare il superamento o il risultato di valutazione sottostante.

```text
if source_attribution_used_for_public_information = true:
    public_information_metadata_ready = true
    source_category_label_available = true
    affected_area_available = true
    pollutant_metric_period_identified = true
```

### REQ-SOURCE-REPORTING_INTERFACE

**Regola**  
Quando l’attribuzione delle fonti è usata per fonti naturali, sabbiatura/salatura invernale, contributo transfrontaliero o rendicontazione di informazioni su concentrazioni/fonti, il modulo deve fornire un pacchetto di evidenze pronto per la rendicontazione.

```text
if attribution_case_reportable = true:
    reporting_payload_ready = true
    affected_zones_or_AETUs included
    concentration_information included
    source_information included
    evidence_reference included
```

### REQ-SOURCE-LEGAL_EFFECT_BOUNDARY

**Regola**  
Questo modulo identifica e documenta l’attribuzione delle fonti. Non svolge direttamente le seguenti funzioni:

- omettere un superamento ai fini della Direttiva;
- determinare lo stato finale di conformità;
- esonerare da un piano per la qualità dell’aria;
- concedere o validare una proroga;
- completare la cooperazione transfrontaliera.

```text
source_attribution_status produced
legal_consequence delegated_to_downstream_module = true
```

## Logica decisionale di attribuzione delle fonti

```text
for each exceedance, exposure input, modelled exceedance area or planning case:
    identify pollutant, metric, period and territorial domain
    classify attribution_purpose
    identify candidate source categories
    collect evidence:
        measurements
        modelling outputs
        event data
        emission/source data
        meteorological or transport data
        activity data
        documentation of measures where required
    validate evidence:
        data quality
        model validity if modelling is used
        traceability
        temporal and spatial consistency
    estimate or document source contribution
    determine attribution status:
        natural_source_case
        winter_sanding_salting_case
        transboundary_case
        other_source_contribution_profile
    prepare outputs for downstream modules
```

## Interazioni con altri moduli

- `M_ZONE` — fornisce zone interessate, unità territoriali di esposizione media e aree transfrontaliere
- `M_LIMITS` — usa lo stato di attribuzione per qualificazione della conformità e stato giuridico dei superamenti
- `M_EXPOSURE` — usa il supporto all’aggiustamento da fonti naturali per input IEM/AEI
- `M_MOD` — fornisce output di contributo modellato, trasporto e ripartizione per fonti
- `M_MODEL_QA` — valida la modellizzazione usata per l’attribuzione
- `M_DATA_QUALITY` — valida misurazioni e dataset usati come evidenze
- `M_REPR` — collega contributi di fonte e aree modellate di superamento alla rappresentatività spaziale
- `M_PLANS` — usa profili di fonte per piani e tabelle di marcia
- `M_ATTAINMENT_EXTENSION` — usa evidenze transfrontaliere o di contesto delle fonti per richieste di proroga
- `M_TRANSBOUNDARY` — usa e coordina casi di contributo transfrontaliero
- `M_PUBLIC_INFORMATION` — usa sintesi del contesto delle fonti per l’informazione al pubblico
- `M_REPORTING` — usa elenchi, informazioni concentrazione/fonte e pacchetti di evidenze pronti per la rendicontazione
- `T_SOURCE_CATEGORIES` — definizioni delle categorie di fonte e regole di classificazione
- `T_ATTRIBUTION_METHODS` — metodi di attribuzione accettati o documentati

## Output

```text
source_attribution_status:
  attribution_case_id
  pollutant
  metric
  assessment_period
  territorial_domain
  affected_zones
  affected_average_exposure_territorial_units
  affected_area_geometry
  source:
    source_category
    source_subcategory
    source_location
    source_member_state_or_third_country
    source_contribution_estimate
    contribution_units
    contribution_uncertainty_or_confidence
  case_status:
    natural_source_case
    winter_sanding_salting_case
    transboundary_case
    other_source_contribution_case
    attribution_evidence_sufficient
    evidence_status
  evidence:
    evidence_package_complete
    measurement_evidence_used
    modelling_evidence_used
    model_validity_status
    data_quality_status
    method_reference
    event_or_activity_documentation
    reasonable_measures_documented_for_winter_case
  legal_effect_interfaces:
    natural_source_omission_candidate
    winter_sanding_salting_plan_omission_candidate
    postponement_ground_support
    transboundary_cooperation_trigger
    planning_source_profile_ready
  downstream:
    limits_input_ready
    exposure_input_ready
    plans_input_ready
    attainment_extension_input_ready
    transboundary_input_ready
    public_information_metadata_ready
    reporting_payload_ready
```

## Note

- L’attribuzione delle fonti è basata su evidenze e specifica per scopo.
- Attribuzione a fonti naturali e attribuzione a sabbiatura/salatura invernale hanno effetti giuridici diversi e non devono essere fuse.
- La sabbiatura/salatura invernale si applica specificamente ai superamenti di PM10 dovuti a risospensione in seguito a sabbiatura o salatura invernale delle strade.
- Il contributo transfrontaliero può supportare obblighi di cooperazione e anche motivi di proroga, ma non concede direttamente la proroga.
- Le concentrazioni aggiustate e non aggiustate devono essere preservate quando i contributi di fonte sono sottratti o omessi per finalità downstream.
