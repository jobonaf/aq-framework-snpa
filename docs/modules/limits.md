# M_LIMITS — Limit Values and Compliance

## Riferimenti normativi

- Annex I Directive (EU) 2024/2881 (limit values)
- Art. 8 (assessment)
- Art. 16 (natural sources)
- Art. 17 (exceedances and plans)
- Art. 18 (time extensions)
- Annex V (data quality and uncertainty)

---

## Descrizione

Il modulo definisce la verifica della conformità ai:

- valori limite (LV)
- valori obiettivo (TV)

La conformità è valutata considerando:

- dimensione temporale (media, massimo, percentili)
- dimensione spaziale (integrazione misure + modellistica)
- qualità e validità dei dati

Il modulo rappresenta l’output finale del sistema.

---

## Definizioni

```

C\_metric(x, t) = concentrazione per metrica specifica
(annuale, giornaliera, oraria, 8h)

LV(pollutant, metric, year) =
valore normativo (vedi tables/limits.md)

N\_exceed =
numero di superamenti

MAX\_exceed =
massimo numero consentito

```

---

## Logica

### Selezione valore applicabile

```

APPLICABLE\_LV =
LV\_transitional if year < 2030
else LV\_2030

```

---

### Conformità media

```

COMPLIANT\_MEAN =
C\_metric ≤ APPLICABLE\_LV

```

(Per metriche annuali)

---

### Conformità per superamenti

```

COMPLIANT\_EXCEEDANCE =
N\_exceed ≤ MAX\_exceed

```

(Per metriche giornaliere/orarie/8h)

---

### Conformità complessiva

```

COMPLIANT =
COMPLIANT\_MEAN
AND COMPLIANT\_EXCEEDANCE

```

---

## Determinazione della concentrazione

### Regola generale

```

C(x) =
measurement if x ∈ AREA\_REPR
else model if VALID\_MODEL

```

---

### Requisiti

- dati validi (vedi M_DATA_QUALITY)
- uso modello solo se VALID_MODEL (M_MODEL_QA)

---

## Regole operative

---

### Uso delle misure

```

if x ∈ AREA\_REPR:
use measurement

```

---

### Uso del modello

```

if x ∉ AREA\_REPR AND VALID\_MODEL:
use model

```

---

### Superamenti modellistici

---

#### Caso 1 — coerenza misura + modello

```

if model\_exceedance
AND measurement\_exceedance:
→ valid exceedance

```

---

#### Caso 2 — conflitto misura-modello

```

if measurement\_exceedance
AND model\_no\_exceedance:
→ model NOT usable

```

---

#### Caso 3 — modello con superamento interno AREA_REPR

```

if model\_exceedance
AND x ∈ AREA\_REPR
AND measurement\_no\_exceedance:
→ NOT valid exceedance

```

---

#### Caso 4 — modello fuori rete

```

if model\_exceedance outside all AREA\_REPR:
→ valid exceedance
→ new station required (M\_NETWORK)

```

---

## Conteggio superamenti

Applicabile a:

- valori giornalieri
- valori orari
- medie mobili (8h)

```

N\_exceed =
count(periods where C\_metric > LV)

```

---

## Qualità dei dati

```

use only data where:
DATA\_VALID = True

```
```

if data invalid:
exclude from calculation

```

---

## Contributi naturali (Art. 16)

```

if exceedance attributable to natural sources:
EXCLUDED\_FROM\_COMPLIANCE = True

```

Condizioni:

- identificazione del contributo
- quantificazione
- documentazione
- accettazione della Commissione

---

## Eventi eccezionali

Esempio:

- sabbiatura stradale (PM10)

```

if event qualifies:
may be excluded from exceedance count

```

---

## Deroghe (Art. 18)

```

if extension granted:
TEMPORARY\_NON\_COMPLIANCE\_ALLOWED

```

Condizioni:

- piano approvato
- dimostrazione tecnica
- rispetto scadenze

---

## Interazioni con altri moduli

---

### M_REPR

```

defines spatial validity of exceedance

```

---

### M_MOD

```

identifies exceedances outside monitoring network

```

---

### M_NETWORK

```

exceedances may trigger new stations

```

---

### M_MODEL_QA

```

model usable only if VALID\_MODEL

```

---

### M_DATA_QUALITY

```

only valid data used for compliance

```

---

## Output

```

compliance:
zone\_id
pollutant
metric
compliant (boolean)

    mean_value
    exceedance_count
    exceedance_allowed

    adjusted:
        natural (bool)
        exceptional (bool)
        derogation (bool)

```

---

## Note

- La modellistica non può ridurre artificialmente i superamenti
- La conformità è determinata su base zonale
- Le decisioni sono soggette a validazione Commissione
