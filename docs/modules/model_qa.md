# M_MODEL_QA — Validazione delle applicazioni modellistiche

## Riferimenti normativi

- Direttiva (UE) 2024/2881, Allegato V
- Art. 8 Direttiva (UE) 2024/2881
- Atti di esecuzione (metodologia di validazione dei modelli)

---

## Descrizione

Il modulo definisce i **criteri normativi di validazione**
delle applicazioni di modellizzazione della qualità dell’aria.

Un modello validato è **condizione necessaria**
per l’utilizzo dei risultati nei moduli:

- `M_MOD` — uso della modellistica
- `M_REPR` — rappresentatività spaziale
- `M_LIMITS` — verifica di conformità
- `M_NETWORK` — decisioni sulla rete di monitoraggio

Il modulo **non disciplina**:
- accreditamenti istituzionali
- programmi di interconfronto
- governance della modellistica

---

## Definizioni

```

OBS\_VALID(p, m) =
osservazioni che soddisfano i requisiti
di qualità dei dati definiti in M\_DATA\_QUALITY

```
```

validation\_data =
insieme di osservazioni indipendenti
non utilizzate come input del modello

```
```

U\_meas(sp) =
incertezza delle misurazioni nel punto sp,
definita in T\_DATA\_QUALITY

```
```

U\_model(sp) =
incertezza dell’applicazione di modellizzazione
nel punto sp, determinata secondo Allegato V

```
```

N\_val =
numero di punti di campionamento
utilizzati per la validazione

```

---

## Definizione dell’indicatore di qualità della modellizzazione (MQI)

L’indicatore di qualità della modellizzazione (**MQI**)
è definito, per ciascun punto di campionamento `sp`,
come il rapporto tra l’errore della modellizzazione
e l’incertezza complessiva associata.

### Definizione formale

```

MQI(sp) =
RMSE(sp)
\--------------------------------
sqrt( U\_model(sp)^2 + U\_meas(sp)^2 )

```

dove:

- `RMSE(sp)` è l’errore quadratico medio tra
  i valori modellati e quelli osservati nel punto `sp`,
  calcolato sull’intero periodo di valutazione;

- `U_model(sp)` è l’incertezza dell’applicazione di modellizzazione;

- `U_meas(sp)` è l’incertezza delle misurazioni,
  determinata in conformità a `M_DATA_QUALITY`.

---

## Ambito di applicazione del MQI

- il MQI è calcolato utilizzando **solo osservazioni valide**
- le osservazioni devono essere **indipendenti**
  dai dati utilizzati come input del modello
- il MQI è calcolato:
  - per concentrazioni a lungo termine (medie annue)
  - per concentrazioni a breve termine (orario, 8 ore, 24 ore),
    secondo la metrica normativa applicabile

---

## Criteri di validazione del modello

### Regola generale

Un modello soddisfa l’obiettivo di qualità della modellizzazione se:

```

MQI ≤ 1

```

---

### Criterio di copertura della validazione

Il criterio `MQI ≤ 1` deve essere soddisfatto:

```

in almeno il 90 % dei punti di campionamento disponibili

```

La verifica è effettuata:

- sull’insieme dei punti che soddisfano `OBS_VALID`
- nell’area di valutazione
- per il periodo di riferimento considerato

---

### Caso con numero limitato di punti

Se il numero di punti di validazione è inferiore a 10:

```

VALID\_MODEL =
∀ sp ∈ validation\_stations :
MQI(sp) ≤ 1

```

---

## Requisiti di validazione

### 1. Controllo qualità degli input

Per ogni esecuzione del modello:

- dati di emissione
- dati meteorologici
- concentrazioni di background

devono essere verificati per coerenza e qualità.

---

### 2. Indipendenza dei dati di validazione

```

validation\_data ∩ model\_input\_data = ∅

```

I dati utilizzati per:
- calibrazione
- assimilazione
- ottimizzazione

**non possono** essere utilizzati per la validazione.

---

### 3. Selezione dei punti di validazione

I punti di validazione devono:

- coprire la variabilità spaziale dell’area
- includere ambienti diversi

Tipologie minime raccomandate:

- fondo urbano
- traffico
- suburbano o rurale

---

### 4. Metodo di validazione

Il metodo raccomandato è la **Leave‑One‑Out Cross‑Validation (LOOCV)**:

```

for each sp:
eseguire il modello escludendo sp
confrontare C\_model(sp) con C\_observed(sp)

```

Altri metodi sono ammessi se adeguatamente documentati.

---

### 5. Requisiti sui dati di validazione

```

∀ obs ∈ validation\_data :
OBS\_VALID = true

```

La validità dei dati è definita in `M_DATA_QUALITY`.

---

## Uso dei risultati modellistici

```

if NOT VALID\_MODEL:
model\_output NOT usable

```

Conseguenze:

- esclusione da `M_LIMITS`
- esclusione da `M_REPR`
- esclusione da riduzioni della rete (`M_NETWORK`)

---

## Interazioni con altri moduli

- `M_DATA_QUALITY` — definisce la validità dei dati
- `M_MOD` — produce i risultati modellistici
- `M_REPR` — utilizza il modello solo se validato
- `M_LIMITS` — accetta superamenti modellistici solo se validato
- `M_NETWORK` — consente riduzioni della rete solo se validato

---

## Output

```

model\_validation:
valid (boolean)
MQI\_summary
N\_val
validation\_method

```

---

## Note

- Il criterio `MQI ≤ 1` deriva dall’Allegato V.
- Il criterio del 90 % dei punti è vincolante.
- Il modulo definisce requisiti minimi normativi
  e non sostituisce la valutazione esperta.
