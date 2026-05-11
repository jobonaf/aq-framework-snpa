
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

N_active = numero stazioni attive

N_min = numero minimo richiesto (Tabella network)

VALID(station) = rispetto criteri di posizionamento (Allegato IV)

```

---

## Logica

```

NETWORK_OK =
(N_active ≥ N_min)
AND ∀ st : VALID(st)

```

---

## Regole operative

### Riduzione della rete

```

if ABOVE_THRESHOLD
AND C_ann ≤ LIMIT_VALUE:
→ N_min_eff = ceil(N_min \* 0.5)

```

Condizioni:

- uso di modellistica validata (vedi M_MODEL_QA)
- oppure integrazione con misure indicative
- mantenimento adeguata informazione spaziale

---

### Nuove stazioni

```

if model_exceedance outside all AREA_REPR:
→ ADDITIONAL_STATION_REQUIRED = True

```

Tempistiche:

- 1 anno (misure indicative)
- 2 anni (misure fisse)

---

### Spostamento stazioni

```

RELOCATION_FORBIDDEN =
∃ y negli ultimi 3 anni :
C(station, y) > LIMIT_VALUE

```

Eccezione:

```

if new_location ∈ same AREA_REPR:
relocation allowed

```

---

### Copertura spaziale

```

NETWORK_COVERAGE =
union(AREA_REPR(stations))

```

Condizione implicita:

```

zone ⊆ NETWORK_COVERAGE

```

---

### Punti critici

La rete deve garantire copertura di:

- aree ad alta concentrazione
- zone trafficate
- aree industriali
- zone ad alta esposizione della popolazione

---

## Moduli e tabelle correlati

La progettazione e la verifica della rete dipendono da criteri normativi e da altri moduli.

- [M_REPR](repr.md): ogni stazione deve avere un’area di rappresentatività definita.
- [M_MOD](modelling.md): la modellistica validata consente la riduzione della rete o l’individuazione di nuove stazioni.
- [Numero minimo di stazioni](../tables/network.md): requisiti quantitativi basati sulla popolazione.
- [Criteri di posizionamento](../tables/siting.md): requisiti tecnici per la validità delle stazioni.
