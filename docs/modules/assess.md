# M_ASSESS — Regime di valutazione

## Riferimenti normativi

- Art. 8 Direttiva (UE) 2024/2881
- Allegato II (soglie di valutazione)
- Allegato IV (metodi di valutazione)

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

### Uso combinato (atti di esecuzione)

```

ASSESSMENT\_DATA =
combinazione di:
misure
modellistica
misure indicative

```

---

### Transizione (atti di esecuzione)

```

dopo gli atti di esecuzione:
modellistica diventa componente principale

```

---

## Interazioni con altri moduli

### M_NETWORK
```

numero di stazioni dipende da ABOVE\_THRESHOLD

```

---

### M_MOD
```

determina quando la modellistica può/deve essere utilizzata

```

---

### M_LIMITS
```

il regime di valutazione influenza la decisione di conformità

```

---

## Tabelle

- ../tables/assess_thresholds.md
