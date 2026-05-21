# Changelog

Questo progetto segue una versione semantica orientata alla maturità normativa del framework, non al software eseguibile.

## [Non rilasciato]

_Nessuna modifica non rilasciata al momento._

## [v0.3] — Specifica tecnica formale estesa

_Data: 2026-05-21_

### 🔵 Added

- Rilasciata la versione **v0.3 — specifica tecnica formale estesa**.
- Estesa la copertura modulare oltre il core normativo v0.2, includendo i moduli relativi a:
  - `M_SOURCE_ATTRIBUTION` — attribuzione delle fonti e qualificazione dei contributi;
  - `M_ATTAINMENT_EXTENSION` — proroga dei termini di conseguimento;
  - `M_PLANS` — piani per la qualità dell’aria, tabelle di marcia e piani d’azione a breve termine;
  - `M_PUBLIC_INFORMATION` — informazione al pubblico e comunicazione;
  - `M_REPORTING` — rendicontazione regolatoria e scambio dati;
  - `M_TRANSBOUNDARY` — cooperazione e coordinamento per l’inquinamento atmosferico transfrontaliero;
  - `M_EXPOSURE` — indicatori di esposizione media e obblighi di esposizione;
  - `M_ZONE` — suddivisione territoriale e domini di valutazione.
- Aggiunti output strutturati per i nuovi domini funzionali, tra cui:
  - `source_attribution_status`;
  - `attainment_extension`;
  - `plan_status`;
  - `public_information`;
  - `reporting_package`;
  - `transboundary_status`;
  - `exposure_status`;
  - `territorial_context`.
- Aggiunta la versione italiana dei moduli e della documentazione principale in `docs/`, con allineamento terminologico tramite glossario.
- Aggiunta una versione inglese dei file repository-facing alla radice del repository, in particolare `README.md` e `CONTRIBUTING.md`.
- Chiarito che `README.md` e `CONTRIBUTING.md` sono rivolti agli utenti che accedono direttamente al repository e non costituiscono contenuto primario per ReadTheDocs.

### 🔵 Changed

- Aggiornato il framework da core normativo a **specifica tecnica formale estesa**, includendo pianificazione, proroghe, attribuzione delle fonti, informazione al pubblico, rendicontazione e cooperazione transfrontaliera.
- Riorganizzata la terminologia italiana/inglese secondo un’impostazione bilingue del repository.
- Aggiornato `CONTRIBUTING.md` per riflettere che il testo descrittivo può essere in **italiano o inglese**, mentre nomi di moduli, variabili, funzioni, identificatori REQ e vocabolari EIONET restano stabili e in inglese ove previsto.
- Aggiornato `README.md` per descrivere lo stato attuale del repository, la copertura modulare estesa e la distinzione tra documentazione in `docs/` e file root del repository.
- Rafforzata la distinzione tra:
  - valutazione tecnica;
  - conformità giuridica;
  - effetti downstream quali piani, proroghe, omissioni per fonti naturali, rendicontazione e informazione al pubblico.
- Allineati i moduli alla terminologia italiana del glossario, mantenendo invariati identificatori tecnici, output machine-readable e nomi `M_*` / `T_*`.

### 🔵 Fixed

- Corrette incoerenze terminologiche tra versioni italiane e inglesi dei moduli.
- Chiarito che la proroga dei termini di conseguimento non è una deroga generale, ma una valutazione condizionata e basata su evidenze.
- Chiarito che l’attribuzione delle fonti produce evidenze e stato di attribuzione, ma non determina direttamente gli effetti giuridici finali.
- Chiarito che la rendicontazione aggrega e trasmette risultati già prodotti dai moduli upstream e non ricalcola la conformità.
- Chiarito che la rappresentatività spaziale non è un semplice buffer geometrico, ma una determinazione regolatoria dipendente da inquinante, metrica, periodo, tipo di stazione e contesto territoriale.

### ⚠️ Known limitations / Out of scope

- Alcune tabelle previste restano da creare o stabilizzare, tra cui:
  - `T_MODEL_QA`;
  - `T_SOURCE_CATEGORIES`;
  - `T_ATTRIBUTION_METHODS`;
  - `T_NUTS`, se i riferimenti NUTS saranno gestiti internamente;
  - `T_REPORTING_SCHEMA`, quando i payload di rendicontazione saranno definiti.
- Gli atti implementativi ufficiali dovranno essere verificati e integrati quando pubblicati o stabilizzati.
- Gli schemi machine-readable completi per tutti gli output dei moduli devono ancora essere formalizzati.
- Restano da completare test di coerenza, casi di test ed esempi end-to-end.

### ✅ Note di rilascio

La versione **v0.3** rappresenta l’estensione del framework dal core normativo-tecnico a una specifica più completa, includendo i domini procedurali e informativi necessari per pianificazione, proroghe, attribuzione delle fonti, informazione al pubblico, rendicontazione e cooperazione transfrontaliera.

## [v0.2] — Specifica tecnica formale del core normativo

_Data: 2026-05-13_

### 🔵 Added

#### Formalizzazione dei requisiti (REQ)

- Introdotta la formalizzazione sistematica dei requisiti normativi (`REQ`) in tutti i moduli core, secondo la sintassi definita in `CONTRIBUTING.md`.
- Adozione di identificatori **slot-based** (`REQ-{MODULO}-{SLOT}`), stabili e non ordinali.

#### Copertura completa degli Allegati della Direttiva

- Copertura strutturata e completa dei seguenti allegati:
  - Allegato I — valori limite e valori-obiettivo;
  - Allegato II — soglie di valutazione;
  - Allegato III — numero minimo di punti di campionamento;
  - Allegato IV — criteri di localizzazione e uso combinato;
  - Allegato V — qualità dei dati e modellistica;
  - Allegato VII — supersiti di monitoraggio.

#### Nuove tabelle normative

- `T_ASSESS_THRESHOLDS` rivista e allineata all’Allegato II.
- `T_LIMIT_VALUES` riscritta integralmente sulla base dell’Allegato I.
- `T_MIN_STATIONS` unificata in un unico file, con copertura dell’Allegato III.
- `T_DATA_QUALITY` rivista con distinzione tra:
  - lungo termine / breve termine;
  - misure fisse / indicative / modellistica.
- `T_SUPERSITES` rivista e allineata integralmente all’Allegato VII.

### 🔵 Changed

#### Architettura del framework

- Separazione netta e sistematica tra:
  - **dati normativi** nelle tabelle;
  - **logica normativa** nei moduli.
- Eliminazione di qualsiasi valore numerico hard-coded nei moduli.
- Chiarimento esplicito delle responsabilità tra i moduli core (`ASSESS`, `NETWORK`, `REPR`, `MOD`, `LIMITS`).

#### Moduli completamente rifattorizzati con REQ

- `M_ASSESS`
  - formalizzazione delle regole di classificazione sopra/sotto soglia;
  - definizione esplicita del regime di valutazione.
- `M_NETWORK`
  - formalizzazione degli obblighi dell’Allegato III;
  - composizione minima della rete, riduzione, UFP e ozono rurale.
- `M_LIMITS`
  - formalizzazione della verifica di conformità;
  - gestione di fonti naturali, eventi eccezionali e qualificazioni particolari dei superamenti.
- `M_DATA_QUALITY`
  - formalizzazione dei criteri di validità dei dati;
  - esclusioni e regole speciali esplicite.
- `M_MODEL_QA`
  - definizione formale dell’indicatore MQI;
  - criterio `MQI <= 1`;
  - criterio del 90% dei punti di campionamento.
- `M_REPR`
  - formalizzazione delle regole di rappresentatività spaziale;
  - gestione dei casi critici e del giudizio esperto.
- `M_MOD`
  - formalizzazione dell’uso regolatorio della modellistica;
  - gestione dei conflitti misura–modello.

### 🔵 Fixed

- Rimozione di ambiguità concettuali tra:
  - soglie di valutazione e valori limite;
  - assessment e compliance;
  - rappresentatività e conformità.
- Eliminazione di riferimenti impliciti o non tracciabili alla normativa.
- Chiarimento del ruolo della modellistica in tutte le fasi del framework.

### ⚠️ Known limitations / Out of scope

- Non inclusi in questa versione:
  - piani per la qualità dell’aria;
  - roadmap e traiettorie di riduzione;
  - governance istituzionale e QA/QC di laboratorio;
  - reporting ufficiale verso la Commissione.
- Questi aspetti sono stati esplicitamente rinviati a versioni successive, a partire dalla versione v0.3.

### ✅ Note di rilascio

La versione **v0.2** rappresenta il completamento del **core normativo-tecnico** del framework, con una formalizzazione completa e verificabile dei requisiti principali della Direttiva (UE) 2024/2881.

È la prima versione:

- completamente REQ-based;
- audit-ready;
- coerente per uso SNPA e supporto LLM-assisted.

## [v0.1] — Impostazione iniziale del framework

_Data: precedente a v0.2_

### 🔵 Added

- Impostazione iniziale della struttura del repository.
- Prima definizione dei moduli core e delle tabelle normative.
- Prima organizzazione della documentazione in moduli, tabelle e contenuti introduttivi.
