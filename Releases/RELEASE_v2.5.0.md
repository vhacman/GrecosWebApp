# Release v2.5.0 — 13-06-2026

## GABRIELA ALL'ATTACCO 😂 — Banner Avviso e Messaggi Recenti

---

### Novità

#### Banner avviso puntualità (homepage)
Nuova striscia rossa fissa in cima alla home, sempre visibile finché l'admin la tiene
attiva. Animazione "brillio" (sweep di luce diagonale + pulsazione del bordo) per attirare
l'attenzione. Diverso dal Messaggio del giorno: non è un popup, è un avviso permanente.
Documento Firestore `config/avvisoPuntualita` (`AvvisoBanner`: `titolo`, `testo`, `attivo`),
testo di default precompilato sulla puntualità (`DEFAULT_AVVISO`). Rispetta
`prefers-reduced-motion` (animazioni disattivate per chi le ha disabilitate).

**File modificati:**
- `InterfacceECostanti/config.ts` — interfaccia `AvvisoBanner` + `DEFAULT_AVVISO`
- `services/config.ts` — `getAvviso()`, `updateAvviso()`
- `public/home/home.ts` — signal `avviso`, getter `avvisoVisibile`
- `public/home/home.html` — blocco `.avviso-banner`
- `public/home/css/home-cards.css` — stili + animazioni `avviso-shine` / `avviso-pulse`
- `public/home/css/home.css` — slot messaggi collassabile (`:not(:has(> *))`)
- `admin/dashboard/impostazioni/avviso/` — nuova sezione admin (`imp-avviso.ts/.html`)

---

#### Storico messaggi e avvisi (riselezione rapida)
Nel pannello Messaggio del giorno compaiono gli ultimi testi usati come chip: un click per
riselezionarli, **Ripubblica** per rimetterli online subito (1 click), cestino per rimuoverli
dallo storico. Max 10 voci, ordinate per data (ordinamento client-side per evitare problemi
con `serverTimestamp` pendente), potatura automatica oltre le 10 più recenti. Lo storico è
secondario: se la scrittura fallisce (es. rules non deployate) non blocca il salvataggio
principale (`try/catch`).

**File modificati:**
- `InterfacceECostanti/config.ts` — interfacce `MessaggioRecente` / `AvvisoRecente`
- `services/config.ts` — `getMessaggiRecenti()`, `addMessaggioRecente()`,
  `rimuoviMessaggioRecente()`, equivalenti avvisi + `potaRecenti()`
- `admin/dashboard/impostazioni/messaggio/imp-messaggio.ts/.html` — chip recenti,
  `selezionaRecente()`, `ripubblicaRecente()`, `rimuoviRecente()`
- `admin/dashboard/impostazioni/impostazioni.css` — stili chip recenti
- `firestore.rules` — regole `messaggiRecenti` / `avvisiRecenti` (admin-only)

---

#### Impostazioni come voce dedicata
Impostazioni promossa a card hub nella dashboard (Orari, Messaggio, Banner avviso, Stagione,
Contatti), non più nascosta dentro Strumenti.

**File modificati:**
- `admin/dashboard/dashboard.html` — nuova hub card Impostazioni
- `admin/dashboard/sezione-strumenti/sezione-strumenti.html` — voce rimossa
- `admin/dashboard/impostazioni/impostazioni.ts/.html` — vista `avviso` aggiunta

---

#### Varie
Promemoria weekend in dashboard: "Giordi" → "Giordi e Noemi".

**File modificati:**
- `admin/dashboard/dashboard.html` — testo `.promemoria-mini`
