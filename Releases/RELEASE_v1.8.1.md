# Release v1.8.1 — 16-03-2026

## Sconto Asporto, Consegna, Calcolo Cassa e Disponibilità

---

### Asporto — Sconto per Ordine

Tap sulla riga di un ordine (non sul pencil di modifica) apre un bottom-sheet **"Sconto ordine"**.

**Flusso:**
1. Il modal mostra nome cliente, orario, lista piatti e **totale lordo** (somma prezzi senza sconto)
2. L'admin inserisce il **totale da incassare** (campo numerico con tastiera decimale)
3. Lo sconto viene calcolato automaticamente: `sconto = lordo − da incassare`
4. Tap **"Applica sconto"** → aggiorna il campo `sconto` su Firestore (`ordiniAsporto/{id}`)
5. Tap **"Rimuovi sconto"** → azzera lo sconto (visibile solo se già applicato)

**Effetti visivi nella lista:**
- Se lo sconto > 0: il totale lordo compare barrato a sinistra, il totale scontato a destra in rosso
- Se nessuno sconto: solo il totale normale (comportamento precedente)

**KPI "Totale incasso":** ora considera tutti gli sconti. Formula:
`totaleIncasso = Σ max(0, lordo_ordine − sconto_ordine)`

**Nuovi campi nell'interfaccia `OrdineAsporto`:**
```typescript
sconto?:    number;   // sconto fisso in euro
consegnato?: boolean;
```

**Nuovi metodi nel `ConfigService`:**
```typescript
setScontoAsporto(id: string, sconto: number): Promise<void>
```

---

### Asporto — Toggle "Consegnato"

Ogni riga ordine mostra un **pulsante cerchio** a sinistra. Tap → spunta verde = ordine consegnato.

- Icona: `bi-circle` (non consegnato) / `bi-check-circle-fill` (consegnato)
- Stato persistito in Firestore: campo `consegnato: boolean`
- Le righe consegnate ricevono la classe CSS `.consegnato` (stile visivo attenuato)

**Nuovo metodo nel `ConfigService`:**
```typescript
toggleConsegnatoAsporto(id: string, consegnato: boolean): Promise<void>
```

---

### Calcolo Cassa — Contanti Calcolati Automaticamente

**Prima:** due campi separati — "Contanti incassati" e "POS incassato".

**Dopo:** un solo campo primario + il POS esistente:
- **"Totale scontrino di chiusura cassa"** — il totale che appare sullo scontrino fiscale (include contanti + POS)
- **"Di cui incassati con il POS"** — importo POS dal terminale
- **"Contanti (scontrino − POS)"** — calcolato automaticamente, mostrato in tempo reale

**Perché:** eliminato l'errore umano di sommare manualmente contanti + POS. L'admin legge solo il totale dallo scontrino fiscale e il totale POS dal terminale — il sistema calcola il resto.

**Aggiornamenti:**
- Segnale rinominato: `contantiIncassati` → `totaleScontrino`
- Nuova computed: `contantiCalcolati = totaleScontrino − posIncassato`
- `totaleGuadagni` ora equivale a `totaleScontrino`
- `totaleDaVersare = contantiCalcolati − totFornitoriCalc`
- PDF: sezione INCASSI aggiornata con tre righe (scontrino / di cui POS / di cui contanti)
- Firestore (`chiusureCassa`): aggiunto campo `totaleInCassa` mantenendo retrocompatibilità

---

### Disponibilità — Soglia "Scarso" Portata a 10

**Prima:** qualsiasi quantità > 0 → stato "Scarso" → badge "Poca disponibilità" nel menu pubblico.

**Dopo:** soglia `SOGLIA_SCARSO = 10`:
- `0` → Esaurito
- `1–9` → Scarso (badge nel menu pubblico)
- `≥10` → OK (disponibile, quantità tracciata, nessun badge di allerta)
- `null` → OK (illimitato)

**Applicato a:** `disponibilita-antipasti.ts`, `disponibilita-bevande.ts`, `disponibilita-dolci.ts`

**Menu pubblico:** badge "Poca disponibilità" visibile solo se `disponibili !== null && disponibili > 0 && disponibili < 10`

**Resoconto disponibilità:** il componente `res-voce-row.html` ora mostra "OK" per quantità ≥ 10 (prima mostrava "Scarso" erroneamente).

**Comportamento toggle:** `disponibiliFromStato('ok')` restituisce `10` invece di `null`:
l'admin che porta un piatto da "Esaurito" a "OK" vede subito `10` nel campo quantità, pronto per la modifica — senza dover inserire da zero.

---

### Refactoring CSS — Sottocartelle

| Componente | Prima | Dopo |
|---|---|---|
| Asporto | `asporto-list.css`, `asporto-modal.css` in root | spostati in `css/` |
| Statistiche | `statistiche.css`, `statistiche-charts.css` in root | spostati in `css/` |
| Storico Serate | `storico-serate.css`, `storico-serate-modal.css` in root | spostati in `css/` |

Nessuna modifica agli stili — solo organizzazione del codice. I `styleUrls` nei componenti `.ts` sono stati aggiornati.
