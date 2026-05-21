# M_REPR — Rappresentatività spaziale dei punti di campionamento

## Riferimenti normativi

- Art. 4(26) Direttiva (UE) 2024/2881
- Art. 8 Direttiva (UE) 2024/2881
- Art. 9 Direttiva (UE) 2024/2881
- Art. 10 Direttiva (UE) 2024/2881, quando punti di campionamento presso supersiti sono usati ai fini della valutazione o degli obblighi di numero minimo
- Art. 11 Direttiva (UE) 2024/2881
- Art. 21 Direttiva (UE) 2024/2881, per hook di valutazione transfrontaliera
- Allegato IV — criteri di ubicazione e rappresentatività spaziale
- Allegato V — obiettivi di qualità dei dati
- Allegato VI — metodi di riferimento e condizioni per la modellizzazione
- Atti implementativi ai sensi dell’Art. 8(7), ove applicabili

## Descrizione

Questo modulo definisce e valuta la **rappresentatività spaziale** dei punti di campionamento.

La rappresentatività spaziale è la relazione regolatoria tra un punto di campionamento e l’area geografica per la quale la concentrazione misurata può essere considerata rappresentativa per un inquinante, una metrica e un periodo di valutazione.

Il modulo determina:

- l’area di rappresentatività di ciascun punto di campionamento;
- se una misurazione in sito fisso copre un’area modellata di superamento;
- se un’area di superamento non è coperta da misurazioni in siti fissi;
- se la rappresentatività è sufficiente per la valutazione della conformità;
- se la rappresentatività supporta la riduzione delle misurazioni in siti fissi;
- se la rilocalizzazione di un punto di campionamento è giustificata e non crea una perdita di copertura inaccettabile;
- se le informazioni spaziali sono sufficienti per i moduli downstream di pianificazione, rendicontazione e informazione al pubblico.

Questo modulo **non**:

- determina la conformità ai valori limite o ai valori-obiettivo;
- valida la rete di monitoraggio nel suo insieme;
- valida le prestazioni dei modelli;
- istituisce nuovi punti di campionamento;
- istituisce piani per la qualità dell’aria o piani d’azione a breve termine.

Tali funzioni sono gestite da moduli dedicati.

## Ambito

Il modulo si applica a:

- misurazioni in siti fissi;
- misurazioni indicative quando usate per valutazione spaziale o riduzione della rete;
- analisi di rappresentatività supportata da modelli;
- determinazioni ibride misura–modello della rappresentatività;
- aree modellate di superamento;
- aree hotspot;
- punti di campionamento presso supersiti quando usati come punti di campionamento per finalità di numero minimo o valutazione.

La rappresentatività deve essere valutata per inquinante, metrica, tipo di stazione, periodo di valutazione e contesto spaziale.

## Definizioni

```text
p = inquinante
m = metrica regolatoria
z = zona
u = unità territoriale di esposizione media o unità territoriale pertinente
sp = punto di campionamento
x = ubicazione
A = area geografica
```

```text
C_sp(p,m,period) =
valore centrale di concentrazione misurato presso il punto di campionamento sp
per l’inquinante p, la metrica m e il periodo di valutazione
```

```text
C_field(p,m,x,period) =
campo di concentrazione nell’ubicazione x,
derivato da modellizzazione validata, misurazioni valide o metodo ibrido
```

```text
TOL(p,m) =
tolleranza di rappresentatività per l’inquinante p e la metrica m,
definita in T_REPR_TOLERANCE o nella metodologia applicabile
```

```text
Δ(sp,p,m) =
semiampiezza dell’intervallo di tolleranza di rappresentatività
per il punto sp, l’inquinante p e la metrica m
```

```text
INTERVAL(sp,p,m) =
[C_sp(p,m) - Δ(sp,p,m), C_sp(p,m) + Δ(sp,p,m)]
```

```text
AREA_REPR(sp,p,m,period) =
area geografica in cui le concentrazioni sono sufficientemente simili
alla concentrazione rappresentata dal punto di campionamento sp
per l’inquinante p, la metrica m e il periodo di valutazione
```

```text
MODELLED_EXCEEDANCE_AREA(p,m,period) =
area in cui un’applicazione di modellizzazione indica il superamento
di un valore limite, valore-obiettivo, livello critico,
soglia di allarme o soglia di informazione pertinente
```

```text
COVERED_BY_FIXED_MEASUREMENTS(A,p,m) =
l’area A è coperta da almeno una area di rappresentatività valida
di misurazione in sito fisso per l’inquinante p e la metrica m
```

```text
UNCOVERED_AREA(A,p,m) =
A meno l’unione delle aree di rappresentatività applicabili
delle misurazioni in siti fissi
```

```text
REPR_VALID(sp,p,m,period) =
l’area di rappresentatività del punto sp è valida per l’uso dichiarato
```

## Requisiti normativi

### REQ-REPR-AREA_DEFINITION

**Regola**  
L’area di rappresentatività di un punto di campionamento deve essere definita come l’area geografica per la quale la concentrazione misurata presso il punto è rappresentativa, tenendo conto di inquinante, metrica, tipo di stazione, contesto emissivo, orografia e condizioni di dispersione.

**Criterio di accettazione**

```text
AREA_REPR(sp,p,m,period) =
    { x |
      C_field(p,m,x,period) ∈ INTERVAL(sp,p,m)
      AND spatial_context_compatible(x,sp) = true
      AND station_type_compatible(x,sp) = true
    }
```

### REQ-REPR-POLLUTANT_METRIC_SPECIFICITY

**Regola**  
La rappresentatività deve essere determinata separatamente per ciascun inquinante, metrica regolatoria e periodo di valutazione.

**Criterio di accettazione**

```text
AREA_REPR(sp,p1,m1,period) is not automatically valid for p2 or m2
unless pollutant_metric_equivalence_documented = true
```

### REQ-REPR-STATION_TYPE_AND_CONTEXT

**Regola**  
Un’area di rappresentatività deve essere coerente con il tipo di stazione e la situazione di esposizione rappresentata dal punto di campionamento.

Il contesto rilevante include, ove applicabile:

- esposizione al traffico;
- fondo urbano;
- fondo suburbano;
- fondo rurale;
- influenza industriale;
- contesto hotspot;
- contesto di fondo rurale per l’ozono;
- contesto di protezione degli ecosistemi o della vegetazione.

**Criterio di accettazione**

```text
if location_context(x) incompatible with station_type(sp):
    x excluded from AREA_REPR(sp,p,m,period)
```

### REQ-REPR-GEOGRAPHIC_AND_EMISSION_REFINEMENT

**Regola**  
L’area preliminare di rappresentatività deve essere raffinata usando criteri geografici, tipologici, emissivi e di dispersione.

**Criterio di accettazione**

```text
AREA_REPR_refined =
    AREA_REPR_preliminary
    minus locations with incompatible geography
    minus locations with incompatible emission profile
    minus locations with incompatible dispersion conditions
    minus locations outside applicable territorial boundaries
```

### REQ-REPR-TERRITORIAL_BOUNDARIES

**Regola**  
Le aree di rappresentatività devono essere collegate alla zona o unità territoriale pertinente. Quando un’area attraversa confini di zona o di Stato membro, la porzione oltre confine deve essere registrata esplicitamente, non scartata implicitamente.

**Criterio di accettazione**

```text
AREA_REPR(sp,p,m) intersects one_or_more territorial_units

if AREA_REPR crosses zone_boundary:
    cross_boundary_representativeness = true
    affected_zones recorded
```

**Nota**  
Per l’aggregazione ordinaria di conformità zonale, i moduli possono limitare l’uso alla zona pertinente. Per valutazione e rendicontazione transfrontaliere, la geometria completa può essere rilevante.

### REQ-REPR-MODEL_USAGE_FOR_AREA_DEFINITION

**Regola**  
Le applicazioni di modellizzazione possono essere usate per determinare o raffinare la rappresentatività spaziale solo se il modello soddisfa le condizioni regolatorie applicabili e i dati di valutazione sottostanti rispettano gli obiettivi di qualità dei dati.

**Criterio di accettazione**

```text
if model_used_for_representativeness = true:
    require VALID_MODEL = true
    require AnnexVI_conditions_satisfied = true
    require input_data_quality_valid = true
```

### REQ-REPR-HYBRID_METHOD

**Regola**  
La rappresentatività può essere determinata con un metodo ibrido che combina misurazioni, applicazioni di modellizzazione, informazioni emissive e giudizio esperto, purché ciascun componente sia documentato e valido per l’uso previsto.

**Criterio di accettazione**

```text
if method = hybrid:
    measurement_component_valid = true
    AND modelling_component_valid_if_used = true
    AND emission_context_documented = true
    AND expert_judgement_documented_if_used = true
```

### REQ-REPR-COVERAGE_OF_MODELLED_EXCEEDANCE_AREA

**Regola**  
Quando le applicazioni di modellizzazione mostrano un superamento, questo modulo deve determinare se l’area modellata di superamento è coperta da misurazioni in siti fissi e dalle loro aree di rappresentatività spaziale.

**Criterio di accettazione**

```text
fixed_measurements_cover_modelled_exceedance_area(A,p,m) =
    MODELLED_EXCEEDANCE_AREA(A,p,m)
    ⊆ union{ AREA_REPR(sp,p,m) |
              sp is fixed_measurement
              AND REPR_VALID(sp,p,m) = true }

if fixed_measurements_cover_modelled_exceedance_area = true:
    coverage_status = fully_covered_by_fixed_measurements
else:
    coverage_status = not_fully_covered_by_fixed_measurements
    uncovered_area = UNCOVERED_AREA(MODELLED_EXCEEDANCE_AREA,p,m)
```

### REQ-REPR-MODELLED_EXCEEDANCE_NON_EXCEEDANCE_SUPPORT

**Regola**  
Un superamento modellato può essere valutato come non superamento solo quando sono disponibili misurazioni in siti fissi con rappresentatività spaziale che copre l’area modellata di superamento.

Questo modulo fornisce la determinazione della copertura spaziale. La conseguenza giuridica sulla conformità è decisa da `M_LIMITS`.

**Criterio di accettazione**

```text
if modelled_exceedance_area_fully_covered_by_fixed_measurements = true:
    supports_modelled_exceedance_non_exceedance_evaluation = true
else:
    supports_modelled_exceedance_non_exceedance_evaluation = false
```

### REQ-REPR-UNCOVERED_MODELLED_EXCEEDANCE_AREA

**Regola**  
Quando un superamento modellato non è coperto da misurazioni in siti fissi e dalle loro aree di rappresentatività, l’area non coperta deve essere identificata e resa disponibile a `M_NETWORK` e `M_ASSESS`.

**Criterio di accettazione**

```text
if coverage_status = not_fully_covered_by_fixed_measurements:
    uncovered_modelled_exceedance_area_geometry is not null
    affected_population_or_area_estimate recorded where available
    candidate_monitoring_locations may be generated by M_NETWORK or M_MOD
```

### REQ-REPR-ADDITIONAL_MEASUREMENT_TARGETING

**Regola**  
Quando misurazioni aggiuntive fisse o indicative sono richieste o consentite dopo un superamento modellato non coperto, il gap di rappresentatività non coperto deve essere usato per supportare il posizionamento delle misurazioni aggiuntive.

**Criterio di accettazione**

```text
if additional_measurement_required_or_permitted = true:
    measurement_target_area = uncovered_modelled_exceedance_area
    candidate_locations_consistent_with_AnnexIV = true
```

### REQ-REPR-NETWORK_REDUCTION_SUPPORT

**Regola**  
Quando è richiesta la riduzione dei punti di campionamento con misurazioni fisse, questo modulo deve valutare se le misurazioni fisse rimanenti, le misurazioni indicative e le applicazioni di modellizzazione forniscono copertura spaziale sufficiente per la valutazione.

**Criterio di accettazione**

```text
if Art9_3_reduction_requested = true:
    network_reduction_spatial_coverage_ok =
        remaining_fixed_measurement_coverage_sufficient = true
        AND supplementary_modelling_or_indicative_coverage_sufficient = true
        AND spatial_resolution_sufficient = true
        AND uncovered_high_risk_areas = none
```

### REQ-REPR-INDICATIVE_MEASUREMENTS_SUPPORT

**Regola**  
Quando le misurazioni indicative sono usate a supporto della valutazione o della riduzione delle misurazioni fisse, la loro rappresentatività spaziale deve essere valutata e documentata.

**Criterio di accettazione**

```text
if indicative_measurements_used_for_assessment_or_reduction = true:
    AREA_REPR(indicative_point,p,m) defined
    AND data_quality_objectives_satisfied = true
    AND temporal_distribution_requirements_satisfied_if_Art9_3 = true
```

### REQ-REPR-RELOCATION_IMPACT_ASSESSMENT

**Regola**  
Quando è proposta la rilocalizzazione di un punto di campionamento, le aree di rappresentatività prima e dopo la rilocalizzazione devono essere confrontate.

Per i punti di campionamento con superamenti registrati di valori limite o valori-obiettivo pertinenti nei tre anni precedenti, la rilocalizzazione deve essere supportata da applicazioni di modellizzazione o misurazioni indicative e pienamente documentata.

**Criterio di accettazione**

```text
if relocation_proposed(sp) = true:
    old_area = AREA_REPR(sp,p,m,before_relocation)
    new_area = AREA_REPR(sp,p,m,after_relocation)
    coverage_loss_area = old_area - new_area
    coverage_gain_area = new_area - old_area

    if recorded_exceedance_in_previous_3_years(sp) = true:
        relocation_support_valid =
            modelling_or_indicative_support = true
            AND relocation_justification_documented = true
            AND unacceptable_exceedance_coverage_loss = false
```

### REQ-REPR-OVERLAP_RESOLUTION

**Regola**  
Quando una ubicazione appartiene a più aree di rappresentatività, l’assegnazione o l’aggregazione deve essere risolta usando criteri documentati.

I criteri rilevanti includono:

- tipo di stazione;
- coerenza emissiva;
- livello di concentrazione;
- distanza e contesto di dispersione;
- metrica regolatoria;
- qualità dei dati e confidenza del metodo.

**Criterio di accettazione**

```text
if x ∈ AREA_REPR(sp1,p,m) AND x ∈ AREA_REPR(sp2,p,m):
    overlap_resolution_method documented
    selected_or_combined_representative_value justified
```

### REQ-REPR-CONFLICT_BETWEEN_MEASUREMENT_AND_MODEL

**Regola**  
Quando misurazioni e applicazioni di modellizzazione forniscono risultati incoerenti entro un’area di rappresentatività spaziale, il conflitto deve essere esplicitamente segnalato e risolto secondo le regole applicabili di valutazione e conformità.

Una misurazione in sito fisso valida che copre l’area modellata di superamento può supportare la valutazione del superamento modellato come non superamento. Al di fuori delle aree di rappresentatività valide, i risultati della modellizzazione restano rilevanti per la valutazione.

**Criterio di accettazione**

```text
if modelled_exceedance = true
AND fixed_measurement_non_exceedance = true:
    if modelled_exceedance_area fully covered by fixed_measurement_AREA_REPR:
        conflict_resolution = fixed_measurement_supports_non_exceedance_evaluation
    else:
        conflict_resolution = modelled_exceedance_remains_relevant_for_uncovered_area
```

### REQ-REPR-VALIDITY_FOR_COMPLIANCE

**Regola**  
Un’area di rappresentatività può essere usata per la verifica di conformità solo quando i dati di misurazione o modellistici sottostanti soddisfano i requisiti applicabili di metodo e qualità dei dati.

**Criterio di accettazione**

```text
valid_for_compliance(sp,p,m) =
    REPR_VALID(sp,p,m) = true
    AND underlying_data_quality_valid = true
    AND method_valid_under_AnnexVI = true
```

### REQ-REPR-VALIDITY_FOR_PUBLIC_INFORMATION_AND_REPORTING

**Regola**  
Gli output di rappresentatività usati per informazione al pubblico o rendicontazione devono conservare metadati sufficienti a spiegare la base spaziale dei risultati di valutazione.

**Criterio di accettazione**

```text
if representativeness_output_used_for_public_information_or_reporting = true:
    include geometry
    include method
    include pollutant
    include metric
    include assessment_period
    include uncertainty_or_confidence_metadata_where_available
```

### REQ-REPR-TRANSBOUNDARY_CONTEXT

**Regola**  
Quando aree di rappresentatività, aree modellate di superamento o aree di contributo di fonte attraversano o interessano Stati membri confinanti, la geometria transfrontaliera e i metadati di valutazione pertinenti devono essere preservati per i moduli transfrontalieri.

**Criterio di accettazione**

```text
if AREA_REPR or MODELLED_EXCEEDANCE_AREA crosses national_boundary
OR transboundary_contribution_relevant = true:
    transboundary_geometry_recorded = true
    affected_member_states_recorded = true
```

### REQ-REPR-EXPERT_JUDGEMENT

**Regola**  
Il giudizio esperto può essere usato per raffinare o validare la rappresentatività quando eterogeneità spaziale, orografia complessa, dispersione non uniforme o dati sparsi rendono insufficiente una determinazione puramente quantitativa.

Il giudizio esperto deve essere documentato.

**Criterio di accettazione**

```text
if expert_judgement_used = true:
    expert_judgement_documented = true
    rationale_recorded = true
    affected_geometry_recorded = true
```

### REQ-REPR-UPDATE_FREQUENCY

**Regola**  
Le aree di rappresentatività devono essere riesaminate quando cambiano in modo materiale classificazione di valutazione, configurazione della rete, emissioni, pattern di attività, comprensione di orografia/dispersione o evidenze modellistiche.

Come minimo, la rappresentatività dovrebbe essere riesaminata in coordinamento con il ciclo quinquennale di revisione della classificazione e della valutazione.

**Criterio di accettazione**

```text
representativeness_review_due =
    years_since(last_repr_review) >= 5
    OR network_changed = true
    OR sampling_point_relocated = true
    OR significant_emission_change = true
    OR significant_activity_change = true
    OR new_modelled_exceedance_detected = true
    OR updated_methodology_available = true
```

## Logica decisionale a livello di rappresentatività

```text
for each sampling point sp, pollutant p and metric m:
    validate method and data quality
    derive preliminary concentration-similarity area
    refine area using:
        station type
        emission context
        geography
        dispersion context
        territorial boundaries
        expert judgement where necessary
    store AREA_REPR(sp,p,m,period)

for each modelled exceedance area A:
    compare A with union of fixed-measurement AREA_REPR geometries
    if A fully covered:
        support possible non-exceedance evaluation under Art. 8(5)
    else:
        identify uncovered area
        expose uncovered area to M_ASSESS and M_NETWORK
```

## Interazioni con altri moduli

- `M_ZONE` — fornisce zone, unità territoriali e contesto dei confini
- `M_ASSESS` — usa la rappresentatività per lo stato dei superamenti modellati e gli output dei metodi di valutazione
- `M_NETWORK` — usa la rappresentatività per copertura di rete, riduzione, rilocalizzazione e misurazioni aggiuntive
- `M_LIMITS` — usa la rappresentatività per verifica di conformità e qualificazione dei superamenti modellati
- `M_MOD` — fornisce campi di concentrazione e aree modellate di superamento
- `M_MODEL_QA` — valida le applicazioni di modellizzazione usate per determinare la rappresentatività
- `M_DATA_QUALITY` — valida dati di misurazione e misurazioni indicative
- `M_PUBLIC_INFORMATION` — usa metadati spaziali per output di valutazione destinati al pubblico
- `M_REPORTING` — usa geometrie e metadati per la rendicontazione
- `M_TRANSBOUNDARY` — usa informazioni transfrontaliere su rappresentatività e superamenti
- `T_REPR_TOLERANCE` — valori di tolleranza e parametri metodologici
- `T_SITING` — tipo di stazione e criteri di ubicazione

## Output

```text
representativeness_status:
  sampling_point_id
  pollutant
  metric
  assessment_period
  station_type
  zone_id
  territorial_unit_id
  area:
    geometry
    area_size
    method = measurement | modelling | hybrid | expert_judgement
    concentration_interval
    central_value
    tolerance
    confidence_or_uncertainty_metadata
  validity:
    data_quality_valid
    method_valid
    model_valid_if_used
    representativeness_valid
    valid_for_compliance
    valid_for_network_reduction
    valid_for_relocation_support
  coverage:
    covers_modelled_exceedance_area
    modelled_exceedance_area_id
    uncovered_modelled_exceedance_area_geometry
    covering_sampling_points
    overlap_resolution_status
  relocation:
    old_area_geometry
    new_area_geometry
    coverage_loss_area
    coverage_gain_area
    relocation_support_valid
  downstream:
    public_information_metadata_ready
    reporting_metadata_ready
    transboundary_context_available
```

## Note

- La rappresentatività spaziale non è un semplice buffer geometrico attorno a una stazione; è una determinazione regolatoria che collega concentrazione misurata, tipo di stazione, inquinante, metrica, territorio e uso di valutazione.
- Un superamento modellato non può essere ignorato solo perché esiste una misurazione in sito fisso da qualche parte nella zona. La misurazione fissa deve avere un’area di rappresentatività valida che copre l’area modellata di superamento.
- Le aree modellate di superamento non coperte non sono automaticamente violazioni di conformità, ma sono critiche per la valutazione e possono attivare conseguenze di monitoraggio aggiuntivo.
- Le aree di rappresentatività devono essere versionate perché modifiche della rete, delle emissioni e della modellizzazione aggiornata possono alterarne l’uso regolatorio.
- La rappresentatività transfrontaliera dovrebbe essere preservata come metadato ove rilevante per cooperazione transfrontaliera e rendicontazione.
