# M_PUBLIC_INFORMATION — Informazione al pubblico e comunicazione

## Riferimenti normativi

- Art. 22 Direttiva (UE) 2024/2881 — informazione al pubblico
- Art. 15 Direttiva (UE) 2024/2881 — soglie di allarme e soglie di informazione
- Art. 19 Direttiva (UE) 2024/2881 — piani, disponibilità al pubblico
- Art. 21 Direttiva (UE) 2024/2881 — contesto transfrontaliero, comunicazione
- Art. 23 Direttiva (UE) 2024/2881 — interfacce di rendicontazione
- Allegato I — soglie e standard

## Descrizione

Questo modulo definisce i **requisiti per la comunicazione al pubblico delle informazioni sulla qualità dell’aria**, assicurando che le informazioni fornite siano:

```text
accurate
tempestive
chiare
accessibili
tracciabili
```

Trasforma output tecnici in **informazione destinata al pubblico** per:

- condizioni attuali di qualità dell’aria;
- superamenti delle soglie;
- previsioni e rischi a breve termine;
- implicazioni per la salute;
- contesto delle fonti e influenza transfrontaliera.

Questo modulo non genera misurazioni né valida dati: usa output validati.

## Ambito

Si applica alle informazioni riguardanti:

```text
valori limite
valori-obiettivo
soglie di allarme
soglie di informazione
esposizione media
previsioni e allerte
```

## Definizioni

```text
PUBLIC_OUTPUT =
informazione comunicata al pubblico
```

```text
ALERT_EVENT =
superamento o superamento previsto di una soglia di allarme
```

```text
INFO_EVENT =
superamento o superamento previsto di una soglia di informazione
```

```text
PUBLIC_DATA_READY =
true quando i dati sono validati e idonei alla pubblicazione
```

## Requisiti normativi

### REQ-PUB-DATA_VALIDITY

**Regola**  
Possono essere comunicati solo dati validati.

**Criterio di accettazione**

```text
PUBLIC_DATA_READY = M_DATA_QUALITY.DATA_VALID = true
```

### REQ-PUB-TIMELINESS

**Regola**  
Le informazioni devono essere fornite senza ritardo.

**Criterio di accettazione**

```text
publication_time - detection_time <= acceptable_delay
```

### REQ-PUB-CONTENT

**Regola**  
L’informazione al pubblico deve includere gli elementi chiave.

**Criterio di accettazione**

```text
PUBLIC_OUTPUT includes:
  pollutant
  value
  threshold
  location
  time
```

### REQ-PUB-ALERT

**Regola**  
I superamenti delle soglie di allarme devono essere comunicati immediatamente.

**Criterio di accettazione**

```text
if ALERT_EVENT = true:
    alert_message_published = true
```

### REQ-PUB-INFORMATION_THRESHOLD

**Regola**  
I superamenti delle soglie di informazione devono essere comunicati.

**Criterio di accettazione**

```text
if INFO_EVENT = true:
    information_message_published = true
```

### REQ-PUB-FORECAST

**Regola**  
Le previsioni devono essere comunicate quando rilevanti.

**Criterio di accettazione**

```text
if forecast_available = true:
    forecast_published = true
```

### REQ-PUB-HEALTH_INFO

**Regola**  
Devono essere incluse indicazioni relative alla salute.

**Criterio di accettazione**

```text
health_guidance_included = true
```

### REQ-PUB-SENSITIVE_GROUPS

**Regola**  
L’informazione deve rivolgersi alle popolazioni sensibili.

**Criterio di accettazione**

```text
if vulnerable_groups_present:
    tailored_health_advice = true
```

### REQ-PUB-TRANSPARENCY

**Regola**  
Metodi e fonti dei dati devono essere trasparenti.

**Criterio di accettazione**

```text
data_source_available = true
method_explained = true
```

### REQ-PUB-TRANSBOUNDARY_CONTEXT

**Regola**  
Ove rilevante, l’inquinamento transfrontaliero deve essere comunicato.

**Criterio di accettazione**

```text
if TRANSBOUNDARY_CASE = true:
    source_origin_disclosed = true
```

### REQ-PUB-PLAN_ACCESS

**Regola**  
I piani per la qualità dell’aria e le tabelle di marcia devono essere accessibili al pubblico.

**Criterio di accettazione**

```text
if plan_exists:
    plan_publicly_available = true
```

### REQ-PUB-UPDATE

**Regola**  
Le informazioni devono essere aggiornate quando cambiano le condizioni.

**Criterio di accettazione**

```text
if data_updated:
    public_output_updated = true
```

## Logica decisionale

```text
if data validated:
    publish current levels

if alert or forecast:
    publish alerts immediately

if plans exist:
    publish access

include health info and transparency
```

## Output

```text
public_information:
  pollutant
  value
  threshold
  location
  timestamp
  alerts:
    alert_active
    information_active
  forecast:
    available
    summary
  health:
    guidance
    vulnerable_groups
  context:
    source
    transboundary
  metadata:
    data_source
    method
```

## Note

- Il focus è su chiarezza e accessibilità.
- Il modulo interpreta risultati tecnici per il pubblico.
- Deve essere sincronizzato con dati in tempo reale e previsioni.
