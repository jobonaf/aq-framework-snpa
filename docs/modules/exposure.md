# M_EXPOSURE — Indicatore di esposizione media (AEI)

## Riferimenti normativi

- Allegato VI Direttiva (UE) 2024/2881
- Obblighi di riduzione dell’esposizione

---

## Descrizione

Il modulo definisce l’indicatore di esposizione media (AEI), utilizzato per valutare l’impatto sulla popolazione.

Si basa su stazioni:

- background urbano
- background suburbano

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
negli ultimi 3 anni
)

```

---

## Regole operative

### Selezione stazioni

- solo stazioni background urbano/suburbano
- distribuzione rappresentativa della popolazione

---

### Media temporale

```

media mobile su 3 anni

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

dipende dalla corretta classificazione delle stazioni

```

---

### M_DATA_QUALITY
```

usa solo dati validi

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

- ../tables/network.md (filtro implicito delle stazioni)
