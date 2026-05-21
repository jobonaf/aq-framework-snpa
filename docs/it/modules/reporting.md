# M_REPORTING — Rendicontazione regolatoria e scambio dati

## Riferimenti normativi

- Art. 23 Direttiva (UE) 2024/2881 — rendicontazione e scambio dati
- Art. 22 Direttiva (UE) 2024/2881 — interfaccia di informazione al pubblico
- Art. 19 Direttiva (UE) 2024/2881 — piani e tabelle di marcia, contenuto della rendicontazione
- Art. 18 Direttiva (UE) 2024/2881 — proroga, trasmissione delle evidenze
- Art. 21 Direttiva (UE) 2024/2881 — rendicontazione transfrontaliera
- Allegato I — standard e soglie
- Allegato V — obiettivi di qualità dei dati
- Allegato VIII — contenuto dei piani e delle tabelle di marcia
- Atti implementativi, formati dati, INSPIRE, e-reporting ove applicabili

## Descrizione

Questo modulo definisce il **livello di rendicontazione regolatoria** del framework.

Trasforma gli output validati di tutti i moduli in:

```text
dataset ufficiali
rendicontazione della conformità
trasmissioni a livello UE
pacchetti tracciabili di evidenze regolatorie
```

Assicura che tutti i dati rendicontati siano:

```text
completi
coerenti
tracciabili
versionati
idonei all’uso regolatorio
```

Questo modulo non effettua valutazione o modellizzazione; aggrega e standardizza gli output.

## Ambito

Copre la rendicontazione di:

```text
risultati di valutazione
conformità ai valori limite
indicatori di esposizione
piani e tabelle di marcia
richieste di proroga
casi di attribuzione delle fonti
cooperazione transfrontaliera
metadati di qualità dei dati
```

## Definizioni

```text
REPORTING_DATASET =
dataset strutturato predisposto per trasmissione o pubblicazione
```

```text
REPORTABLE_ENTITY =
elemento richiesto per la rendicontazione,
ad esempio zona, inquinante, metrica, anno
```

```text
REPORTING_PACKAGE =
insieme di dataset, metadati ed evidenze
```

```text
TRACEABILITY =
capacità di collegare valori rendicontati a dati e metodi di origine
```

## Requisiti normativi

### REQ-REP-DATA_VALIDITY

**Regola**  
Devono essere rendicontati solo dati validi.

**Criterio di accettazione**

```text
forall dataset:
    DATA_VALID = true OR status = documented_exception
```

### REQ-REP-CONSISTENCY

**Regola**  
I dati rendicontati devono essere internamente coerenti tra i moduli.

**Criterio di accettazione**

```text
M_LIMITS_output == reporting_limits
M_EXPOSURE_output == reporting_exposure
```

### REQ-REP-COMPLETENESS

**Regola**  
Tutte le entità richieste devono essere rendicontate.

**Criterio di accettazione**

```text
forall required_entities:
    present in REPORTING_DATASET
```

### REQ-REP-METADATA

**Regola**  
Ogni dataset deve includere metadati.

**Criterio di accettazione**

```text
metadata includes:
  source
  method
  quality_status
  version
  timestamp
```

### REQ-REP-VERSIONING

**Regola**  
I dataset devono essere versionati e auditabili.

**Criterio di accettazione**

```text
report includes:
  dataset_version
  revision_history
```

### REQ-REP-TRACEABILITY

**Regola**  
I valori rendicontati devono essere tracciabili ai dati sottostanti.

**Criterio di accettazione**

```text
TRACEABILITY = true
link to original dataset exists
```

### REQ-REP-PLAN_REPORTING

**Regola**  
Piani e tabelle di marcia devono essere rendicontati con il contenuto richiesto.

**Criterio di accettazione**

```text
if plan_exists:
    report includes:
      measures
      timeline
      source_profile
      projections
```

### REQ-REP-EXTENSION_REPORTING

**Regola**  
Le richieste di proroga devono includere un pacchetto completo di evidenze.

**Criterio di accettazione**

```text
if postponement_requested:
    include:
      justification
      modelling_results
      plan
      source_attribution
```

### REQ-REP-TRANSBOUNDARY_REPORTING

**Regola**  
I casi transfrontalieri devono essere rendicontati.

**Criterio di accettazione**

```text
if TRANSBOUNDARY_CASE:
    report includes:
      affected_states
      source_states
      contribution_estimates
```

### REQ-REP-PUBLIC_ALIGNMENT

**Regola**  
I dati rendicontati devono essere coerenti con l’informazione al pubblico.

**Criterio di accettazione**

```text
public_information == reporting_summary
```

### REQ-REP-FORMAT

**Regola**  
I dati devono seguire formati standardizzati.

**Criterio di accettazione**

```text
format complies with implementing acts
```

### REQ-REP-LEGAL_BOUNDARY

**Regola**  
Questo modulo non valuta la conformità; la rendiconta.

**Criterio di accettazione**

```text
compliance_status consumed
not recalculated
```

## Logica decisionale

```text
collect outputs from all modules
validate completeness and consistency
attach metadata and traceability
format dataset
publish/report
```

## Output

```text
reporting_package:
  datasets:
    limits
    exposure
    plans
    models
  metadata:
    versions
    sources
    quality
  compliance:
    status
  supporting:
    source_attribution
    transboundary
    extension_requests
  audit:
    traceability_links
```

## Note

- Livello finale di aggregazione del framework.
- Assicura la robustezza giuridica di tutti gli output.
- Abilita l’interoperabilità con i sistemi dell’UE.
- Dipende fortemente da tutti i moduli upstream.
