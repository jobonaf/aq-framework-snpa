# M_MOD — Modelling Applications

## Riferimenti normativi

- Art. 8 Directive (EU) 2024/2881
- Annex IV (uso combinato metodi)
- Annex V (incertezze e qualità)
- Implementing Acts (draft 2026 — modelling requirements)

---

## Descrizione

Il modulo disciplina l’uso delle applicazioni modellistiche per:

- valutazione della qualità dell’aria
- supporto alla distribuzione spaziale delle concentrazioni
- identificazione di hotspot
- determinazione di aree di superamento
- supporto alla rappresentatività spaziale (M_REPR)

La modellistica integra le misure ed è parte essenziale del framework.

---

## Definizioni

```

model = applicazione modellistica

C\_model(x, t) = concentrazione modellata nella localizzazione x

C\_meas(sp, t) = concentrazione misurata nel punto di campionamento

AREA\_REPR(sp) = area di rappresentatività (vedi M\_REPR)

```

---

## Logica

### Uso combinato misura + modello

```

C(x) =
C\_meas se x ∈ AREA\_REPR(station)
else C\_model(x)

```

---

### Dataset per assessment

```

ASSESSMENT\_DATA =
merge(measurements, model\_output)

```

---

## Ruolo operativo della modellistica

Il modello deve fornire:

- campo spaziale continuo di concentrazione
- identificazione hotspot
- delimitazione delle aree di superamento
- supporto alla copertura spaziale della rete

---

## Condizioni di utilizzo

### Validità del modello

```

MODEL\_USABLE = VALID\_MODEL

```

→ vedi M_MODEL_QA

---

### Uso sopra la soglia di valutazione

```

if zone ABOVE\_THRESHOLD:
modelling may be used together with measurements

```

---

### Uso sotto la soglia

```

if zone BELOW\_THRESHOLD:
modelling may be primary method

```

---

### Uso sopra valori limite (implementing acts)

```

if C\_ann > LIMIT\_VALUE
AND implementing\_acts in force:
modelling becomes mandatory (after transition period)

```

---

## Requisiti tecnici (implementing acts)

Un modello è conforme se soddisfa:

```

MODEL\_FIT =
MATCHES\_AVERAGING\_PERIODS
AND HAS\_APPROPRIATE\_SPATIAL\_RESOLUTION
AND INPUTS\_ALIGNED
AND REPRESENTS\_RELEVANT\_PROCESSES

```

---

### Requisiti sugli input

#### Emissioni

```

emissions must:
be spatially gridded
be temporally consistent
reflect relevant sources

```

---

#### Meteorologia

```

meteorological data must:
match spatial/temporal scale
represent variability

```

---

#### Background

```

background concentrations must:
be consistent with model domain

```

---

## Processi fisici rappresentati

Il modello deve catturare:

- dispersione atmosferica
- condizioni meteorologiche
- orografia
- contributi transfrontalieri
- condizioni climatiche avverse

---

## Uso nei superamenti

---

### Caso 1 — coerenza misura + modello

```

if model\_exceedance AND measurement\_exceedance:
→ use model results

```

---

### Caso 2 — superamenti fuori copertura

```

if model\_exceedance outside all AREA\_REPR:
→ trigger new station (M\_NETWORK)

```

---

### Caso 3 — conflitto modello-misura

```

if measurement\_exceedance
AND model\_no\_exceedance:
→ model cannot be used for assessment

```

---

### Caso 4 — superamento modellistico isolato

```

if model\_exceedance
AND measurement\_no\_exceedance
AND x ∈ AREA\_REPR:
→ not valid exceedance

```

---

## Uso esclusivo della modellistica

```

if no measurement coverage:
model may be used

```

Condizione:

```

MODEL\_USABLE = True

```

---

## Interazione con altri moduli

---

### M_MODEL_QA

```

model usable only if VALID\_MODEL = True

```

---

### M_REPR

```

model used to compute AREA\_REPR

```

---

### M_NETWORK

```

model detects gaps in monitoring network

```

---

### M_LIMITS

```

model used to identify exceedance areas

```

---

## Output

```

model\_output:
concentration\_field
exceedance\_areas
hotspots

```

---

## Note

- Il modello non sostituisce le misure nelle aree rappresentate
- L’uso del modello è vincolato alla validazione
- Le regole sono rafforzate dagli implementing acts
