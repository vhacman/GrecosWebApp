# Grecos Pizzeria — Menu Digitale
## Release Note · v1.3.0 · 09-03-2026

---

## Panoramica

Release che introduce la gestione delle **prenotazioni chiuse** (locale pieno) e **asporto chiuso** (cucina piena),
permettendo di bloccare nuovi ordini/prenotazioni per qualsiasi giorno della settimana direttamente dall'admin.
Include anche il PDF per gli ordini da asporto.

- **URL pubblico:** https://grecospizzeria-47768.web.app
- **Sviluppatrice:** Hacman Viorica Gabriela
- **Deploy:** Firebase Hosting

---

## Novità

### Chiusura prenotazioni (locale pieno)

**Admin - Prenotazioni:**
- Nuovo toggle nell'header: **"CHIUDI PRENOTAZIONI"** (rosso) / **"APRI PRENOTAZIONI"** (verde)
- Funziona per qualsiasi giorno della settimana, non solo i giorni di apertura
- Il toggle è attivo (verde) quando le prenotazioni sono chiuse per il giorno selezionato
- Dati salvati in Firestore nella nuova collezione `prenotazioniChiuse`

**Homepage:**
- Se oggi ha prenotazioni chiuse: banner "Prenotazioni chiuse — Il locale è pieno per questa sera. Prova a prenotare per un giorno successivo!"
- Se c'è un giorno futuro con prenotazioni chiuse: mostra il giorno specifico (es. "Prenotazioni chiuse per Venerdì")
- Priorità sul "messaggio del giorno" e sulle chiusure programmate

### Chiusura asporto (cucina piena)

**Admin - Asporto:**
- Nuovo toggle nell'header: **"CHIUDI ASPORTO"** (arancione) / **"APRI ASPORTO"** (verde)
- Funziona per qualsiasi giorno della settimana
- Dati salvati in Firestore nella nuova collezione `asportoChiuso`

**Homepage:**
- Messaggio visualizzato solo se il cliente chiama per prenotare (il banner non è ancora implementato in homepage per asporto - visualizzato solo nell'admin)

### PDF Asporto

**Admin - Asporto:**
- Nuovo bottone PDF nell'header (icona 📄)
- Genera PDF con:
  - Intestazione Grecos + data
  - KPI: numero ordini, totale pizze, totale fritti, incasso totale
  - Tabella: orario, nome cliente, pizze, fritti, note
  - Sfondo alternato per righe

---

## Modifiche tecniche

### Firestore

Nuove collezioni:
- `prenotazioniChiuse`: documenti con campo `data` (YYYY-MM-DD)
- `asportoChiuso`: documenti con campo `data` (YYYY-MM-DD)

### File modificati

- `src/app/services/config.ts`: aggiunte interfacce `PrenotazioneChiusa`, `AsportoChiuso` e metodi CRUD
- `src/app/admin/prenotazioni/prenotazioni.ts/html/css`: toggle e logica
- `src/app/admin/asporto/asporto.ts/html/css`: toggle, PDF e logica
- `src/app/public/home/home.ts/html`: visualizzazione messaggi

---

## Note di deploy

1. Le nuove collezioni Firestore (`prenotazioniChiuse`, `asportoChiuso`) vengono create automaticamente al primo utilizzo
2. Nessuna migrazione dati necessaria
3. Compatibile con versione precedente (funzionalità aggiuntive, non breaking)
