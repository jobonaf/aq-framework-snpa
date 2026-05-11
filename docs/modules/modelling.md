# M_MOD — Applicazioni modellistiche

## Riferimenti normativi

- Art. 8 Direttiva (UE) 2024/2881
- Allegato IV (uso combinato metodi)
- Allegato V (incertezze e qualità)
- Atti di esecuzione (bozza 2026 — requisiti modellistici)

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

C_model(x, t) = concentrazione modellata nella localizzazione x

C_meas(sp, t) = concentrazione misurata nel punto di campionamento

AREA_REPR(sp) = area di rappresentatività (vedi M_REPR)

```

---

## Logica

### Uso combinato misura + modello

```

C(x) =
C_meas se x ∈ AREA_REPR(station)
else C_model(x)

```

---

### Dataset per assessment

```

ASSESSMENT_DATA =
merge(measurements, model_output)

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

MODEL_USABLE = VALID_MODEL

```

→ vedi M_MODEL_QA

---

### Uso sopra la soglia di valutazione

```

if zone ABOVE_THRESHOLD:
la modellistica può essere utilizzata insieme alle misure

```

---

### Uso sotto la soglia

```

if zone BELOW_THRESHOLD:
la modellistica può essere metodo principale

```

---

### Uso sopra valori limite (atti di esecuzione)

```

if C_ann > LIMIT_VALUE
AND implementing_acts in force:
modellistica diventa obbligatoria (dopo periodo di transizione)

```

---

## Requisiti tecnici (atti di esecuzione)

Un modello è conforme se soddisfa:

```

MODEL_FIT =
MATCHES_AVERAGING_PERIODS
AND HAS_APPROPRIATE_SPATIAL_RESOLUTION
AND INPUTS_ALIGNED
AND REPRESENTS_RELEVANT_PROCESSES

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

dati meteorologici devono:
essere coerenti con scala spaziale/temporale
rappresentare la variabilità

```

---

#### Background

```

concentrazioni di background devono:
essere coerenti con il dominio del modello

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

if model_exceedance AND measurement_exceedance:
→ use model results

```

---

### Caso 2 — superamenti fuori copertura

```

if model_exceedance outside all AREA_REPR:
→ trigger new station (M_NETWORK)

```

---

### Caso 3 — conflitto modello-misura

```

if measurement_exceedance
AND model_no_exceedance:
→ model cannot be used for assessment

```

---

### Caso 4 — superamento modellistico isolato

```

if model_exceedance
AND measurement_no_exceedance
AND x ∈ AREA_REPR:
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

MODEL_USABLE = True

```

---

## Interazione con altri moduli

---

### M_MODEL_QA

```

model usable only if VALID_MODEL = True

```

---

### M_REPR

```

model used to compute AREA_REPR

```

---

### M_NETWORK

```

il modello individua lacune nella rete di monitoraggio

```

---

### M_LIMITS

```

model used to identify exceedance areas

```

---

## Output

```

model_output:
concentration_field
exceedance_areas
hotspots

```

---

## Note

- Il modello non sostituisce le misure nelle aree rappresentate
- L’uso del modello è vincolato alla validazione
- Le regole sono rafforzate dagli atti di esecuzione
