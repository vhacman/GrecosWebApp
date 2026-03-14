# GrecosMenu - Applicazione Principale

## 1. Panoramica dell'Applicazione

### Sviluppatore
- **Nome**: Hacman Viorica Gabriela
- **Profilo**: Junior Software Engineer
- **Formazione**:
  - Generation Italy (500h Java Developer, dic 2025 - apr 2026)
  - 42 Roma Luiss (C, C++, System Administration, Linux)
- **Esperienza precedente**: Capo Sala presso Grecos Pizzeria (2022-2025)
- **Tecnologie**: Angular 21, TypeScript 5.9, Firebase, Bootstrap 5.3, Java, SpringBoot

### Caratteristiche Principali
- **Menu digitale QR Code**: Consultabile via smartphone tramite QR code
- **Pannello Admin completo**: Gestione menu, prezzi, categorie, allergeni
- **Sistema prenotazioni**: Gestione tavoli con calendario, note, conferma WhatsApp
- **Gestione ordini asporto**: Ordini da asporto con stato e gestione
- **Statistiche visite**: Analytics integrato con Microsoft Clarity
- **Modalità Estate/Inverno**: Gestione stagionale del menu
- **Esportazione PDF**: Prenotazioni, asporto, storico mensile/annuale
- **PWA installabile**: Installazione su mobile come app nativa
- **Notifiche real-time**: Toast in-app per nuove prenotazioni
- **Integrazione WhatsApp**: Condivisione riepiloghi giornalieri

### Stack Tecnologico
- **Frontend**: Angular 17-21+ (TypeScript 5.9)
- **Backend**: Firebase (Firestore, Authentication, Hosting)
- **PWA**: Service Worker con offline support
- **Design**: Bootstrap 5.3 / Material Design
- **Analytics**: Microsoft Clarity

### Volume di Codice
- Oltre **50 componenti Angular**
- Amministrazione completa (dashboard, prenotazioni, asporto, menu, impostazioni)
- Backend Firestore con struttura dati completa

## Struttura Cartelle

```
GrecosMenu/
├── src/                    # Codice sorgente Angular
│   ├── app/               # Componenti, servizi, modelli
│   ├── styles.css         # Stili globali
│   └── index.html         # Entry point
├── public/                # File statici (assets, QR)
├── dist/                  # Build di produzione
├── scripts/               # Script di automazione
├── .firebase/            # Configurazione hosting
├── firebase.json         # Configurazione Firebase
├── firestore.rules       # Regole database
├── package.json           # Dipendenze npm
└── angular.json          # Configurazione Angular
```

## Funzionalità Principali

- Menu digitale consultabile via QR code
- Pannello admin per gestione menu
- Sistema prenotazioni tavoli
- Gestione ordini asporto
- Statistiche visite
- Modalità estate/inverno
- Esportazione PDF (prenotazioni, asporto, storico)

## Deploy

```bash
cd GrecosMenu
npm run build
firebase deploy
```

## Configurazione

Il file `serviceAccountKey.json` contiene le credenziali Firebase (non committare). Le variabili di ambiente sono gestite tramite Firebase Console.
