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

C_y = concentrazione annuale per anno y

TH = soglia di valutazione (tables/t_assess_thresholds.md)

```

---

## Logica

### Classificazione della zona

```

ABOVE_THRESHOLD =
count(y ∈ ultimi_5_anni where C_y > TH) ≥ 3

```

---

### Determinazione del regime di valutazione

```

if ABOVE_THRESHOLD:
ASSESSMENT_TYPE = fixedMeasurements
else:
ASSESSMENT_TYPE = modelOrObjectiveEstimation

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

### Uso combinato (Implementing Decision)

```

ASSESSMENT_DATA =
combinazione di:
misure
modellistica
misure indicative

```

---

### Transizione (Implementing Decision)

```

dopo gli atti di esecuzione:
modellistica diventa componente principale

```

---

## Moduli e tabelle correlati

Il regime di valutazione definisce quali metodi possono o devono essere utilizzati.

- [M_NETWORK](network.md): il numero minimo di stazioni dipende dalla classificazione sopra/sotto soglia.
- [M_MOD](modelling.md): la modellistica è obbligatoria o opzionale in funzione del regime.
- [Tabelle delle soglie di valutazione](../tables/t_assess_thresholds.md): definiscono i valori di riferimento per la classificazione delle zone.