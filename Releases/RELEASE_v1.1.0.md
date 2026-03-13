# Release v1.1.0 — 08-03-2026

## Miglioramenti dashboard, PDF e modalità estate/inverno

### PDF Export

- **Resoconto disponibilità**: generato con jsPDF (sostituisce `window.print()`), offline-ready, non apre finestre aggiuntive
- **QR Code**: download PDF A4 stampabile con logo, QR centrato e URL menu
- **Storico serate**: PDF via jsPDF. Al download viene salvato automaticamente uno snapshot in Firestore — la lista storica permette di ri-scaricare qualsiasi serata passata

### Statistiche

- Selettore periodo 7 / 30 giorni con KPI dinamico
- Auto-refresh ogni 3 minuti (con cleanup in `ngOnDestroy`)
- Grafico scrollabile orizzontalmente con etichette adattive (`gg/mm` a 30 giorni)

### Modalità estate / inverno

- Toggle in Impostazioni che commuta giorni di apertura (Ven/Sab/Dom ↔ Gio/Ven/Sab/Dom) e zone prenotazione (Sala/Veranda ↔ Terrazzo/Veranda)
- Attivando estate: banner automatico per 20 giorni *"Siamo aperti anche il Giovedì"*

### Homepage

- Bottone "Apri il Menu" ridisegnato: sfondo rosso `#C1272D`, animazione `pulse` (expanding ring)
