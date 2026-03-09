# Grecos Pizzeria — Menu Digitale

Menu digitale con QR code e pannello di gestione per Grecos Pizzeria.

**Cliente:** Grecos Pizzeria (@grecos11)  
**Sviluppatrice:** Hacman Viorica Gabriela  
**URL:** https://grecospizzeria-47768.web.app  
**Versione:** v1.4.0 — 09-03-2026

---

## Cos'è

Un'applicazione web che permette ai clienti di consultare il menu tramite QR code sul proprio telefono, e al personale della pizzeria di gestire tutto (menu, prenotazioni, asporto, disponibilità) da un pannello di amministrazione.

---

## Evoluzione del Progetto

### v1.0.0 — Prima release (04-03-2026)

Lancio del progetto con le funzionalità base:

- **Menu digitale** consultabile dai clienti tramite QR code
- **6 categorie** di prodotti: Antipasti, Pizze Rosse, Pizze Bianche, Focacce & Calzoni, Dolci, Bevande
- **Scheda piatto** completa: nome, descrizione, prezzo, allergeni (simboli EU), indicatore vegano/surgelato
- **Filtro allergeni** in tempo reale
- **Navigazione categorie** con menu orizzontale sticky
- **PWA installabile** sui telefoni (Android/iOS)
- **Pannello admin** con login sicuro per gestire il menu
- **Gestione piatti**: aggiunta, modifica, attivazione/disattivazione, eliminazione
- **Fuori menu**: sezione dedicata ai piatti speciali della serata
- **Disponibilità ingredienti**: possibilità di barrare ingredienti non disponibili
- **Messaggio del giorno**: banner personalizzabile in homepage
- **Statistiche base**: contatore visite

### v1.1.x — Miglioramenti incrementali

- **Modalità estate/inverno**: cambia automaticamente i giorni di apertura (inverno: Ven/Sab/Dom, estate: anche il Giovedì) e le zone disponibili
- **Stagione estiva**: banner automatico che informa i clienti dell'apertura del giovedì
- **Chiusure/feste**: gestione dei periodi di chiusura con data inizio/fine e motivo
- **Storico serate**: archivio dei piatti serviti ogni sera, esportabile in PDF
- **Statistiche avanzate**: grafici visite, dispositivi utilizzati, orari di picco, categorie più visitate

### v1.2.0 — Gestione Prenotazioni (09-03-2026)

Introduzione del sistema completo di prenotazioni tavoli:

- **Sezione prenotazioni** nella dashboard admin
- **Navigazione per settimane** con selezione del giorno
- **Calendario esteso** fino a 12 mesi
- **Lista prenotazioni** con tutti i dettagli: nome, telefono, orario, persone, zona, seggioloni, note
- **Aggiunta prenotazioni** con form guidato
- **Toggle "arrivata"** per segnare i clienti presenti
- **Notifiche in tempo reale** quando un'altra persona aggiunge una prenotazione
- **PDF prenotazioni** scaricabile con riepilogo giornaliero
- **Conferma eliminazione** per evitare cancellazioni accidentali

### v1.3.0 — Chiusure e PDF (09-03-2026)

Funzionalità aggiuntive per gestire il pieno:

- **Chiudi prenotazioni**: toggle per bloccare nuove prenotazioni quando il locale è pieno
- **Messaggio automatico**: in homepage appare "Prenotazioni chiuse — Il locale è pieno per questa sera"
- **Funziona per qualsiasi giorno** della settimana
- **Chiudi asporto**: analogo toggle per bloccare ordini da asporto
- **PDF asporto**: scaricabile con riepilogo ordini del giorno, totale incasso

### v1.4.0 — Autocomplete Asporto (09-03-2026)

Miglioramento dell'esperienza utente per gli ordini da asporto:

- **Ricerca prodotti intelligente**: mentre si inserisce il nome, appaiono suggerimenti dal menu
- **Inserimento automatico**: cliccando su un suggerimento, nome e prezzo si inseriscono da soli
- **Ricerca per categoria**: le pizze cercano solo tra le pizze, i fritti solo tra i fritti, ecc.
- **Fuori menu sempre incluso**: i piatti speciali appaiono nei risultati di tutte le categorie
- **Inserimento manuale**: se non trova corrispondenze, si può inserire a mano

---

## Link Utili

- [Sito web](https://grecospizzeria-47768.web.app)
- [Console Firebase](https://console.firebase.google.com/)
- [Documentazione tecnica](./CHANGELOG.md)
- [Manuale utente](./MANUALE_ADMIN.md)
- [Release precedenti](./RELEASE_v1.3.0.md)
