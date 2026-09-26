# Release v2.6.0 — 26-09-2026

## BELLA RAGA, GABRI QUI 😎 — Dashboard Rinnovata, Prenotazioni e Asporto Migliorati

---

### Novità

#### Campanellina riepilogo serata
Nuova campanella rossa accanto a "Visualizza Menù e Prezzi" in dashboard. Apre un pannello
di riepilogo del giorno: stato aggiornamento menù, cucina aperta/chiusa, prenotati oggi
(con dettaglio per zona in popup + totale persone), ordini asporto, modalità stagione e voce
"Disponibilità alimenti". Se il menù non viene aggiornato da ≥ 10 giorni la voce disponibilità
è evidenziata in rosso ("Da aggiornare!"). Dalla voce si apre un popup con le pagine di
disponibilità (Resoconto, Ingredienti, Dolci, Antipasti, Bevande) modificabili inline.

**File modificati:**
- `admin/dashboard/dashboard.ts/.html/.css` — campanella, popup riepilogo, popup
  disponibilità (`::ng-deep` per embeddare le pagine), computed `menuScaduto`,
  `prenPerZona`, `totalePersone`, conteggi prenotati/asporto
- Popup promemoria weekend riconvertito: appare come popup con bottone "Aggiorna ora" →
  `/admin/fuori-menu`, mostrato solo se menù non aggiornato da ≥ 10 giorni

---

#### Modalità estate/inverno in dashboard
Lo switch stagione è stato spostato nella schermata principale (componente `ImpStagione`
embeddato in dashboard), con lo stesso stile della card cucina. Rimosso da Impostazioni.
Il **terrazzo esterno** è ora disponibile come zona anche in modalità inverno.

**File modificati:**
- `admin/dashboard/dashboard.ts/.html/.css` — `<app-imp-stagione>` in dashboard
- `admin/dashboard/impostazioni/stagione/imp-stagione.ts/.html/.css` — css proprio,
  rimosso toggle messaggio homepage, layout come card cucina
- `admin/dashboard/impostazioni/impostazioni.ts/.html` — voce stagione rimossa
- `admin/dashboard/prenotazioni/prenotazioni.ts` — `ZONE_INVERNO`/`ZONE_ESTATE` con terrazzo

---

#### Prenotazioni e Asporto: nuova navigazione
Lista giorni ora orizzontale in alto (scroll orizzontale), con settimane passate collassate
(intestazione cliccabile con chevron) e settimana corrente allineata a sinistra all'apertura.
Ricerca cliente spostata in cima all'header (accanto alla freccia indietro). Ordini/tavoli
raggruppati per orario. Barra azioni fissa in basso con **Aggiungi** e **Condividi con il
personale** (WhatsApp).

**File modificati:**
- `admin/dashboard/prenotazioni/pren-settimane-nav/` e
  `admin/dashboard/asporto/settimane-nav/` — nav orizzontale, collasso settimane passate,
  auto-scroll orizzontale
- `admin/dashboard/prenotazioni/prenotazioni.*` e `admin/dashboard/asporto/asporto.*` —
  layout body a colonna, barra azioni in basso, ricerca, KPI espandibile
- `admin/dashboard/prenotazioni/pren-header/` e `admin/dashboard/asporto/header/` —
  ricerca + freccia indietro nella stessa riga, azioni raggruppate col titolo

---

#### Condivisione WhatsApp immediata
Bottone verde "Condividi con il personale": un tap apre WhatsApp con la lista prenotati
(o ordini asporto) del giorno come messaggio di testo (`wa.me`), ordinata per orario, con
totali. Nessun modal, nessuna immagine da generare.

**File modificati:**
- `admin/dashboard/prenotazioni/prenotazioni.ts` — `condividiWaTesto()`
- `admin/dashboard/asporto/asporto.ts` — `condividiWaTesto()`

---

#### Etichette e capienza per sala
Alle prenotazioni si possono assegnare etichette rapide (Compleanno, Allergie, Abituale,
Celiaco, Bimbi, Esterno), mostrate come chip colorati. Il riepilogo mostra persone e tavoli
per ogni sala (Sala interna, Veranda, Terrazzo).

**File modificati:**
- `InterfacceECostanti/config.ts` — campo `tag?: string[]` su `Prenotazione`
- `admin/dashboard/prenotazioni/prenotazioni.ts` — `TAG_PRENOTAZIONE`, `personePerZona`
- `admin/dashboard/prenotazioni/pren-modal-prenotazione/` — selettore tag
- `admin/dashboard/prenotazioni/pren-row/` — chip tag

---

#### Serate registrate più leggibili (Calcolo Cassa)
Nello storico serate la settimana corrente resta espansa, le passate si comprimono in una
riga con resoconto inline (serate, totale versato, totale fornitori pagati) e si espandono
al click. Aggiunta card riepilogo settimana (versato in banca / fornitori pagati). Corretto
lo storico dei giovedì passati in modalità inverno: i giorni passati usano l'unione dei
giorni di apertura di entrambe le stagioni.

**File modificati:**
- `admin/dashboard/calcolo-cassa/storico-cassa/storico-cassa.ts/.html/.css` — collasso
  settimane, totali per settimana
- `core/utils/calendar-dates.ts` — `generaGiorniAperti` usa set unione per le date passate

---

#### Rubrica anche per l'asporto
Salvando un ordine asporto con un numero di telefono non presente in rubrica, l'app chiede
se aggiungere il contatto (popup Sì/No). Stessa logica già presente nelle prenotazioni.

**File modificati:**
- `admin/dashboard/asporto/modal-ordine/asp-modal-ordine.ts/.html` — popup salva rubrica

---

#### Sezione Statistiche rimossa
La vista Statistiche in dashboard è stata rimossa (non utilizzata). Il tracking analytics
di base resta attivo su Firestore, semplicemente non c'è più una UI che lo mostra.

**File modificati:**
- `app.routes.ts` — route `admin/statistiche` rimossa
- `admin/dashboard/sezione-strumenti/sezione-strumenti.html` — voce rimossa
- `admin/dashboard/statistiche/` — componente eliminato

---

#### Varie
- Rimosso banner "Menù online: aggiornato X giorni fa" dalla dashboard.
- Rimosso il numero tavolo dalle singole prenotazioni (non utilizzato).
- Fix zoom iOS sul campo di ricerca (font 16px).
- Fix testo "Oggi" invisibile sul chip giorno selezionato.
