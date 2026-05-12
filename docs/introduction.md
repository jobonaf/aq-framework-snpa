# Introduzione

## Scopo

Questo documento definisce una **specifica tecnica formale** per la Direttiva (UE) 2024/2881 sulla qualità dell’aria ambiente.

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

## Stato della specifica

La presente versione:

- è prototipale (v0.1)
- incorpora elementi da atti di esecuzione (bozza)
- è soggetta a revisione

---

## Destinatari

- enti SNPA
- sviluppatori
- data scientist ambientali
