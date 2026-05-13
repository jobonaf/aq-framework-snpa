# AQ Framework SNPA

Formal Compliance Framework for Ambient Air Quality  
Directive (EU) 2024/2881

---

## Descrizione

Questo repository contiene una **specifica tecnica e formale** per l’implementazione della Direttiva (UE) 2024/2881 sulla qualità dell’aria ambiente.

Il framework traduce i requisiti normativi in:
- moduli logici (M_*)
- tabelle normative
- relazioni esplicite tra componenti

con l’obiettivo di supportare:
- la verifica automatica della conformità
- lo sviluppo di sistemi software (inclusi LLM)
- l’armonizzazione tecnica all’interno del SNPA
- l’interoperabilità con AQD / EIONET

---

## Struttura del repository

La documentazione è organizzata in tre livelli principali:

### Moduli logici (`docs/modules/`)
Descrivono la logica operativa e i processi decisionali della Direttiva:
- **M_ZONE** — Suddivisione territoriale e caratterizzazione
- **M_ASSESS** — Regime di valutazione (fixed, model-based, combined)
- **M_NETWORK** — Rete di monitoraggio (adequacy e placement)
- **M_MOD** — Applicazioni modellistiche
- **M_MODEL_QA** — Garanzia qualità dei modelli
- **M_REPR** — Rappresentatività spaziale
- **M_LIMITS** — Conformità ai valori limite
- **M_EXPOSURE** — Indicatore di esposizione media
- **M_DATA_QUALITY** — Qualità dei dati (transversale)

### Tabelle normative (`docs/tables/`)
Contengono i parametri, le soglie e i vocabolari richiesti dalla Direttiva:
- **Valutazione**: soglie di valutazione, soglie di allarme
- **Monitoraggio**: numero minimo stazioni, criteri di posizionamento, obblighi avanzati
- **Qualità e conformità**: valori limite, qualità dati, tolleranze di rappresentatività
- **Esposizione**: obblighi di riduzione dell'esposizione, eventi naturali eccezionali
- **Interoperabilità**: vocabolari EIONET, parametri supersiti

### Documentazione contestuale
- Introduzione e architettura
- TODO e roadmap
- Linee guida per contribuire

La navigazione completa è disponibile su **[Read the Docs / MkDocs](https://readthedocs.org)**.

---

## Stato del progetto

- Versione: **v0.1 (prototipo tecnico)**
- Ambito: SNPA
- Stato normativo:
  - Direttiva (UE) 2024/2881
  - Implementing acts: **parzialmente integrati (draft)**

Il framework è **funzionalmente completo** per:
- valutazione della qualità dell’aria
- integrazione misure–modellistica
- verifica dei valori limite

Alcuni aspetti della Direttiva non sono ancora formalizzati (vedi TODO).

---

## Destinatari

- tecnici e analisti qualità dell’aria
- sviluppatori software
- data scientist ambientali
- gruppi di lavoro SNPA

---

## Nota

Questo repository **non è un documento giuridico**, ma una **specifica tecnica**  
pensata per rendere la Direttiva implementabile e verificabile in modo coerente.
