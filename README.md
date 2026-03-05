# Grecos Pizzeria — Menu Digitale

Sito web menu digitale con QR code, pannello admin e ordini da asporto.

**Cliente:** Grecos Pizzeria (@grecos11)
**Sviluppatrice:** Hacman Viorica Gabriela
**Stack:** Angular 21 + Bootstrap 5 + Bootstrap Icons + Firebase (Firestore + Auth + Hosting)
**URL:** https://grecospizzeria-47768.web.app

---

## Progetti Firebase

- **Produzione:** `grecospizzeria-47768` (https://grecospizzeria-47768.web.app)
- **Legacy:** `grecosmenu` (non più in uso)

---

## Struttura del Progetto

```
GrecosMenu/
├── src/
│   ├── app/
│   │   ├── components/
│   │   │   └── install-prompt.ts    # Componente per installazione PWA
│   │   ├── core/
│   │   │   ├── models/             # Interfacce TypeScript
│   │   │   │   ├── menu-item.ts
│   │   │   │   ├── fuori-menu.ts
│   │   │   │   └── utente.ts
│   │   │   └── guards/
│   │   │       └── auth-guard.ts
│   │   ├── services/
│   │   │   ├── menu.ts             # CRUD menu + fuori menu
│   │   │   ├── auth.ts             # Autenticazione Firebase
│   │   │   └── config.ts           # Configurazione e statistiche
│   │   ├── public/
│   │   │   ├── menu/               # Menu pubblico (lettura)
│   │   │   │   ├── menu.ts + .html + .css
│   │   │   │   ├── antipasti/
│   │   │   │   ├── pizze-rosse/
│   │   │   │   ├── pizze-bianche/
│   │   │   │   ├── focacce-calzoni/
│   │   │   │   ├── dolci/
│   │   │   │   ├── bevande/
│   │   │   │   ├── filtri/
│   │   │   │   └── fuori-menu-sezione/
│   │   │   ├── home/               # Homepage pubblica
│   │   │   └── layout/
│   │   │       ├── navbar-categorie/
│   │   │       └── fuori-menu-hero/
│   │   └── admin/
│   │       ├── login/
│   │       ├── dashboard/
│   │       ├── gestione-menu/
│   │       ├── gestione-fuori-menu/
│   │       └── impostazioni/
│   ├── environments/
│   │   ├── environment.ts           # Produzione (grecospizzeria-47768)
│   │   └── environment.development.ts
│   ├── styles.css                  # Stili globali
│   └── index.html
├── scripts/
│   ├── export-firestore.js         # Esporta dati dal DB
│   ├── import-firestore.js         # Importa dati nel DB
│   ├── service-account.json        # Credenziali vecchio progetto
│   └── service-account-new.json   # Credenziali nuovo progetto
├── seed.js                        # Popolamento database
├── seed-pizze-fuori-menu.js       # Seed pizze fuori menu
├── firestore.rules                 # Regole Firestore
├── firestore.indexes.json          # Indici Firestore
├── ngsw-config.json               # Service Worker config (PWA)
├── public/
│   ├── manifest.webmanifest       # Web App Manifest (PWA)
│   └── icons/                     # Icone PWA
└── firebase.json
```

---

## Caratteristiche

### Menu Pubblico
- Visualizzazione menu per categorie (Antipasti, Pizze Rosse, Pizze Bianche, Focacce/Calzoni, Dolci, Bevande)
- Filtri: Vegano, Esclusione allergeni
- Indicatori visivi: icona foglia verde per vegani, asterisco (*) per surgelati
- **Item disattivati**: visualizzati con etichetta "NON PIÙ DISPONIBILE PER LA SERATA" e nome barrato
- Sezione "Fuori Menu" con piatti speciali/seasonal
- Navigazione responsive con navbar categorie
- **PWA**: installabile come app sul telefono
- **Condivisione**: WhatsApp, Facebook, Instagram

### Pannello Admin
- Autenticazione Firebase (email/password)
- Gestione menu: CRUD completo, toggle attivo/disattivo, riordinamento
- Gestione fuori menu: CRUD completo
- Dashboard: statistiche visite, QR code per il menu, link a homepage

### Database (Firestore)

**Collezioni:**
- `antipasti`, `pizzeRosse`, `pizzeBianche`, `focacceCalzoni`, `dolci`, `bevande` — MenuItem
- `fuoriMenu` — FuoriMenu
- `config` — Configurazione (messaggio del giorno, statistiche)

**Schema MenuItem:**
```typescript
interface MenuItem {
  id?: string;
  ordine: number;
  nome: string;
  descrizione: string;
  prezzo: number;
  allergeni: number[];        // Codici allergeni
  vegano: boolean;
  surgelato: boolean;
  attivo: boolean;            // Visibilità pubblica
  eliminato: boolean;         // Soft delete
  createdAt: Timestamp;
  updatedAt: Timestamp;
}
```

---

## Diario di Sviluppo

### Sessione 8

**Refinements finali**

- Sistemato link admin nella homepage (icona ingranaggio)
- Aggiunto bottone "Torna a Homepage" nella pagina login
- Risolti problemi di indici Firestore per fuori menu
- Aggiunta nota su Google Play Protect nella sezione installazione app

### Sessione 7

**UI/UX e PWA**

- Aggiunto popup condivisione (WhatsApp, Facebook, Instagram)
- Aggiunta responsive design desktop per homepage e barra bottom
- Aggiunta sezione "Scarica la nostra app" con istruzioni dettagliate
- Implementato PWA completo con Service Worker e manifest

### Sessione 6

**Migrazione Firebase + PWA**

- Creato nuovo progetto Firebase: `grecospizzeria-47768`
- Migrati tutti i dati da `grecosmenu` al nuovo progetto
- Aggiornati environment.ts con nuove credenziali
- Configurato PWA con Service Worker e Web App Manifest

### Sessione 5

**Dashboard Admin**

- Aggiunto QR code scaricabile dalla dashboard
- Sistemato layout responsive per desktop
- Implementato sistema disponibilità giornaliero

### Sessione 4

**Gestione Menu**

- Item disattivati visualizzati come "Non più disponibile per la serata"
- Modificato MenuService per rimuovere filtro attivo dalla query pubblica
- Aggiornati tutti i template delle categorie con badge e stili

### Sessione 3

**Database e Seed**

- Creato seed.js per popolamento Firestore
- Configurate regole Firestore e indici compositi
- Corretti prezzi, allergeni e descrizioni dal menu fisico
- Strutturato database con collezioni flat

### Sessione 2

**Indicatori Visivi**

- Aggiunti campi vegano e surgelato al modello MenuItem
- Implementati badge foglia verde per vegani, asterisco per surgelati
- Aggiornati template HTML e CSS per tutti i prodotti

### Sessione 1

**Setup Progetto**

- Creazione progetto Angular 21
- Installazione dipendenze (Bootstrap, Firebase, AngularFire)
- Configurazione Firebase e autenticazione
- Struttura cartelle e generazione componenti
- Setup iniziale Firestore con schema e regole

---

## Avvio in locale

```bash
ng serve
```

Apri `http://localhost:4200/`

## Seed database

```bash
node seed.js
```

> Richiede `scripts/service-account.json` nella cartella scripts.

### Seed Fuori Menu

```bash
node seed-pizze-fuori-menu.js
```

### Esporta/Importa dati

```bash
# Esporta da progetto esistente
node scripts/export-firestore.js

# Importa nel nuovo progetto
node scripts/import-firestore.js
```

> Richiede `scripts/service-account.json` o `scripts/service-account-new.json`

## Build

```bash
ng build
```

## Deploy

```bash
firebase deploy --project=grecospizzeria-47768
```

> **Nota:** Il progetto predefinito è configurato in `.firebaserc`

---

## Comandi Utili

### Sviluppo
```bash
npm start              # Avvia server dev (equivale a ng serve)
npm run watch         # Build in watch mode
```

### Test
```bash
npm test              # Esegue test unitari ( Karma + Jasmine )
```

### Build
```bash
ng build              # Build produzione
ng build --configuration development  # Build sviluppo
```

### Firebase
```bash
firebase login        # Accedi a Firebase
firebase emulators:start  # Avvia emulatori locali
firebase deploy       # Deploy su Firebase Hosting
firestore:delete     # Elimina dati (attenzione!)
```

---

## Variabili d'Ambiente

Le credenziali Firebase sono configurate in:
- `src/environments/environment.ts` — produzione
- `src/environments/environment.development.ts` — sviluppo

Il file `serviceAccountKey.json` (chiave admin) è richiesto per il seed del database e deve essere escluso dal versionamento (già presente in `.gitignore`).

---

## Risorse Esterne

- [Firebase Console](https://console.firebase.google.com/)
- [Angular Docs](https://angular.dev/)
- [Bootstrap 5](https://getbootstrap.com/)
- [Bootstrap Icons](https://icons.getbootstrap.com/)
