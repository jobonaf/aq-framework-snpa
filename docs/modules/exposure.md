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

C_ann(sp, y) = concentrazione media annuale

stations = stazioni di background

```

---

## Logica

```

AEI =
mean(
C_ann(sp, y)
for sp ∈ stations
negli ultimi 3 anni
)

```

---

## Regole operative

### Selezione delle stazioni

- solo stazioni background urbano/suburbano
- distribuzione rappresentativa della popolazione

---

### Media temporale

```

media mobile su 3 anni

```

---

### Utilizzo

- monitoraggio esposizione popolazione
- valutazione trend
- supporto politiche ambientali

---

## Moduli e tabelle correlati

L’indicatore di esposizione utilizza una selezione specifica della rete.

- [M_NETWORK](network.md): fornisce la classificazione delle stazioni di background.
- [M_DATA_QUALITY](data_quality.md): garantisce l’affidabilità dei dati utilizzati.
---

## Output

```

AEI:
value
period

```
