# Release v1.5.0 — 14-03-2026

## Responsive Mobile, UX Prenotazioni & Asporto, PDF, Firestore

---

### Responsive Android & iOS — Fix globali

**Overflow orizzontale e zoom-out su Android**
`overflow-x: hidden` aggiunto a `html` e `body` per impedire lo scroll orizzontale che causava un effetto zoom-out involontario su Chrome Android.

**Text boosting Android Chrome bloccato**
`-webkit-text-size-adjust: 100%` e `text-size-adjust: 100%` su `body` impediscono a Chrome Android di ingrandire autonomamente il testo nei blocchi ristretti.

**Tastiera Android non copre i form**
`interactive-widget=resizes-content` nel viewport meta: quando la tastiera virtuale appare, il contenuto ridimensiona correttamente senza nascondere campi di input.

**Header a due righe su mobile**
Su schermi ≤ 480px l'header di Prenotazioni e Asporto si riorganizza automaticamente su due righe tramite `flex-wrap: wrap` e `order`:
- Riga 1: freccia indietro + titolo
- Riga 2: bottoni azioni (chiudi prenotazioni/asporto, PDF, calendario)

Nessuna sovrapposizione tra titolo e bottoni su qualsiasi schermo.

**Sidebar settimane ridotta su mobile**
La colonna di navigazione settimanale si riduce a 88px (prenotazioni) e 90px (asporto) su schermi ≤ 480px.

**KPI bar senza tagli**
La barra tavoli/persone/seggioloni usa `flex-wrap: wrap` — i dati vanno a capo invece di essere tagliati su schermi stretti.

---

### Prenotazioni — Nuove funzionalità

**Barra azioni sticky**
I tre bottoni principali sono ora in una barra `position: sticky` subito sotto l'header, sempre visibili:
- **Aggiungi** — apre il form nuova prenotazione
- **Non pren.** — apre il modal dei tavoli walk-in
- **WhatsApp** — genera e condivide l'immagine riepilogativa (visibile solo se ci sono dati)

Design a pill con sfondo tenue e bordo sottile (`border-radius: 2rem`, `border: 1.5px solid`), leggero e non invasivo.

**Modal "Non prenotati" (walk-in)**
I tavoli senza prenotazione sono stati spostati dal fondo della pagina a un bottom-sheet dedicato, accessibile dalla barra azioni. Badge numerico sul bottone mostra il conteggio corrente. La nuova collezione Firestore `nonPrenotati` gestisce questi record separatamente dalle prenotazioni.

**Modifica prenotazione**
Aggiunto `updatePrenotazione()` al ConfigService: ora è possibile modificare una prenotazione esistente (nome, telefono, zona, persone, seggioloni, orario, note) senza doverla eliminare e ricreare.

**Notifica real-time nuove prenotazioni**
Toast in-app che appare quando un'altra sessione aggiunge una prenotazione per il giorno visualizzato (rilevato tramite `createdAt.seconds > initTime`). Si chiude automaticamente dopo 5 secondi.

**Condivisione WhatsApp con immagine**
Bottone WhatsApp nella barra azioni: genera un'immagine PNG del riepilogo del giorno (`png-prenotazioni.ts`) e la condivide via Web Share API (con fallback download su browser non supportati). Anteprima immagine nel modal prima dell'invio.

**PDF Prenotazioni — Mese e Anno**
Dropdown PDF con tre opzioni:
- **Giornata** — riepilogo ordini del giorno
- **Mese intero** — tutte le prenotazioni del mese raggruppate per giorno (`pdf-prenotazioni-mese.ts`)
- **Anno intero** — tutte le prenotazioni dell'anno raggruppate per mese e giorno (`pdf-prenotazioni-anno.ts`)

`generaPdfGiornataBlob()` aggiunto a `pdf-prenotazioni.ts` per restituire Blob invece di salvare direttamente su disco (necessario per l'anteprima WhatsApp).

**Swipe to dismiss**
Tutti i modal (nuova prenotazione, non prenotati) si chiudono con uno swipe verso il basso. Implementato con factory `makeSwipeHandler(closeFn)` che registra manualmente il listener `touchmove` con `{ passive: false }` per poter chiamare `preventDefault()` e prevenire il pull-to-refresh del browser.

---

### Asporto — Nuove funzionalità

**Bottone "Aggiungi ordine" nel contenuto**
Sostituito il `+` nell'header con un bottone esplicito nel corpo della pagina, più accessibile e visibile.

**Orario libero per gli ordini**
Toggle "Orario libero" nel form: permette di inserire un orario personalizzato tramite `<input type="time">`, identico al comportamento già presente in Prenotazioni.

**Sezione "Altro" tipizzabile**
La sezione Altro (dolci, bevande, ecc.) ora funziona come Pizze e Fritti: bottone per aggiungere voci con autocomplete e modifica inline.

**Autocomplete corretto**
- Filtro cambiato da `.includes()` a `.startsWith()` — eliminati suggerimenti non pertinenti (es. "Rossa e olive" quando si digitava "M")
- Dropdown sempre visibile su Android: `min-width: 200px`, `max-width: calc(100vw - 3rem)`, `right: auto`

**Layout item a due righe su mobile**
Su schermi ≤ 480px la riga di ogni articolo si divide:
- Riga 1: nome del piatto (larghezza piena)
- Riga 2: quantità + prezzo + rimuovi

**Swipe to dismiss**
Il modal ordine si chiude con swipe verso il basso, con stesso pattern `{ passive: false }` delle prenotazioni.

**PDF Asporto — Mese e Anno**
Dropdown PDF con tre opzioni, come in Prenotazioni:
- **Giornata** — riepilogo ordini del giorno con KPI (`pdf-asporto.ts`)
- **Mese intero** — ordini del mese per giorno (`pdf-asporto-mese.ts`)
- **Anno intero** — ordini dell'anno per mese e giorno (`pdf-asporto-anno.ts`)

---

### Calendario

**Scorrimento dal 1° gennaio 2026**
`generaGiorniAperti()` estesa con parametro opzionale `dal?: string` (formato `YYYY-MM-DD`). Prenotazioni e Asporto passano `'2026-01-01'` per mostrare l'intero storico dell'anno.

**Posizionamento automatico sulla settimana corrente**
All'apertura, la sidebar delle settimane si posiziona sulla settimana selezionata usando `afterNextRender` + calcolo manuale `scrollTop` (non `scrollIntoView`, che spostava anche il contenuto principale).

---

### Firestore Security Rules

Completate le regole per-collezione per i nuovi percorsi introdotti:

| Collezione | Lettura | Scrittura |
|---|---|---|
| `nonPrenotati` | Solo admin | Solo admin |
| `prenotazioniChiuse` | Solo admin | Solo admin |
| `asporto` | Solo admin | Solo admin |
| `asportoChiuso` | Solo admin | Solo admin |
| `storici` | Solo admin | Solo admin |
| `statistiche` | Solo admin | Pubblica (visitatori anonimi) |

---

### Modello dati

**Nuova interfaccia `NonPrenotato`**
```typescript
interface NonPrenotato {
  id?:       string;
  data:      string;   // YYYY-MM-DD
  persone:   number;
  createdAt: any;
}
```

---

### Fix

| Problema | Causa | Soluzione |
|---|---|---|
| Swipe modal non faceva dismiss | `touchmove` Angular è passivo di default, `preventDefault()` ignorato | Listener manuale con `{ passive: false }` |
| Pull-to-refresh attivato durante swipe | Stesso motivo | Stesso fix |
| Testo "Non prenotati" sforava su Android | Bottone `flex: 1` con contenuto troppo lungo | Abbreviato a "Non pren." |
| Bottone WhatsApp tagliato su Android | Due bottoni `flex: 1` con `min-width: auto` spingevano WA fuori schermo | `min-width: 0` sui bottoni flex-1 |
| Autocomplete tagliato su Android | `right: 0` con `position: absolute` causava overflow | `right: auto` + `max-width` vincolato al viewport |
| Modal diventava full-screen (revertito) | `@media` errato aggiunto per sbaglio | Rimosso |
| scrollIntoView spostava tutta la pagina | API nativa scorre il primo antenato scrollabile | Sostituito con `nav.scrollTop` calcolato manualmente |
