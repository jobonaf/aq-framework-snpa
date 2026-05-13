# T_ASSESS_THRESHOLDS — Soglie di valutazione (Allegato II)

Fonte: Direttiva (UE) 2024/2881, Allegato II.

Usata da: [M_ASSESS](../modules/assess.md)

---

## Soglie di valutazione

| Inquinante | Metrica | Soglia | Unità di misura |
|------------|--------------------------|--------|----------------|
| PM₂.₅     | Media annuale (P1Y)     | 10     | µg/m³          |
| PM₂.₅     | Media giornaliera (P1D) | 15     | µg/m³          |
| PM₁₀      | Media annuale (P1Y)     | 15     | µg/m³          |
| PM₁₀      | Media giornaliera (P1D) | 30     | µg/m³          |
| NO₂       | Media annuale (P1Y)     | 10     | µg/m³          |
| NO₂       | Media giornaliera (P1D) | 25     | µg/m³          |
| NO₂       | Media oraria (P1H)      | 100    | µg/m³          |
| SO₂       | Media giornaliera (P1D) | 25     | µg/m³          |
| SO₂       | Media oraria (P1H)      | 150    | µg/m³          |
| O₃        | Max media mobile 8h (P8H)| 100   | µg/m³          |
| B(a)P     | Media annuale (P1Y)     | 0,5    | ng/m³          |
| Benzene   | Media annuale (P1Y)     | 2      | µg/m³          |
| CO        | Max media mobile 8h (P8H)| 5      | mg/m³          |
| As        | Media annuale (P1Y)     | 3,6    | ng/m³          |
| Cd        | Media annuale (P1Y)     | 3      | ng/m³          |
| Ni        | Media annuale (P1Y)     | 12     | ng/m³          |
| Pb        | Media annuale (P1Y)     | 0,25   | µg/m³          |

---

La classificazione delle zone è definita nel modulo [M_ASSESS](../modules/assess.md) tramite il requisito `REQ-ASSESS-THRESHOLD_CLASSIFICATION`.

---

## Conseguenze della classificazione

| Classificazione | `assessmentType` (EIONET) | Misure fisse | Modellistica |
|---|---|---|---|
| Sopra soglia | `aq/assessmenttype/fixedMeasurements` | Obbligatorie | In supporto |
| Sotto soglia | `aq/assessmenttype/modelOrObjectiveEstimation` | Non obbligatorie | Metodo principale |
| Combinato (sotto soglia + misure indicative) | `aq/assessmenttype/indicativeMeasurements` | No | Sì |

---

## Note

- I valori sono da verificare sul testo ufficiale dell'Allegato II pubblicato in GUUE
  prima dell'implementazione. La colonna "% del VL 2030" è calcolata rispetto ai
  valori limite di [T_LIMIT_VALUES](t_limit_values.md).
- Per gli inquinanti con più metriche (NO2, SO2, PM), la classificazione sopra/sotto
  soglia si valuta per ciascuna metrica indipendentemente; il regime di valutazione
  più restrittivo prevale.
- Per As, Cd, Ni, Pb, BaP la soglia si confronta con il valore obiettivo (TV),
  non con il valore limite (LV), in quanto sono regolati come `aq/objectivetype/TV`.
