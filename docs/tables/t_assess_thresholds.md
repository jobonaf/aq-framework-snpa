# T_ASSESS_THRESHOLDS — Soglie di valutazione (Allegato II)

Fonte: Direttiva (UE) 2024/2881, Allegato II.

Usata da: [M_ASSESS](../modules/assess.md)

---

## Soglie di valutazione

La soglia di valutazione è l'unica soglia prevista dalla Dir. 2024/2881 per ciascun
inquinante e metrica (a differenza della Dir. 2008/50, che prevedeva una soglia
superiore e una inferiore). Una zona si classifica **sopra soglia** se la
concentrazione ha superato la soglia in almeno 3 dei 5 anni precedenti.

| Inquinante | Metrica (`reportingMetric`) | Soglia [µg/m³] | % del VL 2030 |
|---|---|---|---|
| PM2.5 | `P1Y` — media annuale | 10 | 100% |
| PM2.5 | `P1D` — media giornaliera | 15 | 60% |
| PM10 | `P1Y` — media annuale | 15 | 75% |
| PM10 | `P1D` — media giornaliera | 30 | 67% |
| NO2 | `P1Y` — media annuale | 10 | 50% |
| NO2 | `P1D` — media giornaliera | 25 | 50% |
| NO2 | `P1H` — media oraria | 100 | 50% |
| SO2 | `P1D` — media giornaliera | 25 | 63% |
| SO2 | `P1H` — media oraria | 150 | 43% |
| O3 | `P8H` — massima media mobile 8h | 100 | 83% |
| BaP | `P1Y` — media annuale [ng/m³] | 0,5 | 50% |
| C6H6 (benzene) | `P1Y` — media annuale | 2 | 59% |
| CO | `P8H` — massima media mobile 8h [µg/m³] | 5.000 | 50% |
| As | `P1Y` — media annuale [ng/m³] | 3,6 | 60% |
| Cd | `P1Y` — media annuale [ng/m³] | 3 | 60% |
| Ni | `P1Y` — media annuale [ng/m³] | 12 | 60% |
| Pb | `P1Y` — media annuale | 0,25 | 50% |

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
