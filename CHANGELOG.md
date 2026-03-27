# Changelog — Grecos Pizzeria Menu Digitale

Tutte le modifiche rilevanti al progetto sono documentate qui.
Formato: `[vX.Y.Z] — GG-MM-AAAA`

---

## [v2.1.0] — 27-03-2026

### Novità

- **Modifica serata cassa**
  Dallo storico cassa (`/admin/storico-cassa`) ogni card ha ora un bottone matita accanto
  al cestino. Il tap apre un bottom sheet pre-compilato con totale scontrino, POS e lista
  fornitori. I valori derivati (contanti, totale fornitori, totale da versare) si ricalcolano
  in tempo reale mentre si digita — stesso pattern di `CalcoloCassa`.
  Al salvataggio viene chiamato `updateDoc` su Firestore; la card si aggiorna automaticamente
  grazie al real-time listener di `toSignal`.

- **Conferma eliminazione serata cassa**
  Il bottone cestino nello storico cassa non elimina più direttamente.
  Appare un bottom sheet "Eliminare questa serata?" con **Annulla** / **Elimina**.
  Cliccando sull'overlay il modal si chiude senza fare nulla.

- **Storico cassa raggruppato per settimana**
  Le serate sono ora divise in gruppi con intestazione testuale: *Questa settimana*,
  *Settimana scorsa* o l'intervallo lunedì–domenica (es. `17 mar – 23 mar`).
  Implementato tramite computed signal `chiusurePerSettimana` che raggruppa per chiave
  ISO del lunedì di ogni settimana.

- **Modifica prenotazione**
  Il bottone matita in ogni riga prenotazione apre il modal pre-compilato con tutti i dati
  esistenti (nome, telefono, zona, persone, seggioloni, orario, note).
  Al salvataggio viene chiamato `updatePrenotazione` invece di `addPrenotazione`:
  data originale e stato "arrivata" vengono preservati.

- **Numero persone con range (es. 33/35)**
  Nel form prenotazione è possibile specificare opzionalmente un numero massimo di persone
  oltre a quello minimo. La visualizzazione mostra `persone/personeMax` (es. `4/5`).
  Il campo è opzionale: se non compilato si mostra solo il numero esatto.

---

## [v2.0.2] — 23-03-2026

### Novità

- **Modifica preconto salvato**
  Dallo storico preconti (`/admin/preconti-storia`) ogni preconto espanso mostra ora
  un bottone "Modifica" (bordo oro) accanto a "Elimina". Il tap naviga a
  `/admin/preconto?id=<docId>`: il componente legge il query param in `ngOnInit`,
  recupera il documento da Firestore con `getPrecontoById` e pre-popola il form
  (numero tavolo, tutte le voci, eventuale sconto).
  Un banner arancione "Stai modificando un preconto salvato" è visibile durante
  tutta la sessione di modifica.
  Al salvataggio viene chiamato `updatePreconto` invece di `addPreconto`:
  data e ora originali sono preservate, lo stato di pagamento già segnato non viene
  toccato. Dopo il salvataggio l'utente viene riportato automaticamente allo storico.
  Il bottone "Azzera" in modalità modifica funge da "Annulla" e torna allo storico
  senza salvare.

---

## [v2.0.1] — 20-03-2026

### Fix

- **Errore Firestore campo telefono vuoto nell'asporto**
  Creare un ordine asporto senza inserire il numero di telefono causava un `FirebaseError:
  Unsupported field value: undefined` e il salvataggio falliva silenziosamente.
  Fix: il campo `telefono` viene omesso con spread condizionale se vuoto, invece di essere
  passato come `undefined` a `addDoc`.

---

## [v2.0.0] — 20-03-2026

### Novità

- **Rubrica clienti**
  Nuova sezione accessibile da Strumenti → Rubrica. Permette di salvare nome e telefono
  dei clienti abituali. Autocomplete nel modal ordine asporto (con bottone "Salva in rubrica")
  e nel form prenotazioni. Badge "In rubrica" se il cliente è già presente.

- **Asporto — campo telefono**
  Il modal ordine asporto include ora un campo telefono con autocomplete rubrica.
  Selezionando un cliente dalla rubrica vengono precompilati sia nome che numero.

- **Preconto — storico integrato, stampa e guida**
  Il bottone "Storico" nell'header del preconto apre la pagina storico preconti direttamente
  (rimosso dalla dashboard). Nuovo bottone stampa che invoca `window.print()` con CSS B&N
  dedicato: solo le voci del preconto vengono stampate, tutto il resto è nascosto.
  Bottone `@info` con 6 punti che spiegano il funzionamento del preconto.

- **Storico preconti — bottone @info**
  Popup informativo con 5 punti che spiega come leggere lo storico, la segnatura pagamento
  e come usare il filtro data.

- **Statistiche — KPI preconti e rubrica**
  Nuova sezione KPI preconti: incasso totale, numero preconti, media per tavolo.
  Breakdown contanti / POS / da segnare. Grafico incasso giornaliero a barre (colore oro).
  KPI rubrica: conteggio clienti salvati.

- **Strumenti — QR unificato**
  Tutti i QR code (menu, dolci, Google Reviews, TripAdvisor) sono ora accessibili da
  un unico bottone "QR Code" che apre un picker bottom-sheet. Interfaccia più pulita.

- **Rubrica — bottone @info**
  Popup con 4 punti che spiega come funziona la rubrica e dove viene usata.

### Fix

- **Tavolo prenotazione — cancellazione non funzionante**
  Premendo X per rimuovere il numero tavolo da una prenotazione, il campo non veniva
  eliminato da Firestore (Firestore ignora `undefined` in `updateDoc`).
  Fix: nuovo metodo `cancelTavoloPren()` che usa `deleteField()` per rimuovere fisicamente
  il campo dal documento Firestore.

---

## [v1.9.0] — 18-03-2026

### Novità

- **QR Recensioni Google e TripAdvisor**
  Sezione Strumenti ridisegnata con modal multi-tipo: QR per menu, dolci, Google Reviews e
  TripAdvisor (generato dinamicamente via libreria `qrcode`). Ogni QR scaricabile come PNG.

- **Impostazioni → Contatti — campi link recensioni**
  Nuovi campi URL per Google Reviews e TripAdvisor, ciascuno con bottone "Condividi"
  (Web Share API su mobile, fallback clipboard su desktop).

- **Homepage — sezione "Lascia una recensione"**
  Card con link diretto a Google Reviews e TripAdvisor, visibile solo se i link sono configurati.

- **Card menu unificata con chip categorie**
  Sostituisce bottone menu + card separata con un unico blocco: header rosso CTA +
  chip per ogni categoria che linkano direttamente a `/menu?cat=<categoria>`.
  Accesso immediato a qualsiasi sezione del menu senza passare dalla homepage.

- **Bottone "Manuale d'Uso" meno prominente**
  Stile aggiornato a testo grigio senza bordo per non competere con le CTA principali.

### Ottimizzazione

- **IntersectionObserver sulla barra bottom homepage**
  Sostituisce `HostListener('scroll')` con IntersectionObserver per rilevare quando l'hero
  è fuori dal viewport. Nessun overhead sul thread principale durante lo scroll.

---

## [v1.8.5] — 17-03-2026

### Fix

- **Disponibilità (antipasti, dolci, bevande) — Lista vuota per indice Firestore mancante**
  Le pagine disponibilità mostravano la lista completamente vuota in produzione.
  Causa: `getCategoria()`, `getCategoriaAdmin()`, `getFuoriMenuByCategoria()`, `getFuoriMenu()` e
  `getFuoriMenuAdmin()` in `menu.ts` usavano `where(...) + orderBy('ordine')` su campi diversi.
  Firestore richiede un indice composito per questa combinazione — assente nel progetto.
  Fix: rimosso `orderBy('ordine')` da tutte le query con `where`, ordinamento spostato lato client
  con `Array.sort((a, b) => (a.ordine ?? 0) - (b.ordine ?? 0))`.
  Lo stesso approccio era già stato adottato per `getPrenotazioni()` in v1.8.4.

---

## [v1.8.4] — 17-03-2026

### Fix

- **Prenotazioni — Fix critico pulsanti modal**
  Tutti i pulsanti della pagina prenotazioni (Aggiungi, Non prenotati, Chiudi prenotazioni, Calendario,
  modifica/elimina/arrivata su ogni riga) non aprivano i relativi modal in produzione.
  Causa: `Prenotazioni` usava `ViewEncapsulation.Emulated` (default Angular), che aggiunge attributi
  `[_ngcontent-ng-cXXXX]` ai selettori CSS. I componenti modal figli usano `ViewEncapsulation.None`
  e non ricevono questi attributi — quindi `.overlay { position: fixed }` e `.modal-bottom { position: fixed }`
  in `prenotazioni.css` non venivano mai applicati.
  Fix: aggiunto `encapsulation: ViewEncapsulation.None` a `Prenotazioni` — CSS ora globale, raggiunge i figli.

- **Prenotazioni — Query Firestore senza indice composito**
  `getPrenotazioni()` usava `orderBy('orario')` in combinazione con `where('data', '==', ...)`.
  Firestore richiede un indice composito per query con `where` + `orderBy` su campi diversi.
  Fix: rimosso `orderBy` dalla query, ordinamento spostato lato client con `Array.sort()`.

- **Prenotazioni — Errori Firestore visibili nel form**
  Il metodo `salva()` in `pren-modal-prenotazione` non gestiva gli errori Firestore.
  Aggiunto `try/catch` + signal `errore` che mostra il messaggio direttamente nel form.

- **Pagina Manutenzione**
  Nuovo componente `/admin/manutenzione` + `maintenanceGuard` sulle route admin.
  Toggle in Impostazioni → Manutenzione per attivare/disattivare la modalità.
  Gli admin con bypass `man_bypass` in sessionStorage accedono sempre.

- **Storico Serate — Path CSS corretto**
  `storico-serate.ts` puntava a `./storico-serate.css` (rimosso). Corretto in `./css/storico-serate.css`.

---

## [v1.8.3] — 16-03-2026 *(interno — non pubblicato)*

### Documentazione / Refactoring

- **Commenti inline su tutti i componenti Angular**
  Aggiunti commenti `//` e blocchi `/* */` su tutti i file `.ts` e `<!-- -->` su tutti i file `.html`
  della cartella `src/app/` (esclusi i `.spec.ts`). Coprono app root, guards, models, pipes, utils,
  servizi, i18n, componenti pubblici (home, menu, fuori menu, filtri, categorie) e l'intero pannello
  admin (login, dashboard, gestione menu, asporto, prenotazioni, disponibilità, impostazioni, chiusure,
  resoconto, calcolo cassa, statistiche, storico serate). Nessuna modifica alla logica applicativa.

- **Fix `styleUrl` prenotazioni**
  `prenotazioni.ts` puntava a `./css/prenotazioni.css` (file rimosso in v1.8.1). Corretto in `./prenotazioni.css`.

> ℹ️ Questa versione non è presente in `releases.constants.ts` — nessun popup novità per gli admin.

---

## [v1.8.2] — 16-03-2026

### Fix

- **Disponibilità — Soglia "Scarso" corretta a ≤ 5**
  La soglia `SOGLIA_SCARSO` era stata impostata a `10` in v1.8.1, rendendo "Scarso" troppo ampio (1–9).
  Corretta a `6`: ora `1–5 → Scarso`, `≥ 6 → OK`.
  Applicato a `disponibilita-antipasti.ts`, `disponibilita-bevande.ts`, `disponibilita-dolci.ts`.

---

## [v1.8.1] — 16-03-2026

### Novità

- **Asporto — Sconto per ordine**
  Tap sulla riga di un ordine apre un bottom-sheet "Sconto ordine": mostra il totale lordo e
  un campo "Totale da incassare". Lo sconto viene calcolato automaticamente (lordo − da incassare)
  e salvato in Firestore (`sconto` in `OrdineAsporto`). Il totale barrato è visibile nella lista.
  Possibilità di rimuovere lo sconto con bottone dedicato. Il KPI "Totale incasso" della giornata
  tiene conto di tutti gli sconti applicati.

- **Asporto — Toggle "Consegnato"**
  Ogni riga ordine mostra un pulsante cerchio/spunta a sinistra. Tap segna/desegna l'ordine
  come consegnato — stato persistito in Firestore (`consegnato` in `OrdineAsporto`).
  Le righe consegnate hanno uno stile visivo distinto.

- **Calcolo Cassa — Contanti calcolati automaticamente**
  Il campo "Soldi incassati in contanti" è stato sostituito con "Totale scontrino di chiusura cassa".
  I contanti vengono calcolati automaticamente come `scontrino − POS` e mostrati in tempo reale.
  Semplifica l'inserimento: basta digitare il totale dello scontrino fiscale e l'importo POS.
  PDF e Firestore aggiornati con la nuova nomenclatura e il campo `totaleInCassa`.

- **Disponibilità — Soglia "Scarso" portata a 10**
  Il badge "Poca disponibilità" nel menu pubblico ora compare solo se `disponibili < 10`
  (prima compariva per qualsiasi quantità > 0). Applicato a antipasti, bevande, dolci.
  Nel resoconto disponibilità, quantità ≥ 10 mostra badge "OK" invece di "Scarso".
  `disponibiliFromStato('ok')` ora restituisce 10 di default così l'admin può subito
  affinare la quantità esatta senza dover ridigitare da zero.

### Refactoring

- **CSS suddivisi in sottocartelle**
  I file CSS monolitici di `statistiche` e `storico-serate` sono stati spostati in sottocartelle
  `css/` (stessa struttura già adottata da `asporto`). `asporto-list.css` e `asporto-modal.css`
  spostati anch'essi in `css/`. Nessuna modifica visiva — solo organizzazione del codice.

---

## [v1.8.0] — 15-03-2026

### Novità

- **Layout desktop e tablet completamente responsivo**
  Rimosso l'effetto "telefono nel browser" (bordo arrotondato + sfondo marrone + max-width 900px)
  che rendeva il sito goffo su PC. Il contenuto ora occupa l'intero schermo.
  Homepage su desktop (≥ 1024px): layout a due colonne CSS Grid — brand e info a sinistra,
  CTA e menu a destra. Su tablet (≥ 768px) padding ottimizzati, logo più grande, font più ampi.

- **Card "Visualizza Menù e Prezzi" nella dashboard admin**
  Aggiunta come primo elemento della dashboard (prima del toggle cucina), con stile bianco e
  bordo rosso. Permette all'admin di aprire il menu pubblico direttamente dalla dashboard
  per compilare il preconto. Sottotitolo: "Consulta il menù per compilare il preconto".

- **Calcolo Cassa protetto da password**
  Il link "Calcolo Cassa" nella dashboard è ora protetto: al click appare un bottom-sheet che
  chiede la password prima di navigare. La password predefinita è `grecos2026`.
  Archiviata come hash SHA-256 in Firestore (`config/cassa → passwordHash`) — mai in chiaro.
  Al primo accesso (documento non ancora presente), il sistema confronta contro l'hash del
  default senza richiedere setup iniziale.

- **Cambio password Calcolo Cassa da Impostazioni**
  Nuova sottosezione `🔒 Password Calcolo Cassa` in `Impostazioni`. Richiede la vecchia
  password (verificata lato client tramite hash), la nuova (minimo 6 caratteri) e la conferma.
  La nuova password viene hashata (SHA-256) prima di essere salvata in Firestore.

- **Rimozione "Visualizza Menù" e "Calcolo Cassa" dalla sezione Strumenti**
  Entrambe le voci sono state rimosse da Strumenti: il menu è raggiungibile dalla card
  in dashboard, il Calcolo Cassa dal pannello principale con protezione password.

### Fix

- **Barra flottante homepage non centrata su tablet** — `transform: translateX(-50%)` applicato
  correttamente sulla barra sticky, con stato nascosto combinato `translateX(-50%) translateY(...)`.
  Su desktop (≥ 1024px) la barra è nascosta del tutto (non necessaria col layout a due colonne).

---

## [v1.7.0] — 14-03-2026

### Novità

- **Popup chiusura prenotazioni/asporto con messaggi dinamici**
  I popup mostrati in homepage quando le prenotazioni o l'asporto sono chiusi per la serata
  ora leggono il testo dinamico dalla configurazione admin, invece di mostrare un messaggio
  fisso. I messaggi rifletono in tempo reale le impostazioni della serata.

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
