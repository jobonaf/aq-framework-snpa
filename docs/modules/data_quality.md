# M_DATA_QUALITY — Qualità dei dati

## Riferimenti normativi

- Direttiva (UE) 2024/2881, Allegato V
- Art. 8 Direttiva (UE) 2024/2881

---

## Descrizione

Il modulo definisce le **condizioni normative di validità dei dati**
utilizzati per la valutazione della qualità dell’aria.

La qualità dei dati condiziona:

- la verifica di conformità (`M_LIMITS`)
- la rappresentatività spaziale (`M_REPR`)
- la validazione delle applicazioni modellistiche (`M_MODEL_QA`)

Il modulo **non disciplina** aspetti di governance istituzionale
(laboratori, accreditamento, JRC).

---

## Definizioni

```

coverage(p, m) =
percentuale di dati validi
per l’inquinante p e la metrica m

```
```

uncertainty(p, m) =
incertezza del dato
(livello di confidenza 95 %)

```
```

MIN\_coverage(p, m)
MAX\_uncertainty(p, m)
\= valori normativi definiti in T\_DATA\_QUALITY

```
```

DATA\_VALID(p, m) =
(coverage ≥ MIN\_coverage)
AND (uncertainty ≤ MAX\_uncertainty)

```

---

## Requisiti normativi

### REQ-DATA-VALIDITY_CRITERIA

| Campo | Valore |
|------|-------|
| Fonte | Allegato V Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | T_DATA_QUALITY |

**Regola**  
Un dato può essere utilizzato per la valutazione della qualità dell’aria
solo se soddisfa i requisiti minimi di copertura e di incertezza
definiti dalla normativa.

**Criterio di accettazione**

```

DATA\_VALID(p, m) =
(coverage ≥ MIN\_coverage)
AND (uncertainty ≤ MAX\_uncertainty)

```

---

### REQ-DATA-EXCLUSION_IF_INVALID

| Campo | Valore |
|------|-------|
| Fonte | Allegato V Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | REQ-DATA-VALIDITY_CRITERIA |

**Regola**  
I dati che non soddisfano i requisiti di qualità
sono esclusi dalle valutazioni successive.

**Criterio di accettazione**

```

if DATA\_VALID = false:
data excluded from assessment

```

---

### REQ-DATA-SCOPE_OF_APPLICATION

| Campo | Valore |
|------|-------|
| Fonte | Allegato V Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | — |

**Regola**  
I requisiti di qualità dei dati si applicano a:

- misurazioni in siti fissi
- misurazioni indicative
- dati utilizzati per la validazione modellistica

**Criterio di accettazione**  
Il tipo di dato è correttamente classificato
prima della verifica di qualità.

---

### REQ-DATA-EXCLUDED_METRICS

| Campo | Valore |
|------|-------|
| Fonte | Allegato V Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | — |

**Regola**  
I requisiti di incertezza e copertura **non si applicano** a:

- AOT40
- AEI / indicatore di esposizione media
- soglie di allarme e di informazione
- livelli critici per la vegetazione e gli ecosistemi

**Criterio di accettazione**  
Le metriche escluse non sono sottoposte
a verifica di qualità dei dati.

---

### REQ-DATA-LONG_SHORT_TERM

| Campo | Valore |
|------|-------|
| Fonte | Allegato V Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | T_DATA_QUALITY |

**Regola**  
La qualità dei dati è verificata separatamente per:

- concentrazioni a lungo termine (medie annue)
- concentrazioni a breve termine (oraria, 8 ore, 24 ore)

**Criterio di accettazione**  
La metrica applicabile è quella prevista
dalla normativa per ciascun inquinante.

---

### REQ-DATA-TEMPORAL_TRANSITION

| Campo | Valore |
|------|-------|
| Fonte | Allegato V Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | T_DATA_QUALITY |

**Regola**  
I requisiti di incertezza dei dati
tengono conto delle disposizioni transitorie
previste prima e dopo il 2030.

**Criterio di accettazione**  
Il confronto dell’incertezza utilizza
i valori normativi validi per l’anno considerato.

---

### REQ-DATA-INCOMPLETE_COVERAGE

| Campo | Valore |
|------|-------|
| Fonte | Allegato V Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | — |

**Regola**  
La valutazione della conformità può essere effettuata
anche in presenza di copertura dei dati incompleta,
se i dati disponibili consentono
una valutazione conclusiva.

**Criterio di accettazione**  
Una non conformità può essere segnalata
anche se la copertura minima non è raggiunta,
purché i dati validi siano sufficienti.

---

### REQ-DATA-RANDOM_SAMPLING

| Campo | Valore |
|------|-------|
| Fonte | Allegato V Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | — |

**Regola**  
Per inquinanti con copertura minima inferiore all’80 %,
è ammesso l’uso di campionamento non continuo o casuale,
a condizione che l’incertezza complessiva
soddisfi gli obiettivi di qualità dei dati.

**Criterio di accettazione**  
L’uso del campionamento casuale
è documentato e giustificato.

---

## Interazioni con altri moduli

- `M_LIMITS` — utilizza solo dati validi per la conformità
- `M_REPR` — utilizza solo dati validi per la rappresentatività
- `M_MODEL_QA` — utilizza solo dati validi per la validazione dei modelli

---

## Output

```

data\_quality\_status:
pollutant
metric
data\_type = fixed | indicative | model\_input
coverage
uncertainty
valid (boolean)

```

---

## Note

- Il modulo definisce **requisiti minimi normativi**.
- La validità dei dati non sostituisce
  il giudizio esperto nei casi borderline.
