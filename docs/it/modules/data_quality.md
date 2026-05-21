# M_DATA_QUALITY — Qualità dei dati e validità dei dati di valutazione

## Riferimenti normativi

- Art. 4 Direttiva (UE) 2024/2881 — definizioni di misurazioni in siti fissi, misurazioni indicative, applicazioni di modellizzazione e valutazione
- Art. 8(6) Direttiva (UE) 2024/2881 — misurazioni aggiuntive dopo superamento modellato e collegamento con la copertura minima
- Art. 9(3) Direttiva (UE) 2024/2881 — riduzione dei punti di misurazione fissi mediante modellizzazione o misurazioni indicative
- Art. 11 Direttiva (UE) 2024/2881 — metodi di riferimento, applicazioni di modellizzazione e obiettivi di qualità dei dati
- Art. 15 Direttiva (UE) 2024/2881 — soglie di allarme e di informazione, inclusi i superamenti previsti
- Art. 18 Direttiva (UE) 2024/2881 — proiezioni usate per richieste di proroga
- Art. 19 Direttiva (UE) 2024/2881 — piani per la qualità dell’aria e tabelle di marcia
- Art. 20 Direttiva (UE) 2024/2881 — piani d’azione a breve termine
- Art. 22–23 Direttiva (UE) 2024/2881 — interfacce di informazione al pubblico e rendicontazione
- Allegato V — obiettivi di qualità dei dati
- Allegato VI — metodi di riferimento e condizioni per la modellizzazione
- Allegato VIII — piani e tabelle di marcia per la qualità dell’aria, ove sono usate proiezioni

## Descrizione

Questo modulo definisce le **condizioni di validità regolatoria dei dati e delle evidenze di valutazione** usati nel framework per la qualità dell’aria.

Determina se dati di misurazione, misurazione indicativa, input modellistici, validazione dei modelli, previsione e proiezione possono essere usati per:

- classificazione della valutazione;
- verifica di conformità e superamento;
- rappresentatività spaziale;
- applicazioni di modellizzazione;
- riduzione dei punti di misurazione fissi;
- misurazioni aggiuntive dopo superamenti modellati;
- informazione al pubblico;
- rendicontazione;
- piani per la qualità dell’aria, tabelle di marcia e richieste di proroga.

Il modulo **non**:

- determina il regime di valutazione;
- progetta la rete di monitoraggio;
- valida i modelli in quanto modelli;
- determina le geometrie di rappresentatività spaziale;
- decide lo stato giuridico di conformità;
- istituisce piani, tabelle di marcia o piani d’azione a breve termine.

Tali funzioni sono gestite da moduli dedicati.

## Ambito

Il modulo si applica a:

- misurazioni in siti fissi;
- misurazioni indicative;
- misurazioni fisse aggiuntive;
- misurazioni indicative aggiuntive;
- misurazioni usate per indicatori di esposizione media;
- misurazioni usate presso supersiti;
- dati usati per la validazione dei modelli;
- dati di input usati nelle applicazioni di modellizzazione;
- dati previsionali usati per soglie di allarme o di informazione;
- dati di proiezione usati per tabelle di marcia, piani o richieste di proroga;
- pacchetti di dati usati per rendicontazione o informazione al pubblico.

## Definizioni

```text
p = inquinante
m = metrica regolatoria
z = zona
u = unità territoriale di esposizione media
sp = punto di campionamento
period = periodo di valutazione
```

```text
DATASET =
insieme coerente di osservazioni, misurazioni, input di modello,
dati di validazione dei modelli, previsioni o proiezioni usati per uno scopo regolatorio
```

```text
DATA_PURPOSE =
    assessment_classification
    compliance_verification
    spatial_representativeness
    model_validation
    modelling_input
    network_reduction
    additional_measurement_after_modelled_exceedance
    average_exposure_indicator
    alert_or_information_threshold
    public_information
    reporting
    projection_for_plan_or_roadmap
    projection_for_postponement
```

```text
coverage(dataset,p,m,period) =
percentuale di dati validi disponibili per l’inquinante p,
la metrica m e il periodo di valutazione
```

```text
uncertainty(dataset,p,m) =
incertezza del dataset per l’inquinante p e la metrica m,
valutata secondo l’obiettivo di qualità dei dati applicabile
```

```text
MIN_COVERAGE(p,m,DATA_PURPOSE) =
copertura minima dei dati richiesta dall’Allegato V o dalla regola applicabile
```

```text
MAX_UNCERTAINTY(p,m,DATA_PURPOSE) =
incertezza massima consentita dall’Allegato V o dalla regola applicabile
```

```text
DATA_VALID(dataset,p,m,DATA_PURPOSE) =
coverage >= MIN_COVERAGE
AND uncertainty <= MAX_UNCERTAINTY
AND method_valid = true
AND traceability_complete = true
```

## Requisiti normativi

### REQ-DATA-PURPOSE_CLASSIFICATION

**Fonte:** Art. 11; Allegato V Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `M_ASSESS`; `M_LIMITS`; `M_MOD`; `M_NETWORK`

**Regola**  
Ogni dataset deve essere classificato per scopo regolatorio prima dell’applicazione dei requisiti di qualità dei dati.

**Criterio di accettazione**

```text
if dataset_used = true:
    DATA_PURPOSE is assigned
    applicable_quality_objectives are selected from DATA_PURPOSE, pollutant and metric
```

### REQ-DATA-METHOD_VALIDITY

**Fonte:** Art. 11(1); Allegato VI Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory / permission  
**Dipendenze:** `M_NETWORK`; `M_MODEL_QA`; `T_METHODS`

**Regola**  
I metodi di misurazione di riferimento sono validi ai sensi dell’Allegato VI. Altri metodi di misurazione possono essere usati solo se sono soddisfatte le condizioni applicabili dell’Allegato VI.

**Criterio di accettazione**

```text
if measurement_method = reference_method:
    method_valid = true
else:
    method_valid = AnnexVI_conditions_or_equivalence_satisfied
```

### REQ-DATA-VALIDITY_CRITERIA

**Fonte:** Art. 11(3); Allegato V Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `T_DATA_QUALITY`

**Regola**  
I dati di valutazione della qualità dell’aria devono rispettare gli obiettivi di qualità dei dati applicabili. I dati possono essere usati solo quando, per lo scopo previsto, sono soddisfatte le condizioni di copertura minima, incertezza, validità del metodo e tracciabilità.

**Criterio di accettazione**

```text
DATA_VALID(dataset,p,m,DATA_PURPOSE) =
    coverage(dataset,p,m,period) >= MIN_COVERAGE(p,m,DATA_PURPOSE)
    AND uncertainty(dataset,p,m) <= MAX_UNCERTAINTY(p,m,DATA_PURPOSE)
    AND method_valid = true
    AND traceability_complete = true
```

### REQ-DATA-EXCLUSION_IF_INVALID

**Fonte:** Allegato V; Art. 11 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `REQ-DATA-VALIDITY_CRITERIA`

**Regola**  
Dati invalidi non devono essere usati come dati validi di valutazione per lo scopo regolatorio per il quale non soddisfano i requisiti applicabili.

**Criterio di accettazione**

```text
if DATA_VALID(dataset,p,m,DATA_PURPOSE) = false:
    dataset excluded for DATA_PURPOSE
```

**Qualificazione**  
L’esclusione di dati di misurazione invalidi non invalida di per sé un superamento modellato. Quando si applica l’Art. 8(6) e non è disponibile una misurazione aggiuntiva fissa o indicativa valida, lo stato del superamento modellato è determinato da `M_ASSESS` e `M_LIMITS`.

### REQ-DATA-INCOMPLETE_BUT_CONCLUSIVE_DATA

**Fonte:** Allegato V Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** conditional  
**Dipendenze:** `M_LIMITS`; `M_ASSESS`

**Regola**  
Quando la copertura minima dei dati non è raggiunta, la valutazione può comunque essere marcata come conclusiva se i dati validi disponibili sono sufficienti a sostenere la conclusione regolatoria secondo la regola applicabile.

**Criterio di accettazione**

```text
if coverage < MIN_COVERAGE
AND conclusive_evaluation_possible = true:
    data_quality_status = conclusive_with_incomplete_coverage
else if coverage < MIN_COVERAGE:
    data_quality_status = insufficient_coverage
```

### REQ-DATA-FIXED_MEASUREMENTS

**Fonte:** Art. 4(22); Art. 11; Allegato V Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `M_NETWORK`; `T_DATA_QUALITY`

**Regola**  
Le misurazioni in siti fissi devono essere effettuate presso punti di campionamento, in continuo o mediante campionamento casuale, in ubicazioni costanti per almeno un anno civile e nel rispetto degli obiettivi di qualità dei dati pertinenti.

**Criterio di accettazione**

```text
if data_type = fixed_measurement:
    location_constant = true
    measurement_duration >= 1 calendar_year
    DATA_VALID(dataset,p,m,fixed_measurement_purpose) = true
```

### REQ-DATA-INDICATIVE_MEASUREMENTS

**Fonte:** Art. 4(23); Art. 9(3); Allegato V Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory when used  
**Dipendenze:** `M_NETWORK`; `M_ASSESS`

**Regola**  
Le misurazioni indicative devono soddisfare gli obiettivi di qualità dei dati applicabili alle misurazioni indicative e al loro uso regolatorio previsto.

Quando le misurazioni indicative sono usate per sostituire misurazioni in siti fissi ai sensi dell’Art. 9(3), il loro numero deve essere almeno pari al numero di misurazioni fisse sostituite e devono essere distribuite uniformemente nell’anno civile.

**Criterio di accettazione**

```text
if data_type = indicative_measurement:
    DATA_VALID(dataset,p,m,indicative_measurement_purpose) = true

if indicative_measurements_used_for_Art9_3_reduction = true:
    N_indicative >= N_fixed_replaced
    AND indicative_measurements_evenly_distributed_over_year = true
```

### REQ-DATA-ADDITIONAL_MEASUREMENTS_ART8_6

**Fonte:** Art. 8(6); Allegato V, Punto B Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory when additional measurements are used  
**Dipendenze:** `M_ASSESS`; `M_NETWORK`; `M_REPR`

**Regola**  
Le misurazioni aggiuntive fisse o indicative successive a un superamento modellato devono coprire almeno un anno civile e rispettare i requisiti minimi di copertura dei dati.

**Criterio di accettazione**

```text
if DATA_PURPOSE = additional_measurement_after_modelled_exceedance:
    measurement_duration >= 1_calendar_year
    AND coverage >= MIN_COVERAGE_AnnexV_B
    AND method_valid = true
```

### REQ-DATA-MODEL_INPUT_DATA

**Fonte:** Art. 11(2); Allegato V; Allegato VI Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory when modelling is used  
**Dipendenze:** `M_MOD`; `M_MODEL_QA`

**Regola**  
I dati di input usati in applicazioni di modellizzazione per scopi regolatori devono essere tracciabili e adeguati allo scopo di modellizzazione.

**Criterio di accettazione**

```text
if DATA_PURPOSE = modelling_input:
    input_data_source_documented = true
    input_data_version_recorded = true
    input_data_quality_valid = true
```

### REQ-DATA-MODEL_VALIDATION_DATA

**Fonte:** Art. 11(2); Allegato V; Allegato VI Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory when used for model QA  
**Dipendenze:** `M_MODEL_QA`; `M_MOD`

**Regola**  
I dati usati per validare le applicazioni di modellizzazione devono soddisfare i requisiti applicabili di qualità e tracciabilità per la validazione del modello.

**Criterio di accettazione**

```text
if DATA_PURPOSE = model_validation:
    validation_dataset_traceable = true
    validation_dataset_quality_valid = true
    validation_period_documented = true
```

### REQ-DATA-NETWORK_REDUCTION_ART9_3

**Fonte:** Art. 9(3); Allegato V Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory when reduction is requested  
**Dipendenze:** `M_NETWORK`; `M_MOD`; `M_REPR`; `M_ASSESS`

**Regola**  
Quando i punti di misurazione fissi sono ridotti ai sensi dell’Art. 9(3), le applicazioni di modellizzazione o le misurazioni indicative che supportano la riduzione devono soddisfare gli obiettivi di qualità dei dati applicabili dell’Allegato V e consentire ai risultati di valutazione di rispettare i requisiti di qualità e rendicontazione richiesti.

**Criterio di accettazione**

```text
if Art9_3_reduction_requested = true:
    supporting_data_quality_valid = true
    AND spatial_resolution_sufficient = true
    AND assessment_results_quality_requirements_satisfied = true
    AND public_information_sufficient = true
```

### REQ-DATA-SPATIAL_REPRESENTATIVENESS_DATA

**Fonte:** Art. 8; Allegato IV; Allegato V Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory when used by M_REPR  
**Dipendenze:** `M_REPR`; `M_MOD`

**Regola**  
I dati usati per determinare la rappresentatività spaziale devono essere validi per l’inquinante, la metrica, il periodo, il tipo di stazione e il metodo pertinenti.

**Criterio di accettazione**

```text
if DATA_PURPOSE = spatial_representativeness:
    pollutant_metric_period_consistent = true
    AND underlying_measurement_or_model_data_valid = true
    AND method_metadata_available = true
```

### REQ-DATA-AVERAGE_EXPOSURE_INDICATOR

**Fonte:** Art. 4(33); Art. 9(6); Art. 13(5); Allegato V Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `M_ZONE`; `M_NETWORK`; `M_LIMITS`

**Regola**  
I dati usati per calcolare gli indicatori di esposizione media devono provenire dalle ubicazioni di fondo urbano pertinenti nell’unità territoriale di esposizione media o, se in tale unità non è presente un’area urbana, da ubicazioni di fondo rurale, e devono soddisfare i requisiti di qualità dei dati applicabili.

**Criterio di accettazione**

```text
if DATA_PURPOSE = average_exposure_indicator:
    sampling_point_context_valid_for_AEI = true
    AND DATA_VALID(dataset,p,m,average_exposure_indicator) = true
```

### REQ-DATA-ALERT_INFORMATION_FORECASTS

**Fonte:** Art. 15; Art. 20; Art. 22 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory where forecasts are used  
**Dipendenze:** `M_MOD`; `M_LIMITS`; `M_PUBLIC_INFORMATION`; `M_SHORT_TERM_ACTION`

**Regola**  
I dati e le previsioni usati per prevedere il superamento delle soglie di allarme o di informazione devono essere tracciabili e adeguati allo scopo previsionale. I metadati di qualità della previsione devono essere preservati per i moduli downstream.

**Criterio di accettazione**

```text
if DATA_PURPOSE = alert_or_information_threshold:
    forecast_timestamp recorded
    forecast_domain recorded
    forecast_method recorded
    forecast_quality_metadata_available where required
```

### REQ-DATA-PROJECTIONS_FOR_PLANS_AND_POSTPONEMENT

**Fonte:** Art. 18; Art. 19; Allegato VIII Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory where projections are used  
**Dipendenze:** `M_MOD`; `M_PLANS`; `M_ATTAINMENT_EXTENSION`

**Regola**  
I dati di proiezione usati per piani per la qualità dell’aria, tabelle di marcia o richieste di proroga devono essere documentati, tracciabili e collegati allo scenario, alle ipotesi, alle misure e alla data prevista di conseguimento pertinenti.

**Criterio di accettazione**

```text
if DATA_PURPOSE in [projection_for_plan_or_roadmap, projection_for_postponement]:
    scenario_id recorded
    assumptions_documented = true
    methods_used_documented = true
    input_data_versions_recorded = true
    projected_attainment_date_recorded = true
```

### REQ-DATA-REPORTING_TRACEABILITY

**Fonte:** Art. 23 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory when used for reporting  
**Dipendenze:** `M_REPORTING`; `M_ZONE`; `M_ASSESS`; `M_LIMITS`

**Regola**  
I dati usati per la rendicontazione devono conservare metadati sufficienti a identificare inquinante, metrica, periodo di valutazione, dominio territoriale, metodo, stato di qualità dei dati e versione.

**Criterio di accettazione**

```text
if DATA_PURPOSE = reporting:
    reporting_metadata_complete = true
```

### REQ-DATA-PUBLIC_INFORMATION_TRACEABILITY

**Fonte:** Art. 22 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory when used for public information  
**Dipendenze:** `M_PUBLIC_INFORMATION`; `M_LIMITS`; `M_MOD`

**Regola**  
I dati usati per l’informazione al pubblico devono conservare metadati sufficienti a supportare una comunicazione pubblica chiara, inclusi inquinante, area, periodo, standard o soglia, stato dei dati e stato della previsione ove pertinente.

**Criterio di accettazione**

```text
if DATA_PURPOSE = public_information:
    public_information_metadata_complete = true
    AND public_information_area_identified = true
    AND pollutant_metric_period_identified = true
```

### REQ-DATA-RANDOM_AND_NON_CONTINUOUS_SAMPLING

**Fonte:** Allegato V Dir. (UE) 2024/2881; Art. 4(22), Art. 4(23)  
**Stato:** STABLE  
**Tipo:** conditional permission  
**Dipendenze:** `T_DATA_QUALITY`

**Regola**  
Il campionamento casuale o non continuo può essere usato quando consentito dal tipo di misurazione pertinente e purché i requisiti complessivi di incertezza e copertura per lo scopo previsto restino soddisfatti.

**Criterio di accettazione**

```text
if sampling_mode in [random_sampling, non_continuous_sampling]:
    sampling_mode_allowed_for_data_type = true
    AND overall_uncertainty <= MAX_UNCERTAINTY
    AND coverage >= MIN_COVERAGE
```

### REQ-DATA-EXCLUDED_OR_SPECIAL_METRICS

**Fonte:** Allegato V Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** special rule  
**Dipendenze:** `T_DATA_QUALITY`

**Regola**  
Alcune metriche possono avere requisiti di qualità dei dati speciali o esclusi ai sensi dell’Allegato V. Devono essere gestite mediante regole di applicabilità tabellari, anziché applicando un test generico di copertura/incertezza a tutte le metriche.

**Criterio di accettazione**

```text
if metric in T_DATA_QUALITY.special_or_excluded_metrics:
    apply metric_specific_data_quality_rule
else:
    apply general DATA_VALID rule
```

### REQ-DATA-VERSIONING_AND_AUDIT_TRAIL

**Fonte:** Art. 11; Art. 23; Allegato V; regola di tracciabilità del framework  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `M_REPORTING`

**Regola**  
I dataset usati per scopi regolatori devono essere versionati e auditabili.

**Criterio di accettazione**

```text
dataset_record includes:
    dataset_id
    dataset_version
    source
    acquisition_period
    processing_version
    validation_status
    DATA_PURPOSE
    pollutant
    metric
    territorial_domain
    quality_flags
```

## Logica decisionale sulla qualità dei dati

```text
for each dataset:
    classify DATA_PURPOSE
    identify pollutant, metric, period and territorial domain
    select applicable quality objectives
    verify method validity
    calculate coverage
    calculate or import uncertainty
    verify traceability and versioning

    if all applicable checks pass:
        DATA_VALID = true
    else if incomplete but conclusive assessment is allowed and justified:
        data_quality_status = conclusive_with_incomplete_coverage
    else:
        DATA_VALID = false

    expose data_quality_status to downstream modules
```

## Interazioni con altri moduli

- `M_ZONE` — fornisce il dominio territoriale per validità dei dati e contesto di rendicontazione
- `M_ASSESS` — usa dati validi per classificazione delle soglie e output del regime di valutazione
- `M_NETWORK` — usa la validità dei dati per misurazioni fisse, indicative e aggiuntive
- `M_REPR` — usa dati validi per la rappresentatività spaziale
- `M_MOD` — usa input e dati di validazione validi per applicazioni di modellizzazione
- `M_MODEL_QA` — usa dataset di validazione dei modelli e metadati di qualità
- `M_LIMITS` — usa dati validi per verifica di conformità e superamento
- `M_PLANS` — usa dati di proiezione e valutazione per piani e tabelle di marcia
- `M_ATTAINMENT_EXTENSION` — usa dati di proiezione per richieste di proroga
- `M_SHORT_TERM_ACTION` — usa dati previsionali per trigger a breve termine
- `M_PUBLIC_INFORMATION` — usa dati e metadati pronti per l’informazione al pubblico
- `M_REPORTING` — usa dataset e metadati pronti per la rendicontazione
- `T_DATA_QUALITY` — archivia obiettivi di qualità dell’Allegato V e regole di applicabilità
- `T_METHODS` — archivia metodi di riferimento e regole di equivalenza/validità

## Output

```text
data_quality_status:
  dataset_id
  dataset_version
  pollutant
  metric
  assessment_period
  territorial_domain

  data_type:
    fixed_measurement
    indicative_measurement
    additional_fixed_measurement
    additional_indicative_measurement
    model_input
    model_validation
    forecast
    projection
    reporting_dataset
    public_information_dataset

  purpose:
    assessment_classification
    compliance_verification
    spatial_representativeness
    model_validation
    modelling_input
    network_reduction
    additional_measurement_after_modelled_exceedance
    average_exposure_indicator
    alert_or_information_threshold
    public_information
    reporting
    projection_for_plan_or_roadmap
    projection_for_postponement

  method:
    method_id
    method_valid
    reference_method
    AnnexVI_conditions_satisfied

  quality:
    coverage
    minimum_coverage_required
    uncertainty
    maximum_uncertainty_allowed
    traceability_complete
    valid
    data_quality_status = valid | invalid | insufficient_coverage | conclusive_with_incomplete_coverage | special_metric_rule_applied

  metadata:
    source
    processing_version
    validation_date
    quality_flags
    reporting_metadata_complete
    public_information_metadata_complete
```

## Note

- La validità dei dati è specifica per scopo: un dataset può essere valido per un uso regolatorio e invalido per un altro.
- Dati di misurazione invalidi non invalidano automaticamente superamenti modellati; lo stato del superamento modellato è gestito da `M_ASSESS`, `M_REPR`, `M_MOD` e `M_LIMITS`.
- La riduzione della rete ai sensi dell’Art. 9(3) richiede evidenze di qualità dei dati più robuste rispetto alla modellizzazione supplementare ordinaria.
- Previsioni e proiezioni richiedono tracciabilità anche quando non sono trattate come dataset di misurazione ordinari.
- Il modulo è guidato da tabelle: i valori specifici per inquinante e metrica dell’Allegato V appartengono a `T_DATA_QUALITY`.
