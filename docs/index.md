# AQ Framework — SNPA

## Formal Compliance Framework for Ambient Air Quality  
Directive (EU) 2024/2881

---

## Overview

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

ZONE
→ ASSESS
→ NETWORK
→ MODEL
→ MODEL\_QA
→ REPR
→ LIMITS
→ EXPOSURE

```

---

## Navigazione

### Moduli
- Zoning
- Assessment
- Monitoring Network
- Modelling
- Model Quality Assurance
- Spatial Representativeness
- Limit Values
- Exposure
- Data Quality

### Tabelle
- Valori limite
- Numero stazioni
- Qualità dati
- Tolleranze rappresentatività
- Vocabolari EIONET

---

## Stato del progetto

⚠ Versione: **v0.1 (prototipo)**  
⚠ Include elementi da implementing acts (draft 2026)  
⚠ Richiede validazione normativa (GUUE)

---

## Destinatari

- esperti qualità aria (SNPA)
- sviluppatori software
- analisti dati ambientali
