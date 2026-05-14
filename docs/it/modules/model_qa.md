# M_MODEL_QA — Validazione delle applicazioni modellistiche

## Riferimenti normativi

- Direttiva (UE) 2024/2881, Allegato V
- Art. 8 Direttiva (UE) 2024/2881
- Atti di esecuzione (metodologia di validazione dei modelli)

---

## Descrizione

Il modulo definisce i **requisiti normativi di validazione**
delle applicazioni di modellizzazione della qualità dell’aria.

Un modello validato è **condizione necessaria**
per l’utilizzo dei risultati nei moduli:

- `M_MOD` — uso della modellistica
- `M_REPR` — rappresentatività spaziale
- `M_LIMITS` — verifica di conformità
- `M_NETWORK` — decisioni sulla rete di monitoraggio

Il modulo **non disciplina** aspetti di governance istituzionale
(accreditamento, JRC, interconfronti UE).

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

## Indicatore di qualità della modellizzazione (MQI)

```

MQI(sp) =
RMSE(sp)
\--------------------------------
sqrt( U\_model(sp)^2 + U\_meas(sp)^2 )

```

dove:

- `RMSE(sp)` è l’errore quadratico medio tra
  valori modellati e osservati nel punto `sp`,
  calcolato sull’intero periodo di valutazione;
- `U_model(sp)` è l’incertezza della modellizzazione;
- `U_meas(sp)` è l’incertezza delle misurazioni.

Il MQI è calcolato:

- usando **solo osservazioni valide**
- su dati **indipendenti** dall’input del modello
- per la metrica normativa applicabile
  (lungo o breve termine)

---

## Requisiti normativi

### REQ-MODELQA-MQI_DEFINITION

| Campo | Valore |
|------|-------|
| Fonte | Allegato V Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | T_DATA_QUALITY |

**Regola**  
La qualità di un’applicazione di modellizzazione
è valutata mediante l’indicatore MQI,
definito come rapporto tra errore di modellizzazione
e incertezza complessiva.

**Criterio di accettazione**  
Il MQI è calcolato secondo la definizione normativa.

---

### REQ-MODELQA-MQI_THRESHOLD

| Campo | Valore |
|------|-------|
| Fonte | Allegato V Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | REQ-MODELQA-MQI_DEFINITION |

**Regola**  
Un modello soddisfa l’obiettivo di qualità
se l’indicatore MQI non supera il valore unitario.

**Criterio di accettazione**

```

MQI(sp) ≤ 1

```

---

### REQ-MODELQA-COVERAGE_90_PERCENT

| Campo | Valore |
|------|-------|
| Fonte | Allegato V Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | REQ-MODELQA-MQI_THRESHOLD |

**Regola**  
Il criterio `MQI ≤ 1` deve essere soddisfatto
in almeno il **90 % dei punti di campionamento disponibili**
nell’area di valutazione e nel periodo considerato.

**Criterio di accettazione**

```

COUNT{ sp | MQI(sp) ≤ 1 } / N\_val ≥ 0.9

```

---

### REQ-MODELQA-LOW_STATION_COUNT

| Campo | Valore |
|------|-------|
| Fonte | Allegato V Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | REQ-MODELQA-MQI_THRESHOLD |

**Regola**  
Se il numero di punti di validazione è inferiore a 10,
il modello è considerato valido
solo se il criterio `MQI ≤ 1` è soddisfatto
in **tutti** i punti disponibili.

**Criterio di accettazione**

```

if N\_val < 10:
∀ sp : MQI(sp) ≤ 1

```

---

### REQ-MODELQA-DATA_INDEPENDENCE

| Campo | Valore |
|------|-------|
| Fonte | Allegato V Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | — |

**Regola**  
I dati utilizzati per la validazione
devono essere indipendenti dai dati
utilizzati come input del modello.

**Criterio di accettazione**

```

validation\_data ∩ model\_input\_data = ∅

```

---

### REQ-MODELQA-DATA_VALIDITY

| Campo | Valore |
|------|-------|
| Fonte | Allegato V Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | M_DATA_QUALITY |

**Regola**  
Solo osservazioni che soddisfano
i requisiti di qualità dei dati
possono essere utilizzate per la validazione.

**Criterio di accettazione**

```

∀ obs ∈ validation\_data :
OBS\_VALID = true

```

---

### REQ-MODELQA-VALIDATION_METHOD

| Campo | Valore |
|------|-------|
| Fonte | Allegato V Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | raccomandato |
| Dipendenze | — |

**Regola**  
La validazione del modello utilizza
metodi che consentono il confronto
tra risultati modellistici e osservazioni indipendenti.

Il metodo raccomandato è la
Leave‑One‑Out Cross‑Validation (LOOCV).

**Criterio di accettazione**  
Il metodo di validazione è documentato.

---

### REQ-MODELQA-USAGE_CONSTRAINT

| Campo | Valore |
|------|-------|
| Fonte | Art. 8 Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | REQ-MODELQA-COVERAGE_90_PERCENT |

**Regola**  
I risultati di un modello non validato
non possono essere utilizzati
per scopi regolatori.

**Criterio di accettazione**

```

if VALID\_MODEL = false:
model\_output NOT usable

```

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

- Il criterio `MQI ≤ 1` e la soglia del 90 %
  derivano direttamente dall’Allegato V.
- Il modulo definisce requisiti minimi normativi
  e non sostituisce il giudizio esperto.
