# Release v1.9.0 — 18-03-2026

## QR Recensioni Google/TripAdvisor + Card Menu con Chip Categorie

---

### QR Code Recensioni Google e TripAdvisor

**Sezione Strumenti** ridisegnata con modal multi-tipo per la generazione di QR:

| Tipo QR | Destinazione |
|---------|-------------|
| Menu digitale | Homepage del sito |
| Sezione Dolci | `/menu?cat=dolci` |
| Recensioni Google | Link Google Reviews del locale |
| TripAdvisor | Link profilo TripAdvisor (QR generato dinamicamente via libreria `qrcode`) |

Ogni QR scaricabile come PNG.

**Impostazioni → Contatti** — nuovi campi URL:
- **Link Google Reviews** — URL alla pagina recensioni Google del locale
- **Link TripAdvisor** — URL al profilo TripAdvisor

Entrambi i campi includono un bottone "Condividi" che usa la **Web Share API** su mobile
(WhatsApp, AirDrop, ecc.) con fallback copia-negli-appunti su browser non supportati.

**Homepage** — nuova sezione "Lascia una recensione":
- Card con link diretto a Google Reviews e TripAdvisor
- Visibile solo se i link sono configurati dall'admin

---

### Card Menu Unificata con Chip Categorie

Sostituisce il bottone "Apri il Menu" + card separata con un unico blocco:
- **Header rosso** con CTA principale ("Sfoglia il Menu")
- **Chip per ogni categoria** (Antipasti, Pizze Rosse, Pizze Bianche, Focacce & Calzoni, Dolci, Bevande)
  — ogni chip linka direttamente a `/menu?cat=<categoria>`
- Accesso immediato a qualsiasi sezione del menu senza passare dalla homepage del menu

**Bottone "Manuale d'Uso"** reso meno prominente (testo grigio, senza bordo) per non competere
visivamente con le CTA principali.

---

### Ottimizzazione Performance — IntersectionObserver sulla barra bottom

La barra bottom della homepage (con i bottoni Prenota / Asporto / Menu) sostituisce
`HostListener('scroll')` con **IntersectionObserver** per rilevare quando la sezione hero
è fuori dal viewport.

- **Prima:** listener scroll su ogni frame di scroll → blocco sul thread principale
- **Dopo:** callback solo al cambio visibilità → zero overhead durante lo scroll
