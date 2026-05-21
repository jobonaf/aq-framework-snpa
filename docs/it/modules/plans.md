# M_PLANS — Piani per la qualità dell’aria, tabelle di marcia e piani d’azione a breve termine

## Riferimenti normativi

- Art. 19 Direttiva (UE) 2024/2881 — piani per la qualità dell’aria e tabelle di marcia
- Art. 20 Direttiva (UE) 2024/2881 — piani d’azione a breve termine
- Art. 12 Direttiva (UE) 2024/2881 — mantenimento della qualità dell’aria
- Art. 13 Direttiva (UE) 2024/2881 — valori limite e obblighi di esposizione
- Art. 15 Direttiva (UE) 2024/2881 — soglie di allarme
- Art. 18 Direttiva (UE) 2024/2881 — proroga dei termini di conseguimento
- Allegato VIII — requisiti per piani e tabelle di marcia

## Descrizione

Questo modulo definisce gli **obblighi e gli strumenti di pianificazione** richiesti dalla Direttiva, inclusi:

```text
piani per la qualità dell’aria
tabelle di marcia per la qualità dell’aria
piani d’azione a breve termine
```

Determina quando la pianificazione è richiesta, cosa deve includere e come i piani interagiscono con:

- stato di conformità (`M_LIMITS`);
- obblighi di esposizione (`M_EXPOSURE`);
- attribuzione delle fonti (`M_SOURCE_ATTRIBUTION`);
- modellizzazione e proiezioni (`M_MOD`);
- richieste di proroga (`M_ATTAINMENT_EXTENSION`).

Questo modulo non valuta la conformità né calcola i superamenti; usa tali output come input.

## Ambito

Si applica quando ricorrono:

```text
superamento di un valore limite
obbligo di esposizione non raggiunto
soglia di allarme superata o a rischio di superamento
```

## Definizioni

```text
PLAN_TYPE =
  air_quality_plan
  air_quality_roadmap
  short_term_action_plan
```

```text
PLAN_AREA =
  zone
  AETU
  multi-zone
  transboundary area
```

```text
PLAN_TRIGGER =
condizione che richiede un piano o un aggiornamento
```

## Requisiti normativi

### REQ-PLAN-TRIGGER-LIMIT

**Regola**  
Un piano per la qualità dell’aria è richiesto quando i valori limite sono superati.

**Criterio di accettazione**

```text
if limit_value_exceedance = true:
    PLAN_TRIGGER = air_quality_plan
```

### REQ-PLAN-TRIGGER-EXPOSURE

**Regola**  
Un piano è richiesto quando l’obbligo di riduzione dell’esposizione non è raggiunto.

**Criterio di accettazione**

```text
if exposure_reduction_obligation_status = not_achieved:
    PLAN_TRIGGER = air_quality_plan
```

### REQ-PLAN-TRIGGER-ROADMAP

**Regola**  
Le tabelle di marcia sono richieste quando deve essere dimostrata la futura conformità.

**Criterio di accettazione**

```text
if future_non_compliance_risk = true:
    PLAN_TRIGGER = air_quality_roadmap
```

### REQ-PLAN-TRIGGER-SHORT_TERM

**Regola**  
I piani d’azione a breve termine sono richiesti quando le soglie di allarme sono superate o a rischio di superamento.

**Criterio di accettazione**

```text
if alert_exceedance_or_risk = true:
    PLAN_TRIGGER = short_term_action_plan
```

### REQ-PLAN-CONTENT

**Regola**  
I piani devono includere misure per mantenere il periodo di superamento il più breve possibile.

**Criterio di accettazione**

```text
plan_contains:
  measures
  timeline
  responsible_authorities
  implementation_schedule
```

### REQ-PLAN-SOURCE_LINK

**Regola**  
I piani devono includere l’analisi dei contributi di fonte.

**Criterio di accettazione**

```text
if plan_exists:
    source_profile_available = true
```

### REQ-PLAN-MODELLING

**Regola**  
Piani e tabelle di marcia devono includere proiezioni modellistiche.

**Criterio di accettazione**

```text
MODEL_VALID = true
AND projection_available = true
```

### REQ-PLAN-TIMEFRAME

**Regola**  
Le misure devono essere orientate ai termini di conseguimento.

**Criterio di accettazione**

```text
plan_timeline aligned_with ATTAINMENT_DEADLINE
```

### REQ-PLAN-EXCEPTION-WINTER

**Regola**  
I piani possono essere omessi per superamenti di PM10 causati esclusivamente da sabbiatura/salatura invernale.

**Criterio di accettazione**

```text
if winter_sanding_salting_only = true:
    plan_not_required = true
```

### REQ-PLAN-INTERACTION-EXTENSION

**Regola**  
I piani devono supportare le richieste di proroga.

**Criterio di accettazione**

```text
if postponement_requested = true:
    plan_supports_extension = true
```

### REQ-PLAN-UPDATE

**Regola**  
I piani devono essere aggiornati quando le condizioni cambiano.

**Criterio di accettazione**

```text
if exceedance_changes OR measures_fail:
    plan_update_required = true
```

## Logica decisionale

```text
if trigger_detected:
    identify PLAN_TYPE
    define PLAN_AREA
    include source analysis
    include modelling
    include measures
```

## Output

```text
plan_status:
  plan_type
  plan_area
  trigger:
    type
    source
  content:
    measures
    timeline
    authorities
  modelling:
    projections
    validity
  source:
    main_sources
  result:
    plan_required
    plan_exists
    plan_valid
```

## Note

- Modulo centrale che collega output tecnici e azioni di policy.
- Interagisce sempre con tutti i moduli core.
- È essenziale sia per la conformità sia per i meccanismi di proroga.
