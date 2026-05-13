# M_DATA_QUALITY — Qualità dei dati

## Riferimenti normativi

- Direttiva (UE) 2024/2881, Allegato V
- Art. 8 (valutazione della qualità dell’aria)

---

## Descrizione

Il modulo definisce le **condizioni di validità dei dati**
utilizzati per la valutazione della qualità dell’aria.

La qualità dei dati condiziona:

- la verifica di conformità (`M_LIMITS`)
- la determinazione della rappresentatività (`M_REPR`)
- la validazione delle applicazioni modellistiche (`M_MODEL_QA`)

Il modulo **non** disciplina le procedure di laboratorio,
accreditamento o QA/QC istituzionale.

---

## Definizioni

```

coverage(p, m) = percentuale di dati validi
per l’inquinante p
e la metrica m

```
```

uncertainty(p, m) = incertezza del dato
espressa con livello di confidenza 95 %

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

## Ambito di applicazione

Le regole di qualità dei dati si applicano a:

- misurazioni in siti fissi
- misurazioni indicative
- dati utilizzati per la valutazione e la validazione modellistica

Le regole **non si applicano** a:

- AOT40
- AEI / esposizione media
- soglie di allarme e di informazione
- livelli critici per la vegetazione e gli ecosistemi

---

## Regole di validità dei dati

### Regola generale

```

if DATA\_VALID(p, m) = false:
data excluded from assessment

```

---

### Distinzione per tipologia di misura

- **misure fisse**  
  → requisiti più stringenti di copertura e incertezza

- **misure indicative**  
  → requisiti meno stringenti, come definiti in `T_DATA_QUALITY`

Il tipo di misura è definito nel contesto del modulo che utilizza i dati.

---

## Lungo termine vs breve termine

Le verifiche di qualità dei dati sono effettuate separatamente per:

- concentrazioni a lungo termine (medie annue)
- concentrazioni a breve termine (orari, 8 ore, 24 ore)

La metrica applicabile è quella definita
dalla normativa per ciascun inquinante.

---

## Regole temporali (transizione normativa)

- Prima del 2030 si applicano le percentuali massime di incertezza
  previste per il periodo transitorio.
- A partire dal 2030, l’incertezza dei dati non supera
  il valore assoluto o relativo, se superiore,
  definito in `T_DATA_QUALITY`.

---

## Copertura dei dati

La copertura minima dei dati:

- è valutata sull’intero anno civile
- deve essere garantita anche su sotto‑periodi rilevanti
  (trimestre, mese, settimana, giorno),
  secondo quanto previsto dalla normativa

---

## Campionamento non continuo

Per inquinanti con copertura minima inferiore all’80 %:

- è ammesso il campionamento non continuo o casuale
- a condizione che l’incertezza complessiva
  soddisfi gli obiettivi di qualità dei dati

---

## Interazioni con altri moduli

- `M_LIMITS`  
  → utilizza solo dati validi per la conformità

- `M_REPR`  
  → utilizza solo dati validi per la rappresentatività

- `M_MODEL_QA`  
  → utilizza solo dati validi per la validazione dei modelli

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

- La valutazione della qualità dei dati
  non impedisce la segnalazione di una non conformità
  se i dati disponibili sono sufficienti
  per una valutazione conclusiva, anche in presenza
  di copertura incompleta, come previsto dall’Allegato V.
