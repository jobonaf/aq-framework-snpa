# M_NETWORK — Rete di monitoraggio

## Riferimenti normativi

- Art. 9 Direttiva (UE) 2024/2881
- Allegato III (numero minimo di punti di campionamento)
- Allegato IV (criteri di posizionamento)

---

## Descrizione

Il modulo verifica l’**adeguatezza strutturale** della rete di monitoraggio
per ciascun inquinante e zona.

La rete deve garantire:

- un numero minimo di punti di campionamento
- una copertura spaziale adeguata
- la rappresentazione dei livelli di concentrazione rilevanti
- coerenza con il regime di valutazione definito in `M_ASSESS`

---

## Definizioni

```

N\_active(p, z) =
numero di punti di campionamento attivi
per l’inquinante p nella zona z

```
```

N\_min(p, z) =
numero minimo di punti di campionamento richiesto,
definito in T\_MIN\_STATIONS

```
```

VALID(st) =
punto di campionamento conforme
ai criteri di posizionamento (T\_SITING)

```
```

ASSESSMENT\_TYPE(p, z) =
regime di valutazione definito in M\_ASSESS

```

---

## Requisiti normativi

### REQ-NETWORK-MINIMUM_ADEQUACY

| Campo | Valore |
|------|-------|
| Fonte | Art. 9 Dir. (UE) 2024/2881; Allegato III |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | T_MIN_STATIONS, T_SITING |

**Regola**  
Una rete di monitoraggio è considerata adeguata se il numero di punti
di campionamento attivi è almeno pari al numero minimo richiesto
e tutti i punti rispettano i criteri di posizionamento.

**Criterio di accettazione**

```

NETWORK\_OK =
(N\_active ≥ N\_min)
AND ∀ st : VALID(st)

```

---

### REQ-NETWORK-MINIMUM_COMPOSITION

| Campo | Valore |
|------|-------|
| Fonte | Allegato III, lett. A Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | T_MIN_STATIONS |

**Regola**  
Per ciascuna zona, il numero minimo di punti di campionamento
comprende almeno:
- un punto di fondo,
- un punto critico di inquinamento atmosferico,

conformemente all’Allegato IV, a condizione che ciò
non comporti un aumento del numero totale di punti.

**Criterio di accettazione**  
È verificata la presenza di almeno un punto di fondo
e di almeno un punto critico.

---

### REQ-NETWORK-TRAFFIC_REPRESENTATION

| Campo | Valore |
|------|-------|
| Fonte | Allegato III, lett. A Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | — |

**Regola**  
Per NO₂, PM, benzene e CO,
la rete include almeno un punto di campionamento
finalizzato alla misura del contributo delle emissioni da trasporto.

**Criterio di accettazione**  
Per ciascun inquinante applicabile,
è presente almeno un punto di tipo traffico.

---

### REQ-NETWORK-BACKGROUND_TRAFFIC_BALANCE

| Campo | Valore |
|------|-------|
| Fonte | Allegato III, lett. A Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | — |

**Regola**  
Per NO₂, PM, benzene e CO,
il numero di punti di fondo urbano
e il numero di punti critici
non differiscono per un fattore superiore a 2.

**Criterio di accettazione**

```

max(
N\_background / N\_traffic,
N\_traffic / N\_background
) ≤ 2

```

---

### REQ-NETWORK-REDUCTION_ALLOWED

| Campo | Valore |
|------|-------|
| Fonte | Allegato III Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | M_MODEL_QA, M_ASSESS |

**Regola**  
È ammessa una riduzione fino al 50 % del numero minimo
di punti di campionamento solo se:
- il regime di valutazione lo consente;
- è disponibile modellistica validata.

**Criterio di accettazione**

```

if ASSESSMENT\_TYPE = fixedMeasurements
AND VALID\_MODEL = true:
reduction\_allowed = true

```

---

### REQ-NETWORK-REDUCTION_LIMIT

| Campo | Valore |
|------|-------|
| Fonte | Allegato III Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | T_MIN_STATIONS |

**Regola**  
La riduzione del numero minimo di punti di campionamento
non può superare il 50 %.

**Criterio di accettazione**

```

N\_min\_eff ≥ ceil(0.5 × N\_min)

```

---

### REQ-NETWORK-NEW_STATION_REQUIRED

| Campo | Valore |
|------|-------|
| Fonte | Art. 9 Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | M_REPR, M_MOD |

**Regola**  
Se sono individuate aree con concentrazioni rilevanti
non coperte da alcuna area di rappresentatività,
è obbligatoria l’introduzione di nuovi punti di campionamento.

**Criterio di accettazione**

```

if exceedance detected
AND outside all AREA\_REPR:
ADDITIONAL\_STATION\_REQUIRED = true

```

---

### REQ-NETWORK-RELOCATION_FORBIDDEN

| Campo | Valore |
|------|-------|
| Fonte | Art. 9 Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | M_REPR |

**Regola**  
È vietato lo spostamento di un punto di campionamento
che abbia registrato superamenti rilevanti
negli ultimi tre anni civili.

**Criterio di accettazione**

```

RELOCATION\_FORBIDDEN =
∃ y ∈ ultimi\_3\_anni :
exceedance detected

```

---

### REQ-NETWORK-OZONE_RURAL

| Campo | Valore |
|------|-------|
| Fonte | Allegato III, lett. C.2 Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | — |

**Regola**  
Per la valutazione degli obiettivi a lungo termine dell’ozono,
la rete include punti di fondo rurale con una densità media di:
- almeno un punto ogni 50 000 km²,
- almeno un punto ogni 25 000 km² in orografie complesse.

**Criterio di accettazione**  
La distribuzione dei punti rurali soddisfa le densità minime richieste.

---

### REQ-NETWORK-UFP

| Campo | Valore |
|------|-------|
| Fonte | Allegato III, lett. D Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | — |

**Regola**  
Il particolato ultrafine (UFP) è misurato
in siti in cui è probabile il verificarsi
di concentrazioni elevate, con almeno:
- un punto ogni 5 milioni di abitanti,
- almeno un punto negli Stati membri con popolazione inferiore.

Per Stati membri con meno di 2 milioni di abitanti,
i supersiti non sono conteggiati ai fini di tale obbligo.

**Criterio di accettazione**  
È verificata la presenza del numero minimo di punti UFP
nelle aree a maggiore probabilità di concentrazioni elevate.

---

## Moduli e tabelle correlati

- `M_ASSESS` — definisce il regime di valutazione
- `M_REPR` — definisce le aree di rappresentatività
- `M_MOD` — supporta l’individuazione di aree non coperte
- `M_MODEL_QA` — abilita la riduzione della rete
- `T_MIN_STATIONS` — requisiti quantitativi
- `T_SITING` — criteri di posizionamento
