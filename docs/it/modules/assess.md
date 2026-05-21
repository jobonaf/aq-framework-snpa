# M_ASSESS — Regime di valutazione

## Riferimenti normativi

- Art. 6 Direttiva (UE) 2024/2881
- Art. 7 Direttiva (UE) 2024/2881
- Art. 8 Direttiva (UE) 2024/2881
- Art. 9(3) Direttiva (UE) 2024/2881
- Art. 11 Direttiva (UE) 2024/2881
- Art. 12–18 Direttiva (UE) 2024/2881, per gli hook downstream di conformità e qualificazione
- Art. 19–23 Direttiva (UE) 2024/2881, per gli hook downstream di pianificazione, informazione al pubblico e rendicontazione
- Allegato I — valori limite, valori-obiettivo, livelli critici, soglie di allarme e informazione, obblighi di esposizione
- Allegato II — soglie di valutazione
- Allegato IV — ubicazione dei punti di campionamento, rappresentatività spaziale e contesto di valutazione
- Allegato V — obiettivi di qualità dei dati
- Allegato VI — metodi di riferimento e condizioni per la modellizzazione

## Descrizione

Questo modulo determina il **regime di valutazione** della qualità dell’aria ambiente per ciascun inquinante, zona e, ove rilevante, unità territoriale di esposizione media.

Definisce:

- la classificazione delle zone rispetto alle soglie di valutazione;
- la revisione di tale classificazione;
- i metodi di valutazione richiesti o sufficienti per ciascun inquinante e zona;
- quando le misurazioni in siti fissi sono obbligatorie;
- quando modellizzazione, misurazioni indicative o stima obiettiva possono o devono essere utilizzate;
- come trattare i superamenti modellati ai fini della valutazione;
- quando sono richieste o consentite misurazioni aggiuntive dopo aree modellate di superamento non coperte;
- gli output necessari a `M_NETWORK`, `M_LIMITS`, `M_DATA_QUALITY`, `M_MOD`, `M_REPR`, `M_PLANS`, `M_PUBLIC_INFORMATION` e `M_REPORTING`.

Questo modulo **non**:

- calcola la conformità ai valori limite o ai valori-obiettivo;
- definisce in dettaglio la rete di monitoraggio;
- valida la qualità dei dati;
- valida le applicazioni di modellizzazione;
- istituisce piani per la qualità dell’aria, tabelle di marcia o piani d’azione a breve termine;
- assolve obblighi di informazione al pubblico o rendicontazione.

Tali funzioni sono gestite da moduli dedicati.

## Ambito

Il modulo si applica alla valutazione degli inquinanti e delle metriche disciplinati dagli Allegati I e II, inclusi:

- inquinanti soggetti a valori limite;
- inquinanti soggetti a valori-obiettivo;
- inquinanti soggetti a livelli critici;
- inquinanti soggetti a soglie di allarme e soglie di informazione;
- indicatori di esposizione media e obblighi di riduzione dell’esposizione per PM2,5 e NO2;
- contesti di valutazione specifici per l’ozono;
- inquinanti valutati tramite misurazioni in siti fissi, misurazioni indicative, applicazioni di modellizzazione o stima obiettiva.

L’unità spaziale è generalmente la **zona**. Per gli indicatori di esposizione media e i contesti territoriali di valutazione dell’ozono, il modulo supporta anche **unità territoriali di esposizione media** o altre unità territoriali fornite da `M_ZONE`.

## Definizioni

```text
p = inquinante
z = zona
u = unità territoriale di esposizione media o unità territoriale pertinente
y = anno civile
m = metrica regolatoria
```

```text
TH(p) = soglia di valutazione per l’inquinante p,
definita nell’Allegato II / T_ASSESS_THRESHOLDS
```

```text
C(p,z,y,m) = valore di concentrazione per l’inquinante p,
la zona z, l’anno y e la metrica m,
calcolato usando il periodo di mediazione e il metodo di valutazione applicabili
```

```text
PREVIOUS_5_YEARS(y0) =
i cinque anni civili precedenti l’anno di classificazione pertinente y0
```

```text
ABOVE_THRESHOLD(p,z) =
COUNT{ y ∈ PREVIOUS_5_YEARS | C(p,z,y,m_threshold) > TH(p) } >= 3
```

```text
ASSESSMENT_REGIME(p,z) =
    above_threshold_fixed_measurements
    below_threshold_simplified_assessment
    exceedance_requires_modelling_or_indicative
    reduced_fixed_measurement_regime
    not_classifiable_due_to_insufficient_data
```

```text
ASSESSMENT_METHOD =
    fixed_measurements
    indicative_measurements
    modelling_applications
    objective_estimation
    combined_assessment
```

```text
MODELLED_EXCEEDANCE_STATUS =
    not_applicable
    may_be_evaluated_as_non_exceedance
    evaluated_with_additional_measurements
    used_for_assessment
```

## Requisiti normativi

### REQ-ASSESS-ZONE_AND_UNIT_CONTEXT

**Fonte:** Art. 6 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `M_ZONE`

**Regola**  
La valutazione deve essere effettuata in tutte le zone e, ove pertinente, nelle unità territoriali di esposizione media istituite ai fini della valutazione e gestione della qualità dell’aria.

**Criterio di accettazione**

```text
for each pollutant p:
    assessment_domain = zones supplied by M_ZONE

if metric = average_exposure_indicator:
    assessment_domain = average_exposure_territorial_units supplied by M_ZONE
```

### REQ-ASSESS-THRESHOLD_APPLICABILITY

**Fonte:** Art. 7(1); Allegato II Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** constitutive rule  
**Dipendenze:** `T_ASSESS_THRESHOLDS`

**Regola**  
Le soglie di valutazione stabilite nell’Allegato II si applicano alla classificazione delle zone.

**Criterio di accettazione**

```text
TH(p) = T_ASSESS_THRESHOLDS[p]
```

### REQ-ASSESS-ZONE_CLASSIFICATION

**Fonte:** Art. 7(1), Art. 7(3); Allegato II Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `T_ASSESS_THRESHOLDS`; `M_DATA_QUALITY`

**Regola**  
Ciascuna zona deve essere classificata rispetto alle soglie di valutazione.

Quando sono disponibili dati sufficienti, una soglia di valutazione è considerata superata se è stata superata in almeno **tre anni civili distinti** sui precedenti **cinque anni civili**.

**Criterio di accettazione**

```text
ABOVE_THRESHOLD(p,z) =
    COUNT{ y ∈ previous_5_calendar_years | C(p,z,y,m_threshold) > TH(p) } >= 3

if ABOVE_THRESHOLD(p,z) = true:
    threshold_classification = above_threshold
else:
    threshold_classification = below_threshold
```

**Nota**  
Gli anni rilevanti sono i cinque anni civili precedenti. I tre anni di superamento devono essere anni distinti, ma non necessariamente consecutivi.

### REQ-ASSESS-INSUFFICIENT_DATA_FOR_CLASSIFICATION

**Fonte:** Art. 7(3) Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** permission  
**Dipendenze:** `M_DATA_QUALITY`; `M_MOD`; `M_NETWORK`

**Regola**  
Quando sono disponibili dati per meno di cinque anni, i superamenti delle soglie di valutazione possono essere determinati usando i dati disponibili combinati con altre informazioni di valutazione appropriate.

**Criterio di accettazione**

```text
if available_valid_years(p,z) < 5:
    threshold_determination_method = alternative_combined_method
    classification_basis = insufficient_data_alternative_method
```

**Requisito di documentazione**

```text
if classification_basis = insufficient_data_alternative_method:
    require evidence_sources_documented = true
```

### REQ-ASSESS-CLASSIFICATION_REVIEW

**Fonte:** Art. 7(2) Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `REQ-ASSESS-ZONE_CLASSIFICATION`

**Regola**  
La classificazione delle zone rispetto alle soglie di valutazione deve essere riesaminata almeno ogni cinque anni e più frequentemente in caso di cambiamenti significativi delle attività che incidono sulle concentrazioni nell’aria ambiente.

**Criterio di accettazione**

```text
classification_review_due =
    years_since(last_classification_review) >= 5
    OR significant_activity_change_affecting_concentrations = true
```

### REQ-ASSESS-REGIME_ABOVE_THRESHOLD

**Fonte:** Art. 8(2) Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `REQ-ASSESS-ZONE_CLASSIFICATION`; `M_NETWORK`

**Regola**  
In tutte le zone classificate sopra le soglie di valutazione, la qualità dell’aria ambiente deve essere monitorata tramite misurazioni in siti fissi.

Le misurazioni in siti fissi possono essere integrate, ove opportuno, da applicazioni di modellizzazione o misurazioni indicative per fornire informazioni sulla distribuzione spaziale degli inquinanti e sulla rappresentatività spaziale delle misurazioni in siti fissi.

**Criterio di accettazione**

```text
if ABOVE_THRESHOLD(p,z) = true:
    required_methods include fixed_measurements
    supplementary_methods_allowed = [modelling_applications, indicative_measurements]
```

### REQ-ASSESS-REGIME_BELOW_THRESHOLD

**Fonte:** Art. 8(4) Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** constitutive rule  
**Dipendenze:** `REQ-ASSESS-ZONE_CLASSIFICATION`

**Regola**  
In tutte le zone classificate sotto le soglie di valutazione è sufficiente la valutazione tramite applicazioni di modellizzazione, misurazioni indicative, stima obiettiva o una loro combinazione.

**Criterio di accettazione**

```text
if ABOVE_THRESHOLD(p,z) = false:
    sufficient_methods = [
        modelling_applications,
        indicative_measurements,
        objective_estimation,
        combined_assessment
    ]
```

### REQ-ASSESS-EXCEEDANCE_REQUIRES_MODELLING_OR_INDICATIVE

**Fonte:** Art. 8(3) Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `M_LIMITS`; `M_MOD`; `M_REPR`; `M_NETWORK`

**Regola**  
Quando i livelli di un inquinante superano un valore limite o un valore-obiettivo pertinente dell’Allegato I, la qualità dell’aria ambiente deve essere valutata usando applicazioni di modellizzazione o misurazioni indicative, secondo le tempistiche collegate agli atti implementativi di cui all’Art. 8(7).

Quando tali metodi sono usati, devono fornire informazioni sulla distribuzione spaziale degli inquinanti. Quando sono usate applicazioni di modellizzazione, devono essere fornite anche informazioni sulla rappresentatività spaziale delle misurazioni in siti fissi.

**Criterio di accettazione**

```text
if M_LIMITS.exceeds_limit_or_target_value(p,z) = true:
    required_methods include one_of([
        modelling_applications,
        indicative_measurements
    ])
    require spatial_distribution_information = true

    if modelling_applications used:
        require spatial_representativeness_information = true
```

### REQ-ASSESS-MODELLING_FREQUENCY

**Fonte:** Art. 8(3) Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory with practicability qualifier  
**Dipendenze:** `M_MOD`

**Regola**  
Quando le applicazioni di modellizzazione sono usate ai sensi dell’Art. 8(3), devono essere effettuate almeno ogni cinque anni, per quanto praticabile.

**Criterio di accettazione**

```text
if modelling_applications_used_under_Art8_3 = true:
    modelling_frequency_ok =
        years_since(previous_modelling_run) <= 5
        OR practicability_exception_documented = true
```

### REQ-ASSESS-MODELLED_EXCEEDANCE_STATUS

**Fonte:** Art. 8(5) Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory / permission  
**Dipendenze:** `M_LIMITS`; `M_MOD`; `M_REPR`; `M_NETWORK`

**Regola**  
La valutazione rispetto ai valori limite e ai valori-obiettivo deve tenere conto dei risultati delle applicazioni di modellizzazione o delle misurazioni indicative usate secondo le disposizioni di valutazione pertinenti.

Un superamento modellato può essere valutato come non superamento solo quando sono disponibili misurazioni in siti fissi con rappresentatività spaziale che copre l’area modellata di superamento.

**Criterio di accettazione**

```text
if modelled_exceedance(p,z,area) = true:
    if fixed_measurements_cover_modelled_exceedance_area(area) = true:
        MODELLED_EXCEEDANCE_STATUS = may_be_evaluated_as_non_exceedance
    else:
        MODELLED_EXCEEDANCE_STATUS = used_for_assessment
```

### REQ-ASSESS-ADDITIONAL_MEASUREMENTS_AFTER_MODELLED_EXCEEDANCE

**Fonte:** Art. 8(6); Art. 9(3) Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mixed  
**Dipendenze:** `M_NETWORK`; `M_MOD`; `M_REPR`; `M_DATA_QUALITY`

**Regola**  
Quando le applicazioni di modellizzazione usate ai sensi dell’Art. 8(3) o dell’Art. 8(4) mostrano un superamento non coperto da misurazioni in siti fissi e dalla relativa area di rappresentatività spaziale, può essere usata almeno una misurazione aggiuntiva fissa o indicativa.

Quando le applicazioni di modellizzazione usate ai sensi dell’Art. 9(3) mostrano tale superamento, deve essere usata almeno una misurazione aggiuntiva fissa o indicativa.

Le misurazioni aggiuntive fisse devono essere istituite entro due anni dalla data del superamento modellato. Le misurazioni aggiuntive indicative devono essere istituite entro un anno da tale data. Le misurazioni devono coprire almeno un anno civile e soddisfare i requisiti minimi di copertura dei dati dell’Allegato V.

Se non sono svolte misurazioni aggiuntive fisse o indicative in un caso in cui esse sono facoltative, il superamento modellato deve essere usato per la valutazione della qualità dell’aria.

**Criterio di accettazione**

```text
if modelled_exceedance_not_covered_by_fixed_measurements = true:
    if modelling_trigger in [Art8_3, Art8_4]:
        additional_measurement_permitted = true

    if modelling_trigger = Art9_3:
        additional_measurement_required = true

    if additional_fixed_measurement_used = true:
        establishment_deadline = modelled_exceedance_date + 2 years

    if additional_indicative_measurement_used = true:
        establishment_deadline = modelled_exceedance_date + 1 year

    require measurement_duration >= 1 calendar_year
    require AnnexV_minimum_coverage_satisfied = true

    if additional_measurement_used = false:
        MODELLED_EXCEEDANCE_STATUS = used_for_assessment
```

### REQ-ASSESS-NETWORK_REDUCTION_ELIGIBILITY_LINK

**Fonte:** Art. 9(3) Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** permission / gating rule  
**Dipendenze:** `M_NETWORK`; `M_LIMITS`; `M_DATA_QUALITY`; `M_MOD`; `M_REPR`

**Regola**  
Il regime di valutazione può supportare la riduzione del numero minimo di punti di misurazione fissi solo quando sono soddisfatte le condizioni dell’Art. 9(3).

Questo modulo determina ed espone i prerequisiti lato valutazione. `M_NETWORK` determina l’adeguatezza della rete e applica quantitativamente la riduzione.

**Criterio di accettazione**

```text
network_reduction_assessment_eligible =
    ABOVE_THRESHOLD(p,z) = true
    AND M_LIMITS.exceeds_limit_target_or_critical_level(p,z) = false
    AND supplementary_assessment_sufficient_for_regulatory_values = true
    AND supplementary_assessment_sufficient_for_alert_and_information_thresholds = true
    AND public_information_sufficient = true
    AND spatial_resolution_sufficient = true
    AND AnnexV_requirements_satisfied = true
```

### REQ-ASSESS-METHOD_VALIDITY_LINK

**Fonte:** Art. 11; Allegato V; Allegato VI Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `M_DATA_QUALITY`; `M_MODEL_QA`; `M_NETWORK`

**Regola**  
I metodi di valutazione devono basarsi su metodi di misurazione di riferimento o su altri metodi che soddisfano le condizioni dell’Allegato VI. Le applicazioni di modellizzazione usate per la valutazione devono soddisfare le condizioni dell’Allegato VI. I dati di valutazione devono soddisfare gli obiettivi di qualità dei dati dell’Allegato V.

**Criterio di accettazione**

```text
if fixed_measurements or indicative_measurements used:
    require measurement_method_valid_under_AnnexVI = true
    require data_quality_valid_under_AnnexV = true

if modelling_applications used:
    require model_valid_under_AnnexVI = true
```

### REQ-ASSESS-BIO_INDICATORS

**Fonte:** Art. 8(8) Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** recommendation  
**Dipendenze:** `M_ECOSYSTEMS`; `M_NETWORK`

**Regola**  
Quando devono essere valutati i pattern regionali dell’impatto sugli ecosistemi, deve essere considerato, ove applicabile, l’uso di bioindicatori.

**Criterio di accettazione**

```text
if ecosystem_impact_regional_patterns_assessed = true:
    bio_indicator_use_considered = true
```

### REQ-ASSESS-DOWNSTREAM_COMPLIANCE_HOOKS

**Fonte:** Art. 12–18 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** interface rule  
**Dipendenze:** `M_LIMITS`; `M_SOURCE_ATTRIBUTION`; `M_ATTAINMENT_EXTENSION`

**Regola**  
Gli output di valutazione devono fornire i campi di concentrazione, la base metodologica, la classificazione rispetto alle soglie e lo stato dei superamenti modellati necessari ai moduli downstream di conformità.

Il modulo non decide lo stato finale di conformità, l’omissione per fonti naturali, gli effetti della sabbiatura o salatura invernale o le proroghe valide.

**Criterio di accettazione**

```text
assessment_output includes:
    assessment_methods_used
    concentration_basis
    threshold_classification
    modelled_exceedance_status
    spatial_distribution_information
    spatial_representativeness_information
```

### REQ-ASSESS-DOWNSTREAM_PLANNING_HOOKS

**Fonte:** Art. 19; Art. 20 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** interface rule  
**Dipendenze:** `M_LIMITS`; `M_PLANS`; `M_SHORT_TERM_ACTION`; `M_PUBLIC_INFORMATION`

**Regola**  
I risultati di valutazione devono esporre i trigger necessari per piani per la qualità dell’aria, tabelle di marcia e piani d’azione a breve termine.

Questo modulo non istituisce tali piani. Fornisce solo gli input lato valutazione, inclusi il contesto di individuazione dei superamenti e gli input previsionali/modellistici ove pertinenti.

**Criterio di accettazione**

```text
if assessment identifies or supports exceedance of limit_value_or_target_value:
    expose planning_trigger_input = true

if assessment or forecast identifies alert_threshold_exceeded_or_predicted:
    expose short_term_action_trigger_input = true
```

### REQ-ASSESS-DOWNSTREAM_TRANSBOUNDARY_PUBLIC_REPORTING_HOOKS

**Fonte:** Art. 21; Art. 22; Art. 23 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** interface rule  
**Dipendenze:** `M_TRANSBOUNDARY`; `M_PUBLIC_INFORMATION`; `M_REPORTING`

**Regola**  
Gli output di valutazione devono supportare gli obblighi downstream di cooperazione transfrontaliera, informazione al pubblico e rendicontazione.

Ciò include informazioni su zone, unità territoriali, metodi di valutazione, contesto di superamento, distribuzione spaziale e indicatori di fonte/trasporto, ove disponibili.

**Criterio di accettazione**

```text
assessment_output supports:
    transboundary_exceedance_context
    public_information_assessment_summary
    reporting_to_commission_payload
```

### REQ-ASSESS-STRICTEST_PREVAILS

**Fonte:** regola di framework derivata dagli Art. 7–9 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `REQ-ASSESS-REGIME_ABOVE_THRESHOLD`; `REQ-ASSESS-EXCEEDANCE_REQUIRES_MODELLING_OR_INDICATIVE`; `REQ-ASSESS-NETWORK_REDUCTION_ELIGIBILITY_LINK`

**Regola**  
Quando più obblighi o facoltà di valutazione si applicano allo stesso inquinante e alla stessa zona, prevale il regime di valutazione applicabile più protettivo o restrittivo.

**Ordine di precedenza**

1. misurazione aggiuntiva obbligatoria dopo superamento modellato ai sensi dell’Art. 9(3);
2. misurazioni in siti fissi richieste sopra la soglia di valutazione;
3. modellizzazione o valutazione indicativa richiesta dopo superamento di valore limite/valore-obiettivo;
4. valutazione semplificata sotto soglia;
5. sola stima obiettiva solo quando sufficiente nel regime applicabile.

**Criterio di accettazione**

```text
ASSESSMENT_REGIME(p,z) = highest_precedence_applicable_regime(p,z)
```

## Logica decisionale a livello di valutazione

```text
for each pollutant p and zone z:
    determine threshold_classification under Art. 7

    if ABOVE_THRESHOLD(p,z):
        required_methods include fixed_measurements
    else:
        sufficient_methods include modelling_applications
        sufficient_methods include indicative_measurements
        sufficient_methods include objective_estimation
        sufficient_methods include combined_assessment

    if M_LIMITS.exceeds_limit_or_target_value(p,z):
        required_methods include one_of([modelling_applications, indicative_measurements])
        require spatial_distribution_information

        if modelling_applications used:
            require model_valid_under_AnnexVI
            require spatial_representativeness_information where Art. 8(3) applies

    if modelled_exceedance_not_covered_by_fixed_measurements:
        determine additional_measurement_required_or_permitted
        determine MODELLED_EXCEEDANCE_STATUS

    if Art9_3 network reduction requested:
        determine network_reduction_assessment_eligible
```

## Interazioni con altri moduli

- `M_ZONE` — fornisce zone e unità territoriali di esposizione media
- `M_NETWORK` — applica requisiti sui punti di campionamento e sui supersiti in base al regime di valutazione
- `M_DATA_QUALITY` — valida dati di misurazione e requisiti dell’Allegato V
- `M_MOD` — fornisce applicazioni di modellizzazione, campi di concentrazione modellati e superamenti modellati
- `M_MODEL_QA` — valida le applicazioni di modellizzazione ai sensi dell’Allegato VI
- `M_REPR` — definisce la rappresentatività spaziale delle misurazioni in siti fissi
- `M_LIMITS` — determina conformità e stato di superamento ai sensi dell’Allegato I
- `M_SOURCE_ATTRIBUTION` — gestisce fonti naturali, sabbiatura/salatura invernale e contributi transfrontalieri
- `M_ATTAINMENT_EXTENSION` — gestisce lo stato di proroga ai sensi dell’Art. 18
- `M_PLANS` — gestisce piani per la qualità dell’aria e tabelle di marcia ai sensi dell’Art. 19
- `M_SHORT_TERM_ACTION` — gestisce piani d’azione a breve termine ai sensi dell’Art. 20
- `M_TRANSBOUNDARY` — gestisce cooperazione ex Art. 21 e interfacce sui contributi di fonte
- `M_PUBLIC_INFORMATION` — gestisce gli output di informazione al pubblico ex Art. 22
- `M_REPORTING` — gestisce la rendicontazione alla Commissione ex Art. 23
- `T_ASSESS_THRESHOLDS` — soglie dell’Allegato II
- `T_LIMIT_VALUES` — valori limite e valori-obiettivo dell’Allegato I
- `T_SITING` — criteri di ubicazione e rappresentatività dell’Allegato IV

## Output

```text
assessment_status:
  zone_id
  territorial_unit_id
  pollutant
  metric
  assessment_year

  classification:
    threshold_classification = above_threshold | below_threshold | insufficient_data_alternative_method
    classification_basis
    years_evaluated
    exceedance_year_count
    review_due

  methods:
    required_methods
    sufficient_methods
    supplementary_methods_allowed
    methods_used
    method_validity_status
    data_quality_status
    model_validity_status

  spatial_information:
    spatial_distribution_information_required
    spatial_distribution_information_available
    spatial_representativeness_information_required
    spatial_representativeness_information_available

  modelled_exceedance:
    modelled_exceedance_detected
    fixed_measurements_cover_modelled_area
    additional_measurement_required
    additional_measurement_permitted
    additional_measurement_deadline
    modelled_exceedance_status

  network_reduction:
    Art9_3_reduction_requested
    network_reduction_assessment_eligible
    reduction_blocking_reasons

  downstream_triggers:
    compliance_assessment_input_ready
    planning_trigger_input
    short_term_action_trigger_input
    transboundary_context_available
    public_information_input_ready
    reporting_input_ready
```

## Note

- La classificazione sopra soglia richiede misurazioni in siti fissi; non esclude modellizzazione o misurazioni indicative supplementari.
- La classificazione sotto soglia consente metodi di valutazione semplificati, ma non elimina la necessità di valutazione valida quando sono attivati obblighi downstream.
- I superamenti modellati non sono automaticamente ignorati. Possono essere valutati come non superamenti solo quando misurazioni in siti fissi con adeguata rappresentatività spaziale coprono l’area modellata di superamento.
- La riduzione dei punti di misurazione fissi non è una condizione ordinaria di valutazione; è una facoltà condizionata ai criteri dell’Art. 9(3) e alla validazione della rete in `M_NETWORK`.
