# Changelog — Grecos Pizzeria Menu Digitale

Tutte le modifiche rilevanti al progetto sono documentate qui.
Formato: `[vX.Y.Z] — GG-MM-AAAA`

---

## [v1.6.0] — 14-03-2026

### Novità

- **Predeploy build automatico**
  Aggiunto hook `predeploy: ["npm run build"]` in `firebase.json`: ogni `firebase deploy` compila automaticamente il progetto prima di pubblicarlo. Elimina il problema di deployare accidentalmente una build vecchia.

- **Cache headers Firebase Hosting**
  Configurati header `Cache-Control` per-file in `firebase.json`:
  - `ngsw-worker.js`, `ngsw.json`, `index.html` → `no-cache, no-store, must-revalidate`: il browser verifica sempre se c'è una nuova versione
  - `*.js`, `*.css`, font → `public, max-age=31536000, immutable`: i bundle hashati sono cacheati per un anno

  Risolve il problema per cui gli utenti vedevano la versione vecchia dopo un deploy senza svuotare la cache.

- **Filtro localhost in Microsoft Clarity**
  Clarity non si inizializza più su `localhost` né su indirizzi `192.168.*`: le sessioni di sviluppo non inquinano più le statistiche di produzione.

### Fix

- **Bottone "Invia su WhatsApp" nell'anteprima prenotazioni senza stile**
  La classe `.wa-btn` mancava da `prenotazioni-modal.css`. Aggiunta con stile verde WhatsApp (`#25D366`), larghezza piena e stesso layout del `salva-btn`.

---

## [v1.5.0] — 14-03-2026

### Novità

- **Responsive Android & iOS — fix globali**
  - `overflow-x: hidden` su `html` e `body`: elimina lo scroll orizzontale involontario su Chrome Android che causava uno zoom-out dell'intera pagina.
  - `-webkit-text-size-adjust: 100%` e `text-size-adjust: 100%` su `body`: blocca il text boosting automatico di Chrome Android nei blocchi ristretti.
  - `interactive-widget=resizes-content` nel viewport meta: quando la tastiera virtuale appare su Android il contenuto si ridimensiona correttamente, senza nascondere i campi di input sotto la tastiera.
  - KPI bar (tavoli/persone/seggioloni) con `flex-wrap: wrap`: i dati vanno a capo invece di tagliarsi su schermi stretti.

- **Header a due righe su mobile (Prenotazioni e Asporto)**
  Su schermi ≤ 480px l'header si riorganizza su due righe tramite `flex-wrap: wrap` e `order`:
  - Riga 1: freccia indietro + titolo sezione
  - Riga 2: bottoni azioni (chiudi prenotazioni/asporto, PDF, calendario)
  Nessuna sovrapposizione tra titolo e bottoni su qualsiasi dispositivo.

- **Barra azioni sticky in Prenotazioni**
  Tre bottoni sempre visibili in una barra `position: sticky` subito sotto l'header:
  - **Aggiungi** — apre il form nuova prenotazione
  - **Non pren.** — apre il modal dei tavoli walk-in (con badge numerico se ci sono tavoli)
  - **WhatsApp** — genera e condivide l'immagine riepilogativa (visibile solo se ci sono dati)
  Design a pill con sfondo tenue e bordo sottile (`border-radius: 2rem`, `border: 1.5px solid`).

- **Modal "Non prenotati" (walk-in)**
  I tavoli senza prenotazione sono stati spostati dal fondo pagina a un bottom-sheet dedicato,
  accessibile dalla barra azioni. Nuova collezione Firestore `nonPrenotati` con interfaccia
  `NonPrenotato { id?, data, persone, createdAt }`. Badge numerico sul bottone mostra il conteggio.

- **Modifica prenotazione inline**
  Aggiunto `updatePrenotazione()` nel ConfigService: ora è possibile modificare una prenotazione
  esistente (nome, telefono, zona, persone, seggioloni, orario, note) senza eliminarla e ricrearla.

- **Notifica real-time nuove prenotazioni**
  Toast in-app che appare automaticamente quando un'altra sessione aggiunge una prenotazione
  per il giorno visualizzato (rilevato tramite `createdAt.seconds > initTime`).
  Si chiude automaticamente dopo 5 secondi o al tap.

- **Condivisione WhatsApp con immagine (Prenotazioni)**
  Il bottone WhatsApp genera un PNG del riepilogo della giornata (`png-prenotazioni.ts`)
  e lo condivide via Web Share API con caption automatica. Anteprima immagine nel modal
  prima dell'invio. Fallback download su browser non supportati.

- **PDF Prenotazioni — Mese e Anno**
  Dropdown PDF con tre opzioni:
  - **Giornata** — riepilogo giornaliero (già presente)
  - **Mese intero** — tutte le prenotazioni del mese raggruppate per giorno (`pdf-prenotazioni-mese.ts`)
  - **Anno intero** — tutte le prenotazioni dell'anno raggruppate per mese (`pdf-prenotazioni-anno.ts`)
  Aggiunto `generaPdfGiornataBlob()` in `pdf-prenotazioni.ts` per restituire Blob (necessario per l'anteprima WhatsApp).

- **Swipe to dismiss su tutti i modal**
  Tutti i modal (nuova prenotazione, non prenotati, ordine asporto) si chiudono trascinando
  verso il basso. Implementato con factory `makeSwipeHandler(closeFn)` che registra manualmente
  il listener `touchmove` con `{ passive: false }` per chiamare `preventDefault()` e prevenire
  il pull-to-refresh del browser — comportamento impossibile con i listener Angular (passivi per default).

- **Asporto — Bottone "Aggiungi ordine" nel contenuto**
  Sostituito il `+` nell'header con un bottone esplicito nel corpo della pagina, più accessibile.

- **Asporto — Orario libero**
  Toggle "Orario libero" nel form ordine: permette di inserire un orario personalizzato
  tramite `<input type="time">`, identico al comportamento già presente in Prenotazioni.

- **Asporto — Sezione "Altro" tipizzabile**
  La sezione Altro (dolci, bevande, ecc.) ora funziona come Pizze e Fritti:
  bottone per aggiungere voci con autocomplete e modifica inline.

- **Asporto — Autocomplete migliorato**
  - Filtro cambiato da `.includes()` a `.startsWith()`: eliminati suggerimenti non pertinenti
    (es. "Rossa e olive" appariva cercando "M" perché conteneva la "m" nel nome).
  - Dropdown sempre visibile su Android: `min-width: 200px`, `max-width: calc(100vw - 3rem)`, `right: auto`.

- **Asporto — Layout item a due righe su mobile**
  Su schermi ≤ 480px la riga di ogni articolo si divide:
  - Riga 1: nome del piatto (larghezza intera)
  - Riga 2: quantità + prezzo + rimuovi

- **PDF Asporto — Mese e Anno**
  Dropdown PDF con tre opzioni (come Prenotazioni):
  - **Giornata** — riepilogo ordini con KPI (già presente)
  - **Mese intero** — ordini per giorno (`pdf-asporto-mese.ts`)
  - **Anno intero** — ordini per mese e giorno (`pdf-asporto-anno.ts`)

- **Calendario dal 1° gennaio 2026**
  `generaGiorniAperti()` estesa con parametro opzionale `dal?: string` (YYYY-MM-DD).
  Prenotazioni e Asporto passano `'2026-01-01'` per includere l'intero storico dell'anno.
  All'apertura, la sidebar si posiziona automaticamente sulla settimana selezionata
  usando `afterNextRender` + `scrollTop` calcolato manualmente (non `scrollIntoView`,
  che spostava anche il contenuto principale della pagina).

### Sicurezza

- **Firestore Security Rules — completamento**
  Aggiunte regole per le nuove collezioni introdotte in questa release:

  | Collezione | Lettura | Scrittura |
  |---|---|---|
  | `nonPrenotati` | Solo admin | Solo admin |
  | `prenotazioniChiuse` | Solo admin | Solo admin |
  | `asporto` | Solo admin | Solo admin |
  | `asportoChiuso` | Solo admin | Solo admin |
  | `storici` | Solo admin | Solo admin |
  | `statistiche` | Solo admin | Pubblica (visitatori anonimi) |

### Fix

- **Swipe modal attivava il pull-to-refresh** — listener `touchmove` Angular è passivo per default, `preventDefault()` veniva ignorato. Risolto registrando il listener manualmente con `{ passive: false }`.
- **Testo "Non prenotati" sforava su Android** — abbreviato in "Non pren." per adattarsi agli schermi più stretti.
- **Bottone WhatsApp tagliato a destra su Android** — aggiunto `min-width: 0` ai bottoni `flex: 1` per cedere spazio al bottone WA senza spingerlo fuori dal viewport.
- **`scrollIntoView` spostava tutta la pagina** — sostituito con calcolo manuale `nav.scrollTop` che scorre solo la sidebar.

---

## [v1.4.0] — 13-03-2026

### Novità

- **Banner di conferma "Cucina chiusa" con riapertura automatica**
  - Cliccando sul toggle "Cucina aperta → chiusa" nel pannello admin appare un bottom-sheet
    di conferma che descrive l'effetto: i clienti vedranno solo **Speciali**, **Dolci** e **Bevande**.
  - Il banner suggerisce il caso d'uso tipico (fine serata, pulizie in corso) e informa
    che la cucina si riaprirà automaticamente **domani alle 06:00**.
  - L'admin può annullare (tap sul pulsante o sull'overlay) o confermare con "Sì, chiudi cucina".
  - La riapertura senza cucina non richiede conferma (si riesegue direttamente).
- **Riapertura automatica cucina alle 06:00**
  - Quando l'admin chiude la cucina, in Firestore viene salvato un campo `riaperturaAuto`
    con il timestamp del giorno successivo alle 06:00 (orario locale).
  - Qualsiasi client (cliente o admin) che carica l'app dopo quell'orario triggera
    automaticamente `setSerata(true)` — nessun Firebase Function o piano Blaze necessario.
  - Alla riapertura il campo `riaperturaAuto` viene azzerato.
- **Nomi piatti tutti in maiuscolo**
  - Applicato `text-transform: uppercase` via CSS a tutte le classi dei nomi piatto
    che ne erano prive: card fuori menu (scroll orizzontale), pagina Speciali, popup
    dettaglio fuori menu, bottom-sheet dettaglio piatto menu normale.
  - Funziona automaticamente anche per i piatti inseriti in futuro.

### Sicurezza

- **Firestore Security Rules — implementazione produzione**
  Sostituite le regole aperte (`allow read, write: if true`) con regole per-collezione:
  - **Menu pubblico** (antipasti, pizzeRosse, pizzeBianche, focacceCalzoni, dolci, bevande,
    fuoriMenu, ingredienti, config, chiusure): `allow read: if true` + `allow write: if request.auth != null`
  - **Prenotazioni**: `allow create: if true` + `allow read, update, delete: if request.auth != null`
  - **Ordini asporto**: stessa logica delle prenotazioni
  - **Storica serate**: `allow read, write: if request.auth != null`
  Le regole sono server-side — nessun client può aggirarle indipendentemente dal frontend.

### Fix

- **Sovrapposizione testo nei campi ordine asporto su mobile**
  - Su iOS Safari i campi di input nel form ordini asporto mostravano il testo sovrapposto.
  - Causa: mancanza di `line-height` esplicita sugli input senza bordo; iOS usava un valore interno anomalo.
  - Fix: aggiunto `line-height: 1.4` e `-webkit-appearance: none` a tutti gli input del modal asporto.

---

## [v1.3.0] — 10-03-2026

### Novità

- **Sezione "Specialità" nel menu pubblico**
  Nuova tab ⭐ Specialità come prima voce della navbar del menu. Mostra tutti i fuori menu
  attivi raggruppati per categoria (con card cliccabili, popup dettaglio, allergeni, IT/EN).
  Sempre visibile anche a cucina chiusa. Stato vuoto se nessun fuori menu attivo.

- **QR code con logo nel pannello admin**
  Il QR generato da `Strumenti → QR Code` usa ora `HomepageQR.png` (logo Grecos al centro)
  invece della generazione dinamica via libreria. Download PNG e PDF aggiornati di conseguenza.

- **QR dedicato sezione Dolci**
  Generato `grecos-qr-dolci.pdf` (A4, stile Grecos) con QR puntato a `/menu?cat=dolci`.
  Da stampare e posizionare vicino al porta-dolci.

- **Link diretto per categoria via `?cat=`**
  La route `/menu` ora supporta `?cat=dolci`, `?cat=pizzeRosse`, ecc. per aprire
  direttamente la categoria desiderata. Utile per QR stampati per sezione.

- **Condivisione QR dai clienti**
  Nel popup "Condividi" della homepage aggiunto bottone QR Code. Su mobile usa il Web Share
  API per condividere `HomepageQR.png` nativamente (WhatsApp, AirDrop, ecc.).
  Fallback download automatico su browser non supportati.

- **Popup Condividi ridisegnato**
  Sostituito il modal con gradienti/emoji con un bottom sheet pulito: righe semplici con
  icona colorata e label, coerente con lo stile dell'app.

- **Bottone "Manuale d'Uso dell'App" in homepage**
  Nuovo bottone sotto la sezione "Scarica App" per scaricare il PDF del manuale utente.
  Il file `manuale-utente.pdf` è servito dalla root di Firebase Hosting.

- **LinkedIn nel footer homepage**
  Icona LinkedIn affiancata al copyright in fondo alla homepage.

---

## [v1.2.0] — 09-03-2026

### Novità

- **Prenotazioni tavoli — sistema completo**
  Nuova sezione admin (`/admin/prenotazioni`) per gestire le prenotazioni in tempo reale.
  - Navigazione per settimane (Gio/Ven/Sab/Dom) con gruppo visivo e popup calendario mensile
  - KPI giornalieri: tavoli, coperti totali, seggioloni
  - Aggiunta prenotazione con campi: nome, telefono, zona, persone, seggioloni, orario, note
  - Eliminazione con conferma, toggle "arrivata", toast real-time per nuove prenotazioni
  - PDF scaricabile giornalmente con intestazione Grecos, KPI e tabella completa
  - Calendario dinamico fino al 31/12 dell'anno successivo

- **Statistiche prenotazioni**
  KPI tavoli/coperti, grafico "Prenotazioni giornaliere", insight orario/zona/giorno top.

- **Modalità estate / inverno**
  Toggle che commuta giorni di apertura (Ven/Sab/Dom ↔ Gio/Ven/Sab/Dom) e zone prenotazione.
  In estate compare automaticamente per 20 giorni il banner di apertura giovedì.

- **Chiusure / ferie**
  Nuova sezione admin per gestire periodi di chiusura. Le date chiuse scompaiono dal
  calendario prenotazioni. Homepage: banner rosso (chiusura attiva) o arancio (entro 14 giorni).

- **Chiusura prenotazioni e asporto**
  Toggle per bloccare nuovi arrivi quando il locale è pieno. Homepage mostra il messaggio
  di chiusura per quella sera specificatamente.

- **PDF asporto**
  Riepilogo ordini del giorno con totale incasso, scaricabile dall'admin asporto.

- **Autocomplete asporto con prezzi automatici**
  Ricerca intelligente per nome prodotto con dropdown, inserimento automatico di nome e prezzo.
  Ricerca filtrata per categoria (pizze, fritti, altro) con fuori menu sempre incluso.

- **Bottom-sheet dettaglio piatto**
  Tap su qualsiasi riga del menu apre un modal con descrizione completa, allergeni espansi e prezzo.
  Identificato da Microsoft Clarity: utenti toccavano i nomi aspettandosi un'interazione.

- **Allergeni fuori menu**
  Il popup dei piatti speciali mostra allergeni e disclaimer "fatto in casa" / "surgelato".

- **Statistiche — range date personalizzato e piatti curiosati**
  Selettore da/a libero in sostituzione del fisso 7gg/30gg. Ranking top-10 piatti più aperti.

### Fix

- **Errori IndexedDB Firebase (112 errori in 4 giorni)**
  Rilevati tramite Microsoft Clarity: `refusing to open indexeddb database due to potential corruption`.
  Fix: sostituita la cache IndexedDB con `memoryLocalCache()` — zero corruzioni, nessun dato offline
  (accettabile per un menu di pizzeria).

- **Allergeni non tradotti in inglese**
  I nomi allergeni erano hardcoded in italiano. Sostituito con `lingua.t('all_N')` — ora
  si aggiornano reattivamente al cambio lingua IT/EN.

- **Resoconto disponibilità — tutto segnato "Scarso"**
  Il controllo `=== null` non catturava `undefined` da Firestore. Corretto con `== null`.

- **Statistiche — categoria iniziale mai registrata**
  `registraVisitaCategoria()` non veniva chiamata al caricamento iniziale. Corretto in `ngOnInit`.

- **PWA banner — testo generico uguale per iOS e Android**
  Aggiunto rilevamento `isIOS` via `userAgent` con istruzioni specifiche per piattaforma.

---

## [v1.1.0] — 08-03-2026

### Novità

- **PDF export** — resoconto disponibilità, QR code e storico serate generati con jsPDF
  (sostituisce `window.print()`). Il PDF è offline-ready e non apre finestre aggiuntive.
- **Storico serate** — snapshot automatico in Firestore al download PDF. Lista storica con
  bottone per ri-scaricare ogni serata in qualsiasi momento.
- **Statistiche avanzate** — selettore 7/30 giorni, auto-refresh ogni 3 minuti, grafici
  scrollabili orizzontalmente. KPI: dispositivi, orari di picco, categorie più visitate.
- **Homepage** — bottone "Apri il Menu" ridisegnato con sfondo rosso e animazione `pulse`.
- **Modalità estate/inverno** — giorni di apertura e zone prenotazione cambiano con un toggle.
  Banner automatico in homepage per 20 giorni all'attivazione estate.

---

## [v1.0.0] — 04-03-2026

Prima release pubblica.

### Incluso nella v1.0.0

- Menu digitale pubblico con 6 categorie (115 piatti totali)
- Filtro allergeni in tempo reale (14 allergeni EU)
- Sezione fuori menu con aggiornamenti in tempo reale
- Homepage con stato serata, orari e messaggio del giorno
- Pannello admin completo (login, dashboard, CRUD menu e fuori menu)
- Gestione disponibilità ingredienti, dolci, antipasti, bevande
- Resoconto disponibilità serata
- Storico serate
- Statistiche visite (7 giorni)
- QR code generabile e scaricabile (PNG)
- PWA installabile su Android e iOS
- Deploy su Firebase Hosting
