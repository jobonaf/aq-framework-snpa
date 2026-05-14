# M_EXPOSURE — Indicatore di esposizione media (AEI / IEM)

## Riferimenti normativi

- Direttiva (UE) 2024/2881, Allegato I — Sezione 5
- Obbligo di riduzione dell’esposizione media
- Obiettivi di concentrazione dell’esposizione media

---

## Descrizione

Il modulo definisce l’indicatore di esposizione media
(**AEI / IEM — Indicatore di Esposizione Media**), utilizzato per valutare
l’esposizione della popolazione all’inquinamento atmosferico nel tempo.

L’AEI è uno **strumento di valutazione dell’esposizione**, distinto:
- dai valori limite,
- dalle soglie di informazione/allarme,
- dai piani di qualità dell’aria.

---

## Definizioni

```

C\_ann(sp, y) = concentrazione media annuale
nel punto di campionamento sp
per l’anno y

stations = punti di campionamento in siti
di fondo urbano (e suburbano)

AEI(y) = indicatore di esposizione media
per l’anno y

```

---

## Logica di calcolo

L’AEI per un determinato anno è definito come la
**media delle concentrazioni annuali degli ultimi tre anni civili**,
calcolata su tutti i punti di campionamento di fondo urbano
nelle unità territoriali di esposizione media.

```

AEI(y) =
mean(
C\_ann(sp, y),
C\_ann(sp, y-1),
C\_ann(sp, y-2)
for sp ∈ stations
)

```

---

## Regole operative

### Selezione delle stazioni

- solo punti di campionamento in siti di **fondo urbano**
- eventualmente fondo suburbano, se previsto a livello nazionale
- i punti devono essere allestiti conformemente all’Allegato III

---

### Trattamento delle fonti naturali

Se sono individuati superamenti imputabili a fonti naturali,
i relativi contributi **sono dedotti** prima del calcolo dell’AEI,
a condizione che l’esclusione sia documentata.

---

### Media temporale

- media mobile su **tre anni civili consecutivi**
- per gli anni 2030–2032 è possibile escludere l’anno 2020 dal calcolo

---

## Utilizzo dell’indicatore

L’AEI è utilizzato per:

- verificare il rispetto degli **obblighi di riduzione dell’esposizione media**
- confrontare l’esposizione nel tempo
- valutare il raggiungimento degli **obiettivi di concentrazione dell’esposizione media**

L’AEI **non è utilizzato direttamente** per la verifica di conformità
ai valori limite.

---

## Obblighi e obiettivi (riferimento)

Gli obblighi di riduzione percentuale e gli obiettivi di concentrazione
sono definiti nella tabella:

- `T_EXPOSURE_OBLIGATIONS`

Questo modulo si limita al **calcolo e alla valutazione dell’indicatore**;
la pianificazione delle azioni correttive è trattata in moduli separati.

---

## Moduli e tabelle correlati

- M_NETWORK — definisce i punti di campionamento di fondo urbano
- M_DATA_QUALITY — garantisce la validità dei dati utilizzati
- T_EXPOSURE_OBLIGATIONS — obblighi e obiettivi di esposizione media

---

## Output

```

AEI:
value
year
averaging\_period = 3 years

```
