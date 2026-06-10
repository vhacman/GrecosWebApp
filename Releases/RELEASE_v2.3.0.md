# Release v2.3.0 — 16-04-2026

## Performance +14%, SEO, Accessibilità WCAG AA

---

### Performance (Lighthouse mobile: 76 → 87)

#### CSS ridotto del 56% (transfer size)
Rimosso `bootstrap.min.css` completo (33 KB gzipped). Sostituito con
`bootstrap-reboot.min.css` + `bootstrap-icons.css` caricati direttamente via
`angular.json` (nessun processing Sass). Solo `.ms-auto` aggiunto manualmente come
unica utility Bootstrap effettivamente usata nei template. `styles.css` convertito
in `styles.scss`; warning deprecazione `@import` Sass eliminati.
Transfer size CSS: 34 KB → 15 KB (−56%).

#### Clarity.js spostato fuori dal percorso critico
`Clarity.init()` era eseguito *prima* di `bootstrapApplication()`. Ora viene
inizializzato via `requestIdleCallback` (fallback `setTimeout`) *dopo* il bootstrap
dell'app, eliminando il contributo al TBT della prima visita.

**Metriche:** FCP 3.5 s → 2.0 s | LCP 4.2 s → 2.7 s | SI 3.5 s → 2.3 s

---

### SEO

#### robots.txt creato
`public/robots.txt` — Lighthouse segnalava score 0 per assenza del file.
Il file permette tutto tranne `/admin` e dichiara il sitemap.

---

### Fix aspect ratio immagini

Lighthouse audit "Displays images with incorrect aspect ratio" — tre immagini mostrate
con attributi HTML `width`/`height` non corrispondenti alle dimensioni reali:

| File | Dimensioni reali | Prima | Dopo |
|---|---|---|---|
| `iconaNatalino.png` | 745×946 | `width="160" height="160"` | `width="126" height="160"` + `aspect-ratio:745/946` |
| `iconaNatalino.webp` | 252×320 | `width="40" height="40"` | `width="40" height="51"` + `height:auto` |
| `logo1 (1).png` | 1130×1600 | `width="52" height="52"` | `width="37" height="52"` |

Il CLS fix (spazio riservato pre-caricamento) rimane valido grazie agli attributi aggiornati.
