# M_ATTAINMENT_EXTENSION — Proroga dei termini di conseguimento (Art. 18)

## Nota sul nome

Il modulo è denominato **M_ATTAINMENT_EXTENSION** e non `M_DEROGATION` perché:

- l’Art. 18 utilizza il concetto giuridico di *proroga del termine di conseguimento*;
- si tratta di una possibilità condizionata e basata su evidenze, non di una deroga generale agli obblighi.

## Riferimenti normativi

- Art. 18 Direttiva (UE) 2024/2881 — proroga dei termini di conseguimento
- Art. 19 Direttiva (UE) 2024/2881 — piani per la qualità dell’aria e tabelle di marcia
- Art. 21 Direttiva (UE) 2024/2881 — contributi transfrontalieri
- Art. 16 Direttiva (UE) 2024/2881 — fonti naturali
- Art. 11 Direttiva (UE) 2024/2881 — qualità dei dati e modellizzazione
- Allegato I — valori limite e termini di conseguimento
- Allegato VIII — tabelle di marcia per la qualità dell’aria

## Descrizione

Questo modulo determina se una **proroga dei termini di conseguimento** è:

```text
richiesta
supportata da evidenze
potenzialmente ammissibile ai sensi dell’Art. 18
```

Il modulo **non concede** la proroga. Produce un pacchetto strutturato di valutazione usato da:

- `M_LIMITS`
- `M_PLANS`
- `M_REPORTING`

La decisione giuridica finale resta esterna a questo modulo.

## Ambito

Si applica ai casi in cui:

```text
il superamento di un valore limite persiste oltre il termine di conseguimento
```

e uno Stato membro sostiene che il conseguimento non possa essere raggiunto in tempo a causa di:

```text
condizioni di dispersione sfavorevoli
contributi transfrontalieri
condizioni sito-specifiche
combinazioni di tali fattori
```

## Definizioni

```text
p = inquinante
z = zona
m = metrica
period = periodo di valutazione
```

```text
ATTAINMENT_DEADLINE(p,m) =
termine giuridico definito nell’Allegato I
```

```text
POSTPONEMENT_REQUEST(p,z,m) =
richiesta formale di estendere il termine di conseguimento
```

```text
POSTPONEMENT_SUPPORTED =
true se tutte le condizioni dell’Art. 18 sono soddisfatte
```

## Requisiti normativi

### REQ-EXT-TRIGGER

**Regola**  
La valutazione della proroga è attivata quando il superamento persiste oltre il termine di conseguimento.

**Criterio di accettazione**

```text
if exceedance = true AND current_year > ATTAINMENT_DEADLINE:
    postponement_assessment_required = true
```

### REQ-EXT-PRECONDITION_PLAN

**Regola**  
Deve esistere un piano per la qualità dell’aria o una tabella di marcia validi.

**Criterio di accettazione**

```text
air_quality_plan_exists = true
AND plan_contains_measures = true
```

### REQ-EXT-MEASURE_SUFFICIENCY

**Regola**  
Devono essere adottate tutte le misure appropriate per mantenere il periodo di superamento il più breve possibile.

**Criterio di accettazione**

```text
all_reasonable_measures_implemented = true
AND no_less_restrictive_alternative_available = true
```

### REQ-EXT-JUSTIFICATION

**Regola**  
La proroga deve essere giustificata da cause ammissibili.

**Criterio di accettazione**

```text
justification in [
    unfavourable_dispersion_conditions,
    transboundary_contribution,
    site_specific_conditions
]
```

### REQ-EXT-TRANSBOUNDARY_SUPPORT

**Regola**  
Quando è invocato un contributo transfrontaliero, sono richieste evidenze da `M_SOURCE_ATTRIBUTION`.

**Criterio di accettazione**

```text
if justification = transboundary_contribution:
    source_attribution_evidence_valid = true
```

### REQ-EXT-MODELLING_SUPPORT

**Regola**  
La modellizzazione di proiezione deve dimostrare il futuro conseguimento.

**Criterio di accettazione**

```text
MODEL_VALID = true
AND projected_attainment_date available
```

### REQ-EXT-TIME_LIMIT

**Regola**  
La proroga non deve superare il periodo massimo consentito.

**Criterio di accettazione**

```text
extended_deadline <= legal_max_extension_year
```

### REQ-EXT-REPORTING_PACKAGE

**Regola**  
Deve essere predisposto un pacchetto completo di evidenze.

**Criterio di accettazione**

```text
package includes:
    exceedance_data
    plan_measures
    modelling_projections
    source_attribution_if_applicable
    justification
```

### REQ-EXT-LEGAL_BOUNDARY

**Regola**  
Questo modulo non decide l’approvazione.

**Criterio di accettazione**

```text
POSTPONEMENT_SUPPORTED = evaluation output only
decision_by = external_authority
```

## Logica decisionale

```text
if REQ-EXT-TRIGGER satisfied:
    check plan
    check measures
    check justification
    check modelling
    check time limit

    if all satisfied:
        POSTPONEMENT_SUPPORTED = true
    else:
        POSTPONEMENT_SUPPORTED = false
```

## Output

```text
attainment_extension:
  pollutant
  zone
  metric

  trigger:
    exceedance_persisting
    deadline_passed

  justification:
    type
    evidence_reference

  measures:
    plan_exists
    measures_implemented

  modelling:
    model_valid
    projected_attainment_date

  transboundary:
    contribution_present
    evidence_available

  result:
    postponement_supported
    blocking_conditions

  reporting:
    package_ready
```

## Note

- Non si tratta di un regime di deroga, ma di una proroga condizionata.
- Il modulo dipende fortemente dalla modellizzazione e dall’attribuzione delle fonti.
- È sempre collegato agli obblighi di pianificazione.
