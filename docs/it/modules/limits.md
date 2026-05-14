# M_LIMITS — Verifica di conformità ai valori normativi

## Riferimenti normativi

- Direttiva (UE) 2024/2881, Allegato I (valori limite e valori‑obiettivo)
- Art. 8 (valutazione)
- Art. 16 (fonti naturali)
- Art. 17 (superamenti)
- Art. 18 (proroghe temporali)
- Allegato V (qualità dei dati)

---

## Descrizione

Il modulo verifica la **conformità normativa** della qualità dell’aria
ai valori limite (LV) e ai valori‑obiettivo (TV) definiti nell’Allegato I.

La verifica è effettuata:

- per inquinante
- per metrica normativa
- su base zonale

Il modulo **non** definisce piani di qualità dell’aria
e **non** pianifica misure correttive.

---

## Definizioni

```

C(p, x, t) =
concentrazione dell’inquinante p
nella localizzazione x
al tempo t

```
```

METRIC(p) =
metrica normativa applicabile
per l’inquinante p,
definita in T\_LIMIT\_VALUES

```
```

LV(p) =
valore normativo (LV o TV)
per l’inquinante p
e la metrica METRIC(p),
definito in T\_LIMIT\_VALUES

```
```

EXCEEDANCE(p, t) =
C(p, x, t) > LV(p)

```

---

## Requisiti normativi

### REQ-LIMITS-SPATIAL_INTEGRATION

| Campo | Valore |
|------|-------|
| Fonte | Art. 8 Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | M_REPR, M_MODEL_QA |

**Regola**  
La concentrazione da utilizzare per la verifica di conformità
è determinata integrando misure e modellistica
in funzione della rappresentatività spaziale.

**Criterio di accettazione**

```

if x ∈ AREA\_REPR:
C(p, x, t) = measurement
else if VALID\_MODEL = true:
C(p, x, t) = model
else:
concentration undefined

```

---

### REQ-LIMITS-DATA_VALIDITY

| Campo | Valore |
|------|-------|
| Fonte | Allegato V Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | M_DATA_QUALITY |

**Regola**  
Solo dati che soddisfano i requisiti di qualità
possono essere utilizzati per la verifica di conformità.

**Criterio di accettazione**

```

use only data where DATA\_VALID = true

```

---

### REQ-LIMITS-MEAN_COMPLIANCE

| Campo | Valore |
|------|-------|
| Fonte | Allegato I Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | T_LIMIT_VALUES |

**Regola**  
Per le metriche valutate come media (es. media annua),
la conformità è verificata confrontando il valore medio
con il valore normativo applicabile.

**Criterio di accettazione**

```

COMPLIANT\_MEAN(p) =
mean(C(p)) ≤ LV(p)

```

---

### REQ-LIMITS-EXCEEDANCE_COMPLIANCE

| Campo | Valore |
|------|-------|
| Fonte | Allegato I Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | T_LIMIT_VALUES |

**Regola**  
Per le metriche che prevedono un numero massimo di superamenti,
la conformità è verificata confrontando il numero di superamenti
con il massimo consentito.

**Criterio di accettazione**

```

COMPLIANT\_EXCEEDANCE(p) =
COUNT{ EXCEEDANCE(p, t) } ≤ MAX\_EXCEED(p)

```

dove `MAX_EXCEED(p)` è definito in `T_LIMIT_VALUES`.

---

### REQ-LIMITS-OVERALL_COMPLIANCE

| Campo | Valore |
|------|-------|
| Fonte | Allegato I Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | REQ-LIMITS-MEAN_COMPLIANCE, REQ-LIMITS-EXCEEDANCE_COMPLIANCE |

**Regola**  
La conformità complessiva per un inquinante
è verificata solo se sono soddisfatte
tutte le condizioni applicabili.

**Criterio di accettazione**

```

COMPLIANT(p) =
COMPLIANT\_MEAN(p)
AND COMPLIANT\_EXCEEDANCE(p)

```

---

### REQ-LIMITS-NATURAL_SOURCES

| Campo | Valore |
|------|-------|
| Fonte | Art. 16 Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | — |

**Regola**  
I superamenti attribuibili a fonti naturali
possono essere esclusi dalla verifica di conformità
se adeguatamente documentati e accettati.

**Criterio di accettazione**

```

if exceedance attributable to natural sources:
EXCLUDED\_FROM\_COMPLIANCE = true

```

---

### REQ-LIMITS-EXCEPTIONAL_EVENTS

| Campo | Valore |
|------|-------|
| Fonte | Art. 16 Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | — |

**Regola**  
Eventi eccezionali possono essere esclusi
dal conteggio dei superamenti
se previsti dalla normativa e documentati.

**Criterio di accettazione**  
L’evento è identificato e motivato
con riferimento normativo.

---

### REQ-LIMITS-DEROGATION_RECORD

| Campo | Valore |
|------|-------|
| Fonte | Art. 18 Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | — |

**Regola**  
In presenza di una proroga temporale concessa,
la non conformità è registrata come temporaneamente ammessa.

**Criterio di accettazione**

```

if extension granted:
TEMPORARY\_NON\_COMPLIANCE\_ALLOWED = true

```

Il modulo registra la deroga
ma **non valuta** il piano associato.

---

## Interazioni con altri moduli

- `M_REPR` — definisce la validità spaziale delle misure
- `M_MOD` — fornisce il campo di concentrazione
- `M_MODEL_QA` — abilita l’uso della modellistica
- `M_DATA_QUALITY` — valida i dati
- `M_NETWORK` — può essere attivato in caso di lacune di copertura

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
