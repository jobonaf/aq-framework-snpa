# M_REPR — Rappresentatività spaziale dei punti di campionamento

## Riferimenti normativi

- Art. 4(26) Direttiva (UE) 2024/2881
- Art. 8 Direttiva (UE) 2024/2881
- Art. 9 Direttiva (UE) 2024/2881
- Atti di esecuzione (bozza 2026 — metodologia di rappresentatività spaziale)
- Allegato IV (riferimenti impliciti)

---

## Descrizione

La rappresentatività spaziale definisce l’area geografica in cui le concentrazioni osservate o modellate in un punto di campionamento sono rappresentative entro una tolleranza definita.

È utilizzata per:

- valutazione della qualità dell’aria
- interpretazione dei superamenti
- ottimizzazione della rete di monitoraggio
- integrazione tra misure e modellistica

---

## Definizioni

```

C_sp = concentrazione media annuale nel punto di campionamento

T_min = tolleranza minima ([vedi tabella repr_tolerance](../tables/t_repr_tolerance.md))

Δ = max(0.15 * C_sp, T_min)

interval = \[C_sp - Δ, C_sp + Δ]

```
```

C(x) = concentrazione nella localizzazione x
(misurata o modellata)

```

---

## Logica

### Regola di base

```

REPRESENTED(x, sp) =
C(x) ∈ interval(sp)

```
```

AREA_REPR(sp) =
{ x ∈ zone | REPRESENTED(x, sp) }

```

---

## Procedura operativa

### 1. Determinazione del valore centrale

```

C_sp = annual_mean(sp)

```

Alternative possibili (se giustificate):

- percentili
- medie stagionali
- metriche specifiche (es. O3 8h, AOT40)

---

### 2. Identificazione preliminare dell'area

#### Caso con misure

```

AREA_meas =
{ x | C_meas(x) ∈ interval }

```

---

#### Caso con modellistica

```

grid = model(zone)

AREA_model =
{ cell ∈ grid | C_model(cell) ∈ interval }

```

⚠ Uso consentito solo se il modello è valido ([vedi M_MODEL_QA](../model_qa.md))

---

### 3. Affinamento (obbligatorio)

L’area preliminare deve essere raffinata applicando:

---

#### Vincolo geografico

```

x ∈ zone

```

- inclusi domini non contigui
- limitazione ai confini amministrativi

---

#### Coerenza con il tipo di stazione

Escludere:

- siti traffico per stazioni background
- siti industriali non coerenti

---

#### Coerenza emissiva

```

escludi x dove emission_profile(x) differisce significativamente

```

---

#### Vincoli locali

Possibile limitazione a:

- area urbana
- area di interesse specifico

---

#### Giudizio esperto

Obbligatorio nei casi:

- forte eterogeneità spaziale
- orografia complessa
- condizioni di dispersione non uniformi

---

## Mappa di rappresentatività della zona

Per ogni zona e inquinante:

```

MAP_REPR =
{ AREA_REPR(sp) per tutte le stazioni }

```

---

### Gestione delle sovrapposizioni

Caso:

```

x ∈ AREA_REPR(sp1) AND x ∈ AREA_REPR(sp2)

```
```

assegnare al punto più rappresentativo

```

Criteri:

- tipo stazione
- profilo emissivo
- livello concentrazione

---

### Caso critico (Implementing Decision)

```

if C(x) > LIMIT_VALUE
AND nessuna stazione ha superamento:
x non assegnato a nessuna AREA_REPR

```

Conseguenze:

```

MODEL_REVIEW_REQUIRED = True

```

---

## Uso nella valutazione

### Interazione con le misure

```

if model_exceedance ∈ AREA_REPR
AND measurement non supera:
→ non considerare superamento

```

---

### Nuove stazioni

```

if exceedance outside all AREA_REPR:
ADDITIONAL_STATION_REQUIRED = True

```

→ [vedi M_NETWORK](../network.md)

---

### Coerenza modello–misure

```

if model ≠ measurement (fuori incertezza):
MODEL_REVIEW_REQUIRED = True

```

---

## Moduli e tabelle correlati

La rappresentatività spaziale collega misure, modellistica e territorio.

- [M_NETWORK](network.md): determina la copertura spaziale effettiva della rete.
- [M_MOD](modelling.md): il modello fornisce il campo di concentrazione continuo.
- [M_LIMITS](limits.md): stabilisce se un superamento è attribuibile a una stazione.
- [Tolleranze di rappresentatività](../tables/t_repr_tolerance.md): definiscono l’intervallo di concentrazione accettabile.

---

## Output

```

representativeness:
station_id
geometry (polygon / multipolygon)
central_value (C_sp)
tolerance (Δ)
method:
measurement | modelling | hybrid

```

---

## Frequenza di aggiornamento

```

update at least every 5 years

```

oppure quando:

- cambia la rete
- cambiano le emissioni
- variazioni meteorologiche significative

