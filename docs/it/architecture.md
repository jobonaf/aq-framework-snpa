# Introduzione

## Scopo

Questo documento introduce l’**AQ Framework — SNPA**, una specifica tecnica formale e computabile per l’attuazione della Direttiva (UE) 2024/2881 sulla qualità dell’aria ambiente.

Il repository è disponibile su GitHub:

<https://github.com/jobonaf/aq-framework-snpa>

Lo scopo del framework è tradurre i requisiti regolatori in una struttura:

- esplicita;
- modulare;
- formale;
- tracciabile;
- implementabile;
- adatta all’automazione e al supporto decisionale.

Il framework è progettato per supportare sia l’interpretazione esperta sia l’implementazione software della Direttiva.

---

## Contesto

La Direttiva (UE) 2024/2881 introduce un approccio più integrato alla valutazione e alla gestione della qualità dell’aria ambiente.

Gli elementi principali che richiedono formalizzazione includono:

- l’integrazione tra misurazioni in siti fissi, misurazioni indicative, applicazioni di modellizzazione e stima obiettiva;
- il ruolo centrale della rappresentatività spaziale;
- requisiti più stringenti e specifici per scopo per la validazione dei modelli;
- l’uso della modellizzazione per distribuzione spaziale, hotspot, previsioni e proiezioni;
- standard di qualità dell’aria aggiornati e obblighi orientati all’orizzonte 2030;
- indicatori di esposizione media e obblighi di riduzione dell’esposizione;
- regole di attribuzione per fonti naturali e sabbiatura/salatura invernale;
- cooperazione per l’inquinamento transfrontaliero;
- piani per la qualità dell’aria, tabelle di marcia e piani d’azione a breve termine;
- informazione al pubblico e rendicontazione regolatoria.

Questi elementi richiedono un framework capace di rappresentare non solo la valutazione tecnica, ma anche le conseguenze giuridiche, i requisiti probatori, i trigger di pianificazione e i flussi di rendicontazione connessi al processo di valutazione.

---

## Approccio architetturale

Il framework adotta una **architettura regolatoria stratificata**.

Non è una semplice pipeline lineare. Al contrario, separa la Direttiva in moduli interoperabili che scambiano output strutturati.

I principali livelli architetturali sono:

- contesto territoriale;
- regime di valutazione;
- monitoraggio e validità dei dati;
- modellizzazione e rappresentatività spaziale;
- valutazione della conformità e dell’esposizione;
- attribuzione delle fonti;
- pianificazione e proroga del termine di conseguimento;
- coordinamento transfrontaliero;
- informazione al pubblico e rendicontazione.

Vedi anche:

- [Architettura del framework](architecture.md)

---

## Principi di progettazione

### Progettazione modulare

Ogni funzione regolatoria è rappresentata da un modulo dedicato `M_*`.

Questa separazione evita di mescolare in un unico componente logiche di valutazione, evidenza, conformità, pianificazione e rendicontazione.

### Separazione logica/dati

Il framework distingue tra:

- **moduli (`M_*`)**, che contengono logica regolatoria, condizioni, dipendenze, confini dell’effetto giuridico e output;
- **tabelle (`T_*`)**, che contengono parametri normativi, soglie, valori, tolleranze e vocabolari controllati.

Nei documenti di navigazione sono collegati solo i file tabellari esistenti. Tabelle aggiuntive previste possono essere richiamate concettualmente nelle dipendenze dei moduli, ma sono elencate separatamente finché i file corrispondenti non vengono creati.

### Validità specifica per scopo

La validità è trattata come specifica per scopo.

Un dataset, un modello o un’area di rappresentatività non sono semplicemente validi o invalidi in senso assoluto. Sono validi rispetto a uno specifico uso regolatorio.

### Confini dell’effetto giuridico

Il framework distingue la produzione di evidenze dalle decisioni giuridiche o istituzionali finali.

Esempi:

- `M_SOURCE_ATTRIBUTION` può identificare un caso di fonte naturale, ma non omette direttamente un superamento ai fini della Direttiva.
- `M_ATTAINMENT_EXTENSION` può determinare se una richiesta di proroga è supportata, ma non concede la proroga.
- `M_REPORTING` rendiconta lo stato di conformità, ma non ricalcola la conformità.

### Tracciabilità e auditabilità

Tutti gli output regolatori dovrebbero essere tracciabili rispetto a versioni territoriali, versioni dei dati, versioni dei modelli, metodi, stato di validazione, periodi di valutazione e pacchetti di rendicontazione.

---

## Ruolo della modellizzazione

La modellizzazione svolge diversi ruoli distinti nel framework.

Può essere utilizzata per:

- supporto alla valutazione;
- distribuzione spaziale delle concentrazioni di inquinanti;
- identificazione degli hotspot;
- delineazione dell’area modellata di superamento;
- analisi della rappresentatività spaziale;
- supporto alla riduzione della rete di monitoraggio;
- supporto alla rilocalizzazione;
- previsioni delle soglie di allarme e di informazione;
- proiezioni per piani, tabelle di marcia e proroghe dei termini di conseguimento;
- valutazione del contributo transfrontaliero;
- supporto all’attribuzione delle fonti.

Poiché questi ruoli hanno conseguenze regolatorie diverse, la modellizzazione è validata tramite `M_MODEL_QA` in funzione dello scopo previsto ed è utilizzata tramite `M_MOD`.

---

## Ruolo della rappresentatività spaziale

La rappresentatività spaziale collega le misurazioni puntuali al territorio.

Determina:

- dove una misurazione in sito fisso è spazialmente valida;
- se un’area modellata di superamento è coperta da misurazioni in siti fissi;
- se può essere necessario monitoraggio aggiuntivo;
- se la riduzione della rete resta spazialmente adeguata;
- se la rilocalizzazione di un punto di campionamento crea una perdita di copertura inaccettabile.

La rappresentatività spaziale è gestita da `M_REPR` e interagisce strettamente con `M_NETWORK`, `M_MOD`, `M_LIMITS` e `M_ASSESS`.

---

## Panoramica dei contenuti

## Moduli logici

Il framework include attualmente i seguenti moduli.

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

## Tabelle normative

Il framework contiene attualmente i seguenti file tabellari:

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

Le tabelle seguenti sono richiamate concettualmente dall’architettura o dalle dipendenze dei moduli, ma i file corrispondenti non esistono ancora:

- T_MODEL_QA — Parametri di assicurazione della qualità dei modelli
- T_SOURCE_CATEGORIES — Categorie di fonti
- T_ATTRIBUTION_METHODS — Metodi di attribuzione delle fonti
- T_NUTS — Riferimenti territoriali NUTS
- T_REPORTING_SCHEMA — Schemi di rendicontazione

---

## Output principali

Il framework produce output strutturati, tra cui:

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

Questi output possono essere consumati da sistemi software, pipeline di rendicontazione, dashboard pubbliche e strumenti regolatori di supporto alle decisioni.

---

## Destinatari

Il framework è destinato a:

- soggetti SNPA;
- esperti di qualità dell’aria;
- agenzie ambientali;
- sviluppatori software;
- data scientist ambientali;
- analisti regolatori;
- team di trasformazione digitale del settore pubblico.

---

## Stato del progetto

Questa versione rappresenta una **specifica tecnica formale estesa**.

Include i moduli tecnici core di valutazione e i moduli downstream per attribuzione delle fonti, piani e tabelle di marcia, proroga del termine di conseguimento, coordinamento transfrontaliero, informazione al pubblico e rendicontazione regolatoria.

Sono necessari ulteriori lavori per:

- completare e validare le tabelle normative esistenti;
- creare le tabelle previste quando diventano necessarie;
- allineare il framework agli atti implementativi e agli schemi di rendicontazione;
- testare il framework su casi pratici;
- affinare gli output dei moduli in schemi leggibili da macchina.

---

## Note

Il framework è progettato per essere implementato progressivamente.

Un’implementazione pratica può partire da dati territoriali, tabelle normative, valutazione, monitoraggio e qualità dei dati, per poi aggiungere progressivamente modellizzazione, rappresentatività, conformità, esposizione, attribuzione delle fonti, pianificazione e rendicontazione.
