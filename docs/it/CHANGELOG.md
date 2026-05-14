# Changelog

Questo progetto segue una versione semantica orientata alla maturità normativa
del framework (non al software eseguibile).

---

## [Non rilasciato]
_Data: 2026-05-14_

### 🔵 Added

- Creato glossario YAML (`glossary/glossary.yaml`) per supportare la traduzione automatica e garantire coerenza terminologica tra moduli e tabelle.

---

## [v0.2] — Specifica tecnica formale del core normativo
_Data: 2026-05-13_

### 🔵 Added

#### Formalizzazione dei requisiti (REQ)
- Introdotta la formalizzazione sistematica dei requisiti normativi (REQ)
  in tutti i moduli core, secondo la sintassi definita in `CONTRIBUTING.md`.
- Adozione di identificatori **slot-based** (`REQ-{MODULO}-{SLOT}`),
  stabili e non ordinali.

#### Copertura completa degli Allegati della Direttiva
- Copertura strutturata e completa dei seguenti allegati:
  - Allegato I — Valori limite e valori-obiettivo
  - Allegato II — Soglie di valutazione
  - Allegato III — Numero minimo di punti di campionamento
  - Allegato IV — Criteri di localizzazione e uso combinato
  - Allegato V — Qualità dei dati e modellistica
  - Allegato VII — Supersiti di monitoraggio

#### Nuove tabelle normative
- `T_ASSESS_THRESHOLDS` rivista e allineata all’Allegato II.
- `T_LIMIT_VALUES` riscritta integralmente sulla base dell’Allegato I.
- `T_MIN_STATIONS` unificata in un unico file (Allegato III completo).
- `T_DATA_QUALITY` rivista con distinzione tra:
  - lungo termine / breve termine
  - misure fisse / indicative / modellistica
- `T_SUPERSITES` rivista e allineata integralmente all’Allegato VII.

---

### 🔵 Changed

#### Architettura del framework
- Separazione netta e sistematica tra:
  - **dati normativi** (tabelle)
  - **logica normativa** (moduli)
- Eliminazione di qualsiasi valore numerico hard-coded nei moduli.
- Chiarimento esplicito delle responsabilità tra i moduli
  (ASSESS, NETWORK, REPR, MOD, LIMITS).

#### Moduli completamente rifattorizzati con REQ
- `M_ASSESS`
  - formalizzazione delle regole di classificazione sopra/sotto soglia
  - definizione esplicita del regime di valutazione
- `M_NETWORK`
  - formalizzazione di tutti gli obblighi dell’Allegato III
  - composizione minima della rete, riduzione 50 %, UFP, ozono rurale
- `M_LIMITS`
  - formalizzazione completa della verifica di conformità
  - gestione fonti naturali, eventi eccezionali e deroghe
- `M_DATA_QUALITY`
  - formalizzazione dei criteri di validità dei dati
  - esclusioni esplicite (AOT40, AEI, soglie di allarme)
- `M_MODEL_QA`
  - definizione formale dell’indicatore MQI
  - criterio MQI ≤ 1
  - criterio del 90 % dei punti di campionamento
- `M_REPR`
  - formalizzazione delle regole di rappresentatività spaziale
  - gestione dei casi critici e obbligo di giudizio esperto
- `M_MOD`
  - formalizzazione dell’uso regolatorio della modellistica
  - gestione dei conflitti misura–modello

---

### 🔵 Fixed

- Rimozione di ambiguità concettuali tra:
  - soglie di valutazione vs valori limite
  - assessment vs compliance
  - rappresentatività vs conformità
- Eliminazione di riferimenti impliciti o non tracciabili alla normativa.
- Chiarimento del ruolo della modellistica in tutte le fasi del framework.

---

### ⚠️ Known limitations / Out of scope

- Non inclusi in questa versione:
  - piani per la qualità dell’aria (Art. 17)
  - roadmap e traiettorie di riduzione
  - governance istituzionale e QA/QC di laboratorio
  - reporting ufficiale verso la Commissione
- Questi aspetti sono esplicitamente rinviati a versioni successive (v0.3+).

---

### ✅ Note di rilascio

La versione **v0.2** rappresenta il **completamento del core normativo‑tecnico**
del framework, con una formalizzazione completa e verificabile dei requisiti
della Direttiva (UE) 2024/2881.

È la prima versione:
- completamente REQ‑based
- audit‑ready
- coerente per uso SNPA e per supporto LLM‑assisted