# AQ Framework — SNPA

## Framework formale di conformità per la qualità dell’aria ambiente

Direttiva (UE) 2024/2881

---

## Panoramica

Questo progetto definisce una **specifica formale e computabile** per l’attuazione della Direttiva (UE) 2024/2881 sulla qualità dell’aria ambiente.

Il framework traduce i requisiti regolatori in:

- moduli logici (`M_*`);
- tabelle normative (`T_*`);
- output strutturati;
- dipendenze tra componenti di valutazione, monitoraggio, modellizzazione, conformità, pianificazione, informazione al pubblico e rendicontazione.

Il framework è destinato a supportare:

- la verifica automatica o semi-automatica della conformità;
- l’interpretazione coerente della Direttiva;
- l’implementazione e la validazione software;
- l’auditabilità e la tracciabilità delle decisioni regolatorie;
- l’interoperabilità con i sistemi di rendicontazione e scambio dati sulla qualità dell’aria.

---

## Modello architetturale

Il framework è organizzato come **sistema decisionale regolatorio stratificato**, non come una singola pipeline lineare.

Livelli principali:

- contesto territoriale;
- regime di valutazione;
- monitoraggio e qualità dei dati;
- modellizzazione e rappresentatività spaziale;
- valutazione della conformità e dell’esposizione;
- attribuzione delle fonti;
- pianificazione e proroga del termine di conseguimento;
- coordinamento transfrontaliero;
- informazione al pubblico e rendicontazione.

Vedi:

- [Architettura del framework](architecture.md)

---

## Struttura del framework

Il sistema è organizzato in:

### Moduli

I moduli definiscono la logica operativa e regolatoria:

- condizioni;
- dipendenze;
- flussi decisionali;
- passaggi di validazione;
- confini dell’effetto giuridico;
- output strutturati.

### Tabelle

Le tabelle definiscono parametri regolatori:

- soglie;
- valori;
- numeri minimi;
- obiettivi di qualità dei dati;
- tolleranze;
- obblighi;
- categorie di fonti;
- schemi di rendicontazione.

L’indice seguente contiene link solo ai file tabellari attualmente presenti nel repository. Le tabelle previste sono elencate separatamente, senza link.

---

## Navigazione

### Architettura e documentazione principale

- [Introduzione](introduction.md)
- [Architettura del framework](architecture.md)
- [Changelog](CHANGELOG.md)
- [TODO](TODO.md)

---

## Moduli

### Livello territoriale e di valutazione

- [M_ZONE — Suddivisione territoriale e domini di valutazione](modules/zone.md)
- [M_ASSESS — Regime di valutazione](modules/assess.md)

### Livello di monitoraggio, dati e validazione

- [M_NETWORK — Rete di monitoraggio](modules/network.md)
- [M_DATA_QUALITY — Qualità dei dati e validità dei dati di valutazione](modules/data_quality.md)
- [M_MODEL_QA — Validazione e assicurazione della qualità delle applicazioni di modellizzazione](modules/model_qa.md)

### Livello di modellizzazione e spaziale

- [M_MOD — Applicazioni di modellizzazione](modules/modelling.md)
- [M_REPR — Rappresentatività spaziale dei punti di campionamento](modules/repr.md)

### Livello di conformità ed esposizione

- [M_LIMITS — Verifica della conformità e dei superamenti](modules/limits.md)
- [M_EXPOSURE — Indicatore di esposizione media e obblighi di esposizione](modules/exposure.md)

### Livello di attribuzione delle fonti e qualificazione giuridica

- [M_SOURCE_ATTRIBUTION — Attribuzione delle fonti e qualificazione dei contributi](modules/source_attribution.md)

### Livello di pianificazione e conseguimento

- [M_PLANS — Piani per la qualità dell’aria, tabelle di marcia e piani d’azione a breve termine](modules/plans.md)
- [M_ATTAINMENT_EXTENSION — Proroga dei termini di conseguimento](modules/attainment_extension.md)

### Livello di coordinamento e output

- [M_TRANSBOUNDARY — Cooperazione e coordinamento per l’inquinamento atmosferico transfrontaliero](modules/transboundary.md)
- [M_PUBLIC_INFORMATION — Informazione al pubblico e comunicazione](modules/public_information.md)
- [M_REPORTING — Rendicontazione regolatoria e scambio dati](modules/reporting.md)

---

## Tabelle esistenti

- [T_ADVANCED_MONITORING — Obblighi di monitoraggio avanzato](tables/t_advanced_monitoring.md)
- [T_ALERT_THRESHOLDS — Soglie di allarme e soglie di informazione](tables/t_alert_thresholds.md)
- [T_ASSESS_THRESHOLDS — Soglie di valutazione](tables/t_assess_thresholds.md)
- [T_DATA_QUALITY — Obiettivi di qualità dei dati](tables/t_data_quality.md)
- [T_EIONET — Vocabolari EIONET e standard di interoperabilità](tables/t_eionet.md)
- [T_EXPOSURE_OBLIGATIONS — Obblighi e obiettivi di esposizione](tables/t_exposure_obligations.md)
- [T_LIMIT_VALUES — Valori limite e valori-obiettivo](tables/t_limit_values.md)
- [T_MIN_STATIONS — Numero minimo di punti di campionamento](tables/t_min_stations.md)
- [T_NATURAL_EVENTS — Eventi naturali e criteri per fonti naturali](tables/t_natural_events.md)
- [T_REPR_TOLERANCE — Tolleranze di rappresentatività spaziale](tables/t_repr_tolerance.md)
- [T_SITING — Criteri di ubicazione](tables/t_siting.md)
- [T_SUPERSITES — Parametri dei supersiti](tables/t_supersites.md)

---

## Tabelle previste — non ancora implementate

Le tabelle seguenti sono richiamate dall’architettura logica o dalle dipendenze dei moduli, ma i file corrispondenti non sono ancora stati creati. Sono intenzionalmente elencate senza link per evitare navigazione non valida.

- T_MODEL_QA — Parametri di assicurazione della qualità dei modelli
- T_SOURCE_CATEGORIES — Categorie di fonti
- T_ATTRIBUTION_METHODS — Metodi di attribuzione delle fonti
- T_NUTS — Riferimenti territoriali NUTS
- T_REPORTING_SCHEMA — Schemi di rendicontazione

---

## Output principali dei moduli

Il framework produce un insieme di output strutturati, tra cui:

```text
territorial_context
assessment_status
network_status
data_quality_status
model_validation
model_output
representativeness_status
compliance_result
exposure_status
source_attribution_status
plan_status
attainment_extension
transboundary_status
public_information
reporting_package
```

---

## Stato attuale del progetto

Stato: **v0.3 — specifica tecnica formale estesa**

Questa versione include:

- moduli core di valutazione e monitoraggio;
- modellizzazione, QA dei modelli e rappresentatività spaziale;
- valutazione della conformità e dell’esposizione;
- attribuzione delle fonti;
- piani, tabelle di marcia e proroga del termine di conseguimento;
- coordinamento transfrontaliero;
- informazione al pubblico;
- rendicontazione regolatoria.

Il framework è ancora soggetto a:

- verifica rispetto al testo consolidato della Gazzetta ufficiale;
- affinamento delle tabelle normative esistenti (`T_*`);
- creazione delle tabelle previste, ove necessario;
- allineamento agli atti implementativi e ai formati di scambio dati;
- validazione tramite casi d’uso pratici.

---

## Destinatari

- esperti di qualità dell’aria;
- soggetti SNPA;
- agenzie ambientali;
- sviluppatori software;
- analisti di dati ambientali;
- analisti regolatori;
- progettisti di sistemi di supporto alle decisioni.

---

## Nota di implementazione

L’ordine di implementazione raccomandato è:

- fondazioni territoriali e tabellari;
- moduli di valutazione, monitoraggio e qualità dei dati;
- modellizzazione, QA dei modelli e rappresentatività;
- conformità ed esposizione;
- attribuzione delle fonti;
- piani e proroghe dei termini di conseguimento;
- coordinamento transfrontaliero;
- informazione al pubblico e rendicontazione.

---

## Note

Il framework è progettato per essere:

- esplicito;
- formale;
- modulare;
- auditabile;
- orientato all’automazione;
- compatibile con motori di regole tradizionali;
- compatibile con workflow assistiti da LLM;
- interoperabile con i sistemi ufficiali sui dati di qualità dell’aria.
