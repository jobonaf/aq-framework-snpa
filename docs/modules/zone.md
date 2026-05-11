# M_ZONE — Zoning

## Riferimenti normativi

- Art. 3 Directive (EU) 2024/2881
- Annex II (zone classification)

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

ZONE\_COVERAGE\_VALID =
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

- urban
- suburban
- rural
- agglomerations

---

### Coerenza

```

zone must be consistent with:
emission patterns
population distribution
monitoring objectives

```

---

## Interazioni con altri moduli

### M_NETWORK
```

number of stations depends on zone characteristics

```

---

### M_ASSESS
```

assessment regime defined per zone

```

---

### M_REPR
```

representativeness constrained within zone

```

---

### M_LIMITS
```

compliance determined at zone level

```

---

## Output

```

zone:
id
geometry
population
type

```
