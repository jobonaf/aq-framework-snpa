# M_REPR — Spatial Representativeness of Sampling Points

## Riferimenti normativi

- Art. 4(26) Directive (EU) 2024/2881
- Art. 8 Directive (EU) 2024/2881
- Art. 9 Directive (EU) 2024/2881
- Implementing Acts (draft 2026 — spatial representativeness methodology)
- Annex IV (implicit references)

---

## Descrizione

La rappresentatività spaziale definisce l’area geografica in cui le concentrazioni osservate o modellate in un punto di campionamento sono rappresentative entro una tolleranza definita.

È utilizzata per:

- valutazione della qualità dell’aria
- interpretazione dei superamenti
- ottimizzazione della rete di monitoraggio
- integrazione tra misure e modellistica

---

## Definizioni

```

C\_sp = concentrazione media annuale nel punto di campionamento

T\_min = tolleranza minima (vedi tables/repr\_tolerance.md)

Δ = max(0.15 \* C\_sp, T\_min)

interval = \[C\_sp - Δ, C\_sp + Δ]

```
```

C(x) = concentrazione nella localizzazione x
(misurata o modellata)

```

---

## Logica

### Regola di base

```

REPRESENTED(x, sp) =
C(x) ∈ interval(sp)

```
```

AREA\_REPR(sp) =
{ x ∈ zone | REPRESENTED(x, sp) }

```

---

## Procedura operativa

### 1. Determinazione valore centrale

```

C\_sp = annual\_mean(sp)

```

Alternative possibili (se giustificate):

- percentili
- medie stagionali
- metriche specifiche (es. O3 8h, AOT40)

---

### 2. Identificazione area preliminare

#### Caso misure

```

AREA\_meas =
{ x | C\_meas(x) ∈ interval }

```

---

#### Caso modellistica

```

grid = model(zone)

AREA\_model =
{ cell ∈ grid | C\_model(cell) ∈ interval }

```

⚠ Uso consentito solo se il modello è valido (vedi M_MODEL_QA)

---

### 3. Refinement (obbligatorio)

L’area preliminare deve essere raffinata applicando:

---

#### Vincolo geografico

```

x ∈ zone

```

- inclusi domini non contigui
- limitazione ai confini amministrativi

---

#### Coerenza con tipo di stazione

Escludere:

- siti traffico per stazioni background
- siti industriali non coerenti

---

#### Coerenza emissiva

```

exclude x where emission\_profile(x) differs significantly

```

---

#### Vincoli locali

Possibile limitazione a:

- area urbana
- area di interesse specifico

---

#### Giudizio esperto

Obbligatorio nei casi:

- forte eterogeneità spaziale
- orografia complessa
- condizioni di dispersione non uniformi

---

## Costruzione della mappa di zona

Per ogni zona e inquinante:

```

MAP\_REPR =
{ AREA\_REPR(sp) per tutte le stazioni }

```

---

### Gestione sovrapposizioni

Caso:

```

x ∈ AREA\_REPR(sp1) AND x ∈ AREA\_REPR(sp2)

```
```

assegnare al punto più rappresentativo

```

Criteri:

- tipo stazione
- profilo emissivo
- livello concentrazione

---

### Caso critico (implementing acts)

```

if C(x) > LIMIT\_VALUE
AND nessuna stazione ha superamento:
x non assegnato a nessuna AREA\_REPR

```

Conseguenze:

```

MODEL\_REVIEW\_REQUIRED = True

```

---

## Uso nella valutazione

### Interazione con misure

```

if model\_exceedance ∈ AREA\_REPR
AND measurement non supera:
→ non considerare superamento

```

---

### Nuove stazioni

```

if exceedance outside all AREA\_REPR:
ADDITIONAL\_STATION\_REQUIRED = True

```

→ vedi M_NETWORK

---

### Consistenza modello-misure

```

if model ≠ measurement (fuori incertezza):
MODEL\_REVIEW\_REQUIRED = True

```

---

## Interazioni con altri moduli

### M_MOD
```

il modello fornisce campo di concentrazione

```

---

### M_MODEL_QA
```

uso modellistica solo se VALID\_MODEL = True

```

---

### M_NETWORK
```

le AREA\_REPR definiscono copertura rete

```

---

### M_LIMITS
```

determinano validità dei superamenti

```

---

## Output

```

representativeness:
station\_id
geometry (polygon / multipolygon)
central\_value (C\_sp)
tolerance (Δ)
method:
measurement | modelling | hybrid

```

---

## Frequenza di aggiornamento

```

update at least every 5 years

```

oppure quando:

- cambia la rete
- cambiano le emissioni
- variazioni meteorologiche significative

---

## Tabelle

- ../tables/repr_tolerance.md
