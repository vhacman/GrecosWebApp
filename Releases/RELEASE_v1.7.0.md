# Release v1.7.0 — 14-03-2026

## Popup Chiusura con Messaggi Dinamici

---

### Popup chiusura prenotazioni e asporto — messaggi dinamici

I popup mostrati nella homepage quando le prenotazioni o l'asporto sono chiusi per la serata corrente ora leggono il testo direttamente dalla configurazione admin, invece di mostrare un messaggio predefinito fisso.

**Prima:** il popup mostrava sempre lo stesso testo hardcoded ("Prenotazioni chiuse per stasera" / "Asporto chiuso per stasera").

**Adesso:** il testo del popup rispecchia in tempo reale l'eventuale messaggio personalizzato impostato dall'admin tramite la sezione *Messaggio del giorno* in Impostazioni. Se non è impostato alcun messaggio personalizzato, il fallback rimane il testo standard.

Questo permette all'admin di comunicare ai clienti informazioni specifiche sulla serata (es. "Serata privata", "Chiusura anticipata per evento"), visibili direttamente nella homepage senza dover modificare il codice.
