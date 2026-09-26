# Grecos Pizzeria — Menu Digitale

Web app per la gestione del menu digitale, prenotazioni tavoli e ordini asporto di **Grecos Pizzeria**.

---

## Stack

| Layer | Tecnologia |
|-------|-----------|
| Frontend | Angular 21 — standalone components, signals, `toSignal()` |
| UI | Bootstrap 5.3 + Bootstrap Icons 1.13 |
| Form/Dialog | Angular Material 21 |
| Database | Firebase Firestore (europe-west1) |
| Auth | Firebase Authentication (email/password) + elenco admin/ruoli su Firestore |
| Hosting | Firebase Hosting |
| Linguaggio | TypeScript 5.9 strict mode |

---

## Struttura del repository

```
GrecosMenu/                     ← root Git
├── GrecosMenu/                 ← progetto Angular (cd qui per tutti i comandi)
│   ├── src/app/
│   │   ├── InterfacceECostanti/   interfacce, costanti, allergeni, releases
│   │   ├── services/              auth, menu, config, statistiche, feedback, utenti
│   │   ├── core/                  guards, models, pipes, utils, i18n, env
│   │   ├── public/                home, navbar, menu pubblico, fuori menu
│   │   └── admin/                 login + dashboard (disponibili stasera, recensioni,
│   │                              asporto, prenotazioni, impostazioni e strumenti
│   │                              con utenti, chiusure, calcolo cassa, storico)
│   ├── scripts/                   seed-admins.js, patch-translations-fr.js, …
│   └── firebase.json / .firebaserc
├── Documentazione/
│   └── CHANGELOG.md
└── README.md
```

---

## Comandi

Tutti i comandi vanno eseguiti da `GrecosMenu/GrecosMenu/`:

```bash
npm start          # dev server → http://localhost:4200
ng build           # build produzione → dist/GrecosMenu/browser/
ng test            # test con Vitest
firebase deploy    # build + deploy su Firebase Hosting + Firestore rules
```

---

## Colori brand

| Nome | HEX |
|------|-----|
| Rosso | `#C1272D` |
| Oro | `#F4A800` |
| Scuro | `#1C1008` |
| Sfondo | `#FAFAF7` |

---

## Versione corrente

**v2.7.0** — vedi [`CHANGELOG.md`](../CHANGELOG.md) per lo storico completo e [`Releases/RELEASE_v2.7.0.md`](RELEASE_v2.7.0.md) per il dettaglio.

Il popup "Novità" per gli admin è gestito da `src/app/InterfacceECostanti/constants/releases.constants.ts`.
Le versioni segnate come *interno* nel CHANGELOG non aggiornano `CURRENT_VERSION` e non generano popup.

---

## Firebase

- **Firestore collections:** `antipasti`, `pizzeRosse`, `pizzeBianche`, `focacceCalzoni`, `dolci`, `bevande`, `fuoriMenu`, `ingredienti`, `config`, `chiusure`, `prenotazioni`, `ordiniAsporto`, `nonPrenotati`, `storicaSerate`, `messaggiRecenti`, `avvisiRecenti`, `statistiche`, `feedback`, `admins`, `richiesteAccesso`
- **Cache strategy:** `memoryLocalCache()` — no IndexedDB (evita corruzioni)
- **Security rules:** regole per-collezione; accesso admin solo a chi è in `admins/{uid}` (`isAdmin()`), ruoli `staff`/`titolare` (fondatori e amministratore di sistema in `admin.constants.ts` e `firestore.rules`)
- **Lingue:** IT / EN / FR — campi Firestore `campo`, `campo_en`, `campo_fr` (fallback EN → IT)

---

## Autrice

Hacman Viorica Gabriela — [@grecos11](https://www.instagram.com/grecos11) — 2026
