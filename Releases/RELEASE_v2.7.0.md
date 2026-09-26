# Release v2.7.0 — 26-09-2026

## BELLA RAGA, GABRI QUI 😎 — Disponibili Stasera, Recensioni, Menù in Francese e Dashboard Rinnovata

> Release unica che accorpa le versioni pubblicate/preparate il 26-09-2026: **2.6.0**, **2.6.1** e **2.7.0**.

---

### Novità

#### Disponibili stasera
Nuova card in dashboard (a tutta larghezza) e pagina `/admin/stasera` con tutto il lavoro
di ogni sera sul menù, pensata per i dipendenti:
- **Fuori menù**: categorie una sotto l'altra con il numero di piatti accesi; tocco → popup
  centrato con sezioni richiudibili **ATTIVI / NON ATTIVI**, interruttore acceso/spento,
  modifica, elimina e "Nuovo fuori menù" (categoria preimpostata, nasce acceso). Badge
  "acceso da N gg" oltre 7 giorni.
- **Finiti**: ingredienti, dolci, antipasti, bevande (componenti Disponibilità incorporati);
  "Aggiungi ingrediente" in cima.
- **Resoconto** di ciò che risulta non disponibile.
- In cima: **"N persone hanno visto il menù oggi"** e **chi ha aggiornato il menù per ultimo**
  (avviso arancione se oggi nessuno l'ha fatto). All'accensione di "Cucina aperta", se il
  menù non è stato toccato oggi, compare "Il menù di stasera è a posto?".

**File modificati:**
- `admin/dashboard/stasera/` — nuova pagina + `ultimo-aggiornamento.ts`
- `admin/dashboard/gestione-fuori-menu/` — modalità `incorporato`, lista categorie, popup
- `services/menu.ts` — `updatedBy` (nome account) su ogni scrittura, `attivatoIl`,
  `getUltimoAggiornamento()`
- `services/statistiche.service.ts` — `getOggi()` live
- `admin/dashboard/dashboard.*` — card, promemoria all'apertura cucina; rimossa card
  Disponibilità; "Gestione Menù" → "Modifica menù" (diretto a `/admin/gestione-menu`)
- `app.routes.ts` — `/admin/stasera`; redirect `/admin/fuori-menu` → stasera,
  `/admin/sezione-menu` → gestione-menu; eliminata `sezione-gestione-menu`

---

#### Recensioni dei clienti
- **Promemoria** all'apertura del menù (una volta per visita): "Se alla fine della tua visita
  ti senti soddisfatto, ricordati di tornare qui e lasciare una recensione".
- **Bottone "Recensione"** nel menù e **sezione "Recensioni"** in homepage: faccine 1–5, nome,
  giorno della visita (oggi, modificabile), commento. Con 4–5 → link Google/TripAdvisor;
  con 1–3 il commento resta privato.
- **Card "Recensioni clienti"** in dashboard (media, badge non letti) → pagina con
  distribuzione voti, filtri ed eliminazione.

**File modificati:**
- `public/shared/recensione-promemoria/`, `public/shared/recensione-modal/`
- `services/feedback.ts`, `InterfacceECostanti/feedback.ts`, `admin/dashboard/feedback/`
- `public/menu-component/menu/menu.*`, `public/home/home.*` (+ `css/home-cards.css`)
- `firestore.rules` — collezione `feedback` con create pubblico validato

---

#### Menù in italiano, inglese e francese
Selettore **IT | EN | FR** (homepage, barra categorie, fuori menù); lingua iniziale dal telefono
e ricordata. `lingua.loc(obj, campo)` generico con fallback lingua → EN → IT: una nuova lingua
richiede solo un dizionario. Traduzioni FR di tutti i 194 piatti caricate con
`scripts/patch-translations-fr.js`. Nei form dei piatti sezione "Traduzioni (EN, FR)".
Banner puntualità tradotto (EN/FR), campi per lingua in Impostazioni → Banner avviso.

**File modificati:**
- `core/i18n_internationalization/translations.fr.ts`, `translations.ts` (`LINGUE`)
- `core/languageService/lingua.service.ts`, `public/shared/lingua-selector/`
- `admin/shared/traduzioni-campi/`, form Modifica menù e fuori menù
- `InterfacceECostanti/menu.ts`, `config.ts` — campi `*_fr`

---

#### Utenti, ruoli e sicurezza
- **Elenco admin** (`admins/{uid}`): le regole Firestore danno accesso solo a chi è
  nell'elenco (`isAdmin()`), non più a chiunque sia loggato.
- **Impostazioni e Strumenti → Utenti**: "Aggiungi utente" (popup), richieste di accesso da
  approvare, email per cambiare password, togli accesso.
- **Ruoli**: fondatori (Nunzia, Giuseppe) sempre titolari; il ruolo **Titolare** lo danno
  fondatori e amministratore di sistema (Gabriela, non rimovibile); togliere l'accesso: fondatori
  e amministratore di sistema su chiunque, titolari sugli utenti staff.
- **Schermata**: "Aggiungi utente" apre un popup con scelta ruolo **Staff | Titolare**; lista
  "Chi ha accesso" divisa in Amministratori di sistema / Titolari / Staff.
- **Nomi al posto delle email**: "inserita da" (prenotazioni), "inserito da" (asporto, nuovo
  campo `creatoDa`), "Aggiornato da" (Disponibili stasera) mostrano il nome dell'utente.
- `scripts/seed-admins.js` — migrazione account esistenti + nomi (Giordano, Giuseppe, Gabriela,
  Noemi, Nunzia). `service-account*.json` tolti dal versionamento.

**File modificati:**
- `firestore.rules`, `services/auth.ts`, `services/utenti.ts`, `core/guards/auth-guard.ts`,
  `admin/login/login.ts`, `admin/dashboard/impostazioni/utenti/`,
  `InterfacceECostanti/constants/admin.constants.ts`

---

#### Letture Firestore ridotte
Il 26-09 è stata esaurita la quota gratuita di 50.000 letture/giorno (menù pubblico bloccato con
errore 429). "Ultimo aggiornamento" ora legge un solo documento (`config/ultimoAggiornamento`,
scritto a ogni modifica admin) invece di ascoltare ~250 documenti per componente.
`scripts/rollback-prove-26-09.js` ripristina i 3 fuori menù accesi per prova e inizializza il
documento. Consigliato il piano Blaze con avviso di budget.

**File modificati:**
- `services/menu.ts` — `firma()` aggiorna `config/ultimoAggiornamento`,
  `getUltimoAggiornamento()` con `docData` + `shareReplay`
- `scripts/rollback-prove-26-09.js` — nuovo

---

#### Impostazioni e Strumenti insieme
Un'unica card: orari, messaggio, banner, contatti + gruppo Strumenti (Utenti, Rubrica,
Chiusure, QR Code). Generatore QR estratto in `admin/shared/qr-code`; eliminata la pagina
`sezione-strumenti` (redirect a `/admin/impostazioni`).

---

#### Dashboard più compatta
Card cucina e stagione affiancate e più basse; striscia "Visualizza Menù e Prezzi" più
sottile; nelle card titolo accanto all'icona. Banner puntualità in homepage più compatto.

---

#### Campanellina riepilogo serata
Nuova campanella rossa accanto a "Visualizza Menù e Prezzi" in dashboard. Apre un pannello
di riepilogo del giorno: stato aggiornamento menù, cucina aperta/chiusa, prenotati oggi
(con dettaglio per zona in popup + totale persone), ordini asporto, modalità stagione e voce
"Disponibilità alimenti". Se il menù non viene aggiornato da ≥ 10 giorni la voce disponibilità
è evidenziata in rosso ("Da aggiornare!"). Dalla voce si apre un popup con le pagine di
disponibilità (Resoconto, Ingredienti, Dolci, Antipasti, Bevande) modificabili inline.

**File modificati:**
- `admin/dashboard/dashboard.ts/.html/.css` — campanella, popup riepilogo, popup
  disponibilità (`::ng-deep` per embeddare le pagine), computed `menuScaduto`,
  `prenPerZona`, `totalePersone`, conteggi prenotati/asporto
- Popup promemoria weekend riconvertito: appare come popup con bottone "Aggiorna ora" →
  `/admin/stasera`, mostrato solo se menù non aggiornato da ≥ 10 giorni

---

#### Modalità estate/inverno in dashboard
Il cambio stagione è nella schermata principale (componente `ImpStagione` embeddato), affiancato
alla card "Cucina aperta": selettore a due voci **❄️ Inverno | ☀️ Estate** (la stagione attiva è
piena, azzurro/oro), con conferma. Aggiorna solo i giorni di apertura: **rimosso il popup
stagionale** "Informiamo i nostri clienti che…" in homepage. Il **terrazzo esterno** è
disponibile anche in inverno.

**File modificati:**
- `admin/dashboard/dashboard.ts/.html/.css` — `<app-imp-stagione>` in dashboard, card affiancate
- `admin/dashboard/impostazioni/stagione/imp-stagione.ts/.html/.css` — selettore `stg-*`,
  nessun `messaggioStagione` pubblicato
- `public/home/home.ts/.html` — rimosso popup stagione
- `admin/dashboard/prenotazioni/prenotazioni.ts` — `ZONE_INVERNO`/`ZONE_ESTATE` con terrazzo

---

#### Prenotazioni e Asporto: nuova navigazione
Lista giorni ora orizzontale in alto (scroll orizzontale), con settimane passate collassate
(intestazione cliccabile con chevron) e settimana corrente allineata a sinistra all'apertura.
Ricerca cliente spostata in cima all'header (accanto alla freccia indietro). Ordini/tavoli
raggruppati per orario. Barra azioni fissa in basso con **Aggiungi** e **Condividi con il
personale** (WhatsApp).

**File modificati:**
- `admin/dashboard/prenotazioni/pren-settimane-nav/` e
  `admin/dashboard/asporto/settimane-nav/` — nav orizzontale, collasso settimane passate,
  auto-scroll orizzontale
- `admin/dashboard/prenotazioni/prenotazioni.*` e `admin/dashboard/asporto/asporto.*` —
  layout body a colonna, barra azioni in basso, ricerca, KPI espandibile
- `admin/dashboard/prenotazioni/pren-header/` e `admin/dashboard/asporto/header/` —
  ricerca + freccia indietro nella stessa riga, azioni raggruppate col titolo

---

#### Condivisione WhatsApp
Bottone verde "Condividi con il personale" nella barra in basso. **Prenotazioni:** apre
l'anteprima e invia l'**immagine** riepilogativa della serata (Canvas + `navigator.share`).
**Asporto:** un tap apre WhatsApp con la lista ordini del giorno come messaggio di testo
(`wa.me`), ordinata per orario, con totali.

**File modificati:**
- `admin/dashboard/prenotazioni/prenotazioni.html/.ts` — bottone → `anteprimaAperta.set(true)`
  (modal `pren-modal-anteprima`), rimosso `condividiWaTesto()`
- `admin/dashboard/asporto/asporto.ts` — `condividiWaTesto()`

---

#### Etichette e capienza per sala
Alle prenotazioni si possono assegnare etichette rapide (Compleanno, Allergie, Abituale,
Celiaco, Bimbi, Esterno), mostrate come chip colorati. Il riepilogo mostra persone e tavoli
per ogni sala (Sala interna, Veranda, Terrazzo).

**File modificati:**
- `InterfacceECostanti/config.ts` — campo `tag?: string[]` su `Prenotazione`
- `admin/dashboard/prenotazioni/prenotazioni.ts` — `TAG_PRENOTAZIONE`, `personePerZona`
- `admin/dashboard/prenotazioni/pren-modal-prenotazione/` — selettore tag
- `admin/dashboard/prenotazioni/pren-row/` — chip tag

---

#### Serate registrate più leggibili (Calcolo Cassa)
Nello storico serate la settimana corrente resta espansa, le passate si comprimono in una
riga con resoconto inline (serate, totale versato, totale fornitori pagati) e si espandono
al click. Aggiunta card riepilogo settimana (versato in banca / fornitori pagati). Corretto
lo storico dei giovedì passati in modalità inverno: i giorni passati usano l'unione dei
giorni di apertura di entrambe le stagioni.

**File modificati:**
- `admin/dashboard/calcolo-cassa/storico-cassa/storico-cassa.ts/.html/.css` — collasso
  settimane, totali per settimana
- `core/utils/calendar-dates.ts` — `generaGiorniAperti` usa set unione per le date passate

---

#### Rubrica anche per l'asporto
Salvando un ordine asporto con un numero di telefono non presente in rubrica, l'app chiede
se aggiungere il contatto (popup Sì/No). Stessa logica già presente nelle prenotazioni.

**File modificati:**
- `admin/dashboard/asporto/modal-ordine/asp-modal-ordine.ts/.html` — popup salva rubrica

---

#### Sezione Statistiche rimossa
La vista Statistiche in dashboard è stata rimossa (non utilizzata). Il tracking analytics
di base resta attivo su Firestore, semplicemente non c'è più una UI che lo mostra.

**File modificati:**
- `app.routes.ts` — route `admin/statistiche` rimossa
- `admin/dashboard/statistiche/` — componente eliminato

---

#### Varie
- Rimosso banner "Menù online: aggiornato X giorni fa" dalla dashboard.
- Rimosso il numero tavolo dalle singole prenotazioni (non utilizzato).
- Fix zoom iOS sul campo di ricerca (font 16px).
- Fix testo "Oggi" invisibile sul chip giorno selezionato.
