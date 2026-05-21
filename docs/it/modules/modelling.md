# M_MOD — Applicazioni di modellizzazione

## Riferimenti normativi

- Art. 8 Direttiva (UE) 2024/2881
- Art. 9(3), Art. 9(7) Direttiva (UE) 2024/2881
- Art. 11 Direttiva (UE) 2024/2881
- Art. 15 Direttiva (UE) 2024/2881
- Art. 18 Direttiva (UE) 2024/2881
- Art. 19 Direttiva (UE) 2024/2881
- Art. 20 Direttiva (UE) 2024/2881
- Art. 21 Direttiva (UE) 2024/2881
- Allegato I — valori limite, valori-obiettivo, livelli critici, soglie di allarme e informazione
- Allegato IV — ubicazioni di valutazione, criteri di posizionamento e rappresentatività spaziale
- Allegato V — obiettivi di qualità dei dati
- Allegato VI — metodi di riferimento e applicazioni di modellizzazione
- Allegato VIII — piani per la qualità dell’aria e tabelle di marcia, ove sono usate proiezioni
- Atti implementativi ai sensi dell’Art. 8(7), ove applicabili

## Descrizione

Questo modulo disciplina l’**uso regolatorio delle applicazioni di modellizzazione** nel framework di valutazione della qualità dell’aria.

Le applicazioni di modellizzazione possono essere usate per:

- descrivere la distribuzione spaziale delle concentrazioni di inquinanti;
- integrare le misurazioni in siti fissi nelle zone sopra le soglie di valutazione;
- fornire la base primaria o sufficiente di valutazione nelle zone sotto le soglie di valutazione;
- identificare aree modellate di superamento e hotspot;
- determinare se le aree di superamento sono coperte da misurazioni in siti fissi e dalle relative aree di rappresentatività spaziale;
- supportare la riduzione dei punti di campionamento con misurazioni fisse ai sensi dell’Art. 9(3);
- supportare la rilocalizzazione dei punti di campionamento ai sensi dell’Art. 9(7);
- supportare la previsione delle soglie di allarme e informazione;
- supportare piani, tabelle di marcia e richieste di proroga tramite proiezioni;
- supportare l’analisi dei contributi di fonte transfrontalieri.

Il modulo distingue diversi ruoli giuridici della modellizzazione:

```text
assessment_modelling
spatial_distribution_modelling
representativeness_modelling
hotspot_modelling
network_reduction_modelling
relocation_support_modelling
forecast_modelling
projection_modelling
transboundary_contribution_modelling
```

Il modulo non valida in dettaglio le applicazioni di modellizzazione. La validazione e l’assicurazione della qualità dei modelli sono gestite da `M_MODEL_QA`. Questo modulo usa il risultato di tale validazione e determina se la modellizzazione può essere usata per uno specifico scopo regolatorio.

## Ambito

Questo modulo si applica alle applicazioni di modellizzazione usate per:

- valutazione ai sensi dell’Art. 8;
- riduzione dei punti di misurazione fissi ai sensi dell’Art. 9(3);
- supporto alla rilocalizzazione ai sensi dell’Art. 9(7);
- analisi di rappresentatività in `M_REPR`;
- verifica di conformità in `M_LIMITS`;
- informazione al pubblico e previsione delle soglie di allarme/informazione ai sensi degli Art. 15 e 22;
- piani, tabelle di marcia e richieste di proroga ai sensi degli Art. 18 e 19;
- trigger per piani d’azione a breve termine ai sensi dell’Art. 20;
- cooperazione transfrontaliera e analisi dei contributi di fonte ai sensi dell’Art. 21.

## Definizioni

```text
p = inquinante
z = zona
u = unità territoriale di esposizione media o unità territoriale pertinente
x = ubicazione
t = tempo
m = metrica regolatoria
A = area geografica
```

```text
C_model(p,x,t) = concentrazione modellata dell’inquinante p in x al tempo t
C_meas(p,sp,t) = concentrazione misurata dell’inquinante p presso il punto di campionamento sp al tempo t
```

```text
MODEL_VALID(model, purpose) =
il modello soddisfa i requisiti applicabili dell’Allegato VI / M_MODEL_QA
per lo specifico scopo regolatorio
```

```text
MODEL_USABLE(model, purpose) =
MODEL_VALID(model, purpose) = true
AND input_data_quality_valid = true
AND purpose_allowed_by_assessment_regime = true
```

```text
MODELLED_EXCEEDANCE_AREA(p,m,period) =
{ x | C_model(p,x,period,m) > STANDARD(p,m) }
```

```text
HOTSPOT_AREA(p,m,period) =
area in cui concentrazioni modellate o valutate indicano una concentrazione localizzata elevata
o un superamento che richiede attenzione ai fini della valutazione o del monitoraggio
```

```text
FORECAST_VALUE(p,x,t_future) =
concentrazione modellata o prevista per un tempo futuro t_future
```

```text
PROJECTION_SCENARIO(s) =
scenario futuro modellato basato su ipotesi relative a emissioni,
misure, livelli di attività, meteorologia o contributi transfrontalieri
```

## Requisiti normativi

### REQ-MOD-PURPOSE_CLASSIFICATION

**Regola**  
Ogni uso della modellizzazione deve essere classificato per scopo regolatorio prima di essere usato nei workflow di valutazione, conformità, rete, pianificazione, informazione al pubblico o rendicontazione.

```text
if modelling_application_used = true:
    modelling_purpose ∈ [
        assessment_modelling,
        spatial_distribution_modelling,
        representativeness_modelling,
        hotspot_modelling,
        network_reduction_modelling,
        relocation_support_modelling,
        forecast_modelling,
        projection_modelling,
        transboundary_contribution_modelling
    ]
```

### REQ-MOD-MODEL_VALIDITY_REQUIRED

**Regola**  
Le applicazioni di modellizzazione possono essere usate per valutazioni regolatorie solo se soddisfano le condizioni applicabili allo scopo previsto.

```text
if use_model_for_regulatory_purpose = true:
    MODEL_VALID(model, modelling_purpose) = true
    AND input_data_quality_valid = true
```

### REQ-MOD-ASSESSMENT_ABOVE_THRESHOLD

**Regola**  
Nelle zone sopra le soglie di valutazione sono richieste misurazioni in siti fissi. Le applicazioni di modellizzazione possono integrare tali misurazioni per fornire informazioni su distribuzione spaziale e rappresentatività spaziale.

```text
if M_ASSESS.threshold_classification(p,z) = above_threshold:
    model_role = supplementary
    fixed_measurements_required = true
    model_may_support = [
        spatial_distribution,
        spatial_representativeness,
        hotspot_identification,
        coverage_gap_detection
    ]
```

### REQ-MOD-ASSESSMENT_BELOW_THRESHOLD

**Regola**  
Nelle zone sotto le soglie di valutazione, le applicazioni di modellizzazione possono essere sufficienti per la valutazione, da sole o combinate con misurazioni indicative o stima obiettiva.

```text
if M_ASSESS.threshold_classification(p,z) = below_threshold:
    model_role may be primary_assessment_method
    MODEL_USABLE(model, assessment_modelling) = true
```

### REQ-MOD-LIMIT_TARGET_EXCEEDANCE_ASSESSMENT

**Regola**  
Quando i livelli superano un valore limite o un valore-obiettivo pertinente, la qualità dell’aria ambiente deve essere valutata usando applicazioni di modellizzazione o misurazioni indicative. Se si usa modellizzazione, devono essere fornite informazioni su distribuzione spaziale e rappresentatività spaziale delle misurazioni in siti fissi.

```text
if M_LIMITS.exceeds_limit_or_target_value(p,z) = true
AND modelling_applications_used = true:
    spatial_distribution_output_required = true
    spatial_representativeness_output_required = true
```

### REQ-MOD-MODELLING_FREQUENCY_ART8_3

**Regola**  
Quando le applicazioni di modellizzazione sono usate ai sensi dell’Art. 8(3), devono essere effettuate almeno ogni cinque anni, per quanto praticabile.

```text
if modelling_used_under_Art8_3 = true:
    modelling_frequency_ok =
        years_since(previous_modelling_run) <= 5
        OR practicability_exception_documented = true
```

### REQ-MOD-SPATIAL_DISTRIBUTION_OUTPUT

**Regola**  
Quando la modellizzazione è usata per valutazione o integrazione, deve fornire un campo di concentrazione spaziale sufficiente per lo scopo dichiarato.

```text
if model_role includes spatial_distribution:
    model_output includes concentration_field
    concentration_field has documented spatial_resolution
    concentration_field covers relevant assessment_domain
```

### REQ-MOD-REPRESENTATIVENESS_SUPPORT

**Regola**  
Quando la modellizzazione supporta la rappresentatività spaziale, deve fornire a `M_REPR` il campo di concentrazione e le evidenze spaziali necessari per determinare aree di rappresentatività e copertura delle aree modellate di superamento.

```text
if model_role includes representativeness_modelling:
    provide C_field(p,m,x,period)
    provide spatial_resolution
    provide uncertainty_or_confidence_metadata_where_available
    provide model_validity_status
```

### REQ-MOD-MODELLED_EXCEEDANCE_AREA_DELINEATION

**Regola**  
Quando le applicazioni di modellizzazione indicano un superamento, l’area modellata di superamento deve essere delineata e trasmessa a `M_REPR` e `M_ASSESS`.

```text
if any x in assessment_domain satisfies C_model(p,x,m,period) > STANDARD(p,m):
    MODELLED_EXCEEDANCE_AREA(p,m,period) defined
    modelled_exceedance_area_geometry is not null
    modelled_exceedance_area_metadata recorded
```

### REQ-MOD-MODELLED_EXCEEDANCE_STATUS_LINK

**Regola**  
Un superamento modellato può essere valutato come non superamento solo quando `M_REPR` determina che misurazioni in siti fissi con rappresentatività spaziale valida coprono l’area modellata di superamento. Questo modulo produce l’area modellata di superamento, ma non decide lo stato giuridico di conformità.

```text
if modelled_exceedance_area_defined = true:
    send MODELLED_EXCEEDANCE_AREA to M_REPR
    receive fixed_measurements_cover_modelled_exceedance_area
    send modelled_exceedance_package to M_LIMITS
```

### REQ-MOD-HOTSPOT_IDENTIFICATION

**Regola**  
Le applicazioni di modellizzazione usate per valutazione spaziale devono identificare superamenti localizzati o hotspot rilevanti per la copertura della rete e per le decisioni sulle misurazioni aggiuntive.

```text
if model_role includes hotspot_modelling:
    HOTSPOT_AREA(p,m,period) defined where applicable
    hotspot_covered_by_fixed_measurements = M_REPR.coverage_status(HOTSPOT_AREA)
```

### REQ-MOD-ADDITIONAL_MEASUREMENT_TRIGGER_LINK

**Regola**  
Quando la modellizzazione mostra un superamento non coperto da misurazioni in siti fissi e dalle rispettive aree di rappresentatività, l’area non coperta deve essere fornita a `M_NETWORK` per supportare misurazioni aggiuntive fisse o indicative.

```text
if M_REPR.coverage_status(MODELLED_EXCEEDANCE_AREA) = not_fully_covered_by_fixed_measurements:
    provide uncovered_modelled_exceedance_area to M_NETWORK
    provide modelled_exceedance_date
    provide pollutant, metric, period and source model metadata
```

### REQ-MOD-NETWORK_REDUCTION_SUPPORT

**Regola**  
Quando la modellizzazione supporta la riduzione dei punti di campionamento con misurazioni fisse, deve fornire informazioni sufficienti rispetto a valori limite, valori-obiettivo, livelli critici, soglie di allarme e soglie di informazione, nonché informazione adeguata al pubblico. Deve inoltre avere risoluzione spaziale sufficiente e rispettare le condizioni applicabili di qualità dei dati e modellizzazione.

```text
if model_role = network_reduction_modelling:
    MODEL_USABLE(model, network_reduction_modelling) = true
    AND assessment_information_sufficient_for_regulatory_values = true
    AND assessment_information_sufficient_for_alert_information_thresholds = true
    AND spatial_resolution_sufficient = true
    AND public_information_sufficient = true
    AND M_REPR.network_reduction_spatial_coverage_ok = true
```

### REQ-MOD-RELOCATION_SUPPORT

**Regola**  
Quando un punto di campionamento con superamenti registrati è rilocalizzato, la modellizzazione può supportare la rilocalizzazione solo se dimostra l’effetto spaziale della rilocalizzazione e supporta una giustificazione documentata.

```text
if relocation_support_modelling = true:
    provide old_area_coverage_evidence
    provide new_area_coverage_evidence
    provide coverage_loss_or_gain_estimate
    provide exceedance_area_coverage_effect
    MODEL_VALID(model, relocation_support_modelling) = true
```

### REQ-MOD-MEASUREMENT_MODEL_CONFLICT

**Regola**  
Quando concentrazioni modellate e misurate portano a conclusioni diverse sul superamento, il conflitto deve essere gestito tramite le regole di rappresentatività e conformità. Una misurazione in sito fisso non prevale automaticamente su un superamento modellato per l’intera zona.

```text
if modelled_exceedance = true
AND fixed_measurement_non_exceedance = true:
    if M_REPR.fixed_measurements_cover_modelled_exceedance_area = true:
        conflict_status = fixed_measurement_may_support_non_exceedance_evaluation
    else:
        conflict_status = modelled_exceedance_remains_relevant_for_uncovered_area
```

### REQ-MOD-MODEL_ONLY_USE

**Regola**  
La modellizzazione può essere usata come metodo unico o primario di valutazione quando il regime di valutazione applicabile lo consente e l’applicazione di modellizzazione è valida per tale uso.

```text
if no_valid_fixed_measurement_coverage = true
AND M_ASSESS.regime_allows_model_only_assessment = true:
    model_only_assessment_allowed = MODEL_USABLE(model, assessment_modelling)
```

### REQ-MOD-FORECAST_ALERT_INFORMATION_THRESHOLDS

**Regola**  
Quando strumenti di modellizzazione o previsione prevedono il superamento di soglie di allarme o informazione, la previsione deve essere esposta ai moduli responsabili dell’informazione al pubblico e dei piani d’azione a breve termine.

```text
if FORECAST_VALUE(p,x,t_future) > ALERT_THRESHOLD(p,m):
    alert_threshold_predicted = true
    public_information_trigger_input = true
    short_term_action_trigger_input = true

if FORECAST_VALUE(p,x,t_future) > INFORMATION_THRESHOLD(p,m):
    information_threshold_predicted = true
    public_information_trigger_input = true
```

### REQ-MOD-PROJECTIONS_FOR_ROADMAPS_AND_PLANS

**Regola**  
Quando le proiezioni modellistiche supportano tabelle di marcia, piani per la qualità dell’aria o richieste di proroga, devono documentare scenari, ipotesi, misure, date previste di conseguimento e metadati di incertezza o confidenza ove disponibili.

```text
if model_role = projection_modelling:
    projection_scenarios_defined = true
    assumptions_documented = true
    measures_considered_documented = true
    projected_attainment_date_available = true
    methods_and_data_used_for_projections_justified = true
```

### REQ-MOD-POSTPONEMENT_SUPPORT

**Regola**  
Quando le proiezioni sono fornite come motivazione per una proroga dei termini di conseguimento, questo modulo deve fornire a `M_ATTAINMENT_EXTENSION` il pacchetto di proiezione e la documentazione necessari.

```text
if postponement_request_uses_projections = true:
    provide projection_package:
        baseline_scenario
        measures_scenario
        expected_impact_of_measures
        projected_attainment_date
        methods_used
        data_used
        assumptions
        uncertainty_or_confidence_metadata_where_available
```

### REQ-MOD-TRANSBOUNDARY_CONTRIBUTION_SUPPORT

**Regola**  
Quando il trasporto transfrontaliero di inquinamento atmosferico contribuisce o può contribuire in modo significativo ai superamenti, la modellizzazione può essere usata per stimare contributi di fonte, aree interessate e impatti transfrontalieri sulle concentrazioni.

```text
if model_role = transboundary_contribution_modelling:
    source_contribution_estimates_available = true
    affected_zones_or_territorial_units_identified = true
    cross_boundary_geometry_available = true
    contribution_uncertainty_metadata_recorded_where_available = true
```

### REQ-MOD-SOURCE_ATTRIBUTION_SUPPORT

**Regola**  
La modellizzazione può supportare l’attribuzione delle fonti per fonti naturali, sabbiatura o salatura invernale, riscaldamento domestico, contributi transfrontalieri o altre categorie di fonti, ma questo modulo non decide lo stato giuridico dell’attribuzione.

```text
if source_attribution_modelling_used = true:
    provide source_contribution_evidence_package
    provide concentration_contribution_estimates
    provide spatial_and_temporal_scope
    provide model_validity_status
```

### REQ-MOD-PUBLIC_INFORMATION_SUPPORT

**Regola**  
Gli output modellistici usati per l’informazione al pubblico devono conservare metadati sufficienti a comunicare la base spaziale e temporale della valutazione, delle previsioni o degli input dell’indice di qualità dell’aria.

```text
if model_output_used_for_public_information = true:
    public_information_metadata_ready = true
    include pollutant
    include metric
    include spatial_domain
    include time_period
    include model_purpose
    include confidence_or_uncertainty_metadata_where_available
```

### REQ-MOD-REPORTING_SUPPORT

**Regola**  
Gli output modellistici usati nelle valutazioni rendicontate devono conservare metadati sufficienti per la tracciabilità, inclusi scopo del modello, dominio, risoluzione, periodo, dati di input, stato di validazione e aree di superamento pertinenti.

```text
if model_output_used_for_reporting = true:
    reporting_metadata_ready = true
    model_traceability_package_complete = true
```

### REQ-MOD-VERSIONING_AND_REPRODUCIBILITY

**Regola**  
Le applicazioni di modellizzazione regolatorie devono essere versionate e riproducibili per il periodo di valutazione o proiezione cui si applicano.

```text
model_run_record includes:
    model_identifier
    model_version
    run_date
    assessment_period
    spatial_domain
    spatial_resolution
    input_data_versions
    scenario_identifier_if_applicable
    validation_status
    purpose
```

### REQ-MOD-UNCERTAINTY_AND_CONFIDENCE_METADATA

**Regola**  
Gli output modellistici devono includere metadati di incertezza, confidenza o qualità quando richiesti dall’Allegato V, dall’Allegato VI o dalla metodologia di modellizzazione applicabile.

```text
if model_uncertainty_required_for_purpose = true:
    model_uncertainty_metadata_available = true
```

## Logica decisionale a livello di modellizzazione

```text
for each proposed model use:
    classify modelling_purpose
    verify MODEL_VALID(model, modelling_purpose)
    verify input_data_quality_valid
    verify purpose_allowed_by_assessment_regime

    if purpose = assessment_modelling:
        provide concentration_field
        provide exceedance areas where applicable

    if purpose = representativeness_modelling:
        provide C_field and metadata to M_REPR

    if purpose = hotspot_modelling:
        identify HOTSPOT_AREA and coverage status

    if purpose = network_reduction_modelling:
        verify Art9_3 information sufficiency and spatial resolution

    if purpose = relocation_support_modelling:
        provide old/new coverage evidence and relocation support package

    if purpose = forecast_modelling:
        provide predicted alert/information threshold exceedance triggers

    if purpose = projection_modelling:
        provide scenario and projected attainment package

    if purpose = transboundary_contribution_modelling:
        provide source contribution and cross-boundary impact package
```

## Interazioni con altri moduli

- `M_ASSESS` — determina quando la modellizzazione è richiesta, consentita o sufficiente
- `M_REPR` — usa campi di concentrazione modellati e aree di superamento per decisioni di rappresentatività e copertura
- `M_NETWORK` — usa output su hotspot, gap di copertura, riduzione e rilocalizzazione
- `M_LIMITS` — usa aree modellate di superamento e output del modello per la qualificazione della conformità
- `M_MODEL_QA` — valida idoneità e prestazioni del modello per lo scopo previsto
- `M_DATA_QUALITY` — valida dati di input e dati di validazione del modello
- `M_SOURCE_ATTRIBUTION` — usa evidenze modellistiche sui contributi di fonte
- `M_ATTAINMENT_EXTENSION` — usa pacchetti di proiezione per la valutazione della proroga ex Art. 18
- `M_PLANS` — usa proiezioni, distribuzioni spaziali e scenari di misure per piani e tabelle di marcia
- `M_SHORT_TERM_ACTION` — usa trigger previsionali di superamento
- `M_TRANSBOUNDARY` — usa output su contributo transfrontaliero e impatti oltre confine
- `M_PUBLIC_INFORMATION` — usa previsioni, output modellistici e metadati per la comunicazione al pubblico
- `M_REPORTING` — usa pacchetti modellistici tracciabili per la rendicontazione alla Commissione

## Output

```text
model_output:
  model_id
  model_version
  model_purpose
  pollutant
  metric
  assessment_period
  spatial_domain
  spatial_resolution
  validity:
    model_valid
    validation_reference
    input_data_quality_valid
    AnnexVI_conditions_satisfied
    uncertainty_or_confidence_metadata
  concentration:
    concentration_field
    time_resolution
    averaging_period
  exceedance:
    modelled_exceedance_detected
    modelled_exceedance_area_geometry
    hotspot_area_geometry
    exceedance_standard_type
    exceedance_value
  representativeness_support:
    C_field_for_M_REPR
    coverage_gap_candidate_areas
    spatial_representativeness_metadata
  network_support:
    network_reduction_support
    relocation_support
    additional_measurement_target_area
  forecast:
    alert_threshold_predicted
    information_threshold_predicted
    forecast_period
    public_information_trigger_input
    short_term_action_trigger_input
  projection:
    scenario_id
    baseline_scenario
    measures_scenario
    projected_attainment_date
    methods_and_data_justification
    assumptions
  transboundary:
    source_contribution_estimates
    affected_member_states
    cross_boundary_geometry
  traceability:
    run_date
    input_data_versions
    scenario_identifier
    reproducibility_package_available
    reporting_metadata_ready
```

## Note

- La modellizzazione non sostituisce automaticamente le misurazioni in siti fissi nelle zone in cui esse sono richieste.
- Un superamento modellato non è automaticamente ignorato quando esiste una misurazione in sito fisso nella zona: è richiesta la copertura da parte dell’area di rappresentatività valida della misurazione.
- La modellizzazione può essere primaria, supplementare, previsionale o proiettiva a seconda dello scopo giuridico.
- La riduzione della rete ai sensi dell’Art. 9(3) richiede più di un modello valido: richiede informazione regolatoria sufficiente, risoluzione spaziale, conformità alla qualità dei dati e adeguatezza dell’informazione al pubblico.
- Previsioni e proiezioni sono distinte: le previsioni supportano soglie di allarme/informazione e trigger di azione a breve termine; le proiezioni supportano tabelle di marcia, piani e richieste di proroga.
- Questo modulo fornisce pacchetti di evidenze e output modellistici; lo stato giuridico finale è determinato dai moduli downstream.
