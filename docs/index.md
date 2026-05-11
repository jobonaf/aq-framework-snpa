# AQ Framework — SNPA

## Framework di conformità formale per la qualità dell’aria ambiente  
Direttiva (UE) 2024/2881

---

## Panoramica

Questo progetto definisce una **specifica formale e computazionale** per l’implementazione della Direttiva (UE) 2024/2881 sulla qualità dell’aria.

Il framework traduce i requisiti normativi in:

- moduli logici (M_*)
- tabelle normative
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

## Pipeline logica

```

ZONE → ASSESS → NETWORK → MODEL → MODEL\_QA → REPR → LIMITS → EXPOSURE

```

---

## Navigazione

### Moduli
- Zoning
- Assessment
- Rete di monitoraggio
- Modellistica
- Garanzia della qualità del modello
- Rappresentatività spaziale
- Valori limite
- Esposizione
- Qualità dei dati

### Tabelle
- Valori limite
- Numero stazioni
- Qualità dati
- Tolleranze rappresentatività
- Vocabolari EIONET

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
