# Release v1.3.0 — 10-03-2026

## Specialità, QR dedicati e Manuale utente

---

### Menu pubblico

**Tab Specialità**
Nuova prima tab ⭐ nella navbar del menu pubblico. Mostra tutti i fuori menu attivi raggruppati per categoria, con card cliccabili, popup dettaglio, allergeni, supporto IT/EN. Sempre visibile anche a cucina chiusa. Stato vuoto se nessun fuori menu attivo.

**Link diretto per categoria**
La route `/menu` supporta `?cat=dolci`, `?cat=pizzeRosse`, ecc. per aprire direttamente la categoria desiderata. Utile per QR stampati per sezione del locale.

---

### QR Code

**QR con logo**
Il QR generato da `Strumenti → QR Code` usa `HomepageQR.png` (logo Grecos al centro) invece della generazione dinamica. Download PNG e PDF aggiornati.

**QR dedicato Dolci**
Generato `grecos-qr-dolci.pdf` (A4, stile Grecos) con QR puntato a `/menu?cat=dolci`. Da stampare e posizionare vicino al porta-dolci.

**Condivisione QR dai clienti**
Nel popup "Condividi" della homepage: bottone QR Code che usa la Web Share API per condividere `HomepageQR.png` nativamente (WhatsApp, AirDrop, ecc.). Fallback download automatico su browser non supportati.

---

### Homepage

**Popup Condividi ridisegnato**
Sostituito il modal con gradienti/emoji con un bottom sheet pulito: righe con icona colorata e label.

**Bottone Manuale d'Uso**
Nuovo bottone sotto "Scarica App" per scaricare `manuale-utente.pdf` servito dalla root Firebase Hosting.

**LinkedIn nel footer**
Icona LinkedIn affiancata al copyright.
