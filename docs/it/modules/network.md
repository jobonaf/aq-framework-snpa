# M_NETWORK — Rete di monitoraggio

## Riferimenti normativi

- Art. 8 Direttiva (UE) 2024/2881
- Art. 9 Direttiva (UE) 2024/2881
- Art. 10 Direttiva (UE) 2024/2881
- Art. 11 Direttiva (UE) 2024/2881
- Allegato II — soglie di valutazione
- Allegato III — numero minimo di punti di campionamento
- Allegato IV — criteri di ubicazione e posizionamento
- Allegato V — obiettivi di qualità dei dati
- Allegato VI — metodi di riferimento e condizioni per la modellizzazione
- Allegato VII — misurazioni presso supersiti e precursori dell’ozono

## Descrizione

Questo modulo verifica l’**adeguatezza strutturale e giuridica della rete di monitoraggio** per ciascun inquinante e zona.

Determina se la rete:

- contiene il numero minimo richiesto di punti di campionamento;
- soddisfa i criteri di ubicazione e posizionamento;
- è coerente con il regime di valutazione definito in `M_ASSESS`;
- può ridurre legittimamente i punti di misurazione fissi usando modellizzazione o misurazioni indicative;
- include i componenti richiesti per ozono, IPA, UFP e indicatore di esposizione media;
- rispetta le restrizioni alla rilocalizzazione;
- implementa le misurazioni aggiuntive richieste dopo superamenti modellati non coperti;
- include supersiti di monitoraggio quando richiesto.

Questo modulo **non** determina la conformità ai valori limite. La conformità e la qualificazione dei superamenti sono gestite da `M_LIMITS`.

## Definizioni

```text
p = inquinante
z = zona
u = unità territoriale di esposizione media
st = punto di campionamento
ms = Stato membro
```

```text
N_active(p,z) = numero di punti di campionamento attivi per l’inquinante p nella zona z
N_min(p,z) = numero minimo di punti di campionamento richiesto, come definito in T_MIN_STATIONS / Allegato III
N_min_eff(p,z) = numero minimo effettivo dopo eventuale riduzione legittima
VALID_SITING(st) = il punto st rispetta i criteri dell’Allegato IV
ABOVE_THRESHOLD(p,z) = zona sopra la soglia di valutazione secondo M_ASSESS
EXCEEDS_STANDARD(p,z) = superamento di valore limite, valore-obiettivo o livello critico secondo M_LIMITS
AREA_REPR(st) = area di rappresentatività spaziale del punto st
NETWORK_OK(p,z) = la rete soddisfa i requisiti applicabili
```

## Requisiti normativi

### REQ-NETWORK-SAMPLING_POINT_LOCATION

**Regola**  
L’ubicazione dei punti di campionamento deve essere determinata conformemente all’Allegato IV.

```text
for each sampling_point st:
    VALID_SITING(st) = true
```

### REQ-NETWORK-MINIMUM_ADEQUACY_ABOVE_THRESHOLD

**Regola**  
In ciascuna zona in cui i livelli dell’inquinante superano la soglia di valutazione dell’Allegato II, il numero minimo di punti di campionamento per misurazioni in siti fissi deve rispettare l’Allegato III.

```text
if ABOVE_THRESHOLD(p,z) = true:
    NETWORK_MINIMUM_OK(p,z) =
        N_active_fixed(p,z) >= N_min_eff(p,z)
        AND all active sampling points satisfy VALID_SITING
```

### REQ-NETWORK-MINIMUM_COMPOSITION

**Regola**  
Quando richiesto dagli Allegati III e IV, la rete deve includere punti di campionamento rappresentativi delle situazioni di esposizione pertinenti, incluse ubicazioni di fondo urbano e orientate al traffico.

```text
if pollutant_requires_background_and_traffic_points(p):
    N_background(p,z) >= 1
    AND N_traffic(p,z) >= 1
```

**Nota**  
La composizione quantitativa esatta è governata da `T_MIN_STATIONS` e `T_SITING`.

### REQ-NETWORK-TRAFFIC_REPRESENTATION

**Regola**  
Per gli inquinanti per cui l’esposizione da traffico è rilevante, la rete deve includere punti di campionamento destinati a catturare i contributi emissivi del traffico.

```text
if p in [NO2, PM10, PM2_5, benzene, CO]:
    N_traffic(p,z) >= 1
```

### REQ-NETWORK-BACKGROUND_TRAFFIC_BALANCE

**Regola**  
Per gli inquinanti applicabili, il numero di punti di fondo urbano e punti orientati al traffico deve restare bilanciato secondo l’Allegato III.

```text
if p in [NO2, PM10, PM2_5, benzene, CO]
AND N_background(p,z) > 0
AND N_traffic(p,z) > 0:
    max(
        N_background(p,z) / N_traffic(p,z),
        N_traffic(p,z) / N_background(p,z)
    ) <= 2
```

### REQ-NETWORK-REDUCTION_ALLOWED

**Regola**  
Il numero minimo di punti di campionamento per misurazioni in siti fissi può essere ridotto solo quando tutte le condizioni dell’Art. 9(3) sono soddisfatte.

La riduzione è possibile solo quando i livelli dell’inquinante superano la soglia di valutazione pertinente ma non superano i rispettivi valori limite, valori-obiettivo o livelli critici.

Misurazioni indicative o applicazioni di modellizzazione devono fornire informazioni sufficienti per valutare valori limite, valori-obiettivo, livelli critici, soglie di allarme, soglie di informazione e informazione adeguata al pubblico.

```text
reduction_allowed(p,z) =
    ABOVE_THRESHOLD(p,z) = true
    AND EXCEEDS_STANDARD(p,z) = false
    AND supplementary_assessment_sufficient = true
    AND spatial_resolution_sufficient = true
    AND AnnexV_A_B_E_requirements_satisfied = true
    AND public_information_sufficient = true
    AND ozone_specific_conditions_satisfied = true
```

### REQ-NETWORK-REDUCTION_INDICATIVE_MEASUREMENTS

**Regola**  
Quando misurazioni indicative giustificano la riduzione delle misurazioni fisse, il loro numero deve essere almeno pari al numero di misurazioni fisse sostituite e devono essere distribuite uniformemente nell’anno civile.

```text
if reduction_allowed = true
AND indicative_measurements_used = true:
    N_indicative(p,z) >= N_fixed_replaced(p,z)
    AND indicative_measurements_evenly_distributed_over_year = true
```

### REQ-NETWORK-REDUCTION_OZONE_NO2

**Regola**  
Per l’ozono, il biossido di azoto deve essere misurato presso tutti i punti di campionamento dell’ozono rimanenti, salvo presso ubicazioni di fondo rurale dove possono essere usati altri metodi di misurazione.

```text
if p = ozone:
    for each remaining_ozone_sampling_point st:
        if st.location_type != rural_background:
            NO2_measured_continuously(st) = true
```

### REQ-NETWORK-REDUCTION_LIMIT

**Regola**  
La riduzione dei punti di campionamento con misurazioni fisse non deve portare la rete sotto il minimo consentito dall’Allegato III e dall’Art. 9(3).

```text
if reduction_allowed = true:
    N_active_fixed(p,z) >= N_min_eff(p,z)
else:
    N_active_fixed(p,z) >= N_min(p,z)
```

**Nota**  
Quando il limite quantitativo di riduzione è specifico per tabella, `N_min_eff` deve essere calcolato da `T_MIN_STATIONS`.

### REQ-NETWORK-ADDITIONAL_MEASUREMENTS_AFTER_MODELLED_EXCEEDANCE

**Regola**  
Quando applicazioni di modellizzazione mostrano un superamento non coperto da misurazioni in siti fissi e dalla loro area di rappresentatività spaziale, misurazioni aggiuntive fisse o indicative possono o devono essere usate in base al trigger.

- Nei casi di valutazione Art. 8(3) o Art. 8(4), le misurazioni aggiuntive sono consentite.
- Nei casi di riduzione Art. 9(3), le misurazioni aggiuntive sono obbligatorie.

Le misurazioni aggiuntive fisse devono essere istituite entro due anni dalla data del superamento modellato. Le misurazioni aggiuntive indicative entro un anno. Devono coprire almeno un anno civile e rispettare la copertura minima dell’Allegato V.

```text
if modelled_exceedance_not_covered_by_fixed_measurements = true:
    if trigger = Art9_3_reduction:
        additional_measurement_required = true
    if trigger in [Art8_3, Art8_4]:
        additional_measurement_permitted = true
    if additional_fixed_measurement_used = true:
        establishment_deadline = modelled_exceedance_date + 2 years
    if additional_indicative_measurement_used = true:
        establishment_deadline = modelled_exceedance_date + 1 year
    measurement_duration >= 1 calendar_year
    AND AnnexV_minimum_coverage_satisfied = true
```

### REQ-NETWORK-RELOCATION_RESTRICTION

**Regola**  
I punti di campionamento che hanno registrato superamenti di un valore limite o valore-obiettivo nei tre anni precedenti non devono essere rilocalizzati, salvo che la rilocalizzazione sia supportata da applicazioni di modellizzazione o misurazioni indicative e pienamente documentata secondo l’Allegato IV.

```text
if recorded_exceedance_in_previous_3_years(st) = true:
    relocation_allowed(st) =
        modelling_or_indicative_support = true
        AND relocation_justification_documented = true
        AND new_location_satisfies_AnnexIV = true
else:
    relocation_allowed(st) = evaluate_under_general_siting_rules
```

### REQ-NETWORK-AVERAGE_EXPOSURE_INDICATOR_POINTS

**Regola**  
I punti di campionamento usati per calcolare gli indicatori di esposizione media per PM2,5 e NO2 devono essere distribuiti in modo adeguato per riflettere l’esposizione generale della popolazione e non devono essere meno del numero determinato dall’Allegato III.

```text
if p in [PM2_5, NO2]
AND metric = average_exposure_indicator:
    N_AEI_points(p,u) >= N_min_AEI(p,u)
    AND AEI_spatial_distribution_adequate = true
    AND all AEI sampling points satisfy AnnexIV = true
```

### REQ-NETWORK-OZONE_PRECURSOR_SUBSTANCES

**Regola**  
I punti di campionamento per i precursori dell’ozono devono essere assicurati conformemente all’Allegato VII.

```text
ozone_precursor_sampling_points_present =
    required_ozone_precursor_points(p,z) satisfied
```

### REQ-NETWORK-OZONE_RURAL_BACKGROUND

**Regola**  
Per la valutazione a lungo termine dell’ozono, la rete deve includere punti di fondo rurale secondo densità e requisiti territoriali definiti negli Allegati III e IV.

```text
rural_ozone_background_density_ok = true
```

**Nota**  
Le soglie esatte di densità sono governate da `T_MIN_STATIONS`.

### REQ-NETWORK-PAH_MONITORING

**Regola**  
Presso un numero limitato di punti di campionamento, il contributo del benzo(a)pirene nell’aria ambiente deve essere valutato monitorando gli idrocarburi policiclici aromatici pertinenti.

I punti IPA devono, ove opportuno, essere co-localizzati con i punti di benzo(a)pirene e selezionati in modo da identificare variazioni geografiche e tendenze a lungo termine.

```text
PAH_monitoring_ok =
    required_PAH_species_monitored = true
    AND PAH_points_colocated_with_BaP_where_required = true
    AND PAH_points_support_geographical_variation_and_trend_analysis = true
```

### REQ-NETWORK-UFP_MONITORING

**Regola**  
I livelli di particolato ultrafine devono essere monitorati conformemente agli Art. 9 e 10 e agli Allegati III e VII, inclusi i siti in cui sono probabili concentrazioni elevate e i requisiti applicabili sui supersiti.

```text
UFP_monitoring_ok =
    N_UFP_points >= N_min_UFP
    AND high_concentration_locations_covered = true
    AND applicable_supersite_UFP_requirements_satisfied = true
```

### REQ-NETWORK-SUPERSITES_URBAN_BACKGROUND

**Regola**  
I supersiti di monitoraggio devono essere istituiti presso ubicazioni di fondo urbano almeno secondo i requisiti basati sulla popolazione dell’Art. 10.

```text
N_urban_background_supersites(ms) >= N_min_urban_background_supersites(ms)
```

### REQ-NETWORK-SUPERSITES_RURAL_BACKGROUND

**Regola**  
I supersiti di monitoraggio devono essere istituiti presso ubicazioni di fondo rurale secondo i requisiti basati sulla dimensione territoriale dell’Art. 10.

```text
N_rural_background_supersites(ms) >= N_min_rural_background_supersites(ms)
```

### REQ-NETWORK-SUPERSITE_SITING

**Regola**  
L’ubicazione dei supersiti di monitoraggio deve essere determinata conformemente all’Allegato IV.

```text
for each supersite ss:
    supersite_siting_valid(ss) = true
```

### REQ-NETWORK-SUPERSITE_COUNTING_TOWARDS_MINIMUM

**Regola**  
I punti di campionamento installati presso supersiti possono contribuire al numero minimo richiesto per gli inquinanti pertinenti solo quando soddisfano i requisiti applicabili degli Allegati IV e III.

```text
if sampling_point_at_supersite = true
AND pollutant_specific_requirements_satisfied = true
AND AnnexIV_requirements_satisfied = true:
    count_towards_minimum_number = true
else:
    count_towards_minimum_number = false
```

### REQ-NETWORK-JOINT_SUPERSITES

**Regola**  
Stati membri confinanti possono istituire supersiti comuni. I supersiti comuni non eliminano l’obbligo individuale di ciascuno Stato membro di rispettare i requisiti sui supersiti applicabili.

```text
if joint_supersite_used = true:
    individual_member_state_obligations_preserved = true
```

### REQ-NETWORK-SUPERSITE_POLLUTANTS

**Regola**  
Presso i supersiti di monitoraggio ubicati in siti di fondo urbano e rurale devono essere monitorati gli inquinanti elencati nell’Allegato VII, Sezione 1, Tabelle 1 e 2.

```text
for each supersite ss:
    monitored_pollutants(ss) includes required_AnnexVII_pollutants(ss.type)
```

### REQ-NETWORK-RURAL_SUPERSITE_OPTIONAL_NON_MEASUREMENT

**Regola**  
Quando il numero di supersiti di fondo rurale supera quello dei supersiti di fondo urbano con rapporto almeno 2:1, black carbon, particolato ultrafine o ammoniaca possono essere omessi nella metà dei supersiti di fondo rurale, purché la selezione sia rappresentativa per tali inquinanti.

```text
if N_rural_background_supersites >= 2 * N_urban_background_supersites:
    optional_non_measurement_allowed_for_selected_pollutants = true
    AND representative_selection = true
```

### REQ-NETWORK-MEASUREMENT_METHODS

**Regola**  
I metodi di misurazione di riferimento devono essere usati conformemente all’Allegato VI. Altri metodi possono essere usati solo alle condizioni dell’Allegato VI. Tutti i dati di valutazione della qualità dell’aria devono rispettare gli obiettivi di qualità dei dati dell’Allegato V.

```text
if method = reference_method:
    method_valid = true
else:
    method_valid = AnnexVI_equivalence_or_conditions_satisfied

assessment_data_quality_ok = AnnexV_data_quality_objectives_satisfied
```

### REQ-NETWORK-MODELLING_METHODS_LINK

**Regola**  
Quando le applicazioni di modellizzazione sono usate nella valutazione o a supporto della riduzione della rete, devono soddisfare le condizioni dell’Allegato VI.

```text
if modelling_used_for_network_or_assessment = true:
    VALID_MODEL = AnnexVI_PointE_conditions_satisfied
```

## Decisione di adeguatezza a livello di rete

```text
NETWORK_OK(p,z) =
    sampling_point_location_ok
    AND minimum_adequacy_ok
    AND composition_ok
    AND reduction_conditions_ok_if_reduction_applied
    AND relocation_conditions_ok
    AND additional_measurements_ok_if_triggered
    AND pollutant_specific_requirements_ok
    AND method_requirements_ok
```

## Interazioni con altri moduli

- `M_ZONE` — fornisce zone e unità territoriali di esposizione media
- `M_ASSESS` — determina soglia sopra/sotto e regime di valutazione
- `M_LIMITS` — determina superamenti di valori limite, valori-obiettivo e livelli critici
- `M_DATA_QUALITY` — valida dati di misurazione e requisiti dell’Allegato V
- `M_REPR` — definisce aree di rappresentatività spaziale
- `M_MOD` — fornisce campi di concentrazione modellati e supporta decisioni di rilocalizzazione/riduzione
- `M_MODEL_QA` — valida le applicazioni di modellizzazione ai sensi dell’Allegato VI
- `M_PUBLIC_INFORMATION` — usa output di rete/valutazione quando è attivata l’informazione al pubblico
- `T_MIN_STATIONS` — requisiti quantitativi dell’Allegato III
- `T_SITING` — requisiti di posizionamento e ubicazione dell’Allegato IV
- `T_SUPERSITES` — soglie popolazione/area e obblighi sugli inquinanti per i supersiti

## Output

```text
network_status:
  zone_id
  pollutant
  assessment_threshold_status
  sampling_points:
    active_fixed_count
    effective_minimum_required
    minimum_number_satisfied
    siting_valid
    background_points
    traffic_points
    rural_background_points
  reduction:
    reduction_applied
    reduction_allowed
    fixed_points_replaced
    indicative_measurements_used
    modelling_used
    reduction_conditions_satisfied
  relocation:
    relocation_requested
    relocation_allowed
    relocation_documented
  additional_measurements:
    modelled_exceedance_not_covered
    additional_measurement_required
    additional_measurement_permitted
    additional_measurement_type
    establishment_deadline
    minimum_duration_satisfied
    AnnexV_coverage_satisfied
  pollutant_specific:
    ozone_precursors_ok
    ozone_NO2_monitoring_ok
    PAH_monitoring_ok
    UFP_monitoring_ok
    AEI_points_ok
  supersites:
    urban_background_supersites_required
    urban_background_supersites_present
    rural_background_supersites_required
    rural_background_supersites_present
    supersite_pollutants_ok
  method_status:
    reference_or_equivalent_methods_ok
    modelling_conditions_ok
    data_quality_objectives_ok
  network_ok
```

## Note

- L’adeguatezza della rete non equivale alla conformità ai valori limite o valori-obiettivo.
- La riduzione legittima delle misurazioni in siti fissi richiede condizioni più stringenti della sola disponibilità di un modello valido.
- Un superamento modellato fuori dalla rappresentatività spaziale delle misurazioni in siti fissi può attivare conseguenze di monitoraggio aggiuntivo.
- I supersiti possono contribuire agli obblighi di numero minimo solo quando sono soddisfatti i requisiti specifici per inquinante e di ubicazione.
- La rilocalizzazione di punti di campionamento che hanno registrato superamenti è eccezionale e richiede supporto modellistico o indicativo più documentazione completa.
