# AQ Framework — SNPA

## Formal Compliance Framework for Ambient Air Quality  
Direttiva (UE) 2024/2881

---

## Panoramica

Questo progetto definisce una **specifica formale e computazionale** per l’implementazione della Direttiva (UE) 2024/2881 sulla qualità dell’aria.

Il framework traduce i requisiti normativi in:

- moduli logici (M_*)
- tabelle normative (T_*)
- relazioni tra componenti

---

## Obiettivi

- supportare la verifica automatica della conformità
- facilitare lo sviluppo software (LLM + sistemi tradizionali)
- garantire coerenza e interoperabilità con AQD / EIONET
- fornire una base tecnica condivisa per SNPA

---

## Struttura del framework

Il sistema è organizzato in:

- **Moduli** → logica operativa (rete, modellistica, limiti)
- **Tabelle** → parametri normativi (valori limite, soglie, requisiti)
- **Allegati** → riferimenti normativi e metodologici

---

## Navigazione

### Moduli
- [M_ZONE — Suddivisione territoriale](modules/zone.md)
- [M_ASSESS — Regime di valutazione](modules/assess.md)
- [M_NETWORK — Rete di monitoraggio](modules/network.md)
- [M_MOD — Applicazioni modellistiche](modules/modelling.md)
- [M_MODEL_QA — Garanzia qualità modelli](modules/model_qa.md)
- [M_REPR — Rappresentatività spaziale](modules/repr.md)
- [M_LIMITS — Conformità ai valori limite](modules/limits.md)
- [M_EXPOSURE — Indicatore di esposizione media](modules/exposure.md)
- [M_DATA_QUALITY — Qualità dei dati](modules/data_quality.md)

### Tabelle
- [T_ASSESS_THRESHOLDS — Soglie di valutazione](tables/t_assess_thresholds.md)
- [T_ALERT_THRESHOLDS — Soglie di allarme](tables/t_alert_thresholds.md)
- [T_LIMIT_VALUES — Valori limite](tables/t_limit_values.md)
- [T_MIN_STATIONS — Numero minimo stazioni](tables/t_min_stations.md)
- [T_SITING — Criteri di posizionamento](tables/t_siting.md)
- [T_ADVANCED_MONITORING — Monitoraggio avanzato](tables/t_advanced_monitoring.md)
- [T_DATA_QUALITY — Qualità dei dati](tables/t_data_quality.md)
- [T_REPR_TOLERANCE — Tolleranze di rappresentatività](tables/t_repr_tolerance.md)
- [T_EXPOSURE_OBLIGATIONS — Riduzione esposizione](tables/t_exposure_obligations.md)
- [T_NATURAL_EVENTS — Eventi naturali](tables/t_natural_events.md)
- [T_SUPERSITES — Parametri supersiti](tables/t_supersites.md)
- [T_EIONET — Vocabolari EIONET](tables/t_eionet.md)

---

## Stato del progetto

⚠ Versione: **v0.1 (prototipo)**  
⚠ Include elementi da atti di esecuzione (bozza 2026)  
⚠ Richiede validazione normativa (GUUE)

---

## Destinatari

- esperti qualità aria (SNPA)
- sviluppatori software
- analisti dati ambientali
