# M_LIMITS — Valori limite e conformità

## Riferimenti normativi

- Allegato I Direttiva (UE) 2024/2881 (valori limite)
- Art. 8 (valutazione)
- Art. 16 (fonti naturali)
- Art. 17 (superamenti e piani)
- Art. 18 (proroghe temporali)
- Allegato V (qualità dati e incertezza)

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

C_metric(x, t) = concentrazione per metrica specifica
(annuale, giornaliera, oraria, 8h)

LV(pollutant, metric, year) =
valore normativo (vedi tables/limits.md)

N_exceed =
numero di superamenti

MAX_exceed =
massimo numero consentito

```

---

## Logica

### Selezione valore applicabile

```

APPLICABLE_LV =
LV_transitional if year < 2030
else LV_2030

```

---

### Conformità media

```

COMPLIANT_MEAN =
C_metric ≤ APPLICABLE_LV

```

(Per metriche annuali)

---

### Conformità per superamenti

```

COMPLIANT_EXCEEDANCE =
N_exceed ≤ MAX_exceed

```

(Per metriche giornaliere/orarie/8h)

---

### Conformità complessiva

```

COMPLIANT =
COMPLIANT_MEAN
AND COMPLIANT_EXCEEDANCE

```

---

## Determinazione della concentrazione

### Regola generale

```

C(x) =
measurement if x ∈ AREA_REPR
else model if VALID_MODEL

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

if x ∈ AREA_REPR:
use measurement

```

---

### Uso del modello

```

if x ∉ AREA_REPR AND VALID_MODEL:
use model

```

---

### Superamenti modellistici

---

#### Caso 1 — coerenza misura + modello

```

if model_exceedance
AND measurement_exceedance:
→ valid exceedance

```

---

#### Caso 2 — conflitto misura-modello

```

if measurement_exceedance
AND model_no_exceedance:
→ model NOT usable

```

---

#### Caso 3 — modello con superamento interno AREA_REPR

```

if model_exceedance
AND x ∈ AREA_REPR
AND measurement_no_exceedance:
→ NOT valid exceedance

```

---

#### Caso 4 — modello fuori rete

```

if model_exceedance outside all AREA_REPR:
→ valid exceedance
→ new station required (M_NETWORK)

```

---

## Conteggio superamenti

Applicabile a:

- valori giornalieri
- valori orari
- medie mobili (8h)

```

N_exceed =
count(periods where C_metric > LV)

```

---

## Qualità dei dati

```

use only data where:
DATA_VALID = True

```
```

if data invalid:
exclude from calculation

```

---

## Contributi naturali (Art. 16)

```

if exceedance attributable to natural sources:
EXCLUDED_FROM_COMPLIANCE = True

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
TEMPORARY_NON_COMPLIANCE_ALLOWED

```

Condizioni:

- piano approvato
- dimostrazione tecnica
- rispetto scadenze

---

## Moduli e tabelle correlati

La verifica di conformità integra tutti i risultati precedenti.

- [M_REPR](repr.md): determina la validità spaziale delle misure.
- [M_MOD](modelling.md): individua superamenti al di fuori della rete.
- [M_MODEL_QA](model_qa.md): condiziona l’uso dei risultati modellistici.
- [Valori limite](../tables/limits.md): definiscono i criteri di conformità normativa.

---

## Output

```

compliance:
zone_id
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
