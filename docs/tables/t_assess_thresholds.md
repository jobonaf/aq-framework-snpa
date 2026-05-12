# T_ASSESS_THRESHOLDS — Soglie di valutazione (Allegato II)

Fonte: Direttiva (UE) 2024/2881, Allegato II.

Usata da: [M_ASSESS](../modules/assess.md)

---

## Soglie di valutazione

La soglia di valutazione è l'unica soglia prevista dalla Dir. 2024/2881 per ciascun
inquinante e metrica (a differenza della Dir. 2008/50, che prevedeva una soglia
superiore e una inferiore). Una zona si classifica **sopra soglia** se la
concentrazione ha superato la soglia in almeno 3 dei 5 anni precedenti.

| Inquinante | `aq/pollutant/` | Metrica (`reportingMetric`) | Soglia [µg/m³] | % del VL 2030 |
|---|---|---|---|---|
| PM2.5 | /8 | `P1Y` — media annuale | 10 | 100% |
| PM2.5 | /8 | `P1D` — media giornaliera | 15 | 60% |
| PM10 | /38 | `P1Y` — media annuale | 15 | 75% |
| PM10 | /38 | `P1D` — media giornaliera | 30 | 67% |
| NO2 | /8 | `P1Y` — media annuale | 10 | 50% |
| NO2 | /8 | `P1D` — media giornaliera | 25 | 50% |
| NO2 | /8 | `P1H` — media oraria | 100 | 50% |
| SO2 | /1 | `P1D` — media giornaliera | 25 | 63% |
| SO2 | /1 | `P1H` — media oraria | 150 | 43% |
| O3 | /7 | `P8H` — massima media mobile 8h | 100 | 83% |
| BaP | /5029 | `P1Y` — media annuale [ng/m³] | 0,5 | 50% |
| C6H6 (benzene) | /20 | `P1Y` — media annuale | 2 | 59% |
| CO | /10 | `P8H` — massima media mobile 8h [µg/m³] | 5.000 | 50% |
| As | /2 | `P1Y` — media annuale [ng/m³] | 3,6 | 60% |
| Cd | /3 | `P1Y` — media annuale [ng/m³] | 3 | 60% |
| Ni | /30 | `P1Y` — media annuale [ng/m³] | 12 | 60% |
| Pb | /33 | `P1Y` — media annuale | 0,25 | 50% |

---

## Regola di classificazione

```
ABOVE_THRESHOLD(pollutant, zone) =
  COUNT { y ∈ previous_5_years :
    C(pollutant, zone, y) > ASSESS_THRESHOLD(pollutant) } >= 3
```

La verifica si applica separatamente per ciascuna metrica (annuale, giornaliera,
oraria) di ciascun inquinante. È sufficiente che **una** delle metriche sia sopra
soglia per classificare la zona come sopra soglia per quell'inquinante.

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
- La corrispondenza `aq/pollutant/` segue il vocabolario EIONET:
  `http://dd.eionet.europa.eu/vocabulary/aq/pollutant/`
- Per As, Cd, Ni, Pb, BaP la soglia si confronta con il valore obiettivo (TV),
  non con il valore limite (LV), in quanto sono regolati come `aq/objectivetype/TV`.
