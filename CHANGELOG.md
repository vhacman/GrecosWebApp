# Changelog — Grecos Pizzeria Menu Digitale

Tutte le modifiche rilevanti al progetto sono documentate qui.
Formato: `[vX.Y.Z] — GG-MM-AAAA`

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
