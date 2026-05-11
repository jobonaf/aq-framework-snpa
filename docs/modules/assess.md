# M_ASSESS — Assessment Regime

## Riferimenti normativi

- Art. 8 Directive (EU) 2024/2881
- Annex II (assessment thresholds)
- Annex IV (assessment methods)

---

## Descrizione

Il modulo determina il regime di valutazione della qualità dell’aria per ciascun inquinante e zona.

Definisce se utilizzare:

- misure fisse
- misure indicative
- modellistica
- stime oggettive

---

## Definizioni

```

C\_y = concentrazione annuale per anno y

TH = soglia di valutazione (tables/assess\_thresholds.md)

```

---

## Logica

### Classificazione zona

```

ABOVE\_THRESHOLD =
count(y ∈ ultimi\_5\_anni where C\_y > TH) ≥ 3

```

---

### Determinazione regime

```

if ABOVE\_THRESHOLD:
ASSESSMENT\_TYPE = fixedMeasurements
else:
ASSESSMENT\_TYPE = modelOrObjectiveEstimation

```

---

## Regole operative

### Zone sopra soglia

- uso obbligatorio di misure fisse
- modellistica utilizzabile in supporto
- possibile integrazione con misure indicative

---

### Zone sotto soglia

- modellistica può essere metodo principale
- misure non obbligatorie
- uso di stime oggettive consentito

---

### Uso combinato (implementing acts)

```

ASSESSMENT\_DATA =
combination of:
measurements
modelling
indicative measurements

```

---

### Transizione (implementing acts)

```

after implementing acts:
modelling becomes core component

```

---

## Interazioni con altri moduli

### M_NETWORK
```

number of stations depends on ABOVE\_THRESHOLD

```

---

### M_MOD
```

determines when modelling can/must be used

```

---

### M_LIMITS
```

assessment regime influences compliance decision

```

---

## Tabelle

- ../tables/assess_thresholds.md
