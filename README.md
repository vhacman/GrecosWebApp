# Grecos Pizzeria — Menu Digitale

Menu digitale con QR code e pannello di gestione per Grecos Pizzeria.

**Cliente:** Grecos Pizzeria (@grecos11)
**Sviluppatrice:** Hacman Viorica Gabriela
**URL:** https://grecospizzeria-47768.web.app
**Versione:** v1.5.0 — 14-03-2026

---

## Cos'è

Un'applicazione web che permette ai clienti di consultare il menu tramite QR code sul proprio telefono, e al personale della pizzeria di gestire tutto (menu, prenotazioni, asporto, disponibilità) da un pannello di amministrazione.

---

## Evoluzione del Progetto

### v1.0.0 — Prima release (04-03-2026)

Lancio del progetto con le funzionalità base:

- **Menu digitale** consultabile dai clienti tramite QR code
- **6 categorie** di prodotti: Antipasti, Pizze Rosse, Pizze Bianche, Focacce & Calzoni, Dolci, Bevande
- **Scheda piatto** completa: nome, descrizione, prezzo, allergeni (simboli EU), indicatore vegano/surgelato
- **Filtro allergeni** in tempo reale
- **Navigazione categorie** con menu orizzontale sticky
- **PWA installabile** sui telefoni (Android/iOS)
- **Pannello admin** con login sicuro per gestire il menu
- **Gestione piatti**: aggiunta, modifica, attivazione/disattivazione, eliminazione
- **Fuori menu**: sezione dedicata ai piatti speciali della serata
- **Disponibilità ingredienti**: possibilità di barrare ingredienti non disponibili
- **Messaggio del giorno**: banner personalizzabile in homepage
- **Statistiche base**: contatore visite

### v1.1.0 — Miglioramenti dashboard e PDF (08-03-2026)

- **Modalità estate/inverno**: cambia automaticamente i giorni di apertura e le zone disponibili
- **Stagione estiva**: banner automatico che informa i clienti dell'apertura del giovedì
- **PDF export**: resoconto disponibilità, QR code e storico serate generati con jsPDF
- **Storico serate**: archivio dei piatti serviti ogni sera, esportabile in PDF
- **Statistiche avanzate**: grafici visite, dispositivi, orari di picco, categorie più visitate, selettore 7/30 giorni, auto-refresh ogni 3 minuti
- **Homepage**: bottone "Apri il Menu" ridisegnato con animazione pulse

### v1.2.0 — Prenotazioni, Asporto e Fix (09-03-2026)

Grande aggiornamento con nuovi sistemi e fix identificati tramite Microsoft Clarity:

- **Fix IndexedDB**: sostituita la cache persistente con `memoryLocalCache()` — eliminati 112 errori di corruzione rilevati in 4 giorni di analytics
- **Bottom-sheet dettaglio piatto**: tap su qualsiasi riga del menu apre un modal con descrizione completa, allergeni e prezzo
- **Allergeni fuori menu**: il popup dei piatti speciali mostra ora allergeni e disclaimer "fatto in casa"
- **Allergeni multilingua**: i nomi degli allergeni si aggiornano reattivamente al cambio lingua IT/EN
- **Statistiche date personalizzate**: selettore range libero (da/a) e ranking "Piatti più curiosati"
- **Prenotazioni tavoli**: sistema completo con navigazione per settimane, KPI, PDF, notifiche real-time
- **Chiusure e ferie**: gestione periodi di chiusura con banner automatico in homepage
- **Statistiche prenotazioni**: KPI tavoli/coperti, grafico prenotazioni, insight orario/zona/giorno
- **Chiudi prenotazioni / Chiudi asporto**: toggle per bloccare nuovi arrivi quando il locale è pieno
- **PDF asporto**: riepilogo ordini del giorno con totale incasso
- **Autocomplete asporto**: ricerca prodotti intelligente con inserimento automatico di nome e prezzo

### v1.3.0 — Specialità, QR e Manuale (10-03-2026)

- **Tab Specialità nel menu**: nuova prima tab ⭐ con tutti i fuori menu attivi raggruppati per categoria
- **QR con logo**: il QR generato dall'admin usa `HomepageQR.png` con logo Grecos al centro
- **QR dedicato dolci**: PDF stampabile con QR puntato a `/menu?cat=dolci`
- **Link diretto per categoria**: la route `/menu` supporta `?cat=` per aprire una sezione specifica
- **Condivisione QR dai clienti**: bottone nel popup "Condividi" che usa la Web Share API nativa
- **Popup Condividi ridisegnato**: bottom sheet pulito in sostituzione del modal con gradienti
- **Manuale d'uso**: bottone in homepage per scaricare il PDF del manuale utente
- **LinkedIn nel footer homepage**

### v1.5.0 — Responsive Mobile, UX Prenotazioni & Asporto, PDF, Firestore (14-03-2026)

- **Fix globali Android/iOS**: `overflow-x: hidden`, `text-size-adjust: 100%`, `interactive-widget=resizes-content` per tastiera, KPI bar con `flex-wrap`
- **Header a due righe su mobile**: titolo su riga 1, bottoni azioni su riga 2 — nessuna sovrapposizione (Prenotazioni e Asporto)
- **Barra azioni sticky Prenotazioni**: pill "Aggiungi", "Non pren." e WhatsApp sempre visibili, stile leggero con bordo sottile
- **Modal "Non prenotati" (walk-in)**: bottom-sheet dedicato con badge numerico, nuova collezione Firestore `nonPrenotati`
- **Modifica prenotazione**: `updatePrenotazione()` nel ConfigService — modifica inline senza eliminare e ricreare
- **Notifica real-time**: toast quando un'altra sessione aggiunge una prenotazione per il giorno visualizzato
- **Condivisione WhatsApp con immagine**: genera PNG del riepilogo giornata, anteprima + invio via Web Share API
- **PDF Prenotazioni — Mese e Anno**: dropdown tre opzioni, nuovi file `pdf-prenotazioni-mese.ts` e `pdf-prenotazioni-anno.ts`
- **Swipe to dismiss**: tutti i modal si chiudono trascinando verso il basso, senza attivare il pull-to-refresh del browser (`{ passive: false }`)
- **Asporto — Orario libero**: input orario personalizzato nel form ordine, identico a Prenotazioni
- **Asporto — Sezione "Altro"**: voci libere con autocomplete, come pizze e fritti
- **Autocomplete migliorato**: filtro `startsWith` invece di `includes`, dropdown sempre visibile su Android (mai tagliato)
- **Layout item asporto a due righe su mobile**: nome piatto su riga 1, quantità/prezzo/rimuovi su riga 2
- **PDF Asporto — Mese e Anno**: dropdown tre opzioni, nuovi file `pdf-asporto-mese.ts` e `pdf-asporto-anno.ts`
- **Calendario dal 1° gennaio 2026**: `generaGiorniAperti()` con parametro `dal?`, sidebar posizionata automaticamente sulla settimana corrente tramite `scrollTop` manuale
- **Firestore Security Rules**: completate regole per `nonPrenotati`, `prenotazioniChiuse`, `asporto`, `asportoChiuso`, `storici`, `statistiche`

### v1.4.0 — Cucina Intelligente + Sicurezza (13-03-2026)

- **Conferma chiusura cucina**: bottom-sheet che avverte l'admin dell'effetto (solo Speciali/Dolci/Bevande visibili) con pulsante annulla/conferma
- **Riapertura automatica alle 06:00**: alla chiusura viene salvato in Firestore il timestamp di riapertura — nessuna Firebase Function necessaria
- **Nomi piatti uniformi in maiuscolo**: `text-transform: uppercase` applicato a tutti i contesti
- **Fix iOS asporto**: risolto bug Safari mobile con testo sovrapposto nei campi del form
- **Firestore Security Rules**: implementate regole di sicurezza per-collezione — lettura pubblica per il menu, scrittura solo admin autenticato, creazione pubblica per prenotazioni/asporto

---

## Link Utili

- [Sito web](https://grecospizzeria-47768.web.app)
- [Console Firebase](https://console.firebase.google.com/)
- [Documentazione tecnica](./Documentazione/CHANGELOG.md)
- [Release notes](./Documentazione/releases/)
