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

OBS_VALID =
osservazioni che soddisfano M_DATA_QUALITY

DATA_independent =
dataset NON utilizzati nel modello

N_val =
numero stazioni utilizzate per validazione

```

---

## Logica

### Validità del modello

```

VALID_MODEL =
MQI ≤ 1

```

---

### Caso con numero limitato di stazioni

```

if N_val < 10:
VALID_MODEL =
∀ sp ∈ validation_stations :
MQI(sp) ≤ 1

```

---

## Requisiti operativi

---

### 1. Controllo qualità degli input (obbligatorio)

```

for each model_run:
i dataset di input devono essere controllati per qualità

```

Include:

- emissioni
- dati meteorologici
- concentrazioni di background

---

### 2. Uso di dati indipendenti

```

validation_data ∩ model_input_data = ∅

```

I dati usati per:

- calibrazione
- assimilazione
- tuning

NON possono essere usati per la validazione.

---

### 3. Selezione dei dati di validazione

```

validation_stations must:
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

i validation_data devono soddisfare:
OBS_VALID = True

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

if NOT VALID_MODEL:
model_output NON utilizzabile

```

Conseguenze:

- non utilizzabile per valutazione (M_LIMITS)
- non utilizzabile per rappresentatività (M_REPR)
- non utilizzabile per riduzione rete (M_NETWORK)

---

## Moduli e tabelle correlati

La validazione del modello è un prerequisito per l’utilizzo regolatorio dei risultati.

- [M_MOD](modelling.md): la validazione abilita o inibisce l’uso del modello.
- [M_REPR](repr.md): solo modelli validati possono essere usati per la rappresentatività spaziale.
- [M_LIMITS](limits.md): i superamenti modellistici sono validi solo se il modello è validato.
- [Qualità dei dati](../tables/data_quality.md): definisce i requisiti per i dati di validazione.
``

---

## Output

```

model_validation:
valid (boolean)
MQI
N_val
validation_method:
LOO | other

```

---

## Note

- MQI ≤ 1 deriva dagli obiettivi di qualità (Allegato V)
- La validazione è un requisito vincolante, non opzionale
- L’uso di dati non indipendenti invalida la validazione
- Il modulo è prerequisito per l’intero uso della modellistica
