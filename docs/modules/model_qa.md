# M_MODEL_QA — Garanzia qualità modelli e validazione

## Riferimenti normativi

- Atti di esecuzione (bozza 2026), Art. 3
- Allegato V Direttiva (UE) 2024/2881 (obiettivi qualità dei dati)
- Art. 8 Direttiva (UE) 2024/2881 (uso della modellistica)

---

## Descrizione

Il modulo definisce i requisiti di validazione e qualità delle applicazioni modellistiche.

La validazione è una **condizione necessaria** per l’utilizzo del modello nei moduli:

- M_MOD (modellistica)
- M_REPR (rappresentatività spaziale)
- M_LIMITS (valutazione conformità)
- M_NETWORK (decisioni sulla rete)

---

## Definizioni

```

MQI = modelling quality indicator

OBS\_VALID =
osservazioni che soddisfano M\_DATA\_QUALITY

DATA\_independent =
dataset NON utilizzati nel modello

N\_val =
numero stazioni utilizzate per validazione

```

---

## Logica

### Validità del modello

```

VALID\_MODEL =
MQI ≤ 1

```

---

### Caso con numero limitato di stazioni

```

if N\_val < 10:
VALID\_MODEL =
∀ sp ∈ validation\_stations :
MQI(sp) ≤ 1

```

---

## Requisiti operativi

---

### 1. Controllo qualità degli input (obbligatorio)

```

for each model\_run:
i dataset di input devono essere controllati per qualità

```

Include:

- emissioni
- dati meteorologici
- concentrazioni di background

---

### 2. Uso di dati indipendenti

```

validation\_data ∩ model\_input\_data = ∅

```

I dati usati per:

- calibrazione
- assimilazione
- tuning

NON possono essere usati per la validazione.

---

### 3. Selezione dei dati di validazione

```

validation\_stations must:
coprire la variabilità spaziale
includere ambienti diversi

```

Tipologie richieste:

- background urbano
- traffico
- suburbano / rurale

---

### 4. Metodologia di validazione

Metodo raccomandato:

```

Leave-One-Out Cross-Validation (LOOCV)

```
```

per ogni stazione sp:
eseguire il modello escludendo sp
confrontare model(sp) con observed(sp)

```

---

### 5. Requisiti sui dati

```

i validation\_data devono soddisfare:
OBS\_VALID = True

```

dove:

- OBS_VALID è definito in M_DATA_QUALITY

---

### 6. Modelli integrati (fusione dati)

Nel caso di modelli con assimilazione dati:

```

solo osservazioni indipendenti usate per la validazione

```

---

### 7. Uso del modello

```

if NOT VALID\_MODEL:
model\_output NON utilizzabile

```

Conseguenze:

- non utilizzabile per valutazione (M_LIMITS)
- non utilizzabile per rappresentatività (M_REPR)
- non utilizzabile per riduzione rete (M_NETWORK)

---

## Interazioni con altri moduli

---

### M_MOD

```

MODEL\_USABLE = VALID\_MODEL

```

---

### M_REPR

```

representativeness from model allowed only if VALID\_MODEL

```

---

### M_LIMITS

```

exceedances valid only if model is VALID\_MODEL

```

---

### M_NETWORK

```

network reduction allowed only if VALID\_MODEL

```

---

### M_DATA_QUALITY

```

validation depends on data\_quality = valid

```

---

## Output

```

model\_validation:
valid (boolean)
MQI
N\_val
validation\_method:
LOO | other

```

---

## Note

- MQI ≤ 1 deriva dagli obiettivi di qualità (Allegato V)
- La validazione è un requisito vincolante, non opzionale
- L’uso di dati non indipendenti invalida la validazione
- Il modulo è prerequisito per l’intero uso della modellistica
