# M_MOD — Applicazioni modellistiche

## Riferimenti normativi

- Art. 8 Direttiva (UE) 2024/2881
- Allegato IV (uso combinato dei metodi)
- Allegato V (incertezza e qualità)
- Atti di esecuzione (bozza 2026 — requisiti modellistici)

---

## Descrizione

Il modulo disciplina l’**uso regolatorio della modellistica**
nel sistema di valutazione della qualità dell’aria.

La modellistica è utilizzata per:

- supportare la distribuzione spaziale delle concentrazioni
- identificare hotspot
- delimitare aree di superamento
- integrare le misure puntuali
- supportare la rappresentatività spaziale (`M_REPR`)

Il modulo **non** valida i modelli
(vedi `M_MODEL_QA`).

---

## Definizioni

```

C\_model(x, t) =
concentrazione modellata
nella localizzazione x al tempo t

```
```

C\_meas(sp, t) =
concentrazione misurata
nel punto di campionamento sp

```
```

AREA\_REPR(sp) =
area di rappresentatività
definita in M\_REPR

```
```

MODEL\_USABLE =
modello validato
secondo M\_MODEL\_QA

```

---

## Requisiti normativi

### REQ-MOD-MODEL_VALIDATION_REQUIRED

| Campo | Valore |
|------|-------|
| Fonte | Art. 8 Dir. (UE) 2024/2881; Allegato V |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | M_MODEL_QA |

**Regola**  
I risultati di un modello possono essere utilizzati
solo se il modello è validato.

**Criterio di accettazione**

```

if use\_model = true:
MODEL\_USABLE = true

```

---

### REQ-MOD-COMBINED_USE

| Campo | Valore |
|------|-------|
| Fonte | Allegato IV Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | M_REPR |

**Regola**  
Quando sono disponibili misure valide,
la modellistica è utilizzata in combinazione con le misure.

**Criterio di accettazione**

```

if x ∈ AREA\_REPR:
C(x) = C\_meas
else:
C(x) = C\_model

```

---

### REQ-MOD-USE_ABOVE_THRESHOLD

| Campo | Valore |
|------|-------|
| Fonte | Art. 8 Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | M_ASSESS |

**Regola**  
In zone classificate sopra la soglia di valutazione,
la modellistica può essere utilizzata
solo in integrazione alle misure.

**Criterio di accettazione**

```

if zone ABOVE\_THRESHOLD:
model used only with measurements

```

---

### REQ-MOD-USE_BELOW_THRESHOLD

| Campo | Valore |
|------|-------|
| Fonte | Art. 8 Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | M_ASSESS |

**Regola**  
In zone classificate sotto la soglia di valutazione,
la modellistica può essere il metodo principale di valutazione.

**Criterio di accettazione**

```

if zone BELOW\_THRESHOLD:
model may be primary method

```

---

### REQ-MOD-MEASUREMENT_MODEL_CONFLICT

| Campo | Valore |
|------|-------|
| Fonte | Allegato IV Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | M_REPR |

**Regola**  
In caso di conflitto tra misure e modellistica
all’interno di un’area di rappresentatività,
prevalgono le misure.

**Criterio di accettazione**

```

if x ∈ AREA\_REPR
AND C\_meas indicates exceedance
AND C\_model does not:
model not used for assessment

```

---

### REQ-MOD-MODEL_ONLY_USE

| Campo | Valore |
|------|-------|
| Fonte | Allegato IV Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | M_MODEL_QA |

**Regola**  
In assenza di copertura di misure valide,
la modellistica può essere utilizzata come unico metodo
di valutazione, se validata.

**Criterio di accettazione**

```

if no valid measurements
AND MODEL\_USABLE = true:
model may be used exclusively

```

---

## Ruolo operativo della modellistica

Il modello deve fornire:

- un campo spaziale continuo di concentrazione
- identificazione di hotspot
- delimitazione delle aree di superamento
- supporto alla copertura spaziale della rete

---

## Interazioni con altri moduli

- `M_MODEL_QA` — valida i modelli
- `M_REPR` — definisce la rappresentatività spaziale
- `M_LIMITS` — utilizza i risultati modellistici per la conformità
- `M_NETWORK` — utilizza il modello per individuare lacune di copertura

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

- La modellistica non sostituisce le misure
  nelle aree rappresentate.
- L’uso del modello è sempre subordinato
  alla validazione normativa.
