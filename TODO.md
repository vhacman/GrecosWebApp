# TODO — Grecos Pizzeria

## Da fare

### [ ] 1. Nascondere link admin dalla homepage
**Perché:** Dai dati Clarity (07-09 Mar 2026), il link admin (ingranaggio in alto a destra)
riceve 15 clic (11% del totale) da utenti normali che lo toccano per curiosità.
**Soluzione proposta:** Spostarlo nel footer o rimuoverlo completamente dall'interfaccia
pubblica — l'admin può accedere direttamente tramite URL `/admin/login`.

---

### [x] 2. Rendere le righe del menu cliccabili (modal dettaglio) — v1.1.2 · 09-03-2026
Implementato bottom-sheet custom (`ItemDetailModal`) con nome, descrizione, allergeni
espansi come pill e prezzo. Rimosso `AllergeniExpand` inline. Hint "tocca per dettagli
e allergeni" visibile su ogni riga. Deploy eseguito.

---

### [x] 3. Allergeni sui piatti fuori menu — v1.1.3 · 09-03-2026
Aggiunti campi `allergeni?: number[]` e `surgelato?: boolean` al modello `FuoriMenu`.
Popup fuori menu mostra allergeni + disclaimer "fatto in casa" (solo se non surgelato).
Migrazione Firestore eseguita su 46 documenti. Script idempotente riutilizzabile.
