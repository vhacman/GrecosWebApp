# Grecos Pizzeria — Menu Digitale
## Release Note · v1.1.x · 09-03-2026

---

## Panoramica

Serie di patch e nuove funzionalità rilasciate nella settimana 04–09 marzo 2026,
basate in parte sull'analisi comportamentale degli utenti tramite **Microsoft Clarity**
(sessioni 07–09 Mar) e su feedback operativi emersi durante il servizio serale.

- **URL pubblico:** https://grecospizzeria-47768.web.app
- **Sviluppatrice:** Hacman Viorica Gabriela
- **Deploy:** Firebase Hosting

---

## v1.1.2 — 09-03-2026

### Righe menu cliccabili — bottom-sheet dettaglio piatto

I dati Clarity mostravano utenti che toccavano nomi e descrizioni dei piatti
aspettandosi un'azione. Ogni riga del menu è ora cliccabile e apre un
**bottom-sheet** che sale dal basso con:

- Nome del piatto (con badge vegano/surgelato)
- Descrizione completa
- Allergeni EU espansi come pill individuali
- Prezzo in evidenza

Il vecchio componente `AllergeniExpand` inline (accordion per allergene) è stato
rimosso: gli allergeni sono ora accessibili solo dal modal. Ogni riga mostra il
testo *"tocca per dettagli e allergeni"* (IT) / *"tap for details & allergens"* (EN)
come invito all'interazione.

---

## v1.1.1 — 09-03-2026

### Fix — Errori IndexedDB Firebase (112 errori in 4 giorni)

L'analisi Clarity rivelava 111 sessioni con errore:
`refusing to open indexeddb database due to potential corruption`

Firestore usava IndexedDB per la cache offline persistente. Su alcuni
browser/dispositivi mobile il database si corrompeva, generando l'errore
a ogni visita successiva.

**Fix:** sostituita la cache IndexedDB con `memoryLocalCache()` in `app.config.ts`.
Firestore usa ora solo la RAM — zero corruzioni. Conseguenza accettabile:
nessun dato offline (irrilevante per un menu di ristorazione).

---

## v1.1.0 — 08-03-2026

### Fix

**Resoconto disponibilità — tutto segnato "Scarso"**
I piatti senza campo `disponibili` in Firestore restituivano `undefined`.
Il controllo `=== null` non catturava `undefined`. Corretto con `== null`.

**Statistiche — categoria iniziale mai registrata**
`registraVisitaCategoria()` veniva chiamata solo al cambio tab, mai al caricamento.
`antipasti` risultava sempre zero. Aggiunta la chiamata in `ngOnInit()`.

**Statistiche — grafico 30 giorni sfasato**
Con 30 barre le etichette si sovrapponevano. Il grafico è ora scrollabile
orizzontalmente con `min-width` dinamico. Etichette a 30 giorni in formato `gg/mm`.

**PWA banner — testo generico**
Il banner di installazione mostrava testo identico per iOS e Android.
Corretto con rilevamento `isIOS` via `userAgent`.

### Novità

**PDF export** disponibile in tre punti: resoconto disponibilità, QR code (modal
dashboard), storico serate. Tutti generati con jsPDF, offline-ready, senza finestre
aggiuntive. Il PDF del resoconto include gruppi, badge di stato e intestazione Grecos.

**Storico serate** — al download PDF viene salvato automaticamente uno snapshot in
Firestore (collezione `storici`) con data, numero piatti e possibilità di
ri-scaricare il PDF in qualsiasi momento.

**Statistiche** — selettore periodo 7/30 giorni, auto-refresh ogni 3 minuti
tramite `setInterval` con cleanup in `ngOnDestroy`.

**Homepage** — CTA "Apri il Menu" spostato sotto la sezione social, sfondo rosso
pieno `#C1272D` con animazione `pulse` per catturare l'attenzione.

---

## Aggiornamenti Modello Dati

### `FuoriMenu` — nuovi campi

```typescript
allergeni?:  number[];   // allergeni EU indicativi (opzionale)
surgelato?:  boolean;    // true per prodotti industriali da etichetta
```

Il campo `allergeni` è **opzionale** per retrocompatibilità con i documenti
precedenti alla migrazione.

### Migrazione Firestore eseguita — 09-03-2026

Script `scripts/migrate-fuori-menu-allergeni.js` ha aggiornato tutti i 46 documenti
della collezione `fuoriMenu`:

- **39 aggiornati** con allergeni plausibili per categoria
- **7 già OK** (inseriti con allergeni dai precedenti script di seed)

Logica applicata:
- **Prodotti fatti in casa** → allergeni indicativi + disclaimer nel popup:
  *"Fatto in casa — ingredienti possono variare. Chiedere al personale per allergie specifiche."*
- **Prodotti surgelati industriali** (`surgelato: true`) → allergeni da etichetta,
  nessun disclaimer variazione. Es: *Sfere di pollo alla cacciatora panate*
  → `[1, 2, 3, 4, 7, 8, 9, 10, 12, 14]`

### Popup fuori menu aggiornato

Il popup dettaglio ora mostra:
1. Nome piatto
2. Descrizione
3. Allergeni come pill (se presenti)
4. Nota disclaimer *"fatto in casa"* — solo se `!surgelato`
5. Prezzo

---

## File modificati

```
src/app/core/models/fuori-menu.ts
src/app/core/models/menu-item.ts (invariato)
src/app/core/i18n/translations.it.ts
src/app/core/i18n/translations.en.ts
src/app/shared/item-detail-modal/           ← nuovo componente
src/app/public/menu/antipasti/
src/app/public/menu/pizze-rosse/
src/app/public/menu/pizze-bianche/
src/app/public/menu/focacce-calzoni/
src/app/public/menu/dolci/
src/app/public/menu/bevande/
src/app/public/menu/fuori-menu-sezione/
src/styles.css
scripts/migrate-fuori-menu-allergeni.js     ← nuovo script
scripts/seed.js                             ← fuoriMenuItem() aggiornata + allergeni
```

---

## Legenda allergeni EU (riferimento)

| ID | Allergene | ID | Allergene |
|---|---|---|---|
| 1 | Glutine | 8 | Frutta a guscio |
| 2 | Crostacei | 9 | Sedano |
| 3 | Uova | 10 | Senape |
| 4 | Pesce | 11 | Sesamo |
| 5 | Arachidi | 12 | Anidride solforosa |
| 6 | Soia | 13 | Lupini |
| 7 | Latte | 14 | Molluschi |

---

*Documento generato il 09-03-2026 — Grecos Pizzeria, Roma*
