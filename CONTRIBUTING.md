# Contributing

Repository: https://github.com/jobonaf/aq-framework-snpa

Questo documento spiega come contribuire al repository in modo semplice e chiaro.
È pensato per persone e per sistemi LLM.

---

## Segnala un bug o un problema

Se trovi un errore, un contenuto confuso o una regola sbagliata, apri un issue.
Non serve sapere come correggere il problema: basta raccontare cosa hai visto.

- una segnalazione chiara è già molto utile
- indica sempre il file o la pagina interessata
- se possibile, aggiungi un estratto del testo o lo screenshot

---

## Come aprire un issue

1. Vai a: https://github.com/jobonaf/aq-framework-snpa/issues/new
2. Scrivi un titolo breve e specifico.
3. Spiega:
   - cosa hai visto
   - dove lo hai trovato
   - cosa ti aspettavi
4. Se possibile, aggiungi:
   - il percorso del file (`docs/...`)
   - un estratto di testo
   - uno screenshot

### Esempio

- Titolo: `Bug in docs/modules/assess.md`
- Descrizione: `La formula della soglia sembra errata; mi aspettavo una soglia annuale, non giornaliera.`

---

## Struttura del repository

La documentazione è organizzata così:

```
docs/
  modules/        # regole e logica dei moduli
  tables/         # parametri normativi e soglie
  introduction.md
  architecture.md
  TODO.md
  CONTRIBUTING.md
```

- I **moduli** contengono regole operative.
- Le **tabelle** contengono solo dati normativi.
- Non mischiare regole e dati nello stesso file.

---

## Regole di base

- Testo descrittivo: **italiano**
- Nomi di moduli, variabili e funzioni: **inglese**
- Identificatori dei requisiti: **inglese**
- Vocabolari EIONET: **invariati**
- Cerca sempre se esiste già un issue o un requisito simile.

---

## Requisiti (REQ)

Ogni regola importante viene descritta con un blocco `REQ`.

Formato:

```
REQ-{MODULO}-{SLOT}
```

Dove:

- `{MODULO}` è il nome del modulo (ASSESS, REPR, NETWORK, …)
- `{SLOT}` è una parola breve e descrittiva, permanente

Esempi:

- REQ-ASSESS-CLASSIFICATION
- REQ-ASSESS-THRESHOLD_COUNT
- REQ-REPR-TOLERANCE_INTERVAL
- REQ-NETWORK-MIN_STATIONS

Regole principali:

- l’identificatore è permanente
- non rinominare requisiti esistenti
- non usare numeri sequenziali come un ordine logico
- nuovi requisiti devono usare un nuovo identificatore descrittivo

---

## Formato del requisito

Ogni regola deve contenere:

- identifier
- source
- status
- type
- dependencies
- rule
- acceptance criterion
- pseudocode

### Template

#### REQ-{MODULO}-{SLOT}

- Fonte: Art. X Dir. 2024/2881 [+ Allegato Y]
- Stato: STABLE / DRAFT / AMBIGUOUS / PENDING
- Tipo: obbligatorio / raccomandato / facoltativo
- Dipendenze: REQ-XXX-YYY, tables/ZZZ

**Regola**
Descrizione breve e chiara della regola.

**Criterio di accettazione**
Dato [condizione], il sistema [comportamento atteso].

**Pseudocode**
Descrittivo, non eseguibile.

---

## Note per LLM e revisori

- Il blocco `REQ` è l’unità minima.
- Non dividere un requisito su più file.
- Non cambiare la fonte senza verificarla.
- Per un requisito `PENDING`, non completare la logica.
- Prima di aggiungere un requisito, verifica che non esista già.

