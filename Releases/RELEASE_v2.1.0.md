# Release v2.1.0 — 27-03-2026

## Modifica Cassa, Conferma Eliminazione, Storico per Settimana, Modifica Prenotazioni

---

### Novità

#### Modifica serata cassa
Dallo storico cassa (`/admin/storico-cassa`) ogni card mostra ora un bottone matita
accanto al cestino. Il tap apre un bottom sheet pre-compilato con:
- Totale scontrino
- POS incassato
- Lista fornitori (aggiungere/rimuovere singoli fornitori)

I valori derivati (contanti, totale fornitori, totale da versare) si ricalcolano
in tempo reale mentre si digita — stesso pattern di `CalcoloCassa`.
Al salvataggio viene chiamato `updateDoc` su Firestore; la card si aggiorna
automaticamente grazie al real-time listener di `toSignal`.

**File modificati:**
- `storico-cassa.ts` — signals `editId`, `editScontrino`, `editPos`, `editFornitori`,
  computed `editContanti`, `editTotFornitori`, `editTotaleDaVersare`,
  metodi `apriModifica`, `chiudiModifica`, `salvaModifica`, handler input
- `storico-cassa.html` — bottone matita, bottom sheet form modifica
- `storico-cassa.css` — stili form modifica (`.edit-modal`, `.edit-campo`, `.edit-fornitore`, ecc.)

---

#### Conferma eliminazione serata cassa
Il bottone cestino nello storico cassa non elimina più direttamente.
Appare un bottom sheet "Eliminare questa serata?" con **Annulla** / **Elimina**.
Cliccando sull'overlay il modal si chiude senza fare nulla.

**File modificati:**
- `storico-cassa.ts` — signal `idDaEliminare`, metodi `confermaElimina`, `annullaElimina`
- `storico-cassa.html` — modal conferma eliminazione
- `storico-cassa.css` — stili overlay e modal conferma

---

#### Storico cassa raggruppato per settimana
Le serate sono ora divise in gruppi con intestazione testuale:
- *Questa settimana*
- *Settimana scorsa*
- Intervallo lunedì–domenica (es. `17 mar – 23 mar`)

Implementato tramite computed signal `chiusurePerSettimana` che raggruppa
per chiave ISO del lunedì di ogni settimana. Il raggruppamento rispetta
l'ordinamento `desc` già presente in Firestore.

**File modificati:**
- `storico-cassa.ts` — computed `chiusurePerSettimana`, metodo `labelSettimana`
- `storico-cassa.html` — `@for` annidato con intestazione settimana
- `storico-cassa.css` — `.settimana-gruppo`, `.settimana-header`

---

#### Modifica prenotazione
Il bottone matita in ogni riga prenotazione (`pren-row`) apre il modal
`pren-modal-prenotazione` pre-compilato con tutti i dati esistenti.
Al salvataggio viene chiamato `updatePrenotazione` invece di `addPrenotazione`:
data originale e stato *arrivata* non vengono toccati.
L'`effect()` nel costruttore del modal gestisce il pre-popolamento del form
quando `editingPrenotazione` cambia da `null` a un oggetto `Prenotazione`.

**File modificati:**
- `pren-modal-prenotazione.ts` — `editingPrenotazione` input, `effect()` pre-popolamento,
  branch `updatePrenotazione` vs `addPrenotazione` in `salva()`
- `pren-modal-prenotazione.html` — supporto modalità modifica
- `pren-modal-prenotazione.css` — aggiustamenti stile
- `pren-row.html` — bottone `apriModifica.emit()`

---

#### Numero persone con range (es. 33/35)
Nel form prenotazione è possibile specificare opzionalmente un numero massimo di persone
oltre a quello minimo (`personeMax: number | null`).
La visualizzazione in `pren-row.html` mostra `persone/personeMax` se presente,
altrimenti solo il numero esatto.

**File modificati:**
- `pren-modal-prenotazione.ts` — campo `personeMax` in `FormPrenotazione`
- `pren-row.html` — render condizionale `{{ persone }}{{ personeMax != null ? '/' + personeMax : '' }}`
