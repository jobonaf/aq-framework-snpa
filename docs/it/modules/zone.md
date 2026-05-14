# M_ZONE — Suddivisione territoriale

## Riferimenti normativi

- Art. 3 Direttiva (UE) 2024/2881
- Allegato II (classificazione delle zone)

---

## Descrizione

Il modulo definisce la suddivisione del territorio in zone e agglomerati per la valutazione della qualità dell’aria.

---

## Definizioni

```

zone = unità amministrativa o funzionale

population(zone)

area(zone)

```

---

## Logica

```

ZONE_COVERAGE_VALID =
∀ x ∈ territorio :
∃ zona z : x ∈ z

```

---

## Regole operative

### Copertura territoriale

- l’intero territorio deve essere suddiviso in zone
- le zone non devono sovrapporsi

---

### Aggiornamento

```

update zoning:
at least every 5 years
OR when significant changes occur

```

---

### Classificazione

Zone possono essere:

- urbane
- suburbane
- rurali
- agglomerati

---

### Coerenza

```

zone must be consistent with:
emission patterns
population distribution
monitoring objectives

```

---

## Moduli e tabelle correlati

Questo modulo fornisce il contesto territoriale per l’intero framework.

- [M_ASSESS](assess.md): il regime di valutazione è determinato per ciascuna zona.
- [M_NETWORK](network.md): il numero minimo e la tipologia di stazioni dipendono dalle caratteristiche della zona.
- [M_REPR](repr.md): le aree di rappresentatività sono sempre limitate ai confini zonali.

---

## Output

```

zone:
id
geometry
population
type

```
