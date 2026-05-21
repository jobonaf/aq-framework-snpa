# M_EXPOSURE — Indicatore di esposizione media e obblighi di esposizione

## Riferimenti normativi

- Art. 4(18) Direttiva (UE) 2024/2881 — unità territoriale di esposizione media
- Art. 4(28) Direttiva (UE) 2024/2881 — ubicazioni di fondo urbano
- Art. 4(29) Direttiva (UE) 2024/2881 — ubicazioni di fondo rurale
- Art. 4(33) Direttiva (UE) 2024/2881 — indicatore di esposizione media
- Art. 4(34) Direttiva (UE) 2024/2881 — obbligo di riduzione dell’esposizione media
- Art. 4(35) Direttiva (UE) 2024/2881 — obiettivo di concentrazione dell’esposizione media
- Art. 4(40) Direttiva (UE) 2024/2881 — contributi da fonti naturali
- Art. 6 Direttiva (UE) 2024/2881 — istituzione delle unità territoriali di esposizione media
- Art. 9(6) Direttiva (UE) 2024/2881 — punti di campionamento per gli indicatori di esposizione media
- Art. 12(3) Direttiva (UE) 2024/2881 — mantenimento sotto gli obiettivi di concentrazione dell’esposizione media
- Art. 13(3), Art. 13(5) Direttiva (UE) 2024/2881 — obblighi di riduzione dell’esposizione media e valutazione degli indicatori di esposizione media
- Art. 16 Direttiva (UE) 2024/2881 — interfaccia con l’attribuzione a fonti naturali
- Art. 19(3) Direttiva (UE) 2024/2881 — piani per la qualità dell’aria quando l’obbligo di riduzione dell’esposizione media non è raggiunto
- Art. 23 Direttiva (UE) 2024/2881 — interfaccia di rendicontazione
- Allegato I, Sezione 5 — indicatori di esposizione media, obblighi di riduzione e obiettivi di concentrazione
- Allegato III — numero minimo e distribuzione dei punti di campionamento
- Allegato IV — criteri di ubicazione e posizionamento
- Allegato V — obiettivi di qualità dei dati

## Descrizione

Questo modulo calcola e valuta l’**indicatore di esposizione media** (`AEI`, in italiano anche `IEM`) per gli inquinanti soggetti a obblighi relativi all’esposizione della popolazione, principalmente PM2,5 e NO2.

L’IEM/AEI è uno strumento di valutazione dell’esposizione della popolazione usato per determinare se un’unità territoriale di esposizione media soddisfa:

- l’obiettivo di concentrazione dell’esposizione media;
- l’obbligo di riduzione dell’esposizione media;
- gli obblighi di mantenimento quando i livelli sono già inferiori all’obiettivo di esposizione applicabile;
- i trigger di pianificazione quando gli obblighi di riduzione dell’esposizione non sono raggiunti.

Questo modulo è distinto dalla verifica ordinaria di conformità ai valori limite. Non determina la conformità a valori limite, valori-obiettivo, soglie di allarme o soglie di informazione.

Questo modulo **non**:

- definisce zone o unità territoriali di esposizione media;
- determina la rete di monitoraggio;
- valida la qualità dei dati;
- attribuisce contributi da fonti naturali;
- istituisce piani per la qualità dell’aria;
- effettua informazione al pubblico o rendicontazione.

Tali funzioni sono gestite da moduli dedicati.

## Ambito

Il modulo si applica a:

```text
pollutants = [PM2_5, NO2]
```

quando l’Allegato I, Sezione 5 definisce:

- indicatori di esposizione media;
- obiettivi di concentrazione dell’esposizione media;
- obblighi di riduzione dell’esposizione media;
- periodi di base o di riferimento;
- anni obiettivo o periodi di conformità;
- regole di calcolo transitorie o eccezionali.

Il dominio territoriale è l’**unità territoriale di esposizione media** (`AETU`) fornita da `M_ZONE`.

## Definizioni

```text
p = inquinante
u = unità territoriale di esposizione media
y = anno civile
sp = punto di campionamento
```

```text
AETU(u) =
unità territoriale di esposizione media definita da M_ZONE
```

```text
AEI_STATIONS(p,u,y) =
punti di campionamento validi usati per il calcolo dell’IEM/AEI
per l’inquinante p, l’unità territoriale u e l’anno y
```

```text
C_ann(sp,p,y) =
concentrazione media annua per l’inquinante p
presso il punto di campionamento sp nell’anno y
```

```text
C_ann_adj(sp,p,y) =
concentrazione media annua dopo eventuale aggiustamento regolatorio valido,
incluso l’aggiustamento per fonti naturali ove applicabile
```

```text
AEI(p,u,y) =
indicatore di esposizione media per l’inquinante p,
l’unità territoriale u e l’anno di riferimento y
```

```text
AECO(p,u) =
obiettivo di concentrazione dell’esposizione media applicabile
all’inquinante p e all’unità u
```

```text
AERO(p,u) =
obbligo di riduzione dell’esposizione media applicabile
all’inquinante p e all’unità u
```

```text
BASELINE_AEI(p,u) =
indicatore di esposizione media di base o di riferimento
definito dall’Allegato I / T_EXPOSURE_OBLIGATIONS
```

```text
AEI_REDUCTION(p,u,y) =
riduzione percentuale dell’IEM/AEI rispetto alla baseline applicabile
```

## Requisiti normativi

### REQ-EXPOSURE-TERRITORIAL_DOMAIN

**Fonte:** Art. 4(18); Art. 6 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `M_ZONE`

**Regola**  
L’IEM/AEI deve essere calcolato per ciascuna unità territoriale di esposizione media applicabile.

**Criterio di accettazione**

```text
for each AETU u supplied by M_ZONE:
    AEI domain = u.geometry
    AETU_VALID(u) = true
```

### REQ-EXPOSURE-POLLUTANT_SCOPE

**Fonte:** Art. 12(3); Art. 13(3); Art. 13(5); Allegato I Sezione 5 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `T_EXPOSURE_OBLIGATIONS`

**Regola**  
La valutazione dell’IEM/AEI si applica agli inquinanti e agli obblighi di esposizione definiti nell’Allegato I, Sezione 5.

**Criterio di accettazione**

```text
if p in T_EXPOSURE_OBLIGATIONS.pollutants:
    AEI assessment required
else:
    AEI assessment not applicable
```

### REQ-EXPOSURE-STATION_SELECTION

**Fonte:** Art. 4(33); Art. 9(6); Allegato III; Allegato IV Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `M_NETWORK`; `M_ZONE`; `M_DATA_QUALITY`

**Regola**  
L’IEM/AEI deve essere determinato sulla base di misurazioni presso ubicazioni di fondo urbano nell’intera unità territoriale di esposizione media.

Quando nell’unità territoriale non è localizzata alcuna area urbana, possono essere usate ubicazioni di fondo rurale, conformemente alla definizione di IEM/AEI e alle regole di monitoraggio applicabili.

I punti di campionamento devono essere distribuiti in modo adeguato per riflettere l’esposizione generale della popolazione e soddisfare i requisiti degli Allegati III e IV.

**Criterio di accettazione**

```text
if urban_area_exists(u) = true:
    AEI_STATIONS(p,u,y) = valid urban_background sampling points in u
else:
    AEI_STATIONS(p,u,y) = valid rural_background sampling points in u
```

```text
AEI_station_selection_valid =
    N_AEI_points(p,u) >= N_min_AEI(p,u)
    AND AEI_spatial_distribution_adequate = true
    AND all selected stations satisfy M_NETWORK and AnnexIV siting rules
```

### REQ-EXPOSURE-DATA_VALIDITY

**Fonte:** Art. 11(3); Allegato V Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `M_DATA_QUALITY`

**Regola**  
Possono essere usati solo dati validi ai fini del calcolo dell’indicatore di esposizione media.

**Criterio di accettazione**

```text
for each sp in AEI_STATIONS(p,u,y):
    M_DATA_QUALITY.DATA_VALID(dataset(sp,p,y), p, annual_mean, average_exposure_indicator) = true
```

### REQ-EXPOSURE-ANNUAL_MEAN_INPUT

**Fonte:** Art. 4(33); Allegato I Sezione 5; Allegato V Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `M_DATA_QUALITY`

**Regola**  
L’IEM/AEI deve usare concentrazioni medie annue calcolate da dati validi per l’inquinante, la stazione e l’anno civile pertinenti.

**Criterio di accettazione**

```text
if annual_mean_valid(sp,p,y) = true:
    C_ann(sp,p,y) usable_for_AEI = true
else:
    C_ann(sp,p,y) excluded_from_AEI
```

### REQ-EXPOSURE-AEI_CALCULATION

**Fonte:** Art. 4(33); Allegato I Sezione 5 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `REQ-EXPOSURE-STATION_SELECTION`; `REQ-EXPOSURE-DATA_VALIDITY`

**Regola**  
Per un anno di riferimento, l’IEM/AEI è calcolato come media delle concentrazioni medie annue nel periodo di mediazione applicabile di tre anni civili e sui punti di campionamento IEM/AEI validi nell’unità territoriale di esposizione media.

**Criterio di accettazione**

```text
AEI(p,u,y) =
    mean{
        C_ann_adj(sp,p,yy)
        | yy in AEI_AVERAGING_YEARS(y)
        | sp in AEI_STATIONS(p,u,yy)
    }

AEI_AVERAGING_YEARS(y) = [y-2, y-1, y]
```

### REQ-EXPOSURE-THREE_YEAR_MOVING_AVERAGE

**Fonte:** Allegato I Sezione 5 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `T_EXPOSURE_OBLIGATIONS`

**Regola**  
L’IEM/AEI è basato su una media mobile di tre anni civili, salvo applicazione di una regola transitoria o eccezionale specifica.

**Criterio di accettazione**

```text
if no_special_AEI_rule_applies(y):
    AEI_AVERAGING_YEARS(y) = [y-2, y-1, y]
```

### REQ-EXPOSURE-TRANSITIONAL_2020_EXCLUSION

**Fonte:** Allegato I Sezione 5 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** conditional  
**Dipendenze:** `T_EXPOSURE_OBLIGATIONS`

**Regola**  
Quando la Direttiva o l’Allegato I consentono l’esclusione dell’anno 2020 per specifici calcoli o periodi IEM/AEI, il calcolo deve applicare la regola definita in tabella.

**Criterio di accettazione**

```text
if T_EXPOSURE_OBLIGATIONS.allows_2020_exclusion(p,u,y) = true:
    AEI_AVERAGING_YEARS(y) = averaging_years_excluding_2020_as_defined_in_table
else:
    AEI_AVERAGING_YEARS(y) = [y-2, y-1, y]
```

**Nota**  
Gli anni e le condizioni esatte sono governati dalla tabella `T_EXPOSURE_OBLIGATIONS`.

### REQ-EXPOSURE-NATURAL_SOURCE_ADJUSTMENT

**Fonte:** Art. 16; Art. 4(40) Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** conditional  
**Dipendenze:** `M_SOURCE_ATTRIBUTION`; `M_LIMITS`; `M_DATA_QUALITY`

**Regola**  
Quando contributi da fonti naturali sono validamente identificati e possono essere omessi o aggiustati per lo scopo regolatorio pertinente, la media annua aggiustata deve essere usata per il calcolo dell’IEM/AEI.

**Criterio di accettazione**

```text
if natural_source_adjustment_valid(sp,p,y) = true:
    C_ann_adj(sp,p,y) = C_ann(sp,p,y) - natural_source_contribution(sp,p,y)
else:
    C_ann_adj(sp,p,y) = C_ann(sp,p,y)
```

**Requisito di tracciabilità**

```text
if natural_source_adjustment_valid = true:
    adjustment_evidence_reference recorded
    adjusted_and_unadjusted_values retained
```

### REQ-EXPOSURE-BASELINE_AEI

**Fonte:** Allegato I Sezione 5 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory where reduction obligation applies  
**Dipendenze:** `T_EXPOSURE_OBLIGATIONS`

**Regola**  
Quando si applica un obbligo di riduzione dell’esposizione media, l’IEM/AEI di base deve essere determinato secondo l’Allegato I e `T_EXPOSURE_OBLIGATIONS`.

**Criterio di accettazione**

```text
BASELINE_AEI(p,u) =
    AEI calculated over baseline_years defined in T_EXPOSURE_OBLIGATIONS
```

### REQ-EXPOSURE-REDUCTION_CALCULATION

**Fonte:** Art. 4(34); Art. 13(3); Allegato I Sezione 5 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory where reduction obligation applies  
**Dipendenze:** `REQ-EXPOSURE-AEI_CALCULATION`; `REQ-EXPOSURE-BASELINE_AEI`; `T_EXPOSURE_OBLIGATIONS`

**Regola**  
La riduzione dell’esposizione media conseguita deve essere calcolata rispetto all’IEM/AEI di base applicabile.

**Criterio di accettazione**

```text
AEI_REDUCTION(p,u,y) =
    100 * (BASELINE_AEI(p,u) - AEI(p,u,y)) / BASELINE_AEI(p,u)

if AEI_REDUCTION(p,u,y) >= REQUIRED_REDUCTION(p,u,y):
    exposure_reduction_obligation_status = achieved
else:
    exposure_reduction_obligation_status = not_achieved
```

### REQ-EXPOSURE-CONCENTRATION_OBJECTIVE

**Fonte:** Art. 4(35); Art. 12(3); Allegato I Sezione 5 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `T_EXPOSURE_OBLIGATIONS`

**Regola**  
L’IEM/AEI deve essere confrontato con l’obiettivo di concentrazione dell’esposizione media applicabile.

Quando l’IEM/AEI è inferiore all’obiettivo, si applica un obbligo di mantenimento.

**Criterio di accettazione**

```text
if AEI(p,u,y) <= AECO(p,u,y):
    exposure_concentration_objective_status = attained
    maintenance_obligation = true
else:
    exposure_concentration_objective_status = not_attained
```

### REQ-EXPOSURE-OVERALL_EXPOSURE_STATUS

**Fonte:** Art. 12(3); Art. 13(3); Allegato I Sezione 5 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `REQ-EXPOSURE-REDUCTION_CALCULATION`; `REQ-EXPOSURE-CONCENTRATION_OBJECTIVE`

**Regola**  
Lo stato di esposizione di un’unità territoriale di esposizione media deve riflettere sia l’obiettivo di concentrazione sia l’obbligo di riduzione, ove applicabile.

**Criterio di accettazione**

```text
if exposure_concentration_objective_status = attained
AND exposure_reduction_obligation_status in [achieved, not_applicable]:
    exposure_status = compliant_or_attained
else:
    exposure_status = not_attained_or_not_achieved
```

### REQ-EXPOSURE-PLAN_TRIGGER

**Fonte:** Art. 19(3) Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** interface rule  
**Dipendenze:** `M_PLANS`; `M_LIMITS`

**Regola**  
Quando l’obbligo di riduzione dell’esposizione media non è raggiunto in una data unità territoriale di esposizione media, il risultato deve esporre un trigger di pianificazione a `M_PLANS`.

**Criterio di accettazione**

```text
if exposure_reduction_obligation_status = not_achieved:
    air_quality_plan_trigger = true
    planning_domain = average_exposure_territorial_unit
```

### REQ-EXPOSURE-NOT_LIMIT_VALUE_COMPLIANCE

**Fonte:** Art. 4(33); Art. 13; Allegato I Sezione 5 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** framework boundary rule  
**Dipendenze:** `M_LIMITS`

**Regola**  
La valutazione dell’IEM/AEI non sostituisce la verifica di conformità ai valori limite o ai valori-obiettivo.

**Criterio di accettazione**

```text
AEI_result not used as limit_value_compliance_result
```

### REQ-EXPOSURE-REPRESENTATIVENESS_INTERFACE

**Fonte:** Art. 4(33); Art. 9(6); Allegato IV Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** interface rule  
**Dipendenze:** `M_REPR`; `M_NETWORK`

**Regola**  
I punti di campionamento usati per l’IEM/AEI devono rappresentare l’esposizione generale della popolazione nell’unità territoriale di esposizione media.

Questo modulo usa il contesto di stazione e gli output di rappresentatività; non calcola le geometrie di rappresentatività.

**Criterio di accettazione**

```text
for each sp in AEI_STATIONS:
    station_context_valid_for_population_exposure = true
    representativeness_status_available = true
```

### REQ-EXPOSURE-DATA_GAPS_AND_STATION_CHANGES

**Fonte:** Allegato V; Art. 9(6) Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** mandatory  
**Dipendenze:** `M_DATA_QUALITY`; `M_NETWORK`; `M_REPR`

**Regola**  
Quando disponibilità delle stazioni IEM/AEI, ubicazione delle stazioni, rappresentatività o validità dei dati cambiano durante il periodo di mediazione, il calcolo deve documentare la modifica e determinare se l’IEM/AEI resta valido.

**Criterio di accettazione**

```text
if AEI_station_set_changes_during_averaging_period = true:
    station_change_documented = true
    AEI_continuity_assessed = true
    AEI_validity_status determined
```

### REQ-EXPOSURE-REPORTING_TRACEABILITY

**Fonte:** Art. 23 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** interface rule  
**Dipendenze:** `M_REPORTING`; `M_ZONE`; `M_DATA_QUALITY`

**Regola**  
I risultati IEM/AEI devono conservare la tracciabilità verso unità territoriali, insiemi di stazioni, stato di qualità dei dati, stato di aggiustamento e periodi di calcolo ai fini della rendicontazione.

**Criterio di accettazione**

```text
if AEI_result_reported = true:
    reporting_metadata_complete = true
    AETU_version recorded
    station_set_version recorded
    data_quality_status recorded
    calculation_years recorded
```

### REQ-EXPOSURE-PUBLIC_INFORMATION_INTERFACE

**Fonte:** Art. 22 Dir. (UE) 2024/2881  
**Stato:** STABLE  
**Tipo:** interface rule  
**Dipendenze:** `M_PUBLIC_INFORMATION`

**Regola**  
Quando le informazioni IEM/AEI sono usate per l’informazione al pubblico, devono essere comunicate con il pertinente contesto territoriale, inquinante, periodo e stato di conseguimento/riduzione.

**Criterio di accettazione**

```text
if AEI_result_used_for_public_information = true:
    public_information_metadata_ready = true
    AETU_label available
    pollutant and period identified
    exposure_status identified
```

## Logica di calcolo dell’esposizione

```text
for each pollutant p subject to AEI obligations:
    for each average exposure territorial unit u:
        validate AETU(u)

        for each year y:
            determine AEI_AVERAGING_YEARS(y)
            select AEI_STATIONS(p,u,yy) for each yy
            verify station selection and representativeness
            verify data validity for annual means
            apply valid natural-source adjustments where applicable
            calculate AEI(p,u,y)
            compare AEI with AECO

            if reduction obligation applies:
                calculate BASELINE_AEI
                calculate AEI_REDUCTION
                compare with REQUIRED_REDUCTION

            determine exposure_status
            expose planning, reporting and public-information outputs
```

## Interazioni con altri moduli

- `M_ZONE` — fornisce unità territoriali di esposizione media e versioni
- `M_NETWORK` — fornisce punti di campionamento IEM/AEI, contesto di ubicazione e adeguatezza della distribuzione
- `M_DATA_QUALITY` — valida i dataset di media annua per l’uso IEM/AEI
- `M_REPR` — fornisce rappresentatività e contesto di esposizione della popolazione
- `M_LIMITS` — usa lo stato di esposizione per rendicontazione più ampia di conformità e obblighi
- `M_SOURCE_ATTRIBUTION` — fornisce evidenze di contributo da fonti naturali quando è consentito l’aggiustamento
- `M_PLANS` — usa trigger di pianificazione quando gli obblighi di riduzione dell’esposizione non sono raggiunti
- `M_PUBLIC_INFORMATION` — usa metadati IEM/AEI destinati al pubblico
- `M_REPORTING` — usa calcolo IEM/AEI, insiemi di stazioni e metadati territoriali
- `T_EXPOSURE_OBLIGATIONS` — archivia obiettivi, obblighi, periodi di base, anni obiettivo e regole transitorie
- `T_DATA_QUALITY` — archivia requisiti di qualità dei dati dell’Allegato V

## Output

```text
exposure_status:
  pollutant
  average_exposure_territorial_unit_id
  AETU_version
  reference_year

  calculation:
    averaging_years
    station_set
    station_set_version
    annual_mean_values
    adjusted_annual_mean_values
    natural_source_adjustment_applied
    AEI_value
    AEI_validity_status

  objectives:
    average_exposure_concentration_objective
    exposure_concentration_objective_status = attained | not_attained | not_applicable
    maintenance_obligation

  reduction:
    baseline_years
    baseline_AEI
    required_reduction
    achieved_reduction
    exposure_reduction_obligation_status = achieved | not_achieved | not_applicable

  downstream:
    air_quality_plan_trigger
    reporting_metadata_complete
    public_information_metadata_ready

  data_quality:
    data_quality_status
    incomplete_but_conclusive
    excluded_data_summary
```

## Note

- `M_EXPOSURE` è mantenuto come modulo separato perché la valutazione IEM/AEI non è né conformità ordinaria ai valori limite né validazione generica della qualità dei dati.
- L’IEM/AEI è calcolato sulle unità territoriali di esposizione media, non sulle zone ordinarie, salvo necessità di collegare il risultato alle zone in un modulo downstream.
- La selezione delle stazioni è importante quanto la media aritmetica: i punti IEM/AEI devono riflettere l’esposizione generale della popolazione.
- Gli aggiustamenti per fonti naturali devono conservare sia i valori aggiustati sia quelli non aggiustati per la tracciabilità.
- Le conseguenze di pianificazione non sono implementate qui; il modulo emette il trigger usato da `M_PLANS`.
