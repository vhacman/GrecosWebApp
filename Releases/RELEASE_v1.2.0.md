# Release v1.2.0 — 09-03-2026

## Prenotazioni, Asporto e Fix da Microsoft Clarity

Aggiornamento principale con nuovi sistemi di gestione e fix identificati tramite l'analisi delle sessioni Microsoft Clarity.

---

### Fix critici

**Errori IndexedDB Firebase (112 errori in 4 giorni)**
Microsoft Clarity rivelava 111 sessioni con errore:
`refusing to open indexeddb database due to potential corruption`
Fix: sostituita la cache IndexedDB con `memoryLocalCache()` in `app.config.ts`.
Firestore ora usa solo la RAM — zero corruzioni. Nessun dato offline (accettabile per un menu).

**Allergeni non tradotti in inglese**
I nomi degli allergeni erano hardcoded in italiano. Sostituito con `lingua.t('all_N')` — aggiornamento reattivo al cambio lingua.

**Resoconto disponibilità — tutto segnato "Scarso"**
Il controllo `=== null` non catturava `undefined` da Firestore. Corretto con `== null`.

**Statistiche — categoria iniziale mai registrata**
`registraVisitaCategoria()` non veniva chiamata al caricamento. Corretto in `ngOnInit`.

**PWA banner — testo generico**
Aggiunto rilevamento `isIOS` via `userAgent` con istruzioni specifiche per piattaforma.

---

### Nuove funzionalità

**Bottom-sheet dettaglio piatto**
Microsoft Clarity mostrava utenti che toccavano nomi e descrizioni aspettandosi un'azione.
Aggiunto bottom-sheet che si apre al tap su qualsiasi riga del menu: nome, descrizione completa, allergeni (pill), prezzo.

**Allergeni fuori menu**
Il popup dei piatti speciali mostra allergeni e disclaimer:
- "Fatto in casa" → disclaimer variabilità ingredienti
- `surgelato: true` → allergeni da etichetta, nessun disclaimer

**Statistiche — range date e piatti curiosati**
- Selettore libero da/a in sostituzione del fisso 7gg/30gg
- Ranking top-10 "Piatti più curiosati" (registra ogni apertura bottom-sheet)

**Prenotazioni tavoli — sistema completo**
Nuova sezione `/admin/prenotazioni`:
- Navigazione per settimane con calendario popup fino a 12 mesi
- KPI: tavoli, coperti, seggioloni
- Form: nome, telefono, zona, persone, seggioloni, orario (slot fissi o libero), note
- Toggle "arrivata" (solo per oggi e passato), toast real-time per nuove prenotazioni
- PDF giornaliero con KPI e tabella completa

**Statistiche prenotazioni**
KPI tavoli/coperti/seggioloni, grafico prenotazioni affiancato al grafico visite, insight orario/zona/giorno top.

**Modalità estate / inverno** *(spostato da v1.1.0 come funzionalità completata)*
Chiusure/ferie con date dal/al, banner automatico homepage (rosso attiva, arancio entro 14 giorni).

**Chiusura prenotazioni e asporto**
- Toggle "CHIUDI PRENOTAZIONI" / "CHIUDI ASPORTO" per quando il locale è pieno
- Homepage mostra il messaggio di chiusura specifico per quella sera

**PDF asporto**
Riepilogo ordini del giorno: orario, nome, pizze, fritti, note, totale incasso.

**Autocomplete asporto**
Ricerca per nome prodotto con dropdown, inserimento automatico nome+prezzo.
Filtrata per categoria (pizze/fritti/altro) con fuori menu sempre incluso.
