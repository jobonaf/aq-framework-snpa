# M_REPR — Rappresentatività spaziale dei punti di campionamento

## Riferimenti normativi

- Art. 4(26) Direttiva (UE) 2024/2881
- Art. 8 Direttiva (UE) 2024/2881
- Art. 9 Direttiva (UE) 2024/2881
- Atti di esecuzione (bozza 2026 — metodologia di rappresentatività spaziale)
- Allegato IV (riferimenti impliciti)

---

## Descrizione

Il modulo definisce la **rappresentatività spaziale** dei punti di campionamento,
ossia l’area geografica in cui le concentrazioni osservate o modellate in un punto
sono rappresentative entro una tolleranza definita.

La rappresentatività spaziale è utilizzata per:

- collegare misure puntuali e territorio
- interpretare correttamente i risultati delle misure
- supportare la progettazione e la verifica della rete di monitoraggio
- integrare misure e modellistica

Il modulo **non** verifica la conformità ai valori limite
e **non** valuta l’adeguatezza complessiva della rete.

---

## Definizioni

```

C\_sp = valore centrale di concentrazione
nel punto di campionamento sp,
calcolato secondo la metrica normativa applicabile

```
```

T\_min(p) = tolleranza minima per l’inquinante p,
definita in T\_REPR\_TOLERANCE

```
```

Δ(sp, p) = max(0.15 × C\_sp, T\_min(p))

```
```

interval(sp, p) = \[C\_sp − Δ, C\_sp + Δ]

```
```

C(x, p) = concentrazione dell’inquinante p
nella localizzazione x,
misurata o modellata

```

---

## Logica di base

Una localizzazione appartiene all’area di rappresentatività
di un punto di campionamento se la concentrazione rientra
nell’intervallo di tolleranza definito.

```

REPRESENTED(x, sp, p) =
C(x, p) ∈ interval(sp, p)

```
```

AREA\_REPR(sp, p) =
{ x ∈ zone | REPRESENTED(x, sp, p) }

```

---

## Procedura operativa

### 1. Determinazione del valore centrale

```

C\_sp = valore centrale misurato nel punto sp

```

Il valore centrale e la metrica utilizzata
sono quelli **definiti dalla normativa applicabile**
per lo specifico inquinante (es. media annua, media su 8 ore).

Il modulo **non decide** quale metrica utilizzare.

---

### 2. Identificazione preliminare dell’area

#### Caso basato su misure

```

AREA\_prelim =
{ x | C\_meas(x, p) ∈ interval(sp, p) }

```

---

#### Caso basato su modellistica

```

grid = model(zone)

AREA\_prelim =
{ cell ∈ grid | C\_model(cell, p) ∈ interval(sp, p) }

```

L’uso della modellistica è consentito **solo se il modello è validato**
(secondo `M_MODEL_QA`).

---

### 3. Affinamento obbligatorio dell’area

L’area preliminare deve essere affinata applicando
criteri spaziali, tipologici ed emissivi.

---

#### Vincolo geografico

```

x ∈ zone

```

- l’area è limitata ai confini della zona
- possono esistere domini non contigui

---

#### Coerenza con il tipo di stazione

Sono escluse localizzazioni non coerenti con la tipologia del punto:

- siti di traffico per stazioni di fondo
- siti industriali non rappresentativi

---

#### Coerenza emissiva

```

escludi x dove emission\_profile(x)
differisce significativamente da emission\_profile(sp)

```

---

#### Vincoli locali

L’area può essere ulteriormente limitata in presenza di:

- contesti urbani complessi
- discontinuità morfologiche
- vincoli amministrativi specifici

---

#### Giudizio esperto

Il giudizio esperto è richiesto nei casi di:

- forte eterogeneità spaziale
- orografia complessa
- condizioni di dispersione non uniformi

Il giudizio deve essere documentato.

---

## Mappa di rappresentatività della zona

Per ciascuna zona e inquinante è definita una mappa di rappresentatività:

```

MAP\_REPR(p, z) =
{ AREA\_REPR(sp, p) per tutti i punti sp }

```

La mappa descrive la **copertura spaziale delle misure**,
ma **non valuta** l’adeguatezza della rete,
che è responsabilità di `M_NETWORK`.

---

## Gestione delle sovrapposizioni

Se una localizzazione appartiene a più aree di rappresentatività:

```

x ∈ AREA\_REPR(sp1) AND x ∈ AREA\_REPR(sp2)

```

l’assegnazione avviene sulla base di criteri qualitativi:

- tipologia del punto
- coerenza emissiva
- livello di concentrazione

---

## Casi critici

### Incoerenza tra campo di concentrazione e aree di rappresentatività

Se una concentrazione modellata risulta:

- significativamente superiore ai valori osservati
- non coerente con alcuna AREA_REPR

allora:

```

area non assegnabile
MODEL\_REVIEW\_REQUIRED = true

```

Il caso richiede la revisione del modello o della rete,
ma **non implica automaticamente una non conformità**.

---

## Uso nei moduli a valle

### Interazione con M_NETWORK

Se una concentrazione rilevante cade al di fuori
di tutte le aree di rappresentatività:

```

ADDITIONAL\_STATION\_REQUIRED = true

```

La decisione sull’introduzione di nuove stazioni
è responsabilità di `M_NETWORK`.

---

### Interazione con M_LIMITS

La rappresentatività spaziale determina **dove**
una misura è valida, ma **non valuta** il superamento
dei valori limite, che è responsabilità di `M_LIMITS`.

---

## Moduli e tabelle correlati

- M_ASSESS — definisce il regime di valutazione
- M_NETWORK — valuta l’adeguatezza della rete
- M_MOD — fornisce il campo di concentrazione continuo
- M_MODEL_QA — valida l’uso della modellistica
- T_REPR_TOLERANCE — definisce le tolleranze normative

---

## Output

```

representativeness:
station\_id
pollutant
geometry (polygon / multipolygon)
central\_value
tolerance
method = measurement | modelling | hybrid

```

---

## Frequenza di aggiornamento

La rappresentatività spaziale deve essere aggiornata:

- almeno ogni 5 anni
- in caso di modifica della rete
- in presenza di variazioni emissive significative
- in caso di cambiamenti rilevanti nelle condizioni di dispersione
