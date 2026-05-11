
# M_NETWORK — Rete di monitoraggio

## Riferimenti normativi
- Art. 9 Direttiva (UE) 2024/2881
- Allegato III (numero minimo di stazioni)
- Allegato IV (criteri di posizionamento)

---

## Descrizione

Il modulo verifica l’adeguatezza della rete di monitoraggio per ciascun inquinante e zona.

La rete deve garantire:

- numero minimo di punti di campionamento
- copertura adeguata delle aree rappresentative
- rappresentazione dei livelli massimi di concentrazione
- coerenza con il regime di valutazione (M_ASSESS)

---

## Definizioni

```

N\_active = numero stazioni attive

N\_min = numero minimo richiesto (Tabella network)

VALID(station) = rispetto criteri di posizionamento (Allegato IV)

```

---

## Logica

```

NETWORK\_OK =
(N\_active ≥ N\_min)
AND ∀ st : VALID(st)

```

---

## Regole operative

### Riduzione della rete

```

if ABOVE\_THRESHOLD
AND C\_ann ≤ LIMIT\_VALUE:
→ N\_min\_eff = ceil(N\_min \* 0.5)

```

Condizioni:

- uso di modellistica validata (vedi M_MODEL_QA)
- oppure integrazione con misure indicative
- mantenimento adeguata informazione spaziale

---

### Nuove stazioni

```

if model\_exceedance outside all AREA\_REPR:
→ ADDITIONAL\_STATION\_REQUIRED = True

```

Tempistiche:

- 1 anno (misure indicative)
- 2 anni (misure fisse)

---

### Spostamento stazioni

```

RELOCATION\_FORBIDDEN =
∃ y negli ultimi 3 anni :
C(station, y) > LIMIT\_VALUE

```

Eccezione:

```

if new\_location ∈ same AREA\_REPR:
relocation allowed

```

---

### Copertura spaziale

```

NETWORK\_COVERAGE =
union(AREA\_REPR(stations))

```

Condizione implicita:

```

zone ⊆ NETWORK\_COVERAGE

```

---

### Punti critici

La rete deve garantire copertura di:

- aree ad alta concentrazione
- zone trafficate
- aree industriali
- zone ad alta esposizione della popolazione

---

## Interazioni con altri moduli

### M_REPR
```

ogni stazione deve avere AREA\_REPR definita

```

### M_MOD
```

riduzione rete ⇒ richiede modello valido

```

### M_LIMITS
```

superamenti ⇒ possono generare nuove stazioni

