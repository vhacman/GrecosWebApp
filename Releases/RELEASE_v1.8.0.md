# Release v1.8.0 — 15-03-2026

## Layout Desktop Professionale, Calcolo Cassa Protetto

---

### Layout Desktop e Tablet Completamente Responsivo

Il sito mostrava un effetto "telefono nel browser" su PC: colonna centrale stretta (max 900px), bordi arrotondati, sfondo marrone, come se il sito fosse una app mobile centrata sullo schermo del computer.

**Fix:** rimosso completamente il blocco `@media (min-width: 768px)` in `styles.css` che impostava `max-width`, `border-radius`, `box-shadow` e il colore di sfondo `#C4B9A8` sull'`html`.

**Homepage su desktop (≥ 1024px):** layout a due colonne CSS Grid che occupa l'intera viewport:
- **Colonna sinistra** — logo, nome pizzeria, stato serata, orari, contatti social
- **Colonna destra** — bottoni CTA (apri menu, scarica app), copyright

**Tablet (≥ 768px):** padding ottimizzati, logo 175px, titolo 3.4rem.

**Barra flottante:** nascosta su desktop (non necessaria col layout a colonne). Su tablet centrata correttamente con `transform: translateX(-50%)`.

---

### Card "Visualizza Menù e Prezzi" nella Dashboard Admin

Aggiunta una card cliccabile come primo elemento della dashboard admin, prima del toggle cucina aperta/chiusa.

- **Stile:** sfondo bianco, bordo rosso `#C1272D` — stesso stile della card Cucina aperta
- **Destinazione:** `/menu` (menu pubblico)
- **Sottotitolo:** "Consulta il menù per compilare il preconto"
- Permette all'admin di consultare i prezzi aggiornati direttamente dal pannello senza passare per la sezione Strumenti

Contestualmente rimossa la voce "Visualizza Menù" dalla sezione Strumenti (ora accessibile direttamente dalla dashboard).

---

### Calcolo Cassa Protetto da Password

Il pulsante "Calcolo Cassa" nella dashboard è ora protetto. Al click appare un bottom-sheet che richiede una password prima di navigare a `/admin/calcolo-cassa`.

**Sicurezza:**
- La password è archiviata come hash SHA-256 in Firestore (`config/cassa → passwordHash`) — mai in chiaro
- Implementato con Web Crypto API (`crypto.subtle.digest`), nessuna dipendenza esterna
- Password predefinita: `grecos2026`
- Al primo accesso (documento Firestore non ancora presente) il sistema confronta automaticamente contro l'hash del default, senza richiedere setup iniziale

Contestualmente rimossa la voce "Calcolo Cassa" dalla sezione Strumenti.

---

### Cambio Password Calcolo Cassa da Impostazioni

Nuova sottosezione **🔒 Password Calcolo Cassa** in `Impostazioni → Password Calcolo Cassa`.

Flusso:
1. Inserire la **password attuale** (verificata tramite confronto hash su Firestore)
2. Inserire la **nuova password** (minimo 6 caratteri)
3. **Confermare** la nuova password
4. La nuova password viene hashata (SHA-256) e salvata in Firestore

Messaggi di errore specifici per ogni caso: password attuale errata, password troppo corta, password di conferma non coincidente.
