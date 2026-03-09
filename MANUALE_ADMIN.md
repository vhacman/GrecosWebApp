# MANUALE UTENTE
## Pannello Admin - Grecos Pizzeria

---

## Accesso al Pannello Admin

1. Accedere alla homepage: **https://grecospizzeria-47768.web.app**
2. Cliccare sull'icona dell'ingranaggio in alto a destra
3. Inserire le credenziali (email e password)
4. Si accede alla dashboard di gestione

---

## Dashboard

La dashboard è la schermata principale di controllo. Da qui è possibile:
- Visualizzare lo stato della serata (cucina aperta/chiusa)
- Accedere alle varie sezioni di gestione
- Visualizzare快速的 riepilogo delle attività

---

## Gestione Menu

### Modifica piatti esistenti

1. Cliccare su **"Gestione Menu"** dalla dashboard
2. Selezionare la categoria desiderata (Pizze Rosse, Pizze Bianche, Antipasti, ecc.)
3. Per ogni piatto è possibile:
   - **Modificare** nome, descrizione, prezzo, allergeni
   - **Attivare/disattivare** il piatto (se disattivo non appare nel menu pubblico)
   - **Eliminare** il piatto (eliminazione logica, il piatto resta in Firestore)
4. Cliccare **"Salva modifiche"** per confermare

### Aggiungere un nuovo piatto

1. Nella sezione della categoria desiderata, cliccare **"Aggiungi piatto"**
2. Compilare i campi:
   - Nome *
   - Descrizione
   - Prezzo *
   - Allergeni (selezione multipla)
   - Vegan (spunta se vegano)
   - Surgelato (spunta se surgelato)
3. Cliccare **"Salva"**

### Fuori Menu (piatti del giorno)

1. Cliccare su **"Gestione Fuori Menu"**
2. Cliccare **"Aggiungi piatto"**
3. Inserire nome, prezzo e categoria
4. I piatti fuori menu appariranno in homepage con uno sfondo evidenziato

---

## Disponibilità Ingredienti

### Gestione ingredienti singoli

1. Cliccare su **"Disponibilità Ingredienti"**
2. Per ogni ingrediente è possibile:
   - **Barrarlo** (non appare nelle descrizioni dei piatti)
   - **Impostare la disponibilità** (numero di porzioni disponibili)
   - Quando la disponibilità arriva a 0, l'ingrediente viene barrato automaticamente

### Gestione categorie

Sono disponibili sezioni separate per:
- **Disponibilità Antipasti**
- **Disponibilità Dolci**
- **Disponibilità Bevande**

Permettono di gestire la disponibilità dei singoli piatti di quelle categorie.

### Resoconto disponibilità

**"Resoconto Disponibilità"** mostra una panoramica di tutti gli ingredienti e piatti con bassa disponibilità.

---

## Prenotazioni Tavoli

### Visualizzazione prenotazioni

1. Cliccare su **"Prenotazioni"** dalla dashboard
2. Selezionare il giorno dalla barra laterale (navigazione settimanale)
3. Visualizzare la lista delle prenotazioni con:
   - Nome cliente
   - Orario
   - Numero persone
   - Zona (Sala interna, Veranda, Terrazzo)
   - Eventuali note

### Aggiungere una prenotazione

1. Cliccare sul pulsante **"+"** in alto a destra
2. Compilare i campi:
   - Nome *
   - Telefono (opzionale)
   - Zona
   - Numero persone
   - Seggioloni (se presenti bambini)
   - Orario (selezione da lista orari 19:00-23:00, oppure "orario libero")
   - Note (eventuali allergie, richieste particolari)
3. Cliccare **"Aggiungi prenotazione"**

### Gestione prenotazioni

- **Toggle arrivata**: spunta il cerchio quando il cliente arriva (disponibile solo per il giorno stesso o giorni passati)
- **Eliminare**: cliccare sull'icona cestino e confermare

### Chiudere le prenotazioni (locale pieno)

1. Selezionare il giorno desiderato
2. Cliccare sul pulsante rosso **"CHIUDI PRENOTAZIONI"**
3. Il pulsante diventa verde: **"APRI PRENOTAZIONI"**
4. In homepage apparirà un messaggio: *"Prenotazioni chiuse — Il locale è pieno per questa sera. Prova a prenotare per un giorno successivo!"*

### Scaricare PDF

Cliccare sull'icona PDF per scaricare la lista delle prenotazioni del giorno in formato PDF.

---

## Asporto

### Visualizzazione ordini

1. Cliccare su **"Asporto"** dalla dashboard
2. Selezionare il giorno dalla barra laterale
3. Visualizzare la lista degli ordini con:
   - Nome cliente
   - Orario ritiro
   - Dettaglio prodotti ordinati

### Aggiungere un ordine

1. Cliccare sul pulsante **"+"** in alto a destra
2. Inserire nome cliente (opzionale)
3. Selezionare orario ritiro
4. Aggiungere i prodotti nelle sezioni:
   - **Pizze**: cliccare "+ Pizza" e inserire nome e quantità
   - **Fritti**: cliccare "+ Fritto" e inserire nome e quantità
   - **Altro**: cliccare "+ Altro" per dolci, bevande, ecc.

### Autocomplete (ricerca prodotti)

Quando si inserisce il nome di un prodotto:
1. Iniziare a digitare (es: "marg")
2. Appare un menu a tendina con i prodotti che contengono quella parte di nome
3. Cliccare su un prodotto per inserire automaticamente nome e prezzo
4. La ricerca è filtrata per categoria:
   - Pizze → cerca in pizze rosse, bianche e fuori menu
   - Fritti → cerca in antipasti, focacce/calzoni e fuori menu
   - Altro → cerca in dolci, bevande e fuori menu
5. Se non trova corrispondenze, inserire nome e prezzo manualmente

### Chiudere gli ordini (cucina piena)

1. Selezionare il giorno desiderato
2. Cliccare sul pulsante arancione **"CHIUDI ASPORTO"**
3. Il pulsante diventa verde: **"APRI ASPORTO"**

### Scaricare PDF

Cliccare sull'icona PDF per scaricare il riepilogo degli ordini del giorno in formato PDF.

---

## Chiusure / Ferie

### Aggiungere una chiusura

1. Cliccare su **"Strumenti"** nella dashboard, poi **"Chiusure"**
2. Cliccare **"Aggiungi chiusura"**
3. Compilare:
   - Data inizio
   - Data fine
   - Motivo (es: "Ferie natalizie", "Eventi", ecc.)
4. Cliccare **"Salva"**

### Effetti delle chiusure

- Le date di chiusura **non appaiono** nel calendario delle prenotazioni
- In homepage appare un banner rosso: *"Il ristorante è chiuso fino al [data] — [motivo]"*
- Se la prossima chiusura è entro 14 giorni, appare un banner arancio

---

## Impostazioni

### Modalità Estate / Inverno

1. Cliccare su **"Impostazioni"**
2. Toggle **"Modalità estate"**:
   - Inverno: aperture Ven/Sab/Dom, zone: Sala interna / Veranda
   - Estate: aperture Gio/Ven/Sab/Dom, zone: Terrazzo / Veranda
3. Attivando la modalità estate, appare un banner automatico in homepage per 20 giorni

### Orari di apertura

Configurare:
- Giorni di apertura della settimana
- Ora di apertura
- Ora di chiusura

### Messaggio del giorno

Inserire un messaggio personalizzato che appare in homepage (es: "Stasera serve menu speciale!").
Opzionalmente impostare una data di scadenza dopo la quale il messaggio scompare automaticamente.

---

## Statistiche

### Visitatori

**"Statistiche"** mostra:
- Numero visite totali al menu
- Grafico visite giornaliere
- Dispositivi utilizzati (mobile/tablet/desktop)
- Pagine più visitate
- Orari di picco

### Prenotazioni

Sezione dedicata con:
- Numero tavoli totali
- Coperti totali
- Media coperti per tavolo
- Grafico prenotazioni giornaliere
- Orario più prenotato
- Zona preferita
- Giorno della settimana più richiesto

---

## Storico Serate

1. Cliccare su **"Storico Serate"**
2. Visualizzare l'archivio dei fuori menu serviti ogni sera
3. Cliccare su una serata per vedere i dettagli

---

## Note utili

- **QR Code**: stampare il QR code dalla sezione Strumenti e posizionarlo sui tavoli per permettere ai clienti di accedere al menu
- **PWA**: i clienti possono installare l'app sul telefono per un accesso più rapido
- **Backup**: tutti i dati sono salvati in Firestore (cloud), non si perdono mai
- **Sicurezza**: solo chi ha le credenziali admin può accedere al pannello di gestione
