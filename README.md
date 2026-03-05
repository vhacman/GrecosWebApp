# Grecos Pizzeria — Menu Digitale

Menu digitale con QR code e pannello admin per Grecos Pizzeria.

**Cliente:** Grecos Pizzeria (@grecos11)
**Sviluppatrice:** Hacman Viorica Gabriela
**Stack:** Angular 21 + Bootstrap 5 + Bootstrap Icons + Firebase (Firestore + Auth + Hosting)
**URL:** https://grecospizzeria-47768.web.app
**Versione:** v1.0.0 — 04-03-2026

---

## Progetti Firebase

- **Produzione:** `grecospizzeria-47768` (https://grecospizzeria-47768.web.app)
- **Legacy:** `grecosmenu` (non piu in uso)

---

## Struttura del Progetto

```
GrecosMenu/
├── src/
│   ├── app/
│   │   ├── components/
│   │   │   └── install-prompt.ts           # Prompt installazione PWA
│   │   ├── core/
│   │   │   ├── models/
│   │   │   │   ├── menu-item.ts            # MenuItem + CategoriaMenu
│   │   │   │   ├── fuori-menu.ts           # FuoriMenu
│   │   │   │   ├── ingrediente.ts          # Ingrediente
│   │   │   │   ├── storico-serata.ts       # StoricaSerata
│   │   │   │   └── utente.ts               # Utente
│   │   │   ├── guards/
│   │   │   │   └── auth-guard.ts           # CanActivateFn
│   │   │   └── pipes/
│   │   │       └── parse-ingredienti.pipe.ts
│   │   ├── services/
│   │   │   ├── menu.ts                     # CRUD menu + fuori menu + ingredienti
│   │   │   ├── auth.ts                     # Autenticazione Firebase
│   │   │   └── config.ts                   # Serata, orari, messaggio, statistiche, storici
│   │   ├── public/
│   │   │   ├── home/                       # Homepage pubblica
│   │   │   ├── menu/                       # Menu pubblico
│   │   │   │   ├── antipasti/
│   │   │   │   ├── pizze-rosse/
│   │   │   │   ├── pizze-bianche/
│   │   │   │   ├── focacce-calzoni/
│   │   │   │   ├── dolci/
│   │   │   │   ├── bevande/
│   │   │   │   ├── filtri/
│   │   │   │   └── fuori-menu-sezione/
│   │   │   └── layout/
│   │   │       ├── navbar-categorie/       # Navbar sticky scroll orizzontale
│   │   │       └── fuori-menu-hero/        # Hero fuori menu
│   │   └── admin/
│   │       ├── login/
│   │       ├── dashboard/
│   │       ├── gestione-menu/
│   │       ├── gestione-fuori-menu/
│   │       ├── disponibilita-ingredienti/
│   │       ├── disponibilita-antipasti/
│   │       ├── disponibilita-dolci/
│   │       ├── disponibilita-bevande/
│   │       ├── resoconto-disponibilita/
│   │       ├── storico-serate/
│   │       ├── statistiche/
│   │       └── impostazioni/
│   ├── environments/
│   │   ├── environment.ts
│   │   └── environment.development.ts
│   ├── styles.css
│   └── index.html
├── scripts/
│   ├── export-firestore.js
│   ├── import-firestore.js
│   ├── service-account.json
│   └── service-account-new.json
├── seed.js
├── seed-pizze-fuori-menu.js
├── firestore.rules
├── firestore.indexes.json
├── ngsw-config.json
├── public/
│   ├── manifest.webmanifest
│   └── icons/
└── firebase.json
```

---

## Funzionalita

### Menu Pubblico (`/menu`)
- 6 categorie: Antipasti (26), Pizze Rosse (23), Pizze Bianche (18), Focacce & Calzoni (20), Dolci (7), Bevande (21)
- Ogni piatto mostra nome, descrizione, prezzo, allergeni EU (14), badge vegano/surgelato
- Piatti disattivati: etichetta "NON PIU DISPONIBILE PER LA SERATA" con nome barrato
- Filtro allergeni in tempo reale
- Indicatore disponibilita numerica per piatto
- Navbar categorie sticky con scroll orizzontale

### Home (`/`)
- Stato serata (aperta/chiusa) visibile al cliente
- Messaggio del giorno configurabile dall'admin
- Orari di apertura configurabili
- Link diretto al menu
- PWA installabile (prompt nativo Android/iOS)

### Fuori Menu
- Sezione dedicata visibile solo se la serata e aperta
- Card aggiornate in tempo reale da Firestore

### Pannello Admin (`/admin`)
Tutte le route sono protette da `AuthGuard` (Firebase Authentication).

| Route | Funzione |
|---|---|
| `/admin/login` | Login email/password |
| `/admin/dashboard` | Toggle serata, QR code scaricabile, accesso rapido |
| `/admin/gestione-menu` | CRUD completo delle 6 categorie del menu fisso |
| `/admin/fuori-menu` | CRUD fuori menu + toggle attivo |
| `/admin/disponibilita-ingredienti` | Toggle disponibile/non disponibile ingredienti |
| `/admin/disponibilita-antipasti` | Disponibilita numerica antipasti |
| `/admin/disponibilita-dolci` | Disponibilita numerica dolci |
| `/admin/disponibilita-bevande` | Disponibilita numerica bevande |
| `/admin/resoconto-disponibilita` | Vista aggregata disponibilita serata |
| `/admin/storico-serate` | Registro serate + stampa PDF fuori menu |
| `/admin/statistiche` | Visite ultimi 7 giorni per giorno/ora/categoria |
| `/admin/impostazioni` | Orari apertura + messaggio del giorno |

---

## Database Firestore

### Collezioni menu
`antipasti` · `pizzeRosse` · `pizzeBianche` · `focacceCalzoni` · `dolci` · `bevande`

```typescript
interface MenuItem {
  id?: string;
  ordine: number;
  nome: string;
  descrizione: string;
  prezzo: number;
  allergeni: number[];
  vegano: boolean;
  surgelato: boolean;
  attivo: boolean;
  eliminato: boolean;        // soft delete
  disponibili: number | null;
  createdAt: Timestamp;
  updatedAt: Timestamp;
}
```

### Collezione `fuoriMenu`

```typescript
interface FuoriMenu {
  id?: string;
  ordine: number;
  nome: string;
  descrizione: string;
  prezzo: number;
  categoria: CategoriaMenu;
  attivo: boolean;
  eliminato: boolean;
  disponibili: number | null;
  createdAt: Timestamp;
  updatedAt: Timestamp;
}
```

### Altre collezioni

| Collezione | Contenuto |
|---|---|
| `ingredienti` | `{ nome, disponibile }` |
| `storici` | `{ dataOra, dataLabel, fuoriMenuAttivi[] }` |
| `statistiche/{YYYY-MM-DD}` | `{ data, count, ore{}, categorie{} }` |
| `config/serata` | `{ aperta }` |
| `config/orari` | `{ giorni[], oraApertura, oraChiusura }` |
| `config/messaggio` | `{ testo, attivo }` |

---

## Avvio in locale

```bash
ng serve
```

Apri `http://localhost:4200/`

## Seed database

```bash
node seed.js
node seed-pizze-fuori-menu.js
```

> Richiede `scripts/service-account.json`.

## Esporta/Importa dati

```bash
node scripts/export-firestore.js
node scripts/import-firestore.js
```

## Build

```bash
ng build
```

## Deploy

```bash
firebase deploy --project=grecospizzeria-47768
```

---

## Comandi Utili

```bash
npm start                                  # Dev server
npm run watch                              # Build watch mode
npm test                                   # Test unitari (Vitest)
firebase emulators:start                   # Emulatori locali
```

---

## Variabili d'Ambiente

Credenziali Firebase in:
- `src/environments/environment.ts` — produzione
- `src/environments/environment.development.ts` — sviluppo

`serviceAccountKey.json` richiesto per seed e script, escluso dal versionamento (`.gitignore`).

---

## Release

Vedi [`RELEASE_v1.0.0.md`](./RELEASE_v1.0.0.md) per la documentazione completa della prima release (04-03-2026).

---

## Risorse

- [Firebase Console](https://console.firebase.google.com/)
- [Angular Docs](https://angular.dev/)
- [Bootstrap 5](https://getbootstrap.com/)
- [Bootstrap Icons](https://icons.getbootstrap.com/)
