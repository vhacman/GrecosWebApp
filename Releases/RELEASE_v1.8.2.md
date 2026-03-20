# Release v1.8.2 — 16-03-2026

## Fix Soglia Scarso Disponibilità

---

### Disponibilità — Soglia "Scarso" Corretta a ≤ 5

**Prima (v1.8.1):** `SOGLIA_SCARSO = 10` → range scarso = 1–9.

**Dopo:** `SOGLIA_SCARSO = 6` → range scarso = 1–5, OK = ≥ 6.

La soglia precedente era troppo ampia: con 6–9 porzioni il piatto veniva classificato "Scarso"
e mostrava il badge "Poca disponibilità" nel menu pubblico, quando in realtà la disponibilità
era ancora confortante. La soglia corretta è ≤ 5 porzioni.

**Tabella stati aggiornata:**

| `disponibili` | Stato | Badge menu pubblico |
|---|---|---|
| `null` | OK | nessuno |
| `≥ 6` | OK (quantità tracciata) | nessuno |
| `1–5` | Scarso | "Poca disponibilità" |
| `0` | Esaurito | piatto non disponibile |

**File modificati:**
- `disponibilita-antipasti.ts` — `SOGLIA_SCARSO = 6`
- `disponibilita-bevande.ts` — `SOGLIA_SCARSO = 6`
- `disponibilita-dolci.ts` — `SOGLIA_SCARSO = 6`

Il valore di default quando si clicca "Scarso" rimane `5` (`disponibiliFromStato('scarso') → 5`),
che rientra correttamente nella nuova soglia.
