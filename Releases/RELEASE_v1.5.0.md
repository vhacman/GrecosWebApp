# Release v1.5.0 — 10-03-2026

## Grecos Pizzeria — Menu Digitale

**URL:** https://grecospizzeria-47768.web.app
**Versione precedente:** v1.4.0
**Tipo:** Feature release

---

## Novità

### 1. Sezione "Specialità" nel menu pubblico

Nuova tab **Specialità** (con icona ⭐) aggiunta come prima voce della navbar del menu digitale.
Mostra tutti i fuori menu attivi della serata raggruppati per categoria (antipasti, pizze, dolci…).
- Sempre visibile, anche a cucina chiusa
- Card cliccabili con nome, descrizione, prezzo e badge "poca disponibilità"
- Popup dettaglio con allergeni e nota ingredienti variabili
- Stato vuoto ("Nessuna specialità stasera") quando non ci sono fuori menu attivi
- Supporto IT/EN

**Motivazione:** i fuori menu erano nascosti all'interno di ogni sezione e i clienti non li trovavano facilmente. Una tab dedicata rende immediata la visibilità degli speciali della serata.

---

### 2. QR code con logo Grecos al centro (admin panel)

Il QR code generato dal pannello admin (`Strumenti → QR Code`) usa ora `HomepageQR.png` — un'immagine già predisposta con il logo Grecos al centro del codice — invece di generarlo dinamicamente via libreria.

- Download PNG: scarica direttamente l'immagine
- Download PDF: genera PDF A4 con layout Grecos (sfondo, bordo rosso, titolo, URL) usando il QR con logo

---

### 3. QR dedicato sezione Dolci

Generato `grecos-qr-dolci.pdf` (disponibile in `public/img/`) con:
- QR che punta a `/menu?cat=dolci`
- Layout identico al QR del menu generale
- Testo "Scansiona per vedere i Dolci"

Utile da stampare e posizionare sul tavolo vicino al porta-dolci.

---

### 4. Link diretto per categoria via query param

La route `/menu` ora supporta il parametro `?cat=` per aprire direttamente una categoria al caricamento:

```
/menu?cat=dolci
/menu?cat=pizzeRosse
/menu?cat=specialita
/menu?cat=bevande
```

Utile per QR code dedicati per categoria (da stampare per ogni sezione del locale).

---

### 5. Condivisione QR code dai clienti

Nel popup "Condividi" della homepage è stato aggiunto il bottone **QR Code**:
- Su mobile (iOS/Android) apre il foglio di condivisione nativo del sistema (WhatsApp, AirDrop, ecc.) con il file `HomepageQR.png`
- Su desktop o browser non supportati, scarica direttamente l'immagine

---

### 6. Popup Condividi ridisegnato

Il popup "Condividi" è stato completamente ridisegnato per coerenza con lo stile dell'app:

| Prima | Dopo |
|---|---|
| Modal centrato con bounce animation | Bottom sheet che sale dal basso |
| Bottoni con gradienti e ombre forti | Righe semplici con icona colorata e label |
| Emoji pizza animata | Handle bar discreto |
| Testo uppercase bold | Testo normale, leggibile |

---

### 7. LinkedIn nel footer homepage

Icona LinkedIn affiancata al copyright `© 2026 Hacman V. Gabriela` in fondo alla homepage.

---

## File modificati

| File | Modifica |
|---|---|
| `src/app/public/layout/navbar-categorie/navbar-categorie.ts` | Aggiunto tipo `CategoriaNav`, tab Specialità |
| `src/app/public/layout/navbar-categorie/navbar-categorie.css` | Stile tab Specialità |
| `src/app/public/menu/menu.ts` | Supporto `CategoriaNav`, `?cat=` query param, import `Speciali` |
| `src/app/public/menu/menu.html` | Case `speciali` nel switch |
| `src/app/public/menu/speciali/` | **Nuovo componente** `Speciali` (ts, html, css) |
| `src/app/admin/dashboard/sezione-strumenti/sezione-strumenti.ts` | QR da `HomepageQR.png` |
| `src/app/admin/dashboard/sezione-strumenti/sezione-strumenti.html` | `<img>` al posto di `<canvas>` |
| `src/app/admin/dashboard/sezione-strumenti/sezione-strumenti.css` | Dimensioni immagine QR |
| `src/app/public/home/home.ts` | Metodo `condividiQr()`, metodo `condividiInstagram()` LinkedIn |
| `src/app/public/home/home.html` | Bottone QR nel popup condividi, LinkedIn nel footer |
| `src/app/public/home/home-condividi.css` | Redesign completo bottom sheet |
| `src/app/public/home/home-cards.css` | Stile `.copyright-linkedin` |
| `src/app/core/i18n/translations.it.ts` | Chiavi `cat_speciali`, `speciali_titolo`, `speciali_vuoto` |
| `src/app/core/i18n/translations.en.ts` | Stesse chiavi in inglese |
| `public/img/HomepageQR.png` | **Nuovo** QR homepage con logo Grecos al centro |
| `public/img/DOLCIQR.png` | **Nuovo** QR dolci con logo Grecos al centro |
| `public/img/grecos-qr-dolci.pdf` | **Nuovo** PDF stampabile QR dolci |
