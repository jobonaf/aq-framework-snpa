# M_DATA_QUALITY — Qualità dei dati

## Riferimenti normativi

- Allegato V Direttiva (UE) 2024/2881
- Atti di esecuzione (regole di utilizzo dei dati)

---

## Descrizione

Il modulo definisce i requisiti di qualità dei dati utilizzati per:

- valutazione (M_LIMITS)
- rappresentatività (M_REPR)
- validazione modellistica (M_MODEL_QA)

---

## Definizioni

```

coverage = percentuale dati validi

uncertainty = incertezza di misura

MIN_coverage, MAX_uncertainty =
valori da tables/data_quality.md

```

---

## Logica

```

DATA_VALID =
(coverage ≥ MIN_coverage)
AND (uncertainty ≤ MAX_uncertainty)

```

---

## Regole operative

### Uso dei dati

```

if NOT DATA_VALID:
i dati devono essere esclusi

```

---

### Applicazione

- calcolo medie
- conteggio superamenti
- validazione modelli

---

### Dati indicativi

```

misure indicative:
incertezza maggiore consentita

```

---

### Integrazione con modellistica

```

solo osservazioni valide usate
per la validazione del modello

```

---

## Moduli e tabelle correlati

La qualità dei dati condiziona tutte le valutazioni successive.

- [M_MODEL_QA](model_qa.md): la validazione modellistica richiede dati di qualità adeguata.
- [M_LIMITS](limits.md): solo dati validi possono essere usati per la conformità.
- [Obiettivi di qualità dei dati](../tables/data_quality.md): specificano copertura e incertezza massime.