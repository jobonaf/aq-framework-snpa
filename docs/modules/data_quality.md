# M_DATA_QUALITY — Data Quality

## Riferimenti normativi

- Annex V Directive (EU) 2024/2881
- Implementing Acts (data usage rules)

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

MIN\_coverage, MAX\_uncertainty =
valori da tables/data\_quality.md

```

---

## Logica

```

DATA\_VALID =
(coverage ≥ MIN\_coverage)
AND (uncertainty ≤ MAX\_uncertainty)

```

---

## Regole operative

### Uso dei dati

```

if NOT DATA\_VALID:
data must be excluded

```

---

### Applicazione

- calcolo medie
- conteggio superamenti
- validazione modelli

---

### Dati indicativi

```

indicative measurements:
higher uncertainty allowed

```

---

### Integrazione con modellistica

```

only valid observations used
for model validation

```

---

## Interazioni con altri moduli

### M_MODEL_QA
```

validation requires DATA\_VALID = True

```

---

### M_LIMITS
```

compliance must use only valid data

```

---

### M_REPR
```

representativeness based on valid data

```

---

## Tabelle

- ../tables/data_quality.md
