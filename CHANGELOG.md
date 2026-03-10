# Changelog — Grecos Pizzeria Menu Digitale

Tutte le modifiche rilevanti al progetto sono documentate qui.
Formato: `[vX.Y.Z] — GG-MM-AAAA`

---

## [v1.5.0] — 10-03-2026

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

- **LinkedIn nel footer homepage**
  Icona LinkedIn affiancata al copyright in fondo alla homepage.

---

## [v1.4.0] — 09-03-2026

### Novità

- **Autocomplete asporto con prezzi automatici**
  - Quando si inserisce un nuovo ordine asporto, digitando il nome del prodotto appare un dropdown con i prodotti del menu che contengono quella parte di nome
  - Cliccando su un prodotto, vengono inseriti automaticamente nome e prezzo
  - Ricerca filtrata per categoria:
    - Pizze → cerca solo pizze rosse, bianche e fuori menu
    - Fritti → cerca solo antipasti, focacce/calzoni e fuori menu
    - Altro → cerca solo dolci, bevande e fuori menu
  - Se non trova corrispondenze, permette inserimento manuale

---

## [v1.3.0] — 09-03-2026

### Novità

- **Chiusura prenotazioni (locale pieno)**
  - Nuovo toggle in admin prenotazioni: "CHIUDI PRENOTAZIONI" / "APRI PRENOTAZIONI"
  - Funziona per qualsiasi giorno della settimana (non solo giorni di apertura)
  - Homepage: mostra banner "Prenotazioni chiuse — Il locale è pieno per questa sera. Prova a prenotare per un giorno successivo!"
  - Se il giorno odierno è aperto ma le prenotazioni sono chiuse, mostra il messaggio per quel giorno specifico

- **Chiusura asporto (cucina piena)**
  - Nuovo toggle in admin asporto: "CHIUDI ASPORTO" / "APRI ASPORTO"
  - Funziona per qualsiasi giorno della settimana
  - Stessa logica di visualizzazione in homepage delle prenotazioni chiuse

- **PDF asporto**
  - Nuovo bottone in admin asporto per scaricare PDF con riepilogo ordini del giorno
  - Include: orario, nome cliente, numero pizze, numero fritti, note, totale incasso

---

## [v1.2.0] — 09-03-2026

### Novità

- **Prenotazioni tavoli — sistema completo**
  Nuova sezione admin (`/admin/prenotazioni`) per gestire le prenotazioni in tempo reale.
  - Navigazione per settimane (Gio/Ven/Sab/Dom) con gruppo visivo e popup calendario mensile per saltare a qualsiasi settimana dell'anno
  - KPI giornalieri: tavoli, coperti totali, seggioloni
  - Aggiunta prenotazione con campi: nome, telefono (opzionale), zona, persone, seggioloni, orario (slot fissi o libero), note
  - Eliminazione con conferma
  - Toggle "arrivata" disponibile solo per date passate/oggi (icona orologio per date future)
  - Toast in-app real-time quando un'altra sessione admin aggiunge una prenotazione
  - Ogni prenotazione registra chi l'ha inserita (`creatoDa`)
  - PDF scaricabile giornalmente con intestazione Grecos, KPI riepilogo e tabella completa
  - Calendario dinamico fino al 31/12 dell'anno successivo (si aggiorna automaticamente ogni anno)

- **Modalità estate / inverno**
  Toggle in Impostazioni che commuta:
  - Giorni di apertura: inverno = Ven/Sab/Dom, estate = Gio/Ven/Sab/Dom
  - Zone prenotazione: inverno = Sala interna / Veranda, estate = Terrazzo esterno / Veranda
  - Attivando la modalità estate compare automaticamente per 20 giorni un banner in homepage:
    *"Informiamo i nostri clienti che siamo aperti anche il Giovedì"*

- **Chiusure / ferie**
  Nuova sezione admin (`/admin/chiusure`) raggiungibile da Strumenti:
  - Lista periodi di chiusura con indicatore colorato (attiva / futura / passata)
  - Aggiunta chiusura con data dal/al e motivo
  - Eliminazione con conferma
  - Le date chiuse scompaiono automaticamente dal calendario prenotazioni
  - Homepage: banner rosso *"Il ristorante è chiuso fino al [data] — [motivo]"* quando attiva; banner arancio se la prossima chiusura è entro 14 giorni

- **Statistiche — sezione prenotazioni**
  La dashboard statistiche integra ora i dati delle prenotazioni in parallelo alle visite:
  - KPI: tavoli totali, coperti totali, media coperti/tavolo, seggioloni
  - Grafico "Prenotazioni giornaliere" (barre verdi) affianca il grafico visite
  - Insight: orario più prenotato, zona preferita, giorno della settimana top
  - Entrambi i grafici evidenziano i giorni di chiusura con pattern a righe diagonali

---

## [v1.1.6] — 09-03-2026

### Novità

- **Statistiche — piatti più curiosati**
  Ogni apertura del bottom-sheet dettaglio piatto (menu normale + fuori menu) registra
  un tap in Firestore (`statistiche/YYYY-MM-DD.piatti.{nome}`).
  La dashboard mostra un ranking top-10 "Piatti più curiosati" sopra il ranking categorie.
  Utile per capire cosa attira l'attenzione dei clienti indipendentemente dagli ordini.

---

## [v1.1.5] — 09-03-2026

### Novità

- **Statistiche — filtro range di date personalizzato**
  Sostituito il selettore fisso 7gg/30gg con due input `date` (da / a) nella barra
  sotto l'header. L'utente può scegliere qualsiasi intervallo dal passato a oggi.
  `getStatisticheAggregate()` ora accetta `da: string, a: string` (ISO YYYY-MM-DD)
  invece di `periodoGiorni`. Il KPI "N giorni" si aggiorna dinamicamente in base
  al range selezionato. La barra data è parte del blocco sticky, sempre visibile.

---

## [v1.1.4] — 09-03-2026

### Fix

- **Allergeni non tradotti in inglese**
  I nomi degli allergeni nei modal (righe menu e fuori menu) erano hardcoded in italiano
  tramite `getAllergeniNomi()` da `allergeni.constants.ts`.
  Sostituito con `lingua.t('all_N')` in entrambi i componenti (`ItemDetailModal`,
  `FuoriMenuSezione`) — ora i nomi si aggiornano reattivamente al cambio lingua
  usando le chiavi `all_1`…`all_14` già presenti nei dizionari IT/EN.

---

## [v1.1.3] — 09-03-2026

### Novità

- **Allergeni sui piatti fuori menu**
  Il modello `FuoriMenu` è stato esteso con i campi `allergeni?: number[]` e
  `surgelato?: boolean`. Il popup dettaglio (tap sulla card fuori menu) mostra ora
  gli allergeni come pill individuali.
  Per i prodotti **fatti in casa** appare un banner oro con disclaimer:
  *"Fatto in casa — ingredienti possono variare. Chiedere al personale per allergie specifiche."*
  Per i prodotti **surgelati industriali** (`surgelato: true`) gli allergeni sono
  quelli da etichetta e il disclaimer non viene mostrato.
  Migrazione Firestore (`scripts/migrate-fuori-menu-allergeni.js`) ha aggiornato
  tutti e 46 i documenti `fuoriMenu` in una singola esecuzione idempotente.

---

## [v1.1.2] — 09-03-2026

### Novità

- **Righe menu cliccabili — bottom-sheet dettaglio piatto**
  I dati Microsoft Clarity (07–09 Mar) mostravano utenti che toccavano nomi e descrizioni
  dei piatti aspettandosi un'azione. Aggiunto un bottom-sheet modale che si apre al tap su
  qualsiasi riga del menu: mostra nome, descrizione completa, lista allergeni espansa (pill
  per allergene) e prezzo. Rimosso il vecchio componente `AllergeniExpand` inline — gli
  allergeni sono ora accessibili solo dal modal. Ogni riga mostra il testo
  *"tocca per dettagli e allergeni"* (IT) / *"tap for details & allergens"* (EN) come
  istruzione visibile.

---

## [v1.1.1] — 09-03-2026

### Fix

- **Errori IndexedDB Firebase (112 errori in 4 giorni)**
  L'analisi di Microsoft Clarity rivelava 111 sessioni con errore:
  `refusing to open indexeddb database due to potential corruption`
  Il problema era causato da Firestore che usava IndexedDB per la cache offline
  persistente. Su alcuni browser/dispositivi (specialmente mobile) questo database
  locale si corrompeva, generando l'errore a ogni visita successiva.
  **Fix:** sostituita la cache IndexedDB con `memoryLocalCache()` in `app.config.ts`.
  Firestore ora usa solo la memoria RAM — nessun dato scritto su disco, zero corruzioni.
  Unica conseguenza accettabile: il sito non mostra dati offline (irrilevante per un menu).

---

## [v1.1.0] — 08-03-2026

### Fix

- **Resoconto disponibilità — tutto segnato "Scarso"**
  I piatti senza campo `disponibili` in Firestore restituivano `undefined`.
  Il controllo `=== null` non catturava `undefined`, facendo cadere tutti i piatti nel branch "Scarso".
  Corretto con `== null` (loose equality, cattura sia `null` che `undefined`).

- **Statistiche — categoria iniziale mai registrata**
  La chiamata a `registraVisitaCategoria()` avveniva solo al cambio tab, mai al caricamento iniziale.
  La categoria `antipasti` (quella di default) risultava sempre zero.
  Corretto aggiungendo la chiamata in `ngOnInit()` di `menu.ts`.

- **Statistiche — grafico 30 giorni sfasato**
  Con 30 barre in uno spazio pensato per 7, le etichette si sovrapponevano.
  Il grafico è ora scrollabile orizzontalmente (`overflow-x: auto`) con `min-width` dinamico basato sul numero di giorni.
  Le etichette a 30 giorni mostrano solo `gg/mm` invece di `giorno gg` per risparmiare spazio.

- **PWA banner — testo generico**
  Il banner di installazione mostrava un testo identico per iOS e Android.
  Corretto con rilevamento `isIOS` via `userAgent`: iOS mostra "Per iOS: aprire da Safari", Android mostra "Aprire da Google Chrome".

### Novità

- **PDF export — Resoconto disponibilità**
  Nuovo bottone PDF nell'header del resoconto. Genera un file A4 con jsPDF contenente
  tutti i gruppi, i badge di stato (OK / Scarso / Esaurito) e intestazione Grecos.
  Sostituisce il precedente approccio `window.print()`.

- **PDF export — QR Code**
  Dal modal QR nel dashboard admin è ora possibile scaricare anche un PDF A4 stampabile
  con logo, QR code centrato e URL del menu. Affianca il download PNG già esistente.

- **PDF export — Storico serate**
  La generazione PDF usa jsPDF al posto di `window.open() + window.print()`.
  Il PDF è identico ma completamente offline-ready e non apre finestre aggiuntive.

- **Storico serate — registro storico**
  Quando si scarica il PDF di una serata, viene salvato automaticamente uno snapshot
  in Firestore (collezione `storici`). La pagina mostra la lista di tutte le serate
  registrate in ordine cronologico inverso. Ogni voce riporta data, numero di piatti
  e un bottone per ri-scaricare il PDF in qualsiasi momento.

- **Statistiche — selettore periodo 7 / 30 giorni**
  Aggiunto selettore nella dashboard statistiche per passare da 7 a 30 giorni.
  `getStatisticheAggregate()` accetta ora un parametro `periodoGiorni`.
  Il KPI "Ultimi N giorni" si aggiorna dinamicamente.

- **Statistiche — auto-refresh ogni 3 minuti**
  Le statistiche si aggiornano automaticamente ogni 3 minuti tramite `setInterval`
  con cleanup in `ngOnDestroy`. Utile durante il servizio serale.

- **Homepage — CTA menu più visibile**
  Il bottone "Apri il Menu" è stato spostato sotto la sezione social e ridisegnato:
  sfondo rosso pieno (#C1272D) con animazione `pulse` (expanding ring) per catturare l'attenzione.

---

## [v1.0.0] — 04-03-2026

Prima release pubblica. Vedi `RELEASE_v1.0.0.md` per i dettagli completi.

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
