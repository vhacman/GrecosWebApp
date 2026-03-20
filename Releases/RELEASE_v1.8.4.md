# Release v1.8.4 — 17-03-2026

## Fix Prenotazioni e Pagina Manutenzione

---

### Fix critico — Pulsanti prenotazioni non funzionanti

**Problema:** Tutti i pulsanti della pagina prenotazioni (Aggiungi, Non prenotati, Chiudi prenotazioni,
Calendario, modifica/elimina/arrivata su ogni riga) non aprivano i relativi modal in produzione.

**Causa:** `Prenotazioni` usava `ViewEncapsulation.Emulated` (default Angular), che aggiunge attributi
`[_ngcontent-ng-cXXXX]` ai selettori CSS. I componenti modal figli usano `ViewEncapsulation.None`
e non ricevono questi attributi — quindi `.overlay { position: fixed }` e `.modal-bottom { position: fixed }`
in `prenotazioni.css` non venivano mai applicati.

**Fix:** Aggiunto `encapsulation: ViewEncapsulation.None` a `Prenotazioni` — CSS ora globale, raggiunge i figli.

---

### Fix — Query Firestore prenotazioni senza indice composito

`getPrenotazioni()` usava `orderBy('orario')` in combinazione con `where('data', '==', ...)`.
Firestore richiede un indice composito per query con `where` + `orderBy` su campi diversi — assente nel progetto.

**Fix:** Rimosso `orderBy` dalla query; ordinamento spostato lato client con `Array.sort()`.

---

### Fix — Errori Firestore visibili nel form prenotazione

Il metodo `salva()` in `pren-modal-prenotazione` non gestiva gli errori Firestore: se il salvataggio
falliva, il form spariva silenziosamente senza feedback.

**Fix:** Aggiunto `try/catch` + signal `errore` che mostra il messaggio di errore direttamente nel form.

---

### Pagina Manutenzione

Nuovo componente `/admin/manutenzione` accessibile a tutti (anche non autenticati).
Mostra un messaggio di manutenzione temporanea al pubblico.

- Toggle **Impostazioni → Manutenzione** per attivare/disattivare la modalità
- `maintenanceGuard` protegge le route admin durante la manutenzione
- Gli admin possono aggirare la modalità impostando `man_bypass` in `sessionStorage`

---

### Fix — Storico Serate path CSS errato

`storico-serate.ts` puntava a `./storico-serate.css` (file rimosso/spostato).
Corretto in `./css/storico-serate.css`.
