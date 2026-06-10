# Release v2.4.0 — 10-06-2026

## GABRIELA COLPISCE ANCORA! — Promemoria Menù e Avviso Cassa

---

### Novità

#### Promemoria aggiornamento menù (giovedì)
Banner in dashboard (`mostraPromemoriaMenu`) che ricorda allo staff di aggiornare il
menù online. Visibile solo il giovedì (`new Date().getDay() === 4`). Pura logica client,
nessun dato su Firestore. Il dismiss vive in `sessionStorage` (`promemoriaMenu_chiuso`):
chiuso con la X, riappare al login successivo.

**File modificati:**
- `dashboard.ts` — signal `mostraPromemoriaMenu`, metodo `chiudiPromemoriaMenu`
- `dashboard.html` — banner `.promemoria-menu`
- `dashboard.css` — stili banner

---

#### Mini promemoria weekend (ven/sab/dom)
Avviso compatto (`mostraPromemoriaWeekend`) per ven (5), sab (6), dom (0) che ricorda
di togliere dal menù i piatti finiti. Stesso meccanismo di dismiss via `sessionStorage`
(`promemoriaWeekend_chiuso`).

**File modificati:**
- `dashboard.ts` — signal `mostraPromemoriaWeekend`, metodo `chiudiPromemoriaWeekend`
- `dashboard.html` — banner `.promemoria-mini`
- `dashboard.css` — stili banner mini

---

#### Stato aggiornamento menù
Nuova card in dashboard mostra da quanto tempo non si tocca il menù online.
Deriva da `MAX(updatedAt)` su tutte le categorie admin + `fuoriMenu`
(`MenuService.getUltimoAggiornamentoMenu()`), realtime via `combineLatest` — nessun
documento extra su Firestore.
- Etichetta: `aggiornato oggi` / `ieri` / `N giorni fa` (`etichettaAggiornamento`).
- Stato colore (`statoAggiornamento`): `ok` ≤ 2 gg (verde), `warn` 3–5 gg (arancione),
  `alert` > 5 gg o mai aggiornato (rosso).

**File modificati:**
- `services/menu.ts` — metodo `getUltimoAggiornamentoMenu()`
- `dashboard.ts` — `giorniDaAggiornamento`, `etichettaAggiornamento`, `statoAggiornamento`
- `dashboard.html` — card `.menu-stato`
- `dashboard.css` — stili stato (ok/warn/alert)

---

#### Avviso cassa in negativo
Nel Calcolo Cassa, se i fornitori pagati in contanti superano i contanti incassati
(`versamentoNegativo = totaleDaVersare() < 0`), compare un avviso `role="alert"`:
non c'è nulla da versare in banca, la cassa è in negativo.

**File modificati:**
- `calcolo-cassa.ts` — computed `versamentoNegativo`
- `calcolo-cassa.html` — blocco `.versamento-avviso`
- `calcolo-cassa.css` — stili avviso
