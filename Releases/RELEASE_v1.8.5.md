# Release v1.8.5 — 17-03-2026

## Fix Disponibilità — Lista Vuota

---

### Fix critico — Disponibilità antipasti, dolci e bevande mostravan lista vuota

**Problema:** Le pagine disponibilità mostravano la lista completamente vuota in produzione.

**Causa:** I metodi `getCategoria()`, `getCategoriaAdmin()`, `getFuoriMenuByCategoria()`, `getFuoriMenu()`
e `getFuoriMenuAdmin()` in `menu.ts` usavano `where(...) + orderBy('ordine')` su campi diversi.
Firestore richiede un indice composito per questa combinazione — indice assente nel progetto (non creato
automaticamente né incluso in `firestore.indexes.json`).

**Fix:** Rimosso `orderBy('ordine')` da tutte le query che usano `where`. Ordinamento spostato
lato client con:

```typescript
Array.sort((a, b) => (a.ordine ?? 0) - (b.ordine ?? 0))
```

**Approccio già adottato in v1.8.4** per `getPrenotazioni()` — ora applicato in modo coerente
a tutte le query con filtri.

**File modificati:**
- `services/menu.ts` — rimosso `orderBy` da 5 metodi, aggiunto sort lato client
