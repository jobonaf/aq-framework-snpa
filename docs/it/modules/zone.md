# M_ZONE — Suddivisione territoriale e domini di valutazione

## Riferimenti normativi

- Art. 4 Direttiva (UE) 2024/2881 — definizioni di zona, unità territoriale di esposizione media, agglomerato, valutazione e concetti territoriali correlati
- Art. 6 Direttiva (UE) 2024/2881 — istituzione di zone e unità territoriali di esposizione media
- Art. 7 Direttiva (UE) 2024/2881 — classificazione delle zone rispetto alle soglie di valutazione
- Art. 8 Direttiva (UE) 2024/2881 — valutazione in tutte le zone
- Art. 9 Direttiva (UE) 2024/2881 — requisiti della rete di monitoraggio per zona e unità territoriale di esposizione media
- Art. 10 Direttiva (UE) 2024/2881 — supersiti di monitoraggio e requisiti territoriali
- Art. 12–15 Direttiva (UE) 2024/2881 — mantenimento, conformità, livelli critici, soglie di allarme e informazione
- Art. 16–18 Direttiva (UE) 2024/2881 — interfacce per fonti naturali, sabbiatura/salatura invernale e proroga del termine di conseguimento
- Art. 19–21 Direttiva (UE) 2024/2881 — piani, piani d’azione a breve termine e cooperazione transfrontaliera
- Art. 22–23 Direttiva (UE) 2024/2881 — informazione al pubblico e rendicontazione
- Allegato I — standard di qualità dell’aria e obblighi di esposizione
- Allegato II — soglie di valutazione
- Allegato III — numero minimo di punti di campionamento
- Allegato IV — ubicazione e rappresentatività spaziale
- Allegato VIII — piani per la qualità dell’aria e tabelle di marcia

## Descrizione

Questo modulo definisce i **domini territoriali di valutazione** usati dal framework per la qualità dell’aria.

Disciplina:

- zone;
- agglomerati;
- unità territoriali di esposizione media;
- unità territoriali pertinenti usate per ozono, piani, tabelle di marcia e piani d’azione a breve termine;
- contesto territoriale transfrontaliero per l’inquinamento transfrontaliero;
- versioning e rendicontazione delle modifiche territoriali.

Il modulo fornisce il contesto territoriale per classificazione di valutazione, requisiti della rete di monitoraggio, rappresentatività spaziale, verifica di conformità, indicatori di esposizione, piani e tabelle di marcia, informazione al pubblico, rendicontazione alla Commissione e cooperazione transfrontaliera.

Questo modulo **non**:

- classifica le zone sopra o sotto le soglie di valutazione;
- determina la conformità ai valori limite o valori-obiettivo;
- calcola indicatori di esposizione media;
- progetta la rete di monitoraggio;
- istituisce piani per la qualità dell’aria o piani d’azione a breve termine.

Tali funzioni sono gestite da moduli dedicati.

## Definizioni

```text
territory(ms) =
territorio di uno Stato membro rilevante per la valutazione della qualità dell’aria ambiente
```

```text
zone =
parte del territorio di uno Stato membro, delimitata da tale Stato membro
ai fini della valutazione e gestione della qualità dell’aria
```

```text
agglomeration =
conurbazione con popolazione > 250 000 abitanti oppure,
quando la popolazione <= 250 000 abitanti,
con densità di popolazione per km² stabilita dallo Stato membro
```

```text
average_exposure_territorial_unit =
parte del territorio di uno Stato membro designata per determinare l’indicatore di esposizione media,
corrispondente a una regione NUTS 1 o NUTS 2, oppure a una combinazione di regioni NUTS 1/NUTS 2 adiacenti,
nel rispetto dei vincoli dimensionali definiti dalla Direttiva
```

```text
assessment_domain =
unità territoriale sulla quale si applica una valutazione, un obbligo di monitoraggio,
una verifica di conformità, un piano, una tabella di marcia,
un output di informazione al pubblico o un obbligo di rendicontazione
```

```text
zone_version =
versione timestamped della geometria, degli attributi e dello stato giuridico della zona
```

```text
ZONE_COVERAGE_VALID =
tutto il territorio rilevante è assegnato a una sola zona attiva,
salvo quando regole giuridiche o di rendicontazione specifiche richiedono unità territoriali sovrapposte aggiuntive
```

## Tipi di oggetti territoriali

```text
territorial_object_type =
  zone
  agglomeration
  average_exposure_territorial_unit
  ozone_territorial_unit
  plan_area
  roadmap_area
  short_term_action_area
  transboundary_affected_area
  reporting_area
```

## Requisiti normativi

### REQ-ZONE-TERRITORIAL_COVERAGE

**Regola**  
Il territorio dello Stato membro deve essere suddiviso in zone ai fini della valutazione e gestione della qualità dell’aria.

```text
ZONE_COVERAGE_VALID =
    for all x in territory(ms):
        exists exactly one active zone z such that x ∈ z
```

**Nota**  
Unità territoriali aggiuntive, come unità territoriali di esposizione media o aree di piano, possono sovrapporsi alle zone. Il requisito di non sovrapposizione si applica al layer primario delle zone, salvo che una funzione giuridica specifica richieda un altro layer.

### REQ-ZONE-ZONE_DEFINITION

**Regola**  
Una zona è una parte del territorio di uno Stato membro delimitata ai fini della valutazione e gestione della qualità dell’aria.

```text
zone:
  id is not null
  geometry is not null
  member_state is not null
  purpose includes [air_quality_assessment, air_quality_management]
```

### REQ-ZONE-AGGLOMERATION_DEFINITION

**Regola**  
Gli agglomerati devono essere identificati come conurbazioni con popolazione superiore a 250 000 abitanti, oppure con popolazione pari o inferiore a 250 000 abitanti quando lo Stato membro stabilisce il criterio pertinente di densità di popolazione.

```text
if conurbation_population > 250000:
    is_agglomeration = true
else if conurbation_population <= 250000
AND member_state_density_criterion_satisfied = true:
    is_agglomeration = true
else:
    is_agglomeration = false
```

### REQ-ZONE-AVERAGE_EXPOSURE_TERRITORIAL_UNITS

**Regola**  
Le unità territoriali di esposizione media devono essere designate per determinare gli indicatori di esposizione media. Devono corrispondere a regioni NUTS 1 o NUTS 2, o a combinazioni di regioni NUTS 1/NUTS 2 adiacenti, nel rispetto dei vincoli dimensionali e territoriali della Direttiva.

```text
AETU_VALID(u) =
    u.geometry is not null
    AND u is based_on NUTS_1_or_NUTS_2_or_valid_adjacent_combination
    AND u.total_area <= 85000 km2
    AND u.total_area < territory(ms).area
```

**Nota**  
L’intero territorio di uno Stato membro non deve essere collassato in una singola unità territoriale di esposizione media quando il vincolo dimensionale della Direttiva lo impedisce.

### REQ-ZONE-ASSESSMENT_DOMAIN_ASSIGNMENT

**Regola**  
Ciascuna valutazione deve essere assegnata al corretto dominio territoriale.

```text
if metric = average_exposure_indicator:
    assessment_domain = average_exposure_territorial_unit
else:
    assessment_domain = zone

if pollutant_or_obligation_requires_specific_territorial_unit:
    assessment_domain = applicable_territorial_unit
```

### REQ-ZONE-ZONE_CLASSIFICATION_INTERFACE

**Regola**  
Questo modulo fornisce a `M_ASSESS` geometria e attributi di zona necessari per classificare le zone rispetto alle soglie di valutazione. Non effettua la classificazione delle soglie.

```text
for each active zone z:
    provide z.geometry
    provide z.population
    provide z.type
    provide z.version
    provide assessment_year
```

### REQ-ZONE-NETWORK_REQUIREMENT_INTERFACE

**Regola**  
Questo modulo fornisce gli attributi territoriali necessari per calcolare gli obblighi della rete di monitoraggio, inclusi popolazione, area, stato di agglomerato, contesto di esposizione e attributi territoriali rilevanti per i supersiti.

```text
network_context(z) includes:
  geometry
  population
  area
  zone_type
  agglomeration_status
  urban_rural_context
  relevant_territorial_units
```

### REQ-ZONE-REPRESENTATIVENESS_BOUNDARY_CONTEXT

**Regola**  
Questo modulo fornisce i confini usati da `M_REPR` per collegare le aree di rappresentatività a zone, unità territoriali e contesti transfrontalieri.

Le aree di rappresentatività possono attraversare confini di zona o nazionali; tali attraversamenti devono essere preservati come metadati ove rilevanti.

```text
if AREA_REPR intersects multiple zones or Member States:
    affected_territorial_objects recorded
    cross_boundary_context_available = true
```

### REQ-ZONE-COMPLIANCE_DOMAIN_INTERFACE

**Regola**  
Questo modulo deve fornire il dominio territoriale applicabile per verifica di conformità, obblighi di mantenimento, livelli critici e obblighi di esposizione media.

```text
if standard_type in [limit_value, target_value]:
    compliance_domain = zone
if standard_type = critical_level:
    compliance_domain = applicable_ecosystem_or_vegetation_context
if standard_type in [average_exposure_indicator,
                    average_exposure_reduction_obligation,
                    average_exposure_concentration_objective]:
    compliance_domain = average_exposure_territorial_unit
```

### REQ-ZONE-ALERT_INFORMATION_DOMAIN_INTERFACE

**Regola**  
Questo modulo deve fornire il dominio territoriale per output relativi a superamenti o previsioni di superamento delle soglie di allarme e informazione, inclusa la popolazione interessata e gruppi sensibili o vulnerabili ove disponibili.

```text
if alert_or_information_threshold_exceeded_or_predicted = true:
    affected_area_geometry is not null
    affected_zone_or_units identified
    affected_population_metadata available where available
```

### REQ-ZONE-PLANNING_DOMAIN_INTERFACE

**Regola**  
Questo modulo deve fornire i domini territoriali per piani per la qualità dell’aria, tabelle di marcia e piani d’azione a breve termine.

Piani e tabelle di marcia possono essere basati su zone, unità territoriali o coprire più oggetti territoriali quando è richiesta pianificazione integrata.

```text
if air_quality_plan_trigger = true:
    plan_area = affected_zone_or_territorial_unit_or_integrated_area
if air_quality_roadmap_trigger = true:
    roadmap_area = affected_zone_or_territorial_unit_or_integrated_area
if short_term_action_plan_trigger = true:
    short_term_action_area = affected_area_or_neighbouring_zones_as_applicable
```

### REQ-ZONE-OZONE_TERRITORIAL_CONTEXT

**Regola**  
Quando sono valutati valori-obiettivo per l’ozono o obiettivi a lungo termine, oppure quando sono richiesti piani relativi all’ozono, l’unità territoriale pertinente deve essere identificata e collegata alle zone che copre.

```text
ozone_territorial_unit:
  id is not null
  geometry is not null
  covered_zones not empty
  ozone_assessment_context recorded
```

### REQ-ZONE-SOURCE_ATTRIBUTION_CONTEXT

**Regola**  
Questo modulo deve fornire gli oggetti territoriali richiesti per identificare zone o unità territoriali di esposizione media interessate da fonti naturali, sabbiatura/salatura invernale o contributi transfrontalieri.

```text
if source_attribution_case = true:
    affected_zones_or_AETUs identified
    source_contribution_area identified where available
    territorial_metadata_available = true
```

### REQ-ZONE-TRANSBOUNDARY_CONTEXT

**Regola**  
Quando un inquinamento transfrontaliero significativo contribuisce a superamenti, o quando zone confinanti in altri Stati membri possono essere interessate, questo modulo deve preservare il contesto territoriale transfrontaliero.

```text
if transboundary_contribution_relevant = true:
    affected_member_states recorded
    affected_zones_or_territorial_units recorded
    cross_boundary_geometry available
```

### REQ-ZONE-PUBLIC_INFORMATION_CONTEXT

**Regola**  
Gli output territoriali usati per l’informazione al pubblico devono essere idonei a comunicare al pubblico lo stato e i rischi della qualità dell’aria.

```text
if public_information_output_required = true:
    public_area_label available
    public_geometry_or_map_area available
    affected_population_metadata available where available
    sensitive_population_context available where available
```

### REQ-ZONE-REPORTING_CHANGES

**Regola**  
Le modifiche all’elenco e alla delimitazione di zone o unità territoriali di esposizione media devono essere rese disponibili ai fini della rendicontazione.

```text
if zone_or_AETU_changed = true:
    change_record created
    old_version retained
    new_version retained
    change_effective_date recorded
    reporting_payload_ready = true
```

### REQ-ZONE-VERSIONING

**Regola**  
Gli oggetti territoriali devono essere versionati affinché output di valutazione, conformità, monitoraggio, pianificazione e rendicontazione siano tracciabili rispetto ai confini territoriali in vigore nel periodo pertinente.

```text
territorial_object_version includes:
  object_id
  object_type
  geometry
  valid_from
  valid_to
  legal_status
  change_reason
  previous_version_id
```

### REQ-ZONE-UPDATE_REVIEW

**Regola**  
Le suddivisioni territoriali devono essere riesaminate quando richiesto per valutazione e gestione, incluso quando modifiche significative incidono su distribuzione della popolazione, pattern emissivi, delimitazioni amministrative, obiettivi di monitoraggio o classificazione di valutazione.

```text
zone_review_due =
    significant_population_change = true
    OR significant_emission_change = true
    OR administrative_boundary_change = true
    OR assessment_classification_review_due = true
    OR monitoring_network_change_requires_zone_review = true
```

**Nota**  
La revisione della classificazione delle zone ai sensi dell’Art. 7 è gestita da `M_ASSESS`; questo modulo fornisce e versiona il layer territoriale usato per tale revisione.

### REQ-ZONE-DATA_INTEGRITY

**Regola**  
I dati territoriali devono essere topologicamente validi e tracciabili.

```text
for each active territorial object:
    geometry_valid = true
    geometry_not_empty = true
    member_state_assigned = true
    valid_from is not null

for zone layer:
    no_unintended_overlap = true
    no_unintended_gaps = true
```

## Logica decisionale territoriale

```text
for each Member State:
    define active zone layer
    verify full territorial coverage
    verify no unintended overlap between zones
    identify agglomerations
    define average exposure territorial units
    verify NUTS basis and size constraints
    provide territorial context to downstream modules
    version all territorial objects
    report changes where required
```

## Interazioni con altri moduli

- `M_ASSESS` — classifica le zone e applica i regimi di valutazione
- `M_NETWORK` — calcola requisiti minimi di monitoraggio e contesto dei supersiti
- `M_REPR` — collega aree di rappresentatività a zone e contesti transfrontalieri
- `M_MOD` — fornisce aree modellate e geometrie transfrontaliere che richiedono attribuzione territoriale
- `M_LIMITS` — verifica la conformità per zona o unità territoriale di esposizione media
- `M_SOURCE_ATTRIBUTION` — usa unità territoriali interessate per fonti naturali, sabbiatura/salatura invernale e casi transfrontalieri
- `M_ATTAINMENT_EXTENSION` — usa zone interessate per richieste di proroga
- `M_PLANS` — usa zone, unità territoriali e aree integrate per piani e tabelle di marcia
- `M_SHORT_TERM_ACTION` — usa aree interessate e confinanti per piani d’azione a breve termine
- `M_TRANSBOUNDARY` — usa il contesto territoriale transfrontaliero
- `M_PUBLIC_INFORMATION` — usa etichette territoriali pubbliche e metadati sulle aree interessate
- `M_REPORTING` — rendiconta elenchi, delimitazioni e modifiche di zone e AETU
- `T_NUTS` — geometrie NUTS 1 e NUTS 2
- `T_ZONE_TYPES` — classificazioni di zona ed esposizione

## Output

```text
territorial_context:
  member_state
  reference_year
  zones:
    - id
      version
      geometry
      valid_from
      valid_to
      population
      area
      zone_type
      agglomeration_status
      urban_rural_context
      legal_status
  agglomerations:
    - id
      geometry
      population
      density
      linked_zones
  average_exposure_territorial_units:
    - id
      version
      geometry
      valid_from
      valid_to
      NUTS_basis
      area
      linked_zones
      valid_for_AEI
  other_territorial_units:
    - id
      type = ozone_territorial_unit | plan_area | roadmap_area | short_term_action_area | transboundary_affected_area | reporting_area
      geometry
      linked_zones
      legal_or_operational_basis
  changes:
    - change_id
      object_id
      object_type
      previous_version_id
      new_version_id
      effective_date
      change_reason
      reporting_payload_ready
```

## Note

- `M_ZONE` fornisce domini territoriali; non classifica le zone sopra o sotto le soglie di valutazione.
- Confini di zona e risultati di valutazione devono essere versionati insieme per preservare la tracciabilità.
- Le unità territoriali di esposizione media non sono intercambiabili con le zone; esistono specificamente per gli indicatori di esposizione media e i relativi obblighi.
- Pianificazione e rendicontazione possono richiedere unità territoriali che aggregano o attraversano zone; queste dovrebbero essere rappresentate esplicitamente invece di sovraccaricare l’oggetto zona primario.
- Le geometrie transfrontaliere dovrebbero essere preservate quando rilevanti per cooperazione transfrontaliera e output pubblici/di rendicontazione.
