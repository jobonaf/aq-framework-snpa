# M_TRANSBOUNDARY — Cooperazione e coordinamento per l’inquinamento atmosferico transfrontaliero

## Riferimenti normativi

- Art. 21 Direttiva (UE) 2024/2881 — inquinamento atmosferico transfrontaliero
- Art. 18 Direttiva (UE) 2024/2881 — proroga dei termini di conseguimento, motivi transfrontalieri
- Art. 19 Direttiva (UE) 2024/2881 — piani per la qualità dell’aria e tabelle di marcia
- Art. 8 Direttiva (UE) 2024/2881 — applicazioni di modellizzazione
- Art. 22–23 Direttiva (UE) 2024/2881 — informazione al pubblico e rendicontazione

## Descrizione

Questo modulo disciplina i **casi di inquinamento atmosferico transfrontaliero** in cui l’inquinamento originato in uno Stato membro o in un paese terzo incide sulla qualità dell’aria in un altro territorio.

Definisce:

- identificazione dei casi transfrontalieri;
- trigger di cooperazione e coordinamento tra Stati membri;
- requisiti di condivisione dei dati e della modellizzazione;
- interazione con pianificazione, conformità e proroga.

Questo modulo non calcola concentrazioni né valida modelli: coordina il modo in cui le evidenze transfrontaliere sono usate.

## Ambito

Si applica quando:

```text
l’inquinamento in un territorio contribuisce in modo significativo a un superamento in un altro territorio
```

Copre:

```text
zone transfrontaliere
trasporto regionale dell’inquinamento
aree di superamento multinazionali
```

## Definizioni

```text
TRANSBOUNDARY_CASE =
caso in cui fonti esterne influenzano significativamente la qualità dell’aria
```

```text
AFFECTED_STATE =
Stato membro che subisce il superamento
```

```text
SOURCE_STATE =
Stato membro o paese terzo che contribuisce all’inquinamento
```

```text
COOPERATION_REQUIRED =
true quando il contributo transfrontaliero è significativo
```

## Requisiti normativi

### REQ-TRANS-IDENTIFICATION

**Regola**  
I casi transfrontalieri devono essere identificati quando esiste un contributo significativo.

**Criterio di accettazione**

```text
if source_contribution_from_external_origin > threshold:
    TRANSBOUNDARY_CASE = true
```

### REQ-TRANS-COOPERATION

**Regola**  
Gli Stati membri devono cooperare quando si verifica inquinamento transfrontaliero.

**Criterio di accettazione**

```text
if TRANSBOUNDARY_CASE = true:
    COOPERATION_REQUIRED = true
```

### REQ-TRANS-DATA_SHARING

**Regola**  
Devono essere condivisi dati e risultati di modellizzazione pertinenti.

**Criterio di accettazione**

```text
share:
  monitoring data
  modelling outputs
  source attribution
```

### REQ-TRANS-MODELLING

**Regola**  
Devono essere usati modelli validati per valutare trasporto e contributo.

**Criterio di accettazione**

```text
MODEL_VALID = true
AND cross_border_modelling_available = true
```

### REQ-TRANS-PLANNING_LINK

**Regola**  
I contributi transfrontalieri devono essere considerati nei piani.

**Criterio di accettazione**

```text
if TRANSBOUNDARY_CASE = true:
    plan_includes_transboundary_context = true
```

### REQ-TRANS-EXTENSION_LINK

**Regola**  
Il contributo transfrontaliero può giustificare una proroga.

**Criterio di accettazione**

```text
if postponement_requested:
    transboundary_evidence_required = true
```

### REQ-TRANS-PUBLIC_INFORMATION

**Regola**  
L’informazione al pubblico deve includere il contesto transfrontaliero ove rilevante.

**Criterio di accettazione**

```text
if TRANSBOUNDARY_CASE = true:
    public_information_includes_source = true
```

### REQ-TRANS-REPORTING

**Regola**  
I casi transfrontalieri devono essere rendicontati con evidenze.

**Criterio di accettazione**

```text
report includes:
  affected_area
  source_country
  contribution
```

### REQ-TRANS-LIMIT_BOUNDARY

**Regola**  
Questo modulo non assegna responsabilità né impone misure.

**Criterio di accettazione**

```text
responsibility_assignment NOT performed
```

## Logica decisionale

```text
if transboundary contribution detected:
    flag TRANSBOUNDARY_CASE
    trigger cooperation
    share data
    integrate into plans
    support extension if requested
```

## Output

```text
transboundary_status:
  affected_state
  source_state
  contribution:
    estimated_value
    uncertainty
  cooperation:
    required
    initiated
  modelling:
    model_used
    valid
  downstream:
    plan_integration
    extension_support
    reporting_ready
```

## Note

- Il focus è sul coordinamento, non sull’attribuzione, gestita da `M_SOURCE_ATTRIBUTION`.
- È critico per la completezza dei piani e per la giustificazione delle proroghe.
- Dipende fortemente dalla modellizzazione e dalla cooperazione internazionale.
