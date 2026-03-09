# Grecos Pizzeria — Menu Digitale
## Release Note · v1.2.0 · 09-03-2026

---

## Panoramica

Release maggiore che introduce la gestione completa delle **prenotazioni tavoli**,
la **modalità estate/inverno** con cambio automatico dei giorni di apertura e delle
zone, le **chiusure/ferie** con filtraggio automatico dal calendario e banner homepage,
e l'integrazione dei dati di prenotazione nella **dashboard statistiche**.

- **URL pubblico:** https://grecospizzeria-47768.web.app
- **Sviluppatrice:** Hacman Viorica Gabriela
- **Deploy:** Firebase Hosting

---

## Prenotazioni tavoli

### Funzionalità

Nuova sezione admin `/admin/prenotazioni` raggiungibile dalla dashboard.

**Navigazione giorni:**
- Giorni di apertura raggruppati per settimana (Gio/Ven/Sab/Dom in modalità estate; Ven/Sab/Dom in modalità inverno)
- Chips verticali con nome giorno + data, chip "Oggi" evidenziata
- Popup calendario mensile (pulsante 📅 nell'header) per saltare a qualsiasi settimana dell'anno
- Calendario dinamico: genera sempre fino al 31/12 dell'anno successivo, si aggiorna ogni anno automaticamente
- Avviso "Data troppo lontana" se si supera il limite

**Lista prenotazioni:**
- KPI bar: numero tavoli, coperti totali, seggioloni (se presenti)
- Ogni riga mostra: nome, orario, persone, seggioloni, zona, telefono (link `tel:`), note, chi ha inserito la prenotazione
- Toggle "arrivata" (cerchio/spunta): abilitato solo per date passate o oggi. Per date future mostra un'icona orologio e il pulsante è disabilitato
- Eliminazione con conferma via modal

**Aggiunta prenotazione:**
- Bottom-sheet form con: nome (obbligatorio), telefono (opzionale), zona, persone, seggioloni, orario (slot fissi 19:00–23:00 a passi di 15 min, o "orario libero" con `input[type=time]`), note
- Al salvataggio registra l'email dell'admin come `creatoDa`

**Toast notifiche real-time:**
- Se un'altra sessione admin aggiunge una prenotazione per il giorno visualizzato, compare un toast in alto con nome, orario, zona e chi l'ha inserita
- Scompare automaticamente dopo 5 secondi
- Tecnica: confronto `createdAt.seconds * 1000 > initTime` per distinguere prenotazioni nuove da quelle caricate all'apertura della pagina

**PDF giornaliero:**
- Pulsante PDF nell'header: genera un PDF A4 con intestazione rossa Grecos, KPI riepilogo, tabella prenotazioni (orario, nome, persone+seggioloni, zona, telefono, note)

---

## Modalità estate / inverno

Toggle in `/admin/impostazioni` che commuta in modo sincrono:

| Parametro | Inverno | Estate |
|---|---|---|
| Giorni di apertura | Ven, Sab, Dom | Gio, Ven, Sab, Dom |
| Zone prenotazione | Sala interna, Veranda | Terrazzo esterno, Veranda |

Attivando la modalità **estate**:
- Aggiunge il giovedì (4) ai giorni di apertura in `config/orari`
- Imposta automaticamente il messaggio del giorno in `config/messaggio`:
  *"Informiamo i nostri clienti che siamo aperti anche il Giovedì"* con `attivo: true` e `scadenza` = data odierna + 20 giorni
- Il banner scompare automaticamente dopo 20 giorni senza intervento manuale

Attivando la modalità **inverno**:
- Rimuove il giovedì dai giorni di apertura
- Disattiva il messaggio del giorno (`attivo: false`)

---

## Chiusure / ferie

Nuova sezione admin `/admin/chiusure` raggiungibile da Strumenti → Chiusure.

**Interfaccia:**
- Lista periodi di chiusura con indicatore colorato:
  - 🔴 Rosso = chiusura attiva oggi
  - 🟡 Arancio = chiusura futura programmata
  - ⚫ Grigio = chiusura già conclusa (opacità ridotta)
- Ogni voce mostra: periodo (es. "15 mag – 22 mag 2026"), motivo, numero di giorni
- Aggiunta tramite bottom-sheet form: dal, al, motivo
- Eliminazione con conferma

**Effetti automatici:**
- Le date chiuse vengono filtrate da `giorniAperti` in prenotazioni → spariscono dal calendario
- Homepage:
  - Banner rosso *"Il ristorante è chiuso fino al [data] — [motivo]"* se oggi è dentro una chiusura
  - Banner arancio *"Chiusi dal [data] al [data] — [motivo]"* se la prossima chiusura è entro 14 giorni

**Modello dati Firestore:**
```
collezione: chiusure
documento: {
  dal:       string;   // YYYY-MM-DD
  al:        string;   // YYYY-MM-DD
  motivo:    string;
  createdAt: Timestamp;
}
```

---

## Statistiche — integrazione prenotazioni

La dashboard `/admin/statistiche` carica ora in parallelo i dati visite (Firestore `statistiche/`) e i dati prenotazioni (collezione `prenotazioni`).

**Nuovi KPI prenotazioni** (in verde):
- Tavoli totali nel periodo selezionato
- Coperti totali
- Media coperti/tavolo (1 decimale)
- Seggioloni totali (se > 0)

**Nuovo grafico "Prenotazioni giornaliere"** — barre verdi, mostrato solo se ci sono prenotazioni nel periodo. Stesso design scrollabile del grafico visite.

**Chiusure sui grafici:**
- Entrambi i grafici segnalano i giorni chiusi con una barra a righe diagonali grigie + icona porta
- Legenda: "Oggi" (rosso), "Chiuso" (pattern)

**Nuovi Insight prenotazioni:**
- Orario più prenotato (es. "20:00 · 5 tavoli")
- Zona preferita (es. "Sala interna · 8 prenotazioni")
- Giorno della settimana più prenotato

---

## Modello dati — nuove interfacce

### `Prenotazione` (config.ts)

```typescript
export interface Prenotazione {
  id?:         string;
  nome:        string;
  telefono:    string;
  zona:        string;        // "Sala interna" | "Veranda" | "Terrazzo esterno"
  persone:     number;
  seggioloni:  number;
  orario:      string;        // "HH:MM"
  data:        string;        // "YYYY-MM-DD"
  note:        string;
  arrivata:    boolean;
  creatoDa:    string;        // email admin
  createdAt:   Timestamp;
}
```

### `Chiusura` (config.ts)

```typescript
export interface Chiusura {
  id?:    string;
  dal:    string;   // "YYYY-MM-DD"
  al:     string;   // "YYYY-MM-DD"
  motivo: string;
}
```

### `Stagione` (config.ts)

```typescript
export interface Stagione {
  estate: boolean;
}
```

### `StatistichePrenotazioni` (config.ts)

```typescript
export interface StatistichePrenotazioni {
  totalePrenotazioni: number;
  totaleCoperti:      number;
  totaleSeggioloni:   number;
  mediaCoperti:       number;
  orarioTop:          { orario: string; count: number } | null;
  zonaTop:            { zona: string; count: number } | null;
  giornoTop:          { nome: string; count: number } | null;
  perGiorno:          { data: string; prenotazioni: number; coperti: number }[];
}
```

### `Messaggio` — campo aggiunto (config.ts)

```typescript
scadenza?: string;  // YYYY-MM-DD — il banner si nasconde automaticamente dopo questa data
```

---

## Indici Firestore

Aggiunto indice composito richiesto per la query prenotazioni per data+orario:

```json
{
  "collectionGroup": "prenotazioni",
  "fields": [
    { "fieldPath": "data",   "order": "ASCENDING" },
    { "fieldPath": "orario", "order": "ASCENDING" }
  ]
}
```

---

## File modificati

```
src/app/services/config.ts
  ← Stagione, Chiusura, Prenotazione (telefono, zona, creatoDa), Messaggio (scadenza)
  ← StatistichePrenotazioni, getStatistichePrenotazioni()
  ← getStagione(), setStagione(), getChiusure(), addChiusura(), deleteChiusura()
  ← getPrenotazioni(), addPrenotazione(), deletePrenotazione(), toggleArrivata()

src/app/app.routes.ts
  ← route /admin/prenotazioni, /admin/chiusure

src/app/admin/prenotazioni/prenotazioni.ts   ← nuovo componente
src/app/admin/prenotazioni/prenotazioni.html ← nuovo componente
src/app/admin/prenotazioni/prenotazioni.css  ← nuovo componente

src/app/admin/chiusure/chiusure.ts           ← nuovo componente
src/app/admin/chiusure/chiusure.html         ← nuovo componente
src/app/admin/chiusure/chiusure.css          ← nuovo componente

src/app/admin/impostazioni/impostazioni.ts   ← stagione toggle + messaggio automatico
src/app/admin/impostazioni/impostazioni.html ← stagione card
src/app/admin/impostazioni/impostazioni.css  ← stagione card styles

src/app/admin/statistiche/statistiche.ts     ← prenotazioni + chiusure
src/app/admin/statistiche/statistiche.html   ← nuovi KPI, grafico, insight
src/app/admin/statistiche/statistiche.css    ← stili nuovi blocchi

src/app/admin/dashboard/sezione-strumenti/sezione-strumenti.html
  ← link "Chiusure"

src/app/public/home/home.ts                  ← chiusureAttiva, chiusuraProssima getters
src/app/public/home/home.html                ← banner chiusura (rosso/arancio)
src/app/public/home/home.css                 ← .messaggio-chiusura styles

firestore.indexes.json                       ← indice prenotazioni (data, orario)
```

---

## Script di migrazione / seed eseguiti

| Script | Descrizione |
|---|---|
| `scripts/seed-prenotazioni-domenica.js` | 7 prenotazioni reali domenica 08-03-2026 |
| `scripts/migrate-tel-prenotazioni.js` | Migrazione numeri di telefono da `note` a `telefono` |
| `scripts/patch-zona-domenica.js` | Impostato `zona: "Sala interna"` per le prenotazioni dell'8/3 |
| `scripts/fix-prenotazioni-data.js` | Correzione data errata durante i test |

---

*Documento generato il 09-03-2026 — Grecos Pizzeria, Ladispoli (RM)*
