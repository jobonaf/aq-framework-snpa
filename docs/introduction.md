# Introduzione

## Scopo

Questo documento definisce una **specifica tecnica formale** per la Direttiva (UE) 2024/2881 sulla qualità dell’aria ambiente.

Il repository è disponibile su GitHub: https://github.com/jobonaf/aq-framework-snpa

L’obiettivo è tradurre i requisiti normativi in una struttura:

- esplicita
- formale
- implementabile

---

## Contesto

La Direttiva 2024/2881 introduce:

- maggiore integrazione tra misure e modellistica
- centralità della rappresentatività spaziale
- requisiti più stringenti per la validazione dei modelli
- nuovi valori limite (orizzonte 2030)

Questi elementi richiedono una formalizzazione per:

- garantire coerenza applicativa
- supportare automazione e verifica

---

## Approccio

Il framework adotta un’architettura:

### Modulare

Ogni componente è rappresentato da un modulo M_*:

- M_NETWORK → rete
- M_MOD → modellistica
- M_REPR → rappresentatività
- M_LIMITS → conformità

---

### Separazione logica e dati

- moduli → regole e logica
- tabelle → parametri normativi

---

### Formalizzazione

Le regole sono espresse tramite:

- pseudocodice
- condizioni logiche
- relazioni tra variabili

---

## Ruolo della modellistica

La modellistica assume un ruolo centrale:

- supporta la distribuzione spaziale
- integra le misure
- abilita la rappresentatività
- contribuisce alla valutazione della conformità

---

## Ruolo della rappresentatività spaziale

La rappresentatività spaziale:

- connette misure e territorio
- determina validità dei superamenti
- guida la progettazione della rete

---

## Panoramica dei contenuti

### Moduli logici
I 9 moduli M_* descrivono il flusso logico di decisione:

1. **M_ZONE** — Classificazione territoriale (urbana, rurale, di fondo)
2. **M_ASSESS** — Scelta del regime di valutazione (fisso, modellistico, misto)
3. **M_NETWORK** — Progettazione della rete di monitoraggio
4. **M_MOD** — Impostazione delle applicazioni modellistiche
5. **M_MODEL_QA** — Validazione e verifica di qualità dei modelli
6. **M_REPR** — Calcolo della rappresentatività spaziale
7. **M_LIMITS** — Verifica della conformità ai valori limite
8. **M_EXPOSURE** — Calcolo dell'indicatore di esposizione media
9. **M_DATA_QUALITY** (trasversale) — Qualità e completezza dei dati

### Tabelle normative
Le 12 tabelle T_* contengono i parametri direttivamente prescritti:

- **T_ASSESS_THRESHOLDS** — Soglie di valutazione per regime
- **T_ALERT_THRESHOLDS** — Soglie di allarme e informazione
- **T_LIMIT_VALUES** — Valori limite per inquinanti
- **T_MIN_STATIONS** — Numero minimo stazioni per zone
- **T_SITING** — Criteri di posizionamento stazioni
- **T_ADVANCED_MONITORING** — Obblighi di monitoraggio rafforzato
- **T_DATA_QUALITY** — Requisiti di completezza e validità
- **T_REPR_TOLERANCE** — Tolleranze di rappresentatività spaziale
- **T_EXPOSURE_OBLIGATIONS** — Obblighi di riduzione dell'esposizione
- **T_NATURAL_EVENTS** — Tipi e criteri di eventi naturali eccezionali
- **T_SUPERSITES** — Parametri speciali per supersiti
- **T_EIONET** — Vocabolari e standard interoperativi

La presente versione:

- incorpora elementi da atti di esecuzione (bozza)
- è soggetta a revisione

---

## Destinatari

- enti SNPA
- sviluppatori
- data scientist ambientali
