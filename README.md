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

- **Moduli** (`docs/modules/`)  
  Descrivono la logica operativa della Direttiva  
  (rete, modellistica, rappresentatività, limiti, esposizione, qualità dati).

- **Tabelle** (`docs/tables/`)  
  Contengono i parametri normativi:  
  valori limite, soglie, requisiti tecnici, vocabolari.

- **Pagine di contesto**  
  Introduzione e architettura del framework.

La navigazione è gestita tramite Read the Docs / MkDocs.

---

## Architettura logica

Il framework segue una pipeline coerente con la Direttiva.

La struttura logica dei moduli è rappresentata in `docs/architecture.md` tramite un diagramma Mermaid.

M_DATA_QUALITY è un prerequisito trasversale che supporta la validazione dei dati per MODEL_QA, LIMITS ed EXPOSURE.

Ogni modulo ha dipendenze esplicite dagli altri moduli e dalle tabelle normative.

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
