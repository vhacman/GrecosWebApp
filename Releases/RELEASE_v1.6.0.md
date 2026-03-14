# Release v1.6.0 — 14-03-2026

## Deploy Affidabile, Cache PWA e Analytics Pulita

---

### Predeploy Build Automatico

`firebase.json` ora include l'hook `predeploy: ["npm run build"]`.

Prima di questa release, `firebase deploy` caricava il contenuto della cartella `dist/` senza ricompilare il progetto. Se il build non era stato eseguito manualmente, veniva deployata la versione precedente — le modifiche locali erano visibili su `localhost` ma non online.

Adesso ogni `firebase deploy` esegue automaticamente `ng build` come primo passo, garantendo che il codice pubblicato corrisponda sempre al codice sorgente corrente.

---

### Cache Headers per Service Worker e Bundle

Aggiunti header `Cache-Control` espliciti in `firebase.json`:

| File | Cache-Control |
|---|---|
| `ngsw-worker.js` | `no-cache, no-store, must-revalidate` |
| `ngsw.json` | `no-cache, no-store, must-revalidate` |
| `index.html` | `no-cache, no-store, must-revalidate` |
| `*.js`, `*.css`, font | `public, max-age=31536000, immutable` |

**Problema risolto:** Firebase Hosting serviva `ngsw-worker.js` e `ngsw.json` con la cache di default (fino a 1 ora). Il Service Worker non rilevava le nuove versioni perché il manifest (`ngsw.json`) veniva servito dalla cache HTTP del browser. `checkForUpdate()` in `app.ts` era inefficace finché la cache non scadeva.

Con `no-cache` su questi file, il browser verifica sempre se c'è una versione aggiornata prima di usare quella in cache. I bundle JS/CSS mantengono `immutable` perché cambiano hash ad ogni build — non c'è rischio di servire versioni obsolete.

---

### Filtro Localhost in Microsoft Clarity

`main.ts` ora non inizializza Clarity su `localhost` o su indirizzi LAN (`192.168.*`):

```typescript
const isLocal = window.location.hostname === 'localhost'
             || window.location.hostname.startsWith('192.168.');
if (!isLocal && !window.location.pathname.startsWith('/admin')) {
  Clarity.init('vqyr871p34');
}
```

**Problema risolto:** le sessioni di sviluppo venivano registrate da Clarity come sessioni reali. Sui dati del periodo 17/02–14/03/2026 (1172 sessioni totali), 211 provenivano da `localhost` o `192.168.178.105` — il 18% del totale. Le statistiche su browser, referrer e percorsi di navigazione erano inquinate da sessioni di test.

---

### Fix: Bottone WhatsApp Senza Stile nell'Anteprima Prenotazioni

La classe CSS `.wa-btn` mancava da `prenotazioni-modal.css`. Il bottone "Invia su WhatsApp" nel modal di anteprima immagine era visibile ma senza stile (testo nero su sfondo trasparente).

Aggiunto con:
- `background: #25D366` (verde WhatsApp ufficiale)
- `width: 100%`, `border-radius: 0.75rem`
- `display: flex`, `align-items: center`, `gap: 0.5rem`
- `:active` → `#1ebe5d`

---

### Dati Analytics al Momento della Release

Dati Microsoft Clarity al 14-03-2026 (periodo 17/02–14/03):

| Metrica | Valore |
|---|---|
| Sessioni reali (tolti bot e localhost) | ~930 |
| Media giornaliera | ~37 sessioni/giorno |
| Profondità di scorrimento media | 91.36% |
| Browser principale | MobileSafari 40.7% |
| Traffico da Instagram | 65 sessioni |
| Traffico da LinkedIn | 75 sessioni |
| Traffico da Google | 37 sessioni |
| LCP | 2.312s |
| INP | 238ms |
