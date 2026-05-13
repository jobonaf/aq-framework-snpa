# M_NETWORK — Rete di monitoraggio

## Riferimenti normativi

- Art. 9 Direttiva (UE) 2024/2881
- Allegato III (numero minimo di stazioni)
- Allegato IV (criteri di posizionamento)

---

## Descrizione

Il modulo verifica l’adeguatezza della rete di monitoraggio
per ciascun inquinante e zona.

La rete deve garantire:

- un numero minimo di punti di campionamento
- una copertura spaziale adeguata
- la rappresentazione dei livelli di concentrazione rilevanti
- coerenza con il regime di valutazione definito in `M_ASSESS`

---

## Definizioni

```

N\_active(p, z) = numero di stazioni attive
per l’inquinante p nella zona z

N\_min(p, z) = numero minimo di stazioni richiesto,
definito in T\_MIN\_STATIONS

VALID(st) = stazione conforme ai criteri di posizionamento
(T\_SITING)

ASSESSMENT\_TYPE(p, z) = regime di valutazione
definito in M\_ASSESS

```

---

## Verifica di adeguatezza della rete

### Regola base

Una rete è considerata **adeguata** se:

```

NETWORK\_OK(p, z) =
(N\_active(p, z) ≥ N\_min(p, z))
AND ∀ st ∈ stations : VALID(st)

```

---

## Regole operative

### Riduzione del numero minimo di stazioni

La Direttiva consente una riduzione del numero minimo di stazioni
in presenza di specifiche condizioni normative.

#### Condizione abilitante

```

if ASSESSMENT\_TYPE = fixedMeasurements
AND modellistica validata disponibile:
riduzione ammessa

```

#### Regola

```

N\_min\_eff = ceil(0.5 × N\_min)

```

#### Vincoli

- la modellistica deve essere validata (`M_MODEL_QA`)
- la copertura spaziale deve rimanere adeguata
- la riduzione non deve compromettere l’informazione sui livelli massimi

---

### Introduzione di nuove stazioni

Se la modellistica individua aree con concentrazioni elevate
non coperte dalle aree di rappresentatività esistenti:

```

if exceedance detected
AND outside all AREA\_REPR:
ADDITIONAL\_STATION\_REQUIRED = true

```

#### Tempistiche indicative

- misure indicative: entro 1 anno
- misure fisse: entro 2 anni

---

### Spostamento delle stazioni

Lo spostamento di una stazione è vietato se la stazione
ha registrato superamenti rilevanti negli ultimi tre anni.

```

RELOCATION\_FORBIDDEN =
∃ y ∈ ultimi\_3\_anni :
exceedance detected

```

#### Eccezione

```

if new\_location ∈ same AREA\_REPR:
relocation allowed

```

---

## Copertura spaziale

La rete deve garantire la copertura dell’intera zona:

```

NETWORK\_COVERAGE =
union(AREA\_REPR(stations))

zone ⊆ NETWORK\_COVERAGE

```

---

## Punti critici

La rete deve includere punti di campionamento rappresentativi di:

- aree ad alta concentrazione
- zone trafficate
- aree industriali
- aree ad elevata esposizione della popolazione

---

## Moduli e tabelle correlati

- M_ASSESS — definisce il regime di valutazione
- M_REPR — definisce le aree di rappresentatività
- M_MOD — supporta l’individuazione di aree non coperte
- M_MODEL_QA — abilita la riduzione della rete
- T_MIN_STATIONS — requisiti quantitativi
- T_SITING — criteri di posizionamento
