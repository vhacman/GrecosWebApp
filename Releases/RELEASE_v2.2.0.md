# Release v2.2.0 — 13-04-2026

## Comande Cucina, Storico Comande, Conferma Salvataggio

---

### Novità

#### Comande e Preconti unificati in un unico componente
Il componente `/admin/preconto` gestisce ora entrambe le modalità tramite un signal
`modo: 'preconto' | 'comanda'`. Dalla dashboard la card "Comande e Preconti" apre
direttamente in modalità Comanda (query param `?modo=comanda`). Un bottone in cima
alla pagina consente di passare all'altra modalità in qualsiasi momento senza perdere
le voci inserite. In modalità Comanda i prezzi, i subtotali e la barra totale sono
nascosti; il PDF generato riporta l'etichetta "COMANDA" senza importi.

---

#### Salvataggio comanda su Firestore
Il bottone "Salva comanda" apre un popup di conferma ("Salvare? Sì / Annulla") prima
di scrivere il documento nella collezione `comande/`. Il documento contiene data, ora,
numero tavolo, nome cliente opzionale e lista voci. Premendo "Aggiorna" (quando la
comanda è già salvata) il documento esistente viene sovrascritto.
Il campo `nomeCliente` è omesso se non compilato (fix errore Firestore `undefined`).

---

#### Storico comande (`/admin/comande-storia`)
Nuova pagina con lista comande filtrabili per giorno tramite date picker.
KPI in testa: numero totale comande e numero "da convertire" (senza preconto collegato).
Ogni card mostra tavolo, nome cliente, ora, numero voci e badge di stato:
verde "Preconto" (collegato) o giallo "Da convertire". Il corpo espanso mostra
la lista voci e i bottoni "Vai al preconto", "Modifica" e "Elimina" con conferma.

---

#### Collegamento comanda → preconto
Premendo "Salva preconto" in modalità Comanda, il componente crea il documento preconto
e chiama `collegaPrecontoAComanda(comandaId, precontoId)` per scrivere il riferimento
nel documento comanda. Nello storico comande il badge diventa verde e il bottone
"Vai al preconto" naviga a `/admin/preconto?id=<precontoId>`.

---

#### Conferma salvataggio anche per il preconto
Tutti i punti di innesco "Salva preconto" (bottone header, barra totale, barra comanda)
aprono un popup di conferma prima di scrivere su Firestore. Il testo è contestuale:
"Aggiornare il preconto?" in modalità modifica, "Salvare il preconto?" per nuovi preconti.

---

#### Regola Firestore per `comande/`
Aggiunta la regola `allow read, write: if request.auth != null` per la nuova
collezione `comande/` nel file `firestore.rules`, deployata su Firebase.
