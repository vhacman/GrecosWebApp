# Release v1.6.0 — 10-03-2026

## Grecos Pizzeria — Menu Digitale

**URL:** https://grecospizzeria-47768.web.app
**Versione precedente:** v1.5.0
**Tipo:** Patch / UX

---

## Novità

### 1. Bottone "Manuale d'Uso dell'App" in homepage

Aggiunto nella homepage pubblica un bottone per scaricare il **Manuale Utente** in formato PDF.

- Posizione: sotto la sezione "Scarica App", sopra il copyright
- Testo: **Manuale d'Uso dell'App**
- Icona: `bi-file-earmark-pdf`
- Cliccando il bottone il PDF si scarica direttamente con nome `Manuale-Grecos.pdf`
- Stile coerente con i bottoni esistenti: sfondo bianco, bordo rosso, testo uppercase

Il file `manuale-utente.pdf` è servito dalla root di Firebase Hosting (`public/manuale-utente.pdf`).

---

## File modificati

| File | Modifica |
|---|---|
| `src/app/public/home/home.html` | Aggiunto `<a download>` bottone manuale |
| `src/app/public/home/home-cards.css` | Aggiunto stile `.manuale-btn` |
| `public/manuale-utente.pdf` | **Nuovo** — Manuale utente scaricabile |
