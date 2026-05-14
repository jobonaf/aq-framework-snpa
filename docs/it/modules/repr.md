# M_REPR — Rappresentatività spaziale dei punti di campionamento

## Riferimenti normativi

- Art. 4(26) Direttiva (UE) 2024/2881
- Art. 8 Direttiva (UE) 2024/2881
- Art. 9 Direttiva (UE) 2024/2881
- Atti di esecuzione (bozza 2026 — metodologia di rappresentatività spaziale)
- Allegato IV (criteri di localizzazione)

---

## Descrizione

Il modulo definisce la **rappresentatività spaziale**
dei punti di campionamento, ossia l’area geografica
in cui le concentrazioni osservate o modellate in un punto
sono rappresentative entro una tolleranza normativa.

Il modulo:

- collega misure puntuali e territorio
- supporta la progettazione della rete
- integra misure e modellistica

Il modulo **non**:
- verifica la conformità ai valori limite
- valuta l’adeguatezza complessiva della rete

---

## Definizioni

```

C\_sp =
valore centrale di concentrazione
nel punto di campionamento sp,
calcolato secondo la metrica normativa applicabile

```
```

T\_min(p) =
tolleranza minima per l’inquinante p,
definita in T\_REPR\_TOLERANCE

```
```

Δ(sp, p) =
max(0.15 × C\_sp, T\_min(p))

```
```

interval(sp, p) =
\[C\_sp − Δ, C\_sp + Δ]

```
```

AREA\_REPR(sp, p) =
{ x ∈ zone | C(x, p) ∈ interval(sp, p) }

```

---

## Requisiti normativi

### REQ-REPR-AREA_DEFINITION

| Campo | Valore |
|------|-------|
| Fonte | Art. 4(26) Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | T_REPR_TOLERANCE |

**Regola**  
L’area di rappresentatività di un punto di campionamento
è definita come l’insieme delle localizzazioni
in cui la concentrazione rientra nell’intervallo di tolleranza
attorno al valore centrale misurato.

**Criterio di accettazione**

```

AREA\_REPR(sp, p) =
{ x | C(x, p) ∈ interval(sp, p) }

```

---

### REQ-REPR-MODEL_USAGE

| Campo | Valore |
|------|-------|
| Fonte | Art. 8–9 Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | M_MODEL_QA |

**Regola**  
La modellistica può essere utilizzata
per la determinazione dell’area di rappresentatività
solo se il modello è validato.

**Criterio di accettazione**

```

if use\_model = true:
VALID\_MODEL = true

```

---

### REQ-REPR-AREA_REFINEMENT

| Campo | Valore |
|------|-------|
| Fonte | Allegato IV Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | — |

**Regola**  
L’area di rappresentatività preliminare
deve essere affinata applicando criteri:

- geografici
- tipologici
- emissivi

**Criterio di accettazione**  
Sono escluse dall’area le localizzazioni
non coerenti con il tipo di stazione
o con il profilo emissivo.

---

### REQ-REPR-EXPERT_JUDGEMENT

| Campo | Valore |
|------|-------|
| Fonte | Atti di esecuzione (metodologia) |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | — |

**Regola**  
Il giudizio esperto è obbligatorio nei casi di:

- forte eterogeneità spaziale
- orografia complessa
- condizioni di dispersione non uniformi

**Criterio di accettazione**  
Il giudizio esperto è documentato
e associato alla definizione dell’area.

---

### REQ-REPR-OVERLAP_RESOLUTION

| Campo | Valore |
|------|-------|
| Fonte | Atti di esecuzione (metodologia) |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | — |

**Regola**  
Se una localizzazione appartiene a più aree di rappresentatività,
l’assegnazione avviene sulla base di criteri qualitativi.

**Criterio di accettazione**  
L’assegnazione considera almeno:

- tipologia del punto
- coerenza emissiva
- livello di concentrazione

---

### REQ-REPR-CRITICAL_CASE

| Campo | Valore |
|------|-------|
| Fonte | Art. 8–9 Dir. (UE) 2024/2881 |
| Stato | STABLE |
| Tipo | obbligatorio |
| Dipendenze | M_MODEL_QA, M_NETWORK |

**Regola**  
Se una concentrazione modellata risulta
non coerente con alcuna area di rappresentatività,
il caso richiede la revisione del modello o della rete.

**Criterio di accettazione**

```

if no AREA\_REPR applicable:
MODEL\_REVIEW\_REQUIRED = true

```

Il caso **non implica automaticamente**
una non conformità normativa.

---

## Uso nei moduli a valle

- `M_NETWORK` utilizza le aree di rappresentatività
  per valutare la copertura della rete
- `M_LIMITS` utilizza la rappresentatività
  per determinare la validità spaziale delle misure

---

## Moduli e tabelle correlati

- `M_ASSESS` — regime di valutazione
- `M_NETWORK` — adeguatezza della rete
- `M_MODEL_QA` — validazione della modellistica
- `T_REPR_TOLERANCE` — tolleranze normative

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

La rappresentatività spaziale è aggiornata:

- almeno ogni 5 anni
- in caso di modifica della rete
- in presenza di variazioni emissive significative
- in caso di cambiamenti rilevanti nelle condizioni di dispersione
