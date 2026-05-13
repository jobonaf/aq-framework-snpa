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

#### REQ-ASSESS-THRESHOLD_CLASSIFICATION

- Fonte: Art. 8 Dir. 2024/2881 [+ Allegato II]
- Stato: DRAFT
- Tipo: obbligatorio
- Dipendenze: tables/t_assess_thresholds.md

**Regola**
La zona è classificata come sopra soglia quando, per un inquinante e una metrica,
il numero di anni nei quali la concentrazione supera la soglia è almeno 3 nei 5 anni precedenti.

**Criterio di accettazione**
Dato un inquinante, una zona e i valori di concentrazione annuali/periodiche,
il sistema restituisce ABOVE_THRESHOLD=true se COUNT{ y ∈ ultimi_5_anni | C_y > TH } ≥ 3.

**Pseudocode**
ABOVE_THRESHOLD = COUNT{ y ∈ ultimi_5_anni where C_y > TH } ≥ 3

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