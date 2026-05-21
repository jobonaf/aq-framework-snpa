# M_LIMITS — Verifica della conformità e dei superamenti

## Riferimenti normativi

- Direttiva (UE) 2024/2881, Allegato I
- Art. 8 Direttiva (UE) 2024/2881
- Art. 12 Direttiva (UE) 2024/2881
- Art. 13 Direttiva (UE) 2024/2881
- Art. 14 Direttiva (UE) 2024/2881
- Art. 15 Direttiva (UE) 2024/2881
- Art. 16 Direttiva (UE) 2024/2881
- Art. 17 Direttiva (UE) 2024/2881
- Art. 18 Direttiva (UE) 2024/2881
- Allegato V — obiettivi di qualità dei dati

## Descrizione

Questo modulo verifica lo **stato regolatorio dei livelli degli inquinanti** rispetto ai valori e alle soglie definiti nell’Allegato I.

Determina, per ciascun inquinante, metrica, zona o unità territoriale:

- conformità ai valori limite;
- conformità ai valori-obiettivo;
- conformità ai livelli critici;
- raggiungimento o superamento delle soglie di allarme e informazione;
- raggiungimento degli indicatori di esposizione media e degli obblighi di riduzione;
- obblighi di mantenimento quando la qualità dell’aria è già sotto i valori regolatori;
- qualificazione giuridica dei superamenti;
- se un superamento è omesso ai fini della Direttiva;
- se un superamento è coperto da una proroga valida del termine di conseguimento.

Il modulo **non**:

- definisce la rete di monitoraggio;
- valida la qualità dei dati grezzi;
- attribuisce in dettaglio le fonti;
- istituisce piani per la qualità dell’aria;
- valuta la piena validità procedurale delle proroghe.

Tali funzioni appartengono a moduli dedicati.

## Ambito

Il modulo si applica a:

- valori limite;
- valori-obiettivo;
- livelli critici;
- indicatori di esposizione media;
- obblighi di riduzione dell’esposizione media;
- obiettivi di concentrazione dell’esposizione media;
- soglie di allarme;
- soglie di informazione.

Il modulo distingue tra:

```text
technical_exceedance
regulatory_exceedance
exceedance_omitted_for_directive_purposes
exceedance_relevant_for_planning
non_compliance_under_valid_postponement
```

## Definizioni

```text
p = inquinante
z = zona
u = unità territoriale di esposizione media
x = ubicazione
t = tempo
m = metrica regolatoria
```

```text
C(p, x, t) = concentrazione dell’inquinante p nell’ubicazione x e al tempo t
VALUE(p, z, m, period) = valore regolatorio calcolato per p, z, m e periodo di valutazione
STANDARD(p, m) = valore regolatorio applicabile da T_LIMIT_VALUES o tabelle equivalenti
LV(p, m) = valore limite
TV(p, m) = valore-obiettivo
CL(p, m) = livello critico
AEI(p, u, y) = indicatore di esposizione media
EXCEEDANCE(p, z, m, period) = VALUE(p, z, m, period) > STANDARD(p, m)
```

```text
COMPLIANCE_STATUS =
    compliant
    non_compliant
    non_compliant_under_valid_postponement
    exceedance_omitted_natural_sources
    exceedance_attributable_to_winter_sanding_or_salting
    not_assessable
```

## Requisiti normativi

### REQ-LIMITS-DATA_VALIDITY

**Regola**  
Possono essere usati per la verifica di conformità e superamento solo dati che soddisfano i requisiti di qualità applicabili.

**Criterio di accettazione**

```text
use only data where DATA_VALID = true
```

**Eccezione / qualificazione**  
Quando le regole regolatorie consentono la valutazione con copertura incompleta, il risultato deve essere marcato come basato su dati incompleti ma conclusivi.

```text
if DATA_VALID = false
AND incomplete_data_conclusive = true:
    assessment_status = conclusive_with_incomplete_coverage
```

### REQ-LIMITS-SPATIAL_INTEGRATION

**Regola**  
La verifica di conformità deve tenere conto di misurazioni in siti fissi, misurazioni indicative e applicazioni di modellizzazione secondo il regime di valutazione e la rappresentatività spaziale.

**Criterio di accettazione**

```text
if valid_fixed_measurement_available
AND x ∈ AREA_REPR(fixed_measurement):
    C_assessment(p,x,t) = fixed_measurement_value
else if valid_indicative_measurement_available:
    C_assessment(p,x,t) = indicative_measurement_value
else if VALID_MODEL = true:
    C_assessment(p,x,t) = modelled_value
else:
    C_assessment(p,x,t) = undefined
```

### REQ-LIMITS-MODELLED_EXCEEDANCE

**Regola**  
Un superamento modellato è rilevante per la valutazione salvo che possa essere valutato come non superamento perché sono disponibili misurazioni in siti fissi con rappresentatività spaziale che copre l’area modellata di superamento.

**Criterio di accettazione**

```text
if modelled_exceedance = true:
    if fixed_measurements_cover_modelled_exceedance_area = true:
        modelled_exceedance_status = may_be_evaluated_as_non_exceedance
    else if additional_valid_measurements_available = true:
        modelled_exceedance_status = evaluated_with_additional_measurements
    else:
        modelled_exceedance_status = used_for_assessment
```

### REQ-LIMITS-MEAN_COMPLIANCE

**Regola**  
Per le metriche regolatorie valutate come media, la conformità è verificata confrontando la media calcolata con il valore regolatorio applicabile.

```text
COMPLIANT_MEAN(p,z,m) =
    MEAN_VALUE(p,z,m) <= STANDARD(p,m)
```

### REQ-LIMITS-EXCEEDANCE_COUNT_COMPLIANCE

**Regola**  
Per le metriche che consentono un numero massimo di superamenti, la conformità è verificata confrontando il numero di superamenti con il massimo consentito.

```text
COMPLIANT_EXCEEDANCE_COUNT(p,z,m) =
    COUNT{ t | VALUE(p,z,m,t) > STANDARD(p,m) } <= MAX_ALLOWED_EXCEEDANCES(p,m)
```

### REQ-LIMITS-OVERALL_COMPLIANCE

**Regola**  
La conformità complessiva per un inquinante e una metrica è raggiunta solo se tutte le condizioni regolatorie applicabili sono soddisfatte.

```text
COMPLIANT(p,z,m) = all applicable compliance tests are true

if COMPLIANT(p,z,m) = true:
    compliance_status = compliant
else:
    compliance_status = non_compliant
```

### REQ-LIMITS-MAINTENANCE_OBLIGATION

**Regola**  
Quando i livelli degli inquinanti sono inferiori ai valori limite o valori-obiettivo applicabili, il sistema registra un obbligo di mantenimento.

```text
if VALUE(p,z,m,period) < STANDARD(p,m):
    maintenance_obligation = true
```

### REQ-LIMITS-LIMIT_VALUES

**Regola**  
I livelli degli inquinanti non devono superare i valori limite applicabili.

```text
if VALUE(p,z,m,period) > LV(p,m):
    limit_value_exceedance = true
    compliance_status = non_compliant
else:
    limit_value_exceedance = false
```

### REQ-LIMITS-TARGET_VALUES

**Regola**  
I livelli degli inquinanti non devono superare i valori-obiettivo applicabili.

```text
if VALUE(p,z,m,period) > TV(p,m):
    target_value_exceedance = true
    compliance_status = non_compliant
else:
    target_value_exceedance = false
```

### REQ-LIMITS-AVERAGE_EXPOSURE_INDICATORS

**Regola**  
Gli indicatori di esposizione media per PM2,5 e NO2 devono essere valutati nelle unità territoriali di esposizione media. Ove applicabili, devono essere verificati gli obblighi di riduzione dell’esposizione media e gli obiettivi di concentrazione dell’esposizione media.

```text
if AEI(p,u,y) <= AEI_OBJECTIVE(p):
    exposure_objective_status = attained
else:
    exposure_objective_status = not_attained

if AEI_REDUCTION(p,u,period) >= REQUIRED_REDUCTION(p,u):
    exposure_reduction_status = compliant
else:
    exposure_reduction_status = non_compliant
```

### REQ-LIMITS-CRITICAL_LEVELS

**Regola**  
I livelli critici per la protezione della vegetazione e degli ecosistemi naturali devono essere rispettati ove applicabile.

```text
if VALUE(p,z,m,period) > CL(p,m):
    critical_level_exceedance = true
else:
    critical_level_exceedance = false
```

### REQ-LIMITS-ALERT_INFORMATION_THRESHOLDS

**Regola**  
Quando le soglie di allarme o informazione sono superate o si prevede che siano superate, il sistema registra il superamento e attiva gli obblighi downstream pertinenti.

```text
if VALUE(p,z,m,t) > ALERT_THRESHOLD(p,m)
OR predicted_value(p,z,m,t) > ALERT_THRESHOLD(p,m):
    alert_threshold_status = exceeded_or_predicted
    short_term_action_plan_trigger = true
    public_information_trigger = true

if VALUE(p,z,m,t) > INFORMATION_THRESHOLD(p,m)
OR predicted_value(p,z,m,t) > INFORMATION_THRESHOLD(p,m):
    information_threshold_status = exceeded_or_predicted
    public_information_trigger = true
```

**Nota**  
Questo modulo registra il trigger. Il contenuto dell’informazione al pubblico e delle misure di emergenza è gestito da moduli dedicati.

### REQ-LIMITS-NATURAL_SOURCES_CLASSIFICATION

**Regola**  
Zone e unità territoriali di esposizione media possono essere classificate come interessate da superamenti attribuibili a fonti naturali. Lo Stato membro deve fornire elenchi delle zone o unità territoriali interessate, informazioni sulle concentrazioni, informazioni sulle fonti ed evidenze di attribuzione a fonti naturali.

```text
if natural_source_attribution = true
AND required_information_submitted = true
AND evidence_submitted = true:
    natural_source_case = true
else:
    natural_source_case = false
```

### REQ-LIMITS-NATURAL_SOURCES_OMISSION

**Regola**  
Quando la Commissione è stata informata di un superamento attribuibile a fonti naturali, il superamento è omesso ai fini della Direttiva, salvo che la Commissione ritenga insufficienti le evidenze.

```text
if natural_source_case = true
AND commission_evidence_insufficient != true:
    exceedance_status = exceedance_omitted_natural_sources
    omitted_for_directive_purposes = true
else:
    omitted_for_directive_purposes = false
```

**Nota**  
La regola si applica all’effetto giuridico del superamento. Non elimina la concentrazione misurata o modellata sottostante.

### REQ-LIMITS-WINTER_SANDING_SALTING_IDENTIFICATION

**Regola**  
Per un dato anno, possono essere identificate zone in cui i valori limite del PM10 sono superati a causa della risospensione di particolato successiva alla sabbiatura o salatura invernale delle strade.

```text
if pollutant = PM10
AND limit_value_exceedance = true
AND attribution = winter_sanding_or_salting
AND required_information_submitted = true
AND evidence_submitted = true:
    winter_sanding_salting_case = true
else:
    winter_sanding_salting_case = false
```

### REQ-LIMITS-WINTER_SANDING_SALTING_EFFECT

**Regola**  
Quando i superamenti di PM10 sono attribuibili alla sabbiatura o salatura invernale delle strade, l’istituzione di un piano per la qualità dell’aria può essere omessa per tali superamenti. La regola non si applica ai superamenti attribuibili ad altre fonti di PM10.

```text
if winter_sanding_salting_case = true
AND other_PM10_sources_cause_exceedance != true:
    air_quality_plan_required_for_this_exceedance = false
    compliance_status = exceedance_attributable_to_winter_sanding_or_salting
else if limit_value_exceedance = true:
    air_quality_plan_required_for_this_exceedance = true
```

**Nota**  
Questa regola incide sulla conseguenza di pianificazione del superamento. Non rende automaticamente la zona conforme.

### REQ-LIMITS-POSTPONED_DEADLINE_STATUS

**Regola**  
Quando il termine di conseguimento per un valore limite è stato validamente prorogato, un superamento durante il periodo di proroga valido deve essere registrato come non conformità in regime di proroga valida, e non come non conformità ordinaria.

```text
if limit_value_exceedance = true
AND valid_postponement = true
AND assessment_date <= postponed_deadline:
    compliance_status = non_compliant_under_valid_postponement
else if limit_value_exceedance = true:
    compliance_status = non_compliant
```

### REQ-LIMITS-POSTPONEMENT_BOUNDARIES

**Regola**  
Il modulo non concede né valida direttamente una proroga. Usa solo il risultato di `M_ATTAINMENT_EXTENSION`.

```text
valid_postponement = M_ATTAINMENT_EXTENSION.status = valid
postponement_status ∈ [pending, valid, objected, invalid, expired]
```

### REQ-LIMITS-STATUS_PRECEDENCE

**Regola**  
Quando si applicano più qualificazioni giuridiche, lo stato di conformità deve essere assegnato secondo il seguente ordine di precedenza.

**Ordine di precedenza**

1. `not_assessable`
2. `exceedance_omitted_natural_sources`
3. `non_compliant_under_valid_postponement`
4. `exceedance_attributable_to_winter_sanding_or_salting`
5. `non_compliant`
6. `compliant`

```text
if assessment_data_sufficient = false:
    compliance_status = not_assessable
else if omitted_for_directive_purposes = true:
    compliance_status = exceedance_omitted_natural_sources
else if limit_value_exceedance = true AND valid_postponement = true:
    compliance_status = non_compliant_under_valid_postponement
else if winter_sanding_salting_case = true:
    compliance_status = exceedance_attributable_to_winter_sanding_or_salting
else if any_applicable_exceedance = true:
    compliance_status = non_compliant
else:
    compliance_status = compliant
```

**Nota**  
La regola di precedenza è una regola di framework. Non modifica le condizioni giuridiche sostanziali delle disposizioni sottostanti.

## Interazioni con altri moduli

- `M_ZONE` — fornisce zone e unità territoriali di esposizione media
- `M_ASSESS` — definisce regime di valutazione e metodi applicabili
- `M_NETWORK` — fornisce il contesto della rete di monitoraggio
- `M_DATA_QUALITY` — valida i dati usati nella verifica di conformità
- `M_REPR` — definisce la rappresentatività spaziale delle misurazioni
- `M_MOD` — fornisce campi di concentrazione modellati e superamenti modellati
- `M_SOURCE_ATTRIBUTION` — determina l’attribuzione a fonti naturali e sabbiatura/salatura invernale
- `M_ATTAINMENT_EXTENSION` — valuta la validità delle proroghe ex Art. 18
- `M_PLANS` — determina le conseguenze di pianificazione ex Art. 19 e Art. 20
- `M_REPORTING` — gestisce rendicontazione alla Commissione e flussi di informazione al pubblico

## Output

```text
compliance_result:
  zone_id
  territorial_unit_id
  pollutant
  metric
  assessment_period

  value:
    calculated_value
    standard_value
    standard_type = limit_value | target_value | critical_level | average_exposure_objective | alert_threshold | information_threshold

  assessment_basis:
    fixed_measurements_used
    indicative_measurements_used
    modelling_used
    spatial_representativeness_applied
    data_quality_status

  exceedance:
    technical_exceedance
    regulatory_exceedance
    exceedance_count
    allowed_exceedances
    modelled_exceedance_status

  adjustments:
    natural_sources_case
    omitted_for_directive_purposes
    winter_sanding_salting_case
    valid_postponement
    postponed_deadline

  obligations:
    maintenance_obligation
    public_information_trigger
    short_term_action_plan_trigger
    air_quality_plan_required

  compliance_status:
    compliant
    non_compliant
    non_compliant_under_valid_postponement
    exceedance_omitted_natural_sources
    exceedance_attributable_to_winter_sanding_or_salting
    not_assessable
```

## Note

- Il modulo verifica la conformità e qualifica i superamenti.
- Non istituisce piani per la qualità dell’aria.
- Non valida proceduralmente le proroghe ex Art. 18.
- Non attribuisce direttamente le fonti degli inquinanti.
- L’omissione per fonti naturali e i casi di sabbiatura/salatura invernale devono conservare la concentrazione originaria misurata o modellata.
- Una proroga valida non rende conforme il livello dell’inquinante; modifica lo stato giuridico della non conformità durante il periodo di proroga.
