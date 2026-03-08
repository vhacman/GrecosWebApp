# Changelog — Grecos Pizzeria Menu Digitale

Tutte le modifiche rilevanti al progetto sono documentate qui.
Formato: `[vX.Y.Z] — GG-MM-AAAA`

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
