# Workshop 09-06-26 — Slide plugin Broadcaster + fix telefono + ricalibrazione timer

**Data**: 2026-05-23
**File target**: `workshop-ivan.html` (e `index.html` se duplicato)
**Repo**: workshop-ivan-09-06-26 (Netlify: workshop)

## 1. Obiettivo

Aggiungere 5 slide dopo l'attuale s25 per presentare il plugin Claude WhatsApp Broadcaster (regalo iscrizione, già installato all'ascoltatore). Rifare il mockup telefono WhatsApp delle slide 24-25 (e nuova s30) con look moderno 2025. Ricalibrare il timer su 60 minuti reali distribuiti sulle 37 slide finali.

## 2. Contesto

Workshop di 60 minuti (9 giugno 2026) strutturato su 3 sezioni: Cowork → Small Business Plugin → automazioni WhatsApp. Le slide 20-25 coprono casi d'uso WhatsApp (lead, prenotazioni, follow-up, fidelizzazione, reminder appuntamenti, recensioni). Manca il bridge "ecco lo strumento concreto per farlo": il plugin. Il plugin viene regalato all'iscrizione, quindi al momento della presentazione i partecipanti lo hanno già.

Il telefono WhatsApp attuale (`.wa-phone`) usa stile vintage 2014 (header `#075E54`, no status bar, no notch, bolle senza tail, spunte grigie). Va modernizzato per credibilità visiva.

Il timer ha 3 bug:
- Array `timings` ha 34 entry per 32 slide (2 orfane)
- Somma timings = 107 min (sforamento di 47 min sul budget dichiarato 60)
- Formula hardcoded `Math.max(0, 75 - elapsed)` usa 75 invece di 60

## 3. Cinque nuove slide (s26 → s30)

Inserite dopo riga 1381 (`</div>` di chiusura s25), prima del commento `<!-- ══ S22: TRANSITION, CONCLUSIONE ══ -->` a riga 1383. Tutte le slide successive si rinumerano (+5): l'attuale s26 diventa s31, l'attuale s32 diventa s37.

### S26 — Reveal "Il motore è già tuo"

- Slide di tipo transition con gradient verde-viola: `style="background:radial-gradient(ellipse at center,#EDFBF3 0%,var(--bg) 70%)"`
- Wrapper: `.transition-slide`
- Icona hero: emoji 🎁 in cerchio gradient
- h2 grande: "Hai visto cosa si può fare.<br>**Il motore è già tuo.**"
- Sub: "Il plugin che orchestra tutte le automazioni WhatsApp è già installato in Claude Cowork insieme alla tua iscrizione."
- 3 stat-card orizzontali con icona + label:
  - ✅ API ufficiale Meta — Zero rischio ban
  - 🔓 Open source — Repo pubblica GitHub
  - ♾️ Multi-account — Tutti i tuoi WABA in un file
- Badge sotto: `<span class="badge g">🎓 Già installato per te</span>`

### S27 — "Tre cose, fatte bene"

- Label verde: `💡 IL PLUGIN, IN BREVE`
- h2.med: "Tre cose,<br><span style='color:var(--wa-d)'>fatte bene.</span>"
- Sub: "Non un tool generalista. Risolve tre problemi concreti di chi gestisce WhatsApp Business in modo professionale."
- Grid 3 colonne (replica le 3 card della landing, riadattate al design system del deck):
  1. **Più WABA, un solo posto** — Configuri ogni account in `waba_config.json` e li orchestri con un comando.
  2. **Campagne template da Excel** — Carichi il foglio, scegli il template, parte. 1,5s tra invii. Log riga per riga.
  3. **Liste grandi, invio a blocchi** — Sopra il limite giornaliero? Il plugin spezza la lista e programma i blocchi via task Cowork.
- Card di chiusura full-width gradient orange-l → wa-l: "Niente tool grigi. Niente rischio ban. **Solo API ufficiali Meta.**"

### S28 — "Tutto quello che serve"

- Label viola: `⚙️ COSA TROVI DENTRO`
- h2.med: "Tutto quello che serve,<br><span style='color:var(--wa-d)'>già pronto.</span>"
- Grid 3x2 mini-card (icona + titolo + 1 riga sub):
  - 🏢 **Multi-account WABA** — Più brand, più sedi, più clienti
  - 📋 **Gestione template** — Crea, elenca, elimina
  - 📊 **Limiti e quality rating** — Sempre sotto controllo
  - ✅ **Validazione E.164** — Numeri normalizzati e verificati
  - 🧩 **Placeholder controllati** — Mapping Excel ↔ template
  - 📝 **Log riga per riga** — Stato, message_id, timestamp, errore
- Footer pill: "**Tu scrivi l'intento. Lui esegue.**"

### S29 — "Installazione in 3 passi"

- Label arancione: `🚀 INSTALLAZIONE`
- h2.med: "Da zero a campagna,<br><span style='color:var(--orange)'>in 2 minuti.</span>"
- Sub: "Nessuna configurazione tecnica complessa. Tre passi."
- 3 step orizzontali con `.auto-step` + `.auto-step-connector` (riusa CSS esistente):
  1. **Apri il plugin in Cowork** — Doppio click su `ivanlao-whatsapp-broadcaster.plugin`. Cowork installa e conferma.
  2. **Installa le dipendenze Python** — Una volta sola: `pip install --break-system-packages requests openpyxl phonenumbers`
  3. **Apri la cartella DEMO** — Scrivi "configura WABA". La skill ti guida.
- Pill di chiusura purple: "⏱ Tempo totale: ~2 minuti"

### S30 — "Demo: una campagna in diretta"

- Label verde: `🎬 DEMO LIVE`
- h2.med: "Excel + 1 comando =<br><span style='color:var(--wa-d)'>campagna massiva.</span>"
- Layout `.auto-flow-layout` (2 col):
  - **Sinistra**: telefono WA moderno (componente `.wa-phone-v2` definito sotto). Conversazione:
    - Avatar/nome: "Ivan Lao Marketing" + "online"
    - Out: "Ciao Marco! 🎉 Hai vinto l'accesso al workshop AI del 9 giugno. Conferma la presenza?" + timestamp 09:00 + ✓✓ azzurre
    - In: "Confermato, grazie!" + 09:04
    - Out: "Perfetto Marco ✅ Ti aspettiamo!" + 09:04 + ✓✓ azzurre
  - **Destra**: flow compatto con 9 mini-step (riusa `.auto-step` ma in versione compressa 2 colonne interne se serve):
    1. Scelta template
    2. Analisi placeholder
    3. Controllo limiti e quality
    4. Conteggio contatti
    5. Modalità: unico / blocchi
    6. Validazione numeri E.164
    7. Mapping Excel ↔ template
    8. Avviso compliance
    9. Conferma + invio (1.5s tra messaggi)
- Sotto: console box stile terminale (background `#0F172A`, color `#10B981`, font monospace):
  ```
  > 234/500 inviati · 12 duplicati · 2 errori · ETA 09:18
  ```
- CTA chiusura: "Apri Cowork. Scrivi: *invia questo Excel su WhatsApp*. Ti guida lui."

## 4. Telefono WhatsApp moderno (`.wa-phone-v2`)

Nuovo componente CSS aggiunto alla sezione "AUTOMATION FLOW" (intorno a riga 353-374). Sostituisce **`.wa-phone`** nelle slide 24, 25 e nuova s30. Il vecchio CSS `.wa-phone` resta per compatibilità con eventuali altri usi (verificato: solo s24, s25 — può essere rimosso, ma per sicurezza lo lasciamo).

### Struttura HTML

```html
<div class="wa-phone-v2">
  <div class="wa-statusbar">
    <span class="wa-time">09:41</span>
    <div class="wa-island"></div>
    <span class="wa-icons">
      <svg>...segnale...</svg>
      <svg>...wifi...</svg>
      <svg>...batteria...</svg>
    </span>
  </div>
  <div class="wa-header">
    <span class="wa-back">‹</span>
    <div class="wa-avatar">S</div>
    <div class="wa-meta">
      <div class="wa-name">Studio Rossi</div>
      <div class="wa-status">online</div>
    </div>
    <div class="wa-actions">
      <svg>...video...</svg>
      <svg>...call...</svg>
      <svg>...menu...</svg>
    </div>
  </div>
  <div class="wa-chat">
    <div class="wa-msg-v2 out">
      Ciao Marco! ...
      <span class="wa-meta-msg">09:00 <span class="wa-ticks">✓✓</span></span>
    </div>
    <div class="wa-msg-v2 in">
      Confermato!
      <span class="wa-meta-msg">09:04</span>
    </div>
  </div>
  <div class="wa-inputbar">
    <span class="wa-emoji">😊</span>
    <span class="wa-input-pill">Messaggio</span>
    <span class="wa-attach">📎</span>
    <span class="wa-mic">🎤</span>
  </div>
</div>
```

### CSS chiave

- `.wa-phone-v2`: `border-radius:42px`, `border:9px solid #1A1A1A`, `background:#1A1A1A`, `box-shadow:0 20px 50px rgba(0,0,0,.25)`, `max-width:300px`, `overflow:hidden`
- `.wa-statusbar`: `background:#000`, `color:#fff`, `display:flex`, `justify-content:space-between`, `padding:10px 22px 6px`, `font-size:12px`, `font-weight:600`, `position:relative`
- `.wa-island`: pill nera centrale `position:absolute`, `left:50%`, `transform:translateX(-50%)`, `top:8px`, `width:90px`, `height:24px`, `background:#000`, `border-radius:14px`
- `.wa-header`: `background:#008069`, `padding:10px 12px`, `display:flex`, `align-items:center`, `gap:10px`, `color:#fff`
- `.wa-back`: `font-size:22px`, `font-weight:300`
- `.wa-avatar`: cerchio 36px, gradient `#16A34A` → `#25D366`, bianco, bold
- `.wa-meta .wa-name`: `font-size:14px`, `font-weight:600`
- `.wa-meta .wa-status`: `font-size:11px`, `opacity:.85`
- `.wa-actions`: `margin-left:auto`, `display:flex`, `gap:14px`, svg 18px
- `.wa-chat`: background `#EFEAE2` con pattern SVG inline doodle subtle (`background-image:url("data:image/svg+xml;utf8,<svg ...>...</svg>")`, opacity .04), `padding:14px 10px`, `min-height:340px`, `display:flex`, `flex-direction:column`, `gap:6px`
- `.wa-msg-v2`: `padding:7px 10px 6px`, `border-radius:8px`, `font-size:13px`, `line-height:1.4`, `max-width:78%`, `position:relative`, `box-shadow:0 1px 1px rgba(0,0,0,.08)`
  - `.wa-msg-v2.out`: `background:#D9FDD3`, `align-self:flex-end`, tail SVG `::after` top-right
  - `.wa-msg-v2.in`: `background:#FFFFFF`, `align-self:flex-start`, tail SVG `::after` top-left
- `.wa-meta-msg`: `font-size:10px`, `color:rgba(0,0,0,.4)`, `display:inline-block`, `margin-left:6px`, `float:right`, `margin-top:2px`
- `.wa-ticks`: `color:#53BDEB`, `font-size:11px`, `font-weight:bold`
- `.wa-inputbar`: `background:#F0F2F5`, `padding:8px 10px`, `display:flex`, `align-items:center`, `gap:8px`
- `.wa-input-pill`: `flex:1`, `background:#fff`, `border-radius:20px`, `padding:8px 14px`, `font-size:13px`, `color:#999`

### Tail SVG (pseudo-element)

Triangolo CSS approssimato o SVG inline `data-url`. Approccio scelto: SVG inline base64 in `::after` per fedeltà visiva. Esempio out tail:

```css
.wa-msg-v2.out::after {
  content: '';
  position: absolute;
  top: 0;
  right: -7px;
  width: 8px;
  height: 13px;
  background: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 8 13'><path fill='%23D9FDD3' d='M0 0 L8 0 L0 13 Z'/></svg>") no-repeat;
}
```

Stessa logica per `.in::after` con `left:-7px` e fill bianco.

### Responsive

A `max-width:600px` (linea ~538 della media query mobile esistente): `.wa-phone-v2 { max-width: 100%; margin: 0 auto }`. Il `.wa-chat` dovrà avere `min-height:260px` su mobile per non occupare tutto lo schermo.

## 5. Ricalibrazione timer

### Array `timings` riscritto (37 entry, somma esatta 60 min)

```js
const timings = [0,1,1,2,2,2,3,1,1,1,2,3,2,1,2,2,1,2,1,3,1,2,2,2,2,2,2,2,1,3,1,1,2,1,1,1,1];
```

Distribuzione per sezione (slide → minuti dedicati):

| Slide range | Sezione contenuto | Minuti |
|---|---|---|
| s1 | Registrazione (pre-start) | 0 |
| s2-s6 | Cover, chi sono, problema, soluzione | 1+1+2+2+2 = 8 |
| s7-s9 | Transition Cowork + Cowork intro + transition SB | 3+1+1 = 5 |
| s10-s13 | Cowork demos + transition SB plugin | 1+2+3+2 = 8 |
| s14-s19 | SB plugin intro/install + demos | 1+2+2+1+2+1 = 9 |
| s20-s25 | Transition WA + 5 automazioni WhatsApp | 3+1+2+2+2+2 = 12 |
| **s26-s30** | **Nuove slide plugin Broadcaster** | **2+2+2+1+3 = 10** |
| s31-s37 | Transition conclusione + risultati + pricing + CTA + grazie | 1+1+2+1+1+1+1 = 8 |

**Totale verificato: 60 min** ✅

### Modifiche JavaScript

- `const TOTAL = 32;` → `const TOTAL = 37;`
- `<div id="counter">1 / 32</div>` → `<div id="counter">1 / 37</div>`
- `const timings = [...]` riscritto come sopra
- `const rem = Math.max(0, 75 - elapsed);` → `const rem = Math.max(0, 60 - elapsed);`

### Modifiche JavaScript

- `const TOTAL = 32;` → `const TOTAL = 37;`
- `<div id="counter">1 / 32</div>` → `<div id="counter">1 / 37</div>`
- `const timings = [...]` riscritto come sopra
- `const rem = Math.max(0, 75 - elapsed);` → `const rem = Math.max(0, 60 - elapsed);`

## 6. Modifiche puntuali al file

| Linea attuale | Modifica |
|---|---|
| 593 | `<div id="timer-badge">⏱ 60 min</div>` — già OK |
| 594 | `<div id="counter">1 / 32</div>` → `1 / 37` |
| 1310-1323 | Riscrittura HTML telefono in s24 da `.wa-phone` a `.wa-phone-v2` con nuova struttura |
| 1361-1373 | Riscrittura HTML telefono in s25 da `.wa-phone` a `.wa-phone-v2` con nuova struttura |
| 1382 (vuota) | **Inserimento 5 nuove slide s26-s30** (~150-200 righe HTML) |
| 1383 → 1384 | Commento `<!-- ══ S22: TRANSITION ══ -->` resta, ma `id="s26"` diventa `id="s31"` |
| 1398 | `id="s27"` → `id="s32"` |
| 1436 | `id="s28"` → `id="s33"` |
| 1461 | `id="s29"` → `id="s34"` |
| 1508 | `id="s30"` → `id="s35"` |
| 1580 | `id="s31"` → `id="s36"` |
| 1641 | `id="s32"` → `id="s37"` |
| ~370 | Aggiunto CSS `.wa-phone-v2` e tutti i sotto-componenti |
| 1674 | `const TOTAL = 32;` → `const TOTAL = 37;` |
| 1701 | Nuovo array `timings` |
| 1704 | Cap `75` → `60` |

Sincronizzare anche `index.html` (è copia identica di `workshop-ivan.html` per Netlify routing).

## 7. Deploy

Repo: `IvanLaoMarketing/workshop-ivan-09-06-26` (privata)  
Branch: `main`  
Deploy: automatico via Deploy Key webhook → Netlify project `workshop`

Workflow:
1. `git add -A` (workshop-ivan.html, index.html, docs/superpowers/specs/...)
2. `git commit -m "Aggiunte 5 slide plugin Broadcaster, telefono WA modernizzato, timer ricalibrato a 60 min"`
3. `git push origin main`
4. Verifica deploy Netlify (timer, slide nuove, telefono)

## 8. Aggiornamento istruzioni-siti.md

Aggiungere riga al log delle modifiche del progetto workshop-ivan-09-06-26: data 23/05/2026 — Aggiunte 5 slide presentazione plugin Claude WhatsApp Broadcaster (s26-s30), modernizzato mockup telefono WA in stile 2025, ricalibrato timer su 60 min reali.

## 9. Out of scope

- Animazioni JS aggiuntive sulle nuove slide (manteniamo `.anim-up.dN` esistente)
- Demo reali dinamiche (la console "234/500 inviati" è statica)
- Localizzazione (workshop è solo IT)
- Refactor del vecchio CSS `.wa-phone` (resta, ma non più usato)
- Sincronizzazione automatica `workshop-ivan.html` ↔ `index.html` (manuale, già pratica esistente)

## 10. Verifica criteri di successo

- [ ] 37 slide totali nel deck, navigabili con frecce/touch
- [ ] Counter "1 / 37" e timer mostra "~60 min rimasti" su s1
- [ ] Su slide ipotetica s37, timer mostra ~0 min (entro 1 min di tolleranza)
- [ ] Slide 26-30 inserite con stile coerente al resto
- [ ] Telefono in s24, s25, s30 mostra status bar, dynamic island, header verde moderno, bolle con tail, spunte azzurre, input bar
- [ ] Responsive mobile mantenuto (testare a ≤600px)
- [ ] Commit pushato, Netlify deploy verde, sito live aggiornato
- [ ] istruzioni-siti.md aggiornato
