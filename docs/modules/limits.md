# M_LIMITS — Verifica di conformità

## Riferimenti normativi

- Direttiva (UE) 2024/2881, Allegato I (standard di qualità dell’aria)
- Art. 8 (valutazione)
- Art. 16 (fonti naturali)
- Art. 17 (superamenti)
- Art. 18 (proroghe temporali)
- Allegato V (qualità dei dati)

---

## Descrizione

Il modulo verifica la **conformità** della qualità dell’aria
ai valori normativi definiti nell’Allegato I della Direttiva.

La verifica di conformità è effettuata:

- per inquinante
- per metrica normativa
- su base zonale

Il modulo **non definisce** piani di qualità dell’aria,
né pianifica misure correttive.

---

## Definizioni

```

C(p, x, t) = concentrazione dell’inquinante p
nella localizzazione x
al tempo t

```
```

METRIC(p) = metrica normativa applicabile
per l’inquinante p
(definita in T\_LIMIT\_VALUES)

```
```

LV(p) = valore normativo per l’inquinante p
e la metrica METRIC(p),
definito in T\_LIMIT\_VALUES

```
```

EXCEEDANCE(p, t) =
C(p, x, t) > LV(p)

```

---

## Determinazione della concentrazione

### Regola di integrazione spaziale

```

if x ∈ AREA\_REPR:
C(p, x, t) = measurement
else if VALID\_MODEL:
C(p, x, t) = model
else:
concentration undefined

```

La rappresentatività spaziale è definita in `M_REPR`.

---

## Regole di conformità

### Conformità per metriche senza conteggio

Per metriche valutate come media (es. media annua):

```

COMPLIANT\_MEAN(p) =
mean(C(p)) ≤ LV(p)

```

---

### Conformità per metriche con conteggio

Per metriche che prevedono un numero massimo di superamenti:

```

COMPLIANT\_EXCEEDANCE(p) =
COUNT{ EXCEEDANCE(p, t) } ≤ MAX\_EXCEED(p)

```

Il valore `MAX_EXCEED(p)` è definito in `T_LIMIT_VALUES`.

---

### Conformità complessiva

```

COMPLIANT(p) =
COMPLIANT\_MEAN(p)
AND COMPLIANT\_EXCEEDANCE(p)

```

---

## Trattamento dei dati

```

use only data where DATA\_VALID = true

```

La validità dei dati è definita in `M_DATA_QUALITY`.

---

## Fonti naturali (Art. 16)

Se un superamento è attribuito a fonti naturali documentate:

```

EXCLUDED\_FROM\_COMPLIANCE = true

```

L’attribuzione deve essere:

- identificata
- quantificata
- documentata
- accettata dalla Commissione

---

## Eventi eccezionali

Eventi eccezionali possono essere esclusi dal conteggio dei superamenti
se previsti dalla normativa e adeguatamente documentati.

---

## Deroghe temporali (Art. 18)

Se è concessa una proroga temporale:

```

TEMPORARY\_NON\_COMPLIANCE\_ALLOWED = true

```

Il modulo **registra** la deroga,
ma **non valuta** la validità del piano associato.

---

## Interazioni con altri moduli

- M_REPR — definisce dove una misura è valida
- M_MOD — fornisce il campo di concentrazione
- M_MODEL_QA — abilita l’uso del modello
- M_DATA_QUALITY — valida i dati
- M_NETWORK — può essere attivato in caso di lacune di copertura

---

## Output

```

compliance\_result:
zone\_id
pollutant
metric
compliant (boolean)

statistics:
mean\_value
exceedance\_count
allowed\_exceedances

adjustments:
natural\_sources (bool)
exceptional\_events (bool)
derogation (bool)

```

---

## Note

- Il modulo non riduce artificialmente i superamenti tramite modellistica.
- La conformità è determinata su base zonale.
- La verifica finale è soggetta a validazione da parte della Commissione.
