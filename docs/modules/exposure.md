# M_EXPOSURE — Average Exposure Indicator (AEI)

## Riferimenti normativi

- Annex VI Directive (EU) 2024/2881
- Exposure reduction obligations

---

## Descrizione

Il modulo definisce l’indicatore di esposizione media (AEI), utilizzato per valutare l’impatto sulla popolazione.

Si basa su stazioni:

- urban background
- suburban background

---

## Definizioni

```

C\_ann(sp, y) = concentrazione media annuale

stations = stazioni di background

```

---

## Logica

```

AEI =
mean(
C\_ann(sp, y)
for sp ∈ stations
over last 3 years
)

```

---

## Regole operative

### Selezione stazioni

- solo urban/suburban background
- distribuzione rappresentativa della popolazione

---

### Media temporale

```

3-year moving average

```

---

### Uso

- monitoraggio esposizione popolazione
- valutazione trend
- supporto politiche ambientali

---

## Interazioni con altri moduli

### M_NETWORK
```

depends on correct station classification

```

---

### M_DATA_QUALITY
```

uses only valid data

```

---

## Output

```

AEI:
value
period

```

---

## Tabelle

- ../tables/network.md (implicit filtering of stations)
