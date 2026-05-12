# TODO — Aspetti normativi da integrare

Questo file elenca gli elementi previsti dalla Direttiva (UE) 2024/2881
che **non sono ancora formalizzati** nella documentazione corrente.

L’elenco serve a:

- guidare le prossime estensioni del framework
- mantenere tracciabilità rispetto alla Direttiva
- evitare lacune concettuali

---

## 1. Piani per la qualità dell’aria (Air Quality Plans)

**Riferimenti normativi**

- Art. 17 Directive (EU) 2024/2881

**Stato**

- ❌ NON documentato

**Descrizione**
La Direttiva richiede l’elaborazione di piani per la qualità dell’aria
in caso di superamento dei valori limite.

**Possibile modulo futuro**

- `M_AIR_PLAN`

**Contenuti da modellare**

- condizioni di attivazione del piano
- relazione con superamenti (M_LIMITS)
- misure strutturali vs temporanee
- monitoraggio dell’efficacia

---

## 2. Roadmap e traiettorie di riduzione

**Riferimenti normativi**

- Art. 17, Art. 19
- Obiettivi intermedi verso il 2030

**Stato**

- ❌ NON documentato

**Descrizione**
La Direttiva introduce una logica di percorso verso la conformità,
non solo uno stato statico compliant/non‑compliant.

**Possibile modulo futuro**

- `M_ROADMAP`

**Contenuti da modellare**

- traiettorie temporali
- milestones
- indicatori di progresso
- coerenza con AEI (M_EXPOSURE)

---

## 3. Deroghe e proroghe temporali

**Riferimenti normativi**

- Art. 18 Directive (EU) 2024/2881

**Stato**

- ⚠️ PARZIALMENTE citato in M_LIMITS
- ❌ NON formalizzato come processo

**Descrizione**
La Direttiva consente proroghe temporanee in condizioni specifiche.

**Possibile modulo futuro**

- `M_DEROGATION`

**Contenuti da modellare**

- criteri di ammissibilità
- durata
- obblighi associati
- interazione con piani aria

---

## 4. Comunicazione e informazione al pubblico

**Riferimenti normativi**

- Art. 20–21 Directive (EU) 2024/2881

**Stato**

- ❌ NON documentato

**Descrizione**
Obblighi di informazione su qualità dell’aria, superamenti e rischi.

**Possibile modulo futuro**

- `M_PUBLIC_INFO`

**Contenuti da modellare**

- trigger di comunicazione
- canali
- contenuti minimi
- tempistiche

---

## 5. Reporting e flussi ufficiali verso la Commissione

**Riferimenti normativi**

- Art. 27
- Decisioni di reporting AQD

**Stato**

- ⚠️ IMPLICITO (vocabolari EIONET)
- ❌ NON formalizzato come processo

**Possibile modulo futuro**

- `M_REPORTING`

**Contenuti da modellare**

- dataset richiesti
- periodicità
- coerenza con EIONET
- validazioni pre‑invio

---

## 6. Sanzioni e misure correttive

**Riferimenti normativi**

- Art. 28

**Stato**

- ❌ NON documentato

**Nota**
Aspetto giuridico più che tecnico, ma con potenziali trigger automatici
a partire dallo stato di non‑conformità.

---

## 7. Aggiornamento dinamico degli implementing acts

**Riferimenti normativi**

- Art. 8(7)
- Art. 9(8)

**Stato**

- ⚠️ PARZIALMENTE integrato (draft)

**Descrizione**
Il framework deve poter assorbire aggiornamenti tecnici futuri
senza ristrutturazioni profonde.

**Azione futura**

- versioning delle tabelle
- tracciabilità delle modifiche

---

## 8. Soglie di valutazione (Assessment Thresholds)

**Riferimenti normativi**

- Art. 8 Directive (EU) 2024/2881
- Annex II (assessment thresholds)

**Stato**

- ⚠️ In corso (tabella creata, valori da definire)
- ⚠️ Citazione implicita in M_ASSESS

**Descrizione**
La Direttiva prevede soglie di valutazione per classificare le zone e determinare
il regime di valutazione (misure fisse, modellistica, stime oggettive).

Tali soglie:

- non coincidono con i valori limite
- sono utilizzate esclusivamente per la scelta dei metodi di assessment
- possono variare per inquinante e metrica

**Azione necessaria**

- introdurre una tabella dedicata: `tables/assess_thresholds.md`

**Possibile contenuto della tabella**

- inquinante
- metrica (annuale, giornaliera, oraria, 8h)
- soglia di valutazione
- riferimento normativo

**Moduli impattati**

- M_ASSESS (diretto)
- M_NETWORK (numero minimo di stazioni)
- M_MOD (uso primario/secondario della modellistica)

---

## 9. Soglie di informazione e di allerta

**Riferimenti normativi**

- Art. 19 Directive (EU) 2024/2881
- Annex I (specifico per O₃ e altri inquinanti)

**Stato**

- ❌ NON documentate

**Descrizione**
La Direttiva prevede soglie di:

- informazione
- allerta

attivate in caso di concentrazioni elevate, indipendentemente dalla conformità ai
valori limite.

Queste soglie sono rilevanti per:

- comunicazione al pubblico
- misure temporanee
- gestione degli episodi acuti

**Azione necessaria**

- decidere se includerle nel framework v0.2
- eventuale tabella: `tables/alert_thresholds.md`

**Possibili moduli futuri**

- M_PUBLIC_INFO
- M_EPISODES (eventi acuti)

---

## 10. Soglie e criteri per eventi naturali ed eccezionali

**Riferimenti normativi**

- Art. 16 Directive (EU) 2024/2881
- Linee guida Commissione (polveri sahariane, eventi naturali)

**Stato**

- ⚠️ Citati in M_LIMITS
- ❌ NON strutturati

**Descrizione**
La Direttiva consente l’esclusione di alcuni superamenti dal calcolo della
conformità quando attribuibili a:

- sorgenti naturali
- eventi eccezionali

Mancano:

- criteri quantitativi
- soglie operative
- strutturazione formale del processo

**Azione necessaria**

- valutare una tabella concettuale: `tables/natural_events.md`

**Moduli impattati**

- M_LIMITS
- futuri moduli su piani aria e deroghe

---

## 11. Soglie per obblighi rafforzati (monitoraggio avanzato)

**Riferimenti normativi**

- Art. 9
- Art. 10
- riferimenti indiretti ai supersiti

**Stato**

- ❌ NON documentate

**Descrizione**
La Direttiva introduce obblighi rafforzati di monitoraggio in presenza di:

- concentrazioni elevate persistenti
- contesti emissivi complessi
- popolazione esposta significativa

Questi obblighi non sono sempre legati a valori limite,
ma a combinazioni di fattori.

**Azione necessaria**

- chiarire se formalizzare: soglie e criteri qualitativi
- possibile estensione del framework: M_SUPERSITE e M_ADVANCED_MONITORING