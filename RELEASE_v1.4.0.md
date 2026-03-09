# Grecos Pizzeria — Menu Digitale
## Release Note · v1.4.0 · 09-03-2026

---

## Panoramica

Release che introduce l'**autocomplete intelligente** per gli ordini da asporto,
permettendo di cercare i prodotti del menu e inserire automaticamente nome e prezzo.
Continua inoltre la logica delle precedenti release v1.3.0 (chiusura prenotazioni/asporto e PDF asporto).

- **URL pubblico:** https://grecospizzeria-47768.web.app
- **Sviluppatrice:** Hacman Viorica Gabriela
- **Deploy:** Firebase Hosting

---

## Novità

### Autocomplete asporto con prezzi automatici

**Admin - Asporto:**
- Quando si aggiunge un nuovo ordine asporto, digitando nel campo nome appare un dropdown con i prodotti del menu che contengono quella parte di nome
- Cliccando su un prodotto dal dropdown:
  - Il nome viene inserito automaticamente
  - Il prezzo viene inserito automaticamente dal listino
- Se non viene trovata nessuna corrispondenza, è possibile inserire nome e prezzo manualmente

**Categorie cercate:**
- Sezione **Pizze** → cerca in: pizze rosse, pizze bianche, fuori menu
- Sezione **Fritti** → cerca in: antipasti, focacce/calzoni, fuori menu
- Sezione **Altro** → cerca in: dolci, bevande, fuori menu

Il dropdown mostra massimo 8 risultati per evitare sovraffollamento.

---

## Note di deploy

1. Nessuna modifica al database necessaria
2. Compatibile con versione precedente
3. Nessuna migrazione dati
