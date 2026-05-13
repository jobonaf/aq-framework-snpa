# M_ASSESS — Regime di valutazione

## Riferimenti normativi

- Art. 8 Direttiva (UE) 2024/2881
- Allegato II (soglie di valutazione)
- Allegato IV (metodi di valutazione)

---

## Descrizione

Il modulo determina il **regime di valutazione** della qualità dell’aria
per ciascun inquinante e zona.

Il regime stabilisce se la valutazione deve basarsi su:
- misure fisse,
- misure indicative,
- modellistica,
- stime oggettive.

---

## Definizioni

```

TH(p) = soglia di valutazione per l’inquinante p,
definita in T\_ASSESS\_THRESHOLDS

C(p, y) = valore di concentrazione rilevante
per l’inquinante p nell’anno y,
calcolato secondo il periodo di mediazione previsto

```

---

## Regole di classificazione

### REQ-ASSESS-CLASSIFICATION

- Fonte: Art. 8 Dir. 2024/2881 + Allegato II
- Stato: STABLE
- Tipo: obbligatorio
- Dipendenze: T_ASSESS_THRESHOLDS

**Regola**

Per ciascun inquinante, una zona è classificata **sopra soglia**
se il valore di concentrazione supera la soglia di valutazione
in **almeno 3 degli ultimi 5 anni civili**.

Il superamento è valutato secondo il periodo di mediazione
specificato per l’inquinante.

**Criterio di accettazione**

Dato un inquinante e una zona, il sistema restituisce
`ABOVE_THRESHOLD = true` se:

```

COUNT{ y ∈ ultimi\_5\_anni | C(p, y) > TH(p) } ≥ 3

```

---

## Determinazione del regime di valutazione

### Regola operativa

```

if ABOVE\_THRESHOLD:
ASSESSMENT\_TYPE = fixedMeasurements
else:
ASSESSMENT\_TYPE = modelOrObjectiveEstimation

```

---

## Regole operative

### Zone sopra soglia

- misure fisse obbligatorie
- modellistica ammessa solo in supporto
- misure indicative utilizzabili come integrazione

---

### Zone sotto soglia

- modellistica come metodo principale
- misure fisse non obbligatorie
- stime oggettive ammesse

---

## Principio di prevalenza

Se per uno stesso inquinante risultano applicabili
più condizioni di valutazione (es. salute umana e vegetazione),
si applica **il regime più restrittivo**.

---

## Moduli e tabelle correlati

- M_NETWORK — il numero minimo di stazioni dipende dal regime
- M_MOD — la modellistica è consentita o limitata dal regime
- T_ASSESS_THRESHOLDS — fornisce le soglie normative
