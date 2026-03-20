# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commit Rules

- Never include `Co-Authored-By: Claude` or any Anthropic co-authorship line in commit messages.

## Repository Context

This is a **documentation repository** for the Grecos Pizzeria digital menu web app. The Angular source code lives in a separate local directory (`GrecosMenu/GrecosMenu/`) and is not tracked here. This repo tracks release notes, changelogs, user manuals, and QR codes.

## Angular Project Commands

All commands run from `GrecosMenu/GrecosMenu/`:

```bash
npm start          # dev server → http://localhost:4200
ng build           # production build → dist/GrecosMenu/browser/
ng test            # run tests with Vitest
firebase deploy    # build + deploy to Firebase Hosting + Firestore rules
```

`firebase deploy` triggers a predeploy `npm run build` hook automatically — no need to build manually before deploying.

## Technology Stack

- **Angular 21** — standalone components, signals, `toSignal()`
- **TypeScript 5.9** strict mode
- **Bootstrap 5.3** + Bootstrap Icons 1.13
- **Angular Material 21** for dialogs/forms
- **Firebase Firestore** (europe-west1), Auth (email/password), Hosting
- **Vitest** for testing
- **jsPDF** for PDF export, **qrcode** library for QR generation
- **Microsoft Clarity** for analytics (disabled on localhost and 192.168.* to avoid polluting stats)

## App Architecture

```
src/app/
├── InterfacceECostanti/   interfaces, constants, allergens, release version data
├── services/              AuthService, MenuService, ConfigService, statistics
├── core/                  guards, models, pipes, utils, i18n (IT/EN), env
├── public/                customer-facing routes (home, menu, reservations)
└── admin/                 login + dashboard (takeaway, reservations, availability,
                           settings, closures, cash calc, statistics, history)
```

**Two main user roles:**
- **Public** (`/`) — customers browse the menu, make reservations, see availability
- **Admin** (`/admin/login` → `/admin/dashboard`) — full management via Firebase Auth

## Firestore Collections

**Menu (public read, admin write):** `antipasti`, `pizzeRosse`, `pizzeBianche`, `focacceCalzoni`, `dolci`, `bevande`, `fuoriMenu`, `ingredienti`

**Config (admin only):** `config`, `chiusure`

**Business (customer create, admin read/update/delete):** `prenotazioni`, `ordiniAsporto`, `nonPrenotati`, `storicaSerate`

**Cache strategy:** `memoryLocalCache()` — no IndexedDB (prevents corruption issues).

## Key Implementation Notes

- **Version notifications:** Admin "What's New" popup is driven by `src/app/InterfacceECostanti/constants/releases.constants.ts` via `CURRENT_VERSION`. Versions marked *interno* in CHANGELOG do not update this constant and do not show a popup.
- **Swipe-to-dismiss modals:** Implemented with `makeSwipeHandler(closeFn)` factory using manual `touchmove` listener with `{ passive: false }` — necessary because Angular's event listeners are passive by default and cannot call `preventDefault()` (needed to block browser pull-to-refresh).
- **PDF generation files:** `pdf-prenotazioni.ts`, `pdf-prenotazioni-mese.ts`, `pdf-prenotazioni-anno.ts`, `pdf-asporto.ts`, `pdf-asporto-mese.ts`, `pdf-asporto-anno.ts`
- **WhatsApp sharing:** `png-prenotazioni.ts` generates a reservation summary PNG shared via Web Share API with fallback download.
- **Firebase cache headers:** `index.html`/SW files → `no-cache`; hashed JS/CSS bundles → `immutable, max-age=31536000`.

## Brand Colors

| Name | HEX |
|------|-----|
| Rosso | `#C1272D` |
| Oro | `#F4A800` |
| Scuro | `#1C1008` |
| Sfondo | `#FAFAF7` |

## This Repository's Contents

- `CHANGELOG.md` — full version history
- `Releases/` — individual release note files (v1.0.0–v1.9.0)
- `ISTRUZIONI D'USO ADMIN E USER/` — admin and user manuals (PDFs)
- `QR/` — printable QR codes for the digital menu
- `OFFERTA_COMMERCIALE_AGGIORNATA_2026.md` — commercial offer document for DigiMenuQ (the product name when sold to other restaurants)
