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

## Interazioni con altri moduli

### M_MODEL_QA
```

la validazione richiede DATA_VALID = True

```

---

### M_LIMITS
```

la conformità deve usare solo dati validi

```

---

### M_REPR
```

rappresentatività basata su dati validi

```

---

## Tabelle

- ../tables/data_quality.md
