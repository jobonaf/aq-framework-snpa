# M_MODEL_QA — Model Quality Assurance and Validation

## Riferimenti normativi

- Implementing Acts (draft 2026), Art. 3
- Annex V Directive (EU) 2024/2881 (data quality objectives)
- Art. 8 Directive (EU) 2024/2881 (use of modelling)

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
input datasets must be quality controlled

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
cover spatial variability
include different environments

```

Tipologie richieste:

- urban background
- traffic
- suburban / rural

---

### 4. Metodologia di validazione

Metodo raccomandato:

```

Leave-One-Out Cross-Validation (LOOCV)

```
```

for each station sp:
run model excluding sp
compare model(sp) vs observed(sp)

```

---

### 5. Requisiti sui dati

```

validation\_data must satisfy:
OBS\_VALID = True

```

dove:

- OBS_VALID è definito in M_DATA_QUALITY

---

### 6. Modelli integrati (data fusion)

Nel caso di modelli con assimilazione dati:

```

only independent observations used for validation

```

---

### 7. Uso del modello

```

if NOT VALID\_MODEL:
model\_output NOT usable

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

- MQI ≤ 1 deriva dagli obiettivi di qualità (Annex V)
- La validazione è un requisito vincolante, non opzionale
- L’uso di dati non indipendenti invalida la validazione
- Il modulo è prerequisito per l’intero uso della modellistica
