### TODO

#### Allineamento generale del repository

- Verificare tutti i moduli `M_*` riscritti rispetto al testo consolidato della Gazzetta ufficiale
- Allineare i nomi dei file dei moduli e i link di navigazione alla struttura finale del repository
- Sostituire, ove necessario, i nomi provvisori dei file dei moduli con i nomi canonici
- Rimuovere o evitare link a tabelle previste finché i file corrispondenti non esistono
- Controllare tutti i link interni dopo eventuali rinomine o spostamenti di file nel repository
- Controllare tutti i diagrammi Markdown e la sintassi Mermaid dopo l’integrazione dei file
- Decidere se la versione italiana o quella inglese è la versione autorevole del repository
- Aggiornare `CHANGELOG.md` con il passaggio alla specifica tecnica formale estesa v0.3

#### Consolidamento dei moduli

- Rivedere e consolidare `M_LIMITS`
- Rivedere e consolidare `M_PLANS`
- Rivedere e consolidare `M_ATTAINMENT_EXTENSION`
- Rivedere e consolidare `M_PUBLIC_INFORMATION`
- Rivedere e consolidare `M_REPORTING`
- Rivedere e consolidare `M_TRANSBOUNDARY`
- Rivedere e consolidare `M_SOURCE_ATTRIBUTION`
- Rivedere e consolidare `M_EXPOSURE`
- Rivedere e consolidare `M_MODEL_QA`

#### Tabelle esistenti da rivedere

- Rivedere le tabelle esistenti e allinearle all’architettura estesa dei moduli
- Aggiornare `T_LIMIT_VALUES`
- Aggiornare `T_ALERT_THRESHOLDS`
- Aggiornare `T_ASSESS_THRESHOLDS`
- Aggiornare `T_MIN_STATIONS`
- Aggiornare `T_SITING`
- Aggiornare `T_DATA_QUALITY`
- Aggiornare `T_REPR_TOLERANCE`
- Aggiornare `T_EXPOSURE_OBLIGATIONS`
- Aggiornare `T_SUPERSITES`
- Aggiornare `T_NATURAL_EVENTS`
- Aggiornare `T_ADVANCED_MONITORING`
- Aggiornare `T_EIONET`

#### Tabelle previste da creare

- Creare `T_MODEL_QA` quando lo schema della tabella di validazione dei modelli sarà stabile
- Creare `T_SOURCE_CATEGORIES` quando le categorie di fonti saranno finalizzate
- Creare `T_ATTRIBUTION_METHODS` quando i metodi di attribuzione accettati saranno formalizzati
- Creare `T_NUTS` se i riferimenti NUTS saranno gestiti internamente
- Creare `T_REPORTING_SCHEMA` quando i payload di rendicontazione saranno definiti

#### Schemi di output e artefatti implementativi

- Definire schemi canonici di output per tutti gli output dei moduli
- Definire uno schema leggibile da macchina per `assessment_status`
- Definire uno schema leggibile da macchina per `compliance_result`
- Definire uno schema leggibile da macchina per `exposure_status`
- Definire uno schema leggibile da macchina per `reporting_package`

#### Casi di test

- Aggiungere un caso di test per un superamento ordinario
- Aggiungere un caso di test per omissione da fonte naturale
- Aggiungere un caso di test per sabbiatura/salatura invernale
- Aggiungere un caso di test per proroga valida
- Aggiungere un caso di test per mancato rispetto dell’IEM/AEI
- Aggiungere un caso di test per contributo transfrontaliero

#### Test di coerenza

- Testare la coerenza tra `M_ASSESS`, `M_NETWORK`, `M_REPR`, `M_MOD` e `M_LIMITS`
- Testare la coerenza tra `M_LIMITS`, `M_SOURCE_ATTRIBUTION`, `M_PLANS` e `M_ATTAINMENT_EXTENSION`
- Testare la coerenza tra `M_PUBLIC_INFORMATION` e `M_REPORTING`

#### Esempi end-to-end

- Preparare un workflow per un inquinante, una zona e un anno di valutazione
- Preparare un workflow con modellizzazione e rappresentatività
- Preparare un workflow con area modellata di superamento
- Preparare un workflow con calcolo dell’IEM/AEI
- Preparare un workflow con obbligo di riduzione dell’esposizione
- Preparare un workflow con richiesta di proroga ai sensi dell’Art. 18
- Preparare un workflow con contributo transfrontaliero ai sensi dell’Art. 21

#### Dopo la pubblicazione degli atti implementativi ufficiali

Rivedere e aggiornare tutti i riferimenti agli atti implementativi ufficiali nei moduli e nelle tabelle.

##### Valutazione e modellizzazione

- Verificare i requisiti di modellizzazione dell’Art. 8(7)
- Verificare i requisiti di valutazione dell’Art. 8(7)
- Verificare `M_ASSESS` rispetto agli atti implementativi ufficiali
- Verificare `M_MOD` rispetto agli atti implementativi ufficiali
- Verificare `M_REPR` rispetto agli atti implementativi ufficiali
- Verificare `M_MODEL_QA` rispetto agli atti implementativi ufficiali

##### Validazione dei modelli

- Verificare i criteri di validazione dei modelli
- Verificare le regole MQI
- Verificare i requisiti di risoluzione spaziale
- Verificare i requisiti sui metadati di incertezza
- Aggiornare `M_MODEL_QA` ove necessario
- Aggiornare la tabella prevista `T_MODEL_QA` ove necessario

##### Rappresentatività spaziale

- Verificare la metodologia di rappresentatività spaziale
- Verificare le regole di tolleranza della rappresentatività
- Verificare la logica di copertura delle aree modellate di superamento
- Aggiornare `M_REPR` ove necessario
- Aggiornare `T_REPR_TOLERANCE` ove necessario

##### Rete di monitoraggio e misurazioni aggiuntive

- Verificare le regole sulle misurazioni aggiuntive dopo superamenti modellati
- Verificare le condizioni di riduzione della rete
- Verificare i requisiti di valutazione supplementare
- Verificare i requisiti di ubicazione
- Verificare i requisiti di classificazione delle stazioni
- Aggiornare `M_ASSESS` ove necessario
- Aggiornare `M_NETWORK` ove necessario
- Aggiornare `T_MIN_STATIONS` ove necessario
- Aggiornare `T_SITING` ove necessario

##### Qualità dei dati

- Verificare gli obiettivi di qualità dei dati
- Verificare le regole di copertura minima dei dati
- Verificare le regole di incertezza
- Verificare le regole di applicabilità specifica per scopo
- Aggiornare `M_DATA_QUALITY` ove necessario
- Aggiornare `T_DATA_QUALITY` ove necessario

##### Attribuzione delle fonti

- Verificare la metodologia di attribuzione delle fonti naturali
- Verificare i requisiti probatori per le fonti naturali
- Verificare la metodologia di attribuzione per sabbiatura/salatura invernale
- Verificare i requisiti probatori per sabbiatura/salatura invernale
- Aggiornare `M_SOURCE_ATTRIBUTION` ove necessario
- Aggiornare `T_NATURAL_EVENTS` ove necessario

##### Proroga del termine di conseguimento e tabelle di marcia

- Verificare le regole dell’Art. 18 sulle tabelle di marcia
- Verificare le regole dell’Art. 18 sulle proiezioni
- Verificare le regole dell’Art. 18 sulla notifica
- Verificare le regole dell’Art. 18 sulla valutazione
- Verificare le regole sull’obiezione della Commissione
- Aggiornare `M_ATTAINMENT_EXTENSION` ove necessario
- Aggiornare `M_PLANS` ove necessario
- Aggiornare `M_MOD` ove necessario

##### Rendicontazione e informazione al pubblico

- Verificare i formati dei dati di rendicontazione
- Verificare i vocabolari di rendicontazione
- Verificare gli schemi di rendicontazione
- Verificare i flussi di trasmissione
- Verificare i requisiti di informazione al pubblico
- Verificare i requisiti dell’indice di qualità dell’aria
- Aggiornare `M_REPORTING` ove necessario
- Aggiornare `M_PUBLIC_INFORMATION` ove necessario
- Aggiornare `T_EIONET` ove necessario
- Aggiornare la tabella prevista `T_REPORTING_SCHEMA` ove necessario

##### Follow-up a livello di repository

- Aggiornare la lista delle tabelle previste
- Creare eventuali nuove tabelle `T_*` richieste
- Eseguire un controllo di coerenza su tutte le dipendenze dei moduli
- Eseguire un controllo di coerenza su tutti i riferimenti alle tabelle
- Eseguire un controllo di coerenza su tutti i link interni
- Eseguire un controllo di coerenza su tutti i diagrammi Mermaid
