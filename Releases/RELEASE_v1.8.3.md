# Release v1.8.3 — 16-03-2026 *(interno — non pubblicata)*

## Documentazione e Refactoring

---

### Commenti inline su tutti i componenti Angular

Aggiunti commenti `//` e blocchi `/* */` su tutti i file `.ts` e `<!-- -->` su tutti i template `.html`
della cartella `src/app/` (esclusi i `.spec.ts`).

**Copertura:**
- App root, guards, models, pipes, utils
- Servizi (`auth`, `menu`, `config`, `statistiche`)
- i18n e traduzione IT/EN
- Componenti pubblici: `home`, `menu`, `fuori-menu`, filtri, categorie
- Pannello admin completo: `login`, `dashboard`, `gestione-menu`, `asporto`, `prenotazioni`,
  `disponibilità`, `impostazioni`, `chiusure`, `resoconto`, `calcolo-cassa`, `statistiche`, `storico-serate`

Nessuna modifica alla logica applicativa — solo documentazione.

---

### Fix `styleUrl` prenotazioni

`prenotazioni.ts` puntava a `./css/prenotazioni.css` (file rimosso in v1.8.1).
Corretto in `./prenotazioni.css`.

---

> ℹ️ Questa versione non è presente in `releases.constants.ts` — nessun popup "Novità" per gli admin.
