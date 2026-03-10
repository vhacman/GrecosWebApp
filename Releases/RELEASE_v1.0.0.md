# Grecos Pizzeria — Menu Digitale
## Release Note · v1.0.0 · 04-03-2026

---

## Panoramica

Prima release pubblica del sistema di menu digitale e gestione interna di **Grecos Pizzeria** (Roma).
Il progetto sostituisce il menu cartaceo con una Progressive Web App consultabile da QR code, affiancata da un pannello amministrativo completo per la gestione quotidiana del locale.

- **URL pubblico:** https://grecospizzeria-47768.web.app
- **Sviluppatrice:** Hacman Viorica Gabriela
- **Deploy:** Firebase Hosting

---

## Stack Tecnologico

| Layer | Tecnologia | Versione |
|---|---|---|
| Framework | Angular (standalone components, signals) | 21.x |
| UI | Bootstrap + Bootstrap Icons | 5.3.8 / 1.13.1 |
| Componenti | Angular Material + CDK | 21.2.x |
| Database | Firebase Firestore (real-time) | 11.x |
| Auth | Firebase Authentication | 11.x |
| Hosting | Firebase Hosting + Service Worker (PWA) | — |
| Analytics | Microsoft Clarity | 1.x |
| QR Code | qrcode | 1.5.4 |
| Linguaggio | TypeScript strict | 5.9.x |

---

## Funzionalita Pubbliche

### Menu Digitale (`/menu`)
- Visualizzazione completa del menu diviso in 6 categorie:
  - Antipasti (26 voci)
  - Pizze Rosse (23 voci)
  - Pizze Bianche (18 voci)
  - Focacce & Calzoni (20 voci)
  - Dolci (7 voci)
  - Bevande (21 voci)
- Ogni piatto mostra: nome, descrizione, prezzo, allergeni EU (14), indicatori vegano/surgelato
- Navbar categorie sticky con scroll orizzontale per navigazione rapida
- Filtro per allergeni: il cliente seleziona gli allergeni da escludere e il menu si aggiorna in tempo reale
- Indicatore disponibilita: i piatti con disponibilita limitata mostrano il contatore residuo

### Fuori Menu (`/menu` — sezione hero)
- Sezione dedicata ai piatti fuori menu della serata, visibile solo se la serata e aperta
- Card con scroll orizzontale, aggiornate in tempo reale da Firestore
- I piatti sono raggruppati per categoria

### Home (`/`)
- Pagina di benvenuto con identita visiva del brand
- Banner stagionale (icona natalizia)
- Link diretto al menu e informazioni locali
- Messaggio del giorno configurabile dall'admin
- Stato serata (aperta/chiusa) visibile al cliente
- Integrazione orari di apertura configurabili

### PWA
- Installabile su smartphone (Android e iOS) tramite prompt nativo
- Service Worker per caching delle risorse statiche
- Funzionamento base anche offline (menu caricato in precedenza)

---

## Pannello Amministrativo (`/admin`)

Accessibile solo con credenziali Firebase Authentication. Tutte le route sono protette da `AuthGuard`.

### Login (`/admin/login`)
- Form email + password
- Redirect automatico alla dashboard se gia autenticato

### Dashboard (`/admin/dashboard`)
- Panoramica generale con accesso rapido a tutte le sezioni
- **Toggle serata aperta/chiusa:** attiva o disattiva la visualizzazione del fuori menu per i clienti
- **Generatore QR code:** genera e scarica il QR code con il link diretto al menu (`grecos-qr.png`)
- Sezioni collassabili: Menu, Disponibilita, Strumenti

### Gestione Menu (`/admin/gestione-menu`)
- Visualizzazione e modifica di tutte le 6 categorie del menu fisso
- Per ogni piatto: toggle attivo/inattivo, modifica nome/descrizione/prezzo/allergeni, elimina (soft delete)
- Aggiunta nuovi piatti con tutti i campi
- Soft delete: i piatti eliminati non vengono cancellati fisicamente dal database

### Gestione Fuori Menu (`/admin/fuori-menu`)
- CRUD completo dei piatti fuori menu (aggiungi, modifica, elimina)
- Toggle attivo/inattivo per ogni piatto
- I piatti attivi appaiono in tempo reale nella sezione pubblica
- Gestione delle disponibilita numeriche per piatto

### Disponibilita Ingredienti (`/admin/disponibilita-ingredienti`)
- Lista ingredienti con toggle disponibile/non disponibile
- Aggiunta di nuovi ingredienti all'archivio
- Gli ingredienti non disponibili vengono evidenziati sul menu pubblico

### Disponibilita Antipasti / Dolci / Bevande
- Pagine dedicate per la gestione rapida della disponibilita numerica
  - `/admin/disponibilita-antipasti`
  - `/admin/disponibilita-dolci`
  - `/admin/disponibilita-bevande`

### Resoconto Disponibilita (`/admin/resoconto-disponibilita`)
- Vista aggregata di tutte le disponibilita impostate nella serata corrente

### Storico Serate (`/admin/storico-serate`)
- Registro delle serate precedenti con i fuori menu attivi di ogni serata
- Funzione **Stampa PDF**: genera una pagina HTML stilizzata con il fuori menu della serata corrente, pronta per la stampa (font Georgia, colori brand, layout tipografico)

### Statistiche (`/admin/statistiche`)
- Dati di accesso degli ultimi 7 giorni
- Visite per giorno, per ora, per categoria consultata
- Top: categoria piu vista, ora di punta, giorno piu frequentato
- Tracking automatico di ogni visita al menu pubblico

### Impostazioni (`/admin/impostazioni`)
- Configurazione orari di apertura (giorni della settimana, ora apertura/chiusura)
- Messaggio del giorno: testo libero con toggle attivo/inattivo (visibile in home)

---

## Struttura Dati Firestore

### Collezioni menu (6)
`antipasti` · `pizzeRosse` · `pizzeBianche` · `focacceCalzoni` · `dolci` · `bevande`

Ogni documento `MenuItem`:
```
id, ordine, nome, descrizione, prezzo, allergeni[], vegano, surgelato,
attivo, eliminato, disponibili, createdAt, updatedAt
```

### Collezione `fuoriMenu`
Ogni documento `FuoriMenu`:
```
id, ordine, nome, descrizione, prezzo, categoria, attivo, eliminato,
disponibili, createdAt, updatedAt
```

### Collezione `ingredienti`
```
id, nome, disponibile
```

### Collezione `storici`
```
id, dataOra, dataLabel, fuoriMenuAttivi[]
```

### Collezione `statistiche`
Documenti indicizzati per data (`YYYY-MM-DD`):
```
data, count, ore{}, categorie{}
```

### Documento `config/serata`
```
aperta, updatedAt
```

### Documento `config/orari`
```
giorni[], oraApertura, oraChiusura, updatedAt
```

### Documento `config/messaggio`
```
testo, attivo, updatedAt
```

---

## Modelli TypeScript

### `MenuItem`
```typescript
id?, ordine, nome, descrizione, prezzo, allergeni: number[],
vegano, surgelato, attivo, eliminato, disponibili: number | null,
createdAt: Timestamp, updatedAt: Timestamp
```

### `FuoriMenu`
```typescript
id?, ordine, nome, descrizione, prezzo, categoria: CategoriaMenu,
attivo, eliminato, disponibili: number | null,
createdAt: Timestamp, updatedAt: Timestamp
```

### `CategoriaMenu`
```typescript
'antipasti' | 'pizzeRosse' | 'pizzeBianche' | 'focacceCalzoni' | 'dolci' | 'bevande'
```

### `Ingrediente`
```typescript
id?, nome, disponibile
```

### `StoricaSerata`
```typescript
id?, dataOra: Timestamp, dataLabel: string,
fuoriMenuAttivi: { nome, categoria, prezzo }[]
```

---

## Servizi Angular

### `AuthService`
- Login/logout Firebase Authentication
- Segnale reattivo `utente()` con lo stato corrente dell'utente

### `MenuService`
- `getCategoria()` / `getCategoriaAdmin()` — lettura real-time Firestore
- `getFuoriMenu()` / `getFuoriMenuAdmin()` — lettura real-time fuori menu
- `toggleAttivo()` / `toggleFuoriMenuAttivo()` — toggle visibilita pubblica
- `addMenuItem()` / `updateMenuItem()` / `softDeleteMenuItem()` — CRUD menu
- `addFuoriMenu()` / `updateFuoriMenu()` / `softDeleteFuoriMenu()` — CRUD fuori menu
- `getIngredienti()` / `getIngredientiNonDisponibili()` / `toggleIngrediente()` / `addIngrediente()`

### `ConfigService`
- `getSerata()` / `setSerata()` — stato apertura serata
- `getStorici()` / `salvaStorico()` — storico serate
- `getOrari()` / `updateOrari()` — orari di apertura
- `getMessaggio()` / `updateMessaggio()` — messaggio del giorno
- `registraVisita()` / `registraVisitaCategoria()` / `getStatisticheAggregate()` — analytics

---

## Routing

| Percorso | Componente | Auth |
|---|---|---|
| `/` | Home | No |
| `/menu` | Menu pubblico | No |
| `/admin/login` | Login | No |
| `/admin/dashboard` | Dashboard | Si |
| `/admin/fuori-menu` | Gestione Fuori Menu | Si |
| `/admin/gestione-menu` | Gestione Menu | Si |
| `/admin/disponibilita-ingredienti` | Disponibilita Ingredienti | Si |
| `/admin/disponibilita-dolci` | Disponibilita Dolci | Si |
| `/admin/disponibilita-antipasti` | Disponibilita Antipasti | Si |
| `/admin/disponibilita-bevande` | Disponibilita Bevande | Si |
| `/admin/resoconto-disponibilita` | Resoconto | Si |
| `/admin/storico-serate` | Storico Serate | Si |
| `/admin/impostazioni` | Impostazioni | Si |
| `/admin/statistiche` | Statistiche | Si |
| `**` | redirect `/` | — |

---

## Pattern Architetturali Adottati

- **Soft delete** su tutti i documenti: `eliminato: true` invece di `deleteDoc()`
- **Real-time con `onSnapshot`** tramite `collectionData()` di `@angular/fire`
- **Signals** Angular per stato locale reattivo (no NgRx)
- **`toSignal()`** per convertire Observable Firestore in signal nel template
- **`serverTimestamp()`** per tutti i campi `createdAt`/`updatedAt` (nessun orario client)
- **`Partial<Omit<T, 'id' | 'createdAt'>>`** per aggiornamenti parziali type-safe
- **Lazy loading** su tutti i componenti (nessun eager import)
- **`authGuard` come `CanActivateFn`** (functional guard, Angular 15+)

---

## Funzionalita Escluse da questa Release

- Ordini asporto online — non implementati per scelta progettuale
- Sistema comande interno (cucina/sala/bar) — fuori scope v1
- Integrazione stampante termica — fuori scope v1
- Account multipli con ruoli — fuori scope v1

---

## Credenziali e Accessi

| Servizio | Account |
|---|---|
| Firebase Project | grecospizzeria-47768 |
| Firebase Auth | Email/password (singolo admin) |
| Hosting | grecospizzeria-47768.web.app |
| Analytics | Microsoft Clarity |
| Instagram | @grecos11 |

---

*Documento generato il 04-03-2026 — Grecos Pizzeria, Roma*
