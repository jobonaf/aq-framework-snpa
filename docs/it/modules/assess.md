# M_ASSESS — Regime di valutazione

## Riferimenti normativi

- Art. 8 Direttiva (UE) 2024/2881
- Allegato II (soglie di valutazione)
- Allegato IV (metodi di valutazione)

---

## Descrizione

Il modulo determina il **regime di valutazione** della qualità dell’aria
per ciascun inquinante e zona.

Il regime di valutazione stabilisce
quali metodi devono o possono essere utilizzati:

- misure fisse
- misure indicative
- modellistica
- stime oggettive

---

## Definizioni

```

TH(p) =
soglia di valutazione per l’inquinante p,
definita in T\_ASSESS\_THRESHOLDS

```
```

C(p, y) =
valore di concentrazione dell’inquinante p
nell’anno civile y,
calcolato secondo il periodo di mediazione previsto

```
```

ASSESSMENT\_TYPE(p, z) =
regime di valutazione per l’inquinante p
nella zona z

```

---

## Requisiti normativi

### REQ-ASSESS-CLASSIFICATION

| Campo | Valore |
|------|-------|
| Fonte | Art. 8 Dir. (UE) 2024/2881; Allegato II |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | T_ASSESS_THRESHOLDS |

**Regola**  
Per ciascun inquinante, una zona è classificata **sopra soglia**
se il valore di concentrazione supera la soglia di valutazione
in almeno **tre dei cinque anni civili precedenti**.

**Criterio di accettazione**  
Dato un inquinante `p` e una zona `z`,
il sistema restituisce `ABOVE_THRESHOLD = true` se:

```

COUNT{ y ∈ ultimi\_5\_anni | C(p, y) > TH(p) } ≥ 3

```

**Pseudo-code**  
Descrittivo, non eseguibile.

---

### REQ-ASSESS-TIME_WINDOW

| Campo | Valore |
|------|-------|
| Fonte | Art. 8 Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | — |

**Regola**  
La classificazione sopra o sotto soglia è effettuata
utilizzando una finestra mobile di **cinque anni civili**.

Gli anni non devono essere consecutivi.

**Criterio di accettazione**  
Dato un insieme di cinque anni civili,
il sistema valuta la condizione di superamento
indipendentemente dall’ordine temporale.

**Pseudo-code**  
Descrittivo, non eseguibile.

---

### REQ-ASSESS-REGIME_DEFINITION

| Campo | Valore |
|------|-------|
| Fonte | Art. 8 Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | REQ-ASSESS-CLASSIFICATION |

**Regola**  
Se una zona è classificata sopra soglia per un inquinante,
il regime di valutazione è basato su **misure fisse**.

Se una zona è classificata sotto soglia,
il regime di valutazione può basarsi su
modellistica o stima oggettiva.

**Criterio di accettazione**  

```

if ABOVE\_THRESHOLD:
ASSESSMENT\_TYPE = fixedMeasurements
else:
ASSESSMENT\_TYPE = modelOrObjectiveEstimation

```

**Pseudo-code**  
Descrittivo, non eseguibile.

---

### REQ-ASSESS-STRICTEST_PREVAILS

| Campo | Valore |
|------|-------|
| Fonte | Art. 8 Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | REQ-ASSESS-REGIME_DEFINITION |

**Regola**  
Se per uno stesso inquinante risultano applicabili
più condizioni di valutazione,
si applica il **regime più restrittivo**.

**Criterio di accettazione**  
Dato un insieme di regimi potenzialmente applicabili,
il sistema seleziona quello con il livello di obbligo più elevato.

**Pseudo-code**  
Descrittivo, non eseguibile.

---

## Moduli e tabelle correlati

- `T_ASSESS_THRESHOLDS` — definisce le soglie normative
- `M_NETWORK` — utilizza il regime di valutazione
- `M_MOD` — abilita o limita l’uso della modellistica
- `M_LIMITS` — utilizza il regime come contesto valutativo
