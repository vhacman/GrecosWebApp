# Release v1.4.0 — 13-03-2026

## Cucina Intelligente, Fix Mobile e Sicurezza Firestore

---

### Cucina intelligente

**Banner di conferma chiusura cucina**
Cliccando "Cucina aperta → chiusa" appare un bottom-sheet di conferma:
- Descrive l'effetto: i clienti vedranno solo Speciali, Dolci e Bevande
- Suggerisce il caso d'uso tipico (fine serata, pulizie in corso)
- Informa che la cucina si riaprirà automaticamente alle 06:00
- Pulsanti: Annulla / Sì, chiudi cucina

**Riapertura automatica alle 06:00**
Alla chiusura viene salvato in Firestore il campo `riaperturaAuto` con il timestamp del giorno successivo alle 06:00. Qualsiasi client che carica l'app dopo quell'orario triggera automaticamente `setSerata(true)` — nessuna Firebase Function o piano Blaze necessario. Il campo viene azzerato alla riapertura.

**Nomi piatti in maiuscolo**
`text-transform: uppercase` applicato uniformemente a tutti i contesti che ne erano privi:
card fuori menu, pagina Speciali, popup dettaglio fuori menu, bottom-sheet piatto normale.
Funziona automaticamente per i piatti inseriti in futuro.

---

### Fix

**Sovrapposizione testo su iOS Safari (form asporto)**
I campi input del modal asporto mostravano il testo sovrapposto su iOS.
Causa: mancanza di `line-height` esplicita; iOS usava un valore interno anomalo.
Fix: `line-height: 1.4`, `-webkit-appearance: none`, `width: 100%`, `box-sizing: border-box`.

---

### Sicurezza

**Firestore Security Rules — produzione**
Sostituite le regole aperte di sviluppo con regole per-collezione:

| Collezione | Lettura | Scrittura |
|---|---|---|
| Menu (antipasti, pizze, ecc.) | Pubblica | Solo admin autenticato |
| Config, chiusure | Pubblica | Solo admin autenticato |
| Prenotazioni, ordini asporto | Pubblica (solo create) | Solo admin autenticato |
| Storica serate | Solo admin | Solo admin |

Deploy: `firebase deploy --only firestore:rules`

Le regole sono valutate server-side — nessun client può aggirarle indipendentemente dal frontend Angular.
