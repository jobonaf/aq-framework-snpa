# M_MODEL_QA — Validazione e assicurazione della qualità delle applicazioni di modellizzazione

## Riferimenti normativi

- Art. 4(24) Direttiva (UE) 2024/2881 — definizione di applicazione di modellizzazione
- Art. 8 Direttiva (UE) 2024/2881 — uso di applicazioni di modellizzazione per valutazione, distribuzione spaziale, rappresentatività e superamenti modellati
- Art. 9(3), Art. 9(7) Direttiva (UE) 2024/2881 — supporto modellistico per riduzione delle misurazioni fisse e rilocalizzazione dei punti di campionamento
- Art. 11(2), Art. 11(3) Direttiva (UE) 2024/2881 — applicazioni di modellizzazione e obiettivi di qualità dei dati
- Art. 15 Direttiva (UE) 2024/2881 — previsione dei superamenti delle soglie di allarme e informazione
- Art. 18 Direttiva (UE) 2024/2881 — proiezioni usate per richieste di proroga
- Art. 19 Direttiva (UE) 2024/2881 — proiezioni e scenari usati per piani e tabelle di marcia
- Art. 21 Direttiva (UE) 2024/2881 — analisi del contributo transfrontaliero
- Art. 22–23 Direttiva (UE) 2024/2881 — interfacce di informazione al pubblico e rendicontazione
- Allegato V — obiettivi di qualità dei dati
- Allegato VI — metodi di riferimento e condizioni per la modellizzazione
- Allegato VIII — piani e tabelle di marcia per la qualità dell’aria, ove sono usate proiezioni
- Atti implementativi ai sensi dell’Art. 8(7), ove applicabili

## Descrizione

Questo modulo definisce i **requisiti regolatori di validazione e assicurazione della qualità** per le applicazioni di modellizzazione usate nel framework per la qualità dell’aria.

Un’applicazione di modellizzazione è trattata come una catena di modelli e sotto-modelli, inclusi tutti i dati di input e il post-processing necessari. La validazione è quindi specifica per scopo e deve coprire il sistema di modellizzazione, i dati di input, i dati di valutazione, il periodo di valutazione e l’uso regolatorio previsto.

Il modulo determina se gli output modellistici possono essere usati per:

- valutazione della qualità dell’aria ambiente;
- distribuzione spaziale delle concentrazioni;
- rappresentatività spaziale;
- delineazione dell’area modellata di superamento;
- identificazione di hotspot;
- riduzione dei punti di misurazione fissi;
- supporto alla rilocalizzazione;
- verifica di conformità;
- previsioni dei superamenti delle soglie di allarme o informazione;
- proiezioni per piani, tabelle di marcia e richieste di proroga;
- attribuzione delle fonti e analisi del contributo transfrontaliero;
- informazione al pubblico e rendicontazione.

Questo modulo **non** decide la conseguenza giuridica degli output modellistici. Determina solo se l’applicazione di modellizzazione è valida e idonea allo scopo regolatorio previsto. L’uso giuridico finale è gestito da `M_MOD`, `M_REPR`, `M_ASSESS`, `M_NETWORK`, `M_LIMITS`, `M_PLANS`, `M_ATTAINMENT_EXTENSION`, `M_PUBLIC_INFORMATION` e `M_REPORTING`.

## Ambito

Il modulo si applica alle applicazioni di modellizzazione usate per i seguenti scopi:

```text
assessment_modelling
spatial_distribution_modelling
representativeness_modelling
modelled_exceedance_delineation
hotspot_modelling
network_reduction_modelling
relocation_support_modelling
compliance_support_modelling
forecast_modelling
projection_modelling
source_attribution_modelling
transboundary_contribution_modelling
public_information_modelling
reporting_modelling
```

La validazione deve essere eseguita separatamente per scopo previsto, inquinante, metrica regolatoria, periodo di valutazione e dominio spaziale.

## Definizioni

```text
model_application =
catena del sistema di modellizzazione, inclusi modelli, sotto-modelli, dati di input e post-processing
```

```text
MODEL_PURPOSE =
scopo regolatorio per cui è usato l’output del modello
```

```text
OBS_VALID(p,m,period) =
osservazioni che soddisfano i requisiti di qualità dei dati in M_DATA_QUALITY
per l’inquinante p, la metrica m e il periodo di valutazione
```

```text
validation_data =
insieme di osservazioni o dataset indipendenti usati per valutare le prestazioni del modello
```

```text
model_input_data =
dati usati per inizializzare, vincolare, addestrare, calibrare, assimilare o alimentare l’applicazione di modellizzazione
```

```text
U_meas(sp,p,m) = incertezza della misura al punto di campionamento sp
U_model(sp,p,m) = incertezza della modellizzazione al punto sp
RMSE(sp,p,m) = errore quadratico medio tra valori modellati e osservati
MQI(sp,p,m) = RMSE(sp,p,m) / sqrt(U_model(sp,p,m)^2 + U_meas(sp,p,m)^2)
N_val = numero di punti o ubicazioni indipendenti di validazione disponibili
```

```text
MODEL_VALID(model_application, MODEL_PURPOSE, p, m, domain, period) =
il modello soddisfa i criteri applicabili di validazione, qualità dei dati,
tracciabilità e idoneità specifica per scopo
```

## Requisiti normativi

### REQ-MODELQA-PURPOSE_SPECIFIC_VALIDATION

**Regola**  
La validazione deve essere eseguita per lo specifico scopo regolatorio per cui l’applicazione di modellizzazione è usata. Un modello valido per uno scopo non è automaticamente valido per un altro.

```text
if model_output_used_for_regulatory_purpose = true:
    MODEL_PURPOSE assigned
    MODEL_VALID(model_application, MODEL_PURPOSE, p, m, domain, period) evaluated
```

### REQ-MODELQA-MODEL_CHAIN_DEFINITION

**Regola**  
L’applicazione di modellizzazione deve essere documentata come catena modellistica completa, inclusi modelli, sotto-modelli, dati di input e post-processing.

```text
model_application_record includes:
    model_id
    model_version
    sub_models
    input_data_sources
    post_processing_steps
    spatial_domain
    temporal_domain
    pollutant
    metric
    purpose
```

### REQ-MODELQA-DATA_VALIDITY

**Regola**  
Solo osservazioni e dataset che soddisfano i requisiti di qualità dei dati applicabili possono essere usati per la validazione dei modelli.

```text
for each obs in validation_data:
    OBS_VALID(obs,p,m,period) = true
```

### REQ-MODELQA-DATA_INDEPENDENCE

**Regola**  
I dati di validazione devono essere indipendenti dai dati di input del modello, salvo che il metodo di validazione applicabile documenti e controlli esplicitamente la dipendenza.

```text
validation_data ∩ model_input_data = ∅
```

**Qualificazione**

```text
if validation_data_not_fully_independent = true:
    independence_status = limited_independence
    limitation_documented = true
```

### REQ-MODELQA-MQI_DEFINITION

**Regola**  
Quando si applica l’indicatore di qualità del modello, la qualità del modello deve essere valutata usando il rapporto tra errore del modello e incertezza totale.

```text
MQI(sp,p,m) =
    RMSE(sp,p,m) / sqrt(U_model(sp,p,m)^2 + U_meas(sp,p,m)^2)
```

### REQ-MODELQA-MQI_THRESHOLD

**Regola**  
Un modello soddisfa l’obiettivo di qualità basato su MQI in un punto di validazione se l’MQI non supera uno.

```text
MQI_pass(sp,p,m) = MQI(sp,p,m) <= 1
```

### REQ-MODELQA-MQI_COVERAGE_90_PERCENT

**Regola**  
Quando sono disponibili dieci o più punti di validazione indipendenti, il criterio MQI deve essere soddisfatto in almeno il 90% dei punti di validazione disponibili nell’area e nel periodo di valutazione.

```text
if N_val >= 10:
    COUNT{ sp | MQI(sp,p,m) <= 1 } / N_val >= 0.9
```

### REQ-MODELQA-LOW_VALIDATION_POINT_COUNT

**Regola**  
Quando sono disponibili meno di dieci punti di validazione indipendenti, il modello deve soddisfare il criterio MQI in tutti i punti disponibili.

```text
if N_val < 10:
    for all sp in validation_points:
        MQI(sp,p,m) <= 1
```

### REQ-MODELQA-METRIC_SPECIFIC_VALIDATION

**Regola**  
La validazione deve essere eseguita per la metrica regolatoria e il periodo di mediazione applicabili. La validazione per medie annue non è automaticamente valida per metriche di breve periodo, soglie di allarme o soglie di informazione.

```text
if model_output_used_for_metric m:
    validation_metric = m
    validation_period matches regulatory_averaging_period(m)
```

### REQ-MODELQA-SPATIAL_DOMAIN_VALIDATION

**Regola**  
La validazione deve essere appropriata al dominio spaziale per cui il modello è usato.

```text
MODEL_VALID_FOR_DOMAIN =
    validation_domain covers intended_model_use_domain
    OR domain_transfer_justification_documented = true
```

### REQ-MODELQA-SPATIAL_RESOLUTION_FITNESS

**Regola**  
La risoluzione spaziale dell’applicazione di modellizzazione deve essere sufficiente per lo scopo previsto.

```text
if MODEL_PURPOSE in [
    spatial_distribution_modelling,
    representativeness_modelling,
    modelled_exceedance_delineation,
    hotspot_modelling,
    network_reduction_modelling,
    relocation_support_modelling
]:
    spatial_resolution_sufficient = true
```

### REQ-MODELQA-HOTSPOT_FITNESS

**Regola**  
Un modello usato per identificare hotspot di inquinamento atmosferico deve essere idoneo a rappresentare concentrazioni localizzate elevate e i pertinenti contesti di fonte.

```text
if MODEL_PURPOSE = hotspot_modelling:
    hotspot_scale_represented = true
    AND relevant_source_context_represented = true
    AND spatial_resolution_sufficient = true
```

### REQ-MODELQA-NETWORK_REDUCTION_FITNESS

**Regola**  
Un modello usato per supportare la riduzione dei punti di campionamento con misurazioni fisse deve essere valido per i valori regolatori e le soglie pertinenti all’Art. 9(3) e deve fornire adeguate informazioni spaziali e supporto all’informazione al pubblico.

```text
if MODEL_PURPOSE = network_reduction_modelling:
    MODEL_VALID(model, network_reduction_modelling, p, m, domain, period) = true
    AND spatial_resolution_sufficient = true
    AND assessment_information_sufficient_for_regulatory_values = true
    AND assessment_information_sufficient_for_alert_information_thresholds = true
    AND public_information_sufficient = true
```

### REQ-MODELQA-REPRESENTATIVENESS_FITNESS

**Regola**  
Un modello usato per la rappresentatività spaziale deve essere valido per determinare similarità di concentrazione, aree modellate di superamento e copertura da parte delle misurazioni in siti fissi.

```text
if MODEL_PURPOSE = representativeness_modelling:
    concentration_field_valid_for_repr = true
    AND spatial_resolution_sufficient = true
    AND validation_metric_matches_repr_metric = true
```

### REQ-MODELQA-MODELLED_EXCEEDANCE_DELINEATION_FITNESS

**Regola**  
Un modello usato per delineare aree modellate di superamento deve essere valido per l’inquinante, la metrica, lo standard e il dominio spaziale pertinenti.

```text
if MODEL_PURPOSE = modelled_exceedance_delineation:
    validation_metric matches exceedance_standard_metric
    AND spatial_domain_valid = true
    AND spatial_resolution_sufficient = true
```

### REQ-MODELQA-RELOCATION_SUPPORT_FITNESS

**Regola**  
Un modello usato per supportare la rilocalizzazione dei punti di campionamento deve essere valido per confrontare la copertura prima e dopo la rilocalizzazione e per valutare la perdita di copertura dell’area di superamento.

```text
if MODEL_PURPOSE = relocation_support_modelling:
    old_location_context_represented = true
    new_location_context_represented = true
    coverage_change_assessment_supported = true
```

### REQ-MODELQA-FORECAST_FITNESS

**Regola**  
I modelli previsionali usati per prevedere il superamento delle soglie di allarme o informazione devono essere validati o sottoposti a controllo qualità per l’orizzonte previsionale, l’inquinante, la soglia e l’area pertinenti.

```text
if MODEL_PURPOSE = forecast_modelling:
    forecast_horizon_documented = true
    threshold_metric_supported = true
    forecast_quality_metadata_available = true
```

### REQ-MODELQA-PROJECTION_FITNESS

**Regola**  
I modelli di proiezione usati per piani, tabelle di marcia o richieste di proroga devono documentare scenari, ipotesi, dati di input, misure, date previste di conseguimento e metadati di incertezza o confidenza ove disponibili.

```text
if MODEL_PURPOSE = projection_modelling:
    scenarios_documented = true
    assumptions_documented = true
    measures_documented = true
    input_data_versions_recorded = true
    projected_attainment_date_available = true
    methods_and_data_used_justified = true
```

### REQ-MODELQA-SOURCE_ATTRIBUTION_AND_TRANSBOUNDARY_FITNESS

**Regola**  
I modelli usati per attribuzione delle fonti o analisi del contributo transfrontaliero devono essere idonei a stimare contributi di fonte, aree interessate e impatti transfrontalieri.

```text
if MODEL_PURPOSE in [source_attribution_modelling, transboundary_contribution_modelling]:
    source_categories_documented = true
    contribution_method_documented = true
    affected_area_geometry_available = true
    contribution_uncertainty_metadata_recorded_where_available = true
```

### REQ-MODELQA-VALIDATION_METHOD_DOCUMENTATION

**Regola**  
Il metodo di validazione deve essere documentato. Leave-One-Out Cross-Validation o un altro metodo appropriato può essere usato quando adatto allo scopo del modello e al contesto dei dati.

```text
validation_method documented
validation_period documented
validation_locations documented
validation_metric documented
```

### REQ-MODELQA-USAGE_CONSTRAINT

**Regola**  
I risultati di un modello non valido per lo scopo regolatorio previsto non devono essere usati per tale scopo.

```text
if MODEL_VALID(model_application, MODEL_PURPOSE, p, m, domain, period) = false:
    model_output not usable for MODEL_PURPOSE
```

### REQ-MODELQA-VERSIONING_AND_REPRODUCIBILITY

**Regola**  
Le esecuzioni modellistiche usate per scopi regolatori devono essere versionate, tracciabili e riproducibili.

```text
model_run_record includes:
    model_id
    model_version
    model_chain_version
    input_data_versions
    post_processing_version
    run_date
    assessment_period
    spatial_domain
    spatial_resolution
    MODEL_PURPOSE
    validation_status
```

### REQ-MODELQA-QUALITY_METADATA_FOR_PUBLIC_INFORMATION_AND_REPORTING

**Regola**  
Quando gli output del modello sono usati per informazione al pubblico o rendicontazione, lo stato di validazione e i metadati di qualità devono essere disponibili per tracciabilità e spiegazione.

```text
if model_output_used_for_public_information_or_reporting = true:
    validation_status_available = true
    model_quality_metadata_available = true
    model_traceability_package_complete = true
```

## Logica decisionale di validazione del modello

```text
for each model_application and intended MODEL_PURPOSE:
    document full model chain
    identify pollutant, metric, period and spatial domain
    verify data validity of validation data
    verify independence of validation data
    select validation method

    if MQI applies:
        calculate MQI at validation points
        if N_val >= 10:
            require MQI <= 1 at at least 90% of validation points
        if N_val < 10:
            require MQI <= 1 at all validation points

    verify purpose-specific fitness:
        spatial resolution
        metric match
        domain match
        hotspot, reduction, relocation, forecast, projection or attribution requirements where relevant

    verify versioning and traceability

    if all applicable checks pass:
        MODEL_VALID = true
    else:
        MODEL_VALID = false

    expose validation status and blocking reasons to downstream modules
```

## Interazioni con altri moduli

- `M_DATA_QUALITY` — valida osservazioni, dataset di validazione e dati di input del modello
- `M_MOD` — usa `MODEL_VALID` e lo stato di validazione specifico per scopo
- `M_REPR` — usa solo modelli validi per rappresentatività, campi di concentrazione e aree di superamento
- `M_LIMITS` — accetta evidenze di superamento modellato solo quando il modello è valido per il supporto alla conformità pertinente
- `M_NETWORK` — usa modelli validi per riduzione, rilocalizzazione e supporto a misurazioni aggiuntive
- `M_ASSESS` — usa la validità del modello per decisioni sui metodi di valutazione
- `M_PLANS` — usa modelli validi per proiezioni e analisi di scenario
- `M_ATTAINMENT_EXTENSION` — usa lo stato di validazione delle proiezioni per richieste di proroga
- `M_SHORT_TERM_ACTION` — usa lo stato di validazione delle previsioni
- `M_TRANSBOUNDARY` — usa lo stato di validazione del contributo transfrontaliero
- `M_SOURCE_ATTRIBUTION` — usa lo stato di validazione dell’attribuzione delle fonti
- `M_PUBLIC_INFORMATION` — usa metadati di qualità previsionale/modellistica
- `M_REPORTING` — usa metadati di validazione e tracciabilità
- `T_DATA_QUALITY` — archivia obiettivi di incertezza per misurazione e modellizzazione
- `T_MODEL_QA` — archivia soglie di validazione specifiche per scopo, se separate da `T_DATA_QUALITY`

## Output

```text
model_validation:
  model_id
  model_version
  model_chain_version
  model_purpose
  pollutant
  metric
  assessment_period
  spatial_domain

  validation_data:
    N_val
    validation_points
    validation_period
    validation_data_quality_valid
    independence_status = independent | limited_independence | not_independent

  method:
    validation_method
    MQI_applies
    RMSE_summary
    U_model_summary
    U_meas_summary
    MQI_summary
    MQI_pass_rate

  purpose_fitness:
    metric_specific_validation_ok
    spatial_domain_validation_ok
    spatial_resolution_sufficient
    hotspot_fitness
    network_reduction_fitness
    representativeness_fitness
    exceedance_delineation_fitness
    relocation_support_fitness
    forecast_fitness
    projection_fitness
    source_attribution_fitness
    transboundary_fitness

  validity:
    valid
    valid_for_purposes
    invalid_for_purposes
    blocking_reasons

  traceability:
    input_data_versions
    post_processing_version
    run_date
    reproducibility_package_available
    public_information_quality_metadata_ready
    reporting_quality_metadata_ready
```

## Note

- La validazione dei modelli è specifica per scopo. Un modello valido per la valutazione spaziale annuale non è automaticamente valido per previsioni a breve termine, individuazione di hotspot o proiezioni.
- La regola MQI è mantenuta come test regolatorio centrale di qualità ove applicabile, ma per gli usi introdotti dagli Art. 8, 9, 15, 18, 19 e 21 sono necessari ulteriori controlli di idoneità specifica.
- La validazione richiede osservazioni valide e indipendenti, salvo accettazione esplicita di una limitazione documentata per lo scopo pertinente.
- Questo modulo fornisce `MODEL_VALID`; non decide se un superamento modellato costituisca violazione di conformità o se un piano, una tabella di marcia o una proroga siano validi.
