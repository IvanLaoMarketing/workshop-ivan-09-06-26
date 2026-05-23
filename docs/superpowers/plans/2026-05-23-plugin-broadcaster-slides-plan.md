# Workshop 09-06-26 — Slide plugin Broadcaster + telefono WA moderno + timer 60min — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Aggiungere 5 slide al deck workshop dopo s25 che presentano il plugin claude-whatsapp-broadcaster (regalo iscrizione), modernizzare il mockup telefono WhatsApp in s24/s25/nuova-s30, ricalibrare il timer su 60 minuti reali su 37 slide totali. Nessun deploy fino a verifica visiva da parte dell'utente.

**Architecture:** Sito statico HTML/CSS/JS in singolo file (`workshop-ivan.html`, duplicato in `index.html` per Netlify root). Nessun build, nessun framework, nessun test runner. Le 5 nuove slide riusano CSS esistente (`.auto-flow-layout`, `.auto-step`, `.label`, `.badge`, `h2.med`, `.card`, `.transition-slide`). Nuovo componente `.wa-phone-v2` aggiunto nella sezione CSS "AUTOMATION FLOW" e usato in 3 punti. Timer e contatore aggiornati nello script in coda al file.

**Tech Stack:** HTML5, CSS3 (DM Sans + Outfit + Font Awesome esistenti), JS vanilla. Preview tramite Claude Preview MCP (`preview_start` su porta locale, `preview_snapshot`, `preview_screenshot`).

**Spec di riferimento:** [docs/superpowers/specs/2026-05-23-plugin-broadcaster-slides-design.md](../specs/2026-05-23-plugin-broadcaster-slides-design.md)

**Vincolo critico:** Nessun `git commit` né `git push` in questo piano. Le modifiche restano nel working tree finché l'utente non approva visivamente. Il commit + push + deploy Netlify saranno un follow-up separato dopo l'OK.

---

## File Structure

| File | Modifica |
|---|---|
| `workshop-ivan.html` | Modificato in 5 zone: CSS componenti telefono v2 (~riga 374), HTML telefono in s24 (1310-1323), HTML telefono in s25 (1361-1373), inserimento 5 nuove slide dopo riga 1381, rinumerazione id slide post-inserimento (1384, 1398, 1436, 1461, 1508, 1580, 1641), counter HTML (594), script in coda (1674, 1701, 1704) |
| `index.html` | Sostituito integralmente con copia di `workshop-ivan.html` finale (sync) |
| `../istruzioni-siti.md` | Append riga al log progetto workshop-ivan-09-06-26 con data 23/05/2026 e modifiche |

Tutte le modifiche sono additive o sostituzioni puntuali. Nessun file nuovo (lo spec già committato è l'unica eccezione).

---

## Note operative comuni

**Preview server**: porta consigliata 8765 servendo `/Users/ivanlao/Documents/Vibe Coding/Claude Code/siti-ivan/workshop-ivan-09-06-26/`. Apertura via `mcp__Claude_Preview__preview_start` con `command: "python3 -m http.server 8765"` e `cwd` impostato sulla directory del progetto, oppure `npx serve -p 8765`. Una volta avviato, navigare a `http://localhost:8765/workshop-ivan.html`.

**Workflow di verifica per ogni step di modifica visiva**:
1. Applicare la modifica con `Edit`
2. `preview_eval`: `window.location.reload()` (oppure ricaricare manualmente se HMR non c'è)
3. `preview_eval` per navigare alla slide target: `goTo(N)` (esposta globalmente nello script)
4. `preview_snapshot` per vedere il DOM/testo
5. `preview_console_logs` per vedere eventuali errori JS
6. Solo se la verifica è OK, passa al task successivo

**Comando di navigazione slide**: lo script definisce `goTo(n)`. Da preview console: `goTo(26)` salta direttamente alla slide 26.

---

### Task 1: Aggiungi nuovo componente CSS `.wa-phone-v2`

**Files:**
- Modify: `workshop-ivan.html` (sezione CSS, dopo riga 374 chiusura `.wa-time`)

- [ ] **Step 1: Avvia preview server e apri il deck nello stato attuale (baseline)**

```bash
cd "/Users/ivanlao/Documents/Vibe Coding/Claude Code/siti-ivan/workshop-ivan-09-06-26"
```

Avvia preview con `mcp__Claude_Preview__preview_start`:
- command: `python3 -m http.server 8765`
- cwd: directory del progetto
- url: `http://localhost:8765/workshop-ivan.html`

`preview_eval`: `goTo(24)` → conferma che il telefono attuale (vecchio stile) è visibile su s24.

- [ ] **Step 2: Inserisci il blocco CSS `.wa-phone-v2` dopo la riga 374**

Trova la riga 374 nel file (`/* termina con .wa-time */`) e aggiungi DOPO la riga `.wa-time{font-size:10px;color:rgba(0,0,0,.4);text-align:right;margin-top:2px}` il seguente blocco CSS:

```css
/* ── WHATSAPP PHONE V2 (modern 2025 mockup) ── */
.wa-phone-v2{max-width:300px;margin:0 auto;background:#1A1A1A;border:9px solid #1A1A1A;border-radius:42px;box-shadow:0 20px 50px rgba(0,0,0,.25),0 6px 16px rgba(0,0,0,.15);overflow:hidden;position:relative}
.wa-statusbar{background:#000;color:#fff;display:flex;justify-content:space-between;align-items:center;padding:8px 22px 6px;font-size:12px;font-weight:600;position:relative;min-height:30px}
.wa-statusbar .wa-sb-time{font-variant-numeric:tabular-nums;z-index:2}
.wa-statusbar .wa-sb-icons{display:inline-flex;gap:5px;align-items:center;z-index:2}
.wa-statusbar .wa-sb-icons svg{width:14px;height:11px;fill:#fff}
.wa-statusbar .wa-sb-icons .wa-batt{width:22px;height:11px}
.wa-island{position:absolute;left:50%;top:6px;transform:translateX(-50%);width:90px;height:24px;background:#000;border-radius:14px;z-index:1}
.wa-header{background:#008069;padding:9px 12px;display:flex;align-items:center;gap:10px;color:#fff}
.wa-back{font-size:22px;font-weight:300;line-height:1;color:#fff;opacity:.95}
.wa-header .wa-av{width:34px;height:34px;border-radius:50%;background:linear-gradient(135deg,#16A34A,#25D366);display:flex;align-items:center;justify-content:center;color:#fff;font-weight:700;font-size:13px;font-family:'Outfit',sans-serif;flex-shrink:0}
.wa-meta{display:flex;flex-direction:column;line-height:1.2;min-width:0;flex:1}
.wa-meta .wa-name{font-size:14px;font-weight:600;color:#fff;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.wa-meta .wa-status{font-size:11px;color:rgba(255,255,255,.85)}
.wa-actions{margin-left:auto;display:flex;gap:16px;align-items:center;color:#fff}
.wa-actions svg{width:18px;height:18px;fill:#fff;opacity:.95}
.wa-chat{background:#EFEAE2;padding:14px 10px;min-height:340px;display:flex;flex-direction:column;gap:6px;background-image:url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='80' height='80' viewBox='0 0 80 80'><g fill='%23000' fill-opacity='0.04'><circle cx='10' cy='10' r='1.2'/><circle cx='50' cy='30' r='1.2'/><circle cx='30' cy='60' r='1.2'/><circle cx='70' cy='70' r='1.2'/></g></svg>")}
.wa-msg-v2{padding:7px 10px 6px;border-radius:8px;font-size:13px;line-height:1.45;max-width:78%;position:relative;box-shadow:0 1px 1px rgba(0,0,0,.08);word-wrap:break-word}
.wa-msg-v2.out{background:#D9FDD3;align-self:flex-end;border-top-right-radius:0}
.wa-msg-v2.in{background:#FFFFFF;align-self:flex-start;border-top-left-radius:0}
.wa-msg-v2.out::after{content:'';position:absolute;top:0;right:-7px;width:8px;height:13px;background:url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 8 13'><path fill='%23D9FDD3' d='M0 0 L8 0 L0 13 Z'/></svg>") no-repeat}
.wa-msg-v2.in::after{content:'';position:absolute;top:0;left:-7px;width:8px;height:13px;background:url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 8 13'><path fill='%23FFFFFF' d='M8 0 L0 0 L8 13 Z'/></svg>") no-repeat}
.wa-meta-msg{font-size:10px;color:rgba(0,0,0,.45);display:inline-block;margin-left:8px;float:right;margin-top:3px;line-height:1;white-space:nowrap}
.wa-ticks{color:#53BDEB;font-weight:bold;margin-left:2px}
.wa-inputbar{background:#F0F2F5;padding:8px 10px;display:flex;align-items:center;gap:8px}
.wa-input-pill{flex:1;background:#fff;border-radius:20px;padding:8px 14px;font-size:13px;color:#999;display:flex;align-items:center;gap:8px}
.wa-input-pill .wa-emoji{opacity:.6}
.wa-attach,.wa-mic{font-size:18px;opacity:.7}
```

- [ ] **Step 3: Aggiungi override responsive per `.wa-phone-v2`**

Trova nella media query mobile (riga ~538-560) la regola `.wa-phone{max-width:100%;margin:0 auto}` (riga 559). DOPO quella riga inserisci:

```css
  .wa-phone-v2{max-width:100%}
  .wa-chat{min-height:260px}
```

Posizione esatta: dentro la media query `@media (max-width: 600px)` aperta a riga 538.

- [ ] **Step 4: Verifica che il CSS sia caricato e non rompa nulla**

`preview_eval`: `window.location.reload()`  
`preview_eval`: `goTo(24)` → la pagina deve caricarsi senza errori console. Il telefono vecchio è ancora visibile (non abbiamo cambiato HTML).  
`preview_console_logs` → nessun errore CSS o JS.  
`preview_eval`: `document.styleSheets[0].cssRules.length` → numero rules deve essere aumentato rispetto al baseline.

---

### Task 2: Sostituisci il telefono nella slide 24 con `.wa-phone-v2`

**Files:**
- Modify: `workshop-ivan.html:1310-1323` (blocco `.wa-phone` dentro s24)

- [ ] **Step 1: Identifica esattamente il blocco vecchio**

Cerca nel file il blocco che inizia con `<div class="wa-phone anim-up d2">` dentro la slide `id="s24"` (intorno a riga 1310). Termina con `</div>` di chiusura del `.wa-phone` (riga 1323).

- [ ] **Step 2: Sostituisci con il nuovo markup `.wa-phone-v2`**

Usa `Edit` per sostituire il blocco esistente (linee 1310-1323) con:

```html
      <div class="wa-phone-v2 anim-up d2">
        <div class="wa-statusbar">
          <span class="wa-sb-time">09:00</span>
          <span class="wa-island"></span>
          <span class="wa-sb-icons">
            <svg viewBox="0 0 16 12"><path d="M0 11h2v1H0zm3-2h2v3H3zm3-3h2v6H6zm3-3h2v9H9zm3-3h2v12h-2z"/></svg>
            <svg viewBox="0 0 16 12"><path d="M8 1.5C5.5 1.5 3.2 2.4 1.4 4l1.4 1.4C4.2 4 6 3.5 8 3.5s3.8.5 5.2 1.9L14.6 4C12.8 2.4 10.5 1.5 8 1.5zm0 4C6.3 5.5 4.8 6.1 3.7 7.2l1.4 1.4C5.9 7.8 6.9 7.5 8 7.5s2.1.3 2.9 1.1l1.4-1.4C11.2 6.1 9.7 5.5 8 5.5zm0 4c-.8 0-1.5.3-2.1.9L8 12.5l2.1-2.1c-.6-.6-1.3-.9-2.1-.9z"/></svg>
            <svg class="wa-batt" viewBox="0 0 22 11"><rect x="0.5" y="0.5" width="19" height="10" rx="2" fill="none" stroke="#fff"/><rect x="2" y="2" width="16" height="7" rx="1"/><rect x="20" y="3.5" width="1.5" height="4" rx=".5"/></svg>
          </span>
        </div>
        <div class="wa-header">
          <span class="wa-back">‹</span>
          <div class="wa-av">S</div>
          <div class="wa-meta">
            <div class="wa-name">Studio Rossi</div>
            <div class="wa-status">online</div>
          </div>
          <div class="wa-actions">
            <svg viewBox="0 0 24 24"><path d="M17 10.5V7c0-.6-.4-1-1-1H4c-.6 0-1 .4-1 1v10c0 .6.4 1 1 1h12c.6 0 1-.4 1-1v-3.5l4 4v-11l-4 4z"/></svg>
            <svg viewBox="0 0 24 24"><path d="M20.01 15.38c-1.23 0-2.42-.2-3.53-.56-.35-.12-.74-.03-1.01.24l-1.57 1.97c-2.83-1.35-5.48-3.9-6.89-6.83l1.95-1.66c.27-.28.35-.67.24-1.02-.37-1.11-.56-2.3-.56-3.53 0-.54-.45-.99-.99-.99H4.19C3.65 3 3 3.24 3 3.99 3 13.28 10.73 21 20.01 21c.71 0 .99-.63.99-1.18v-3.45c0-.54-.45-.99-.99-.99z"/></svg>
            <svg viewBox="0 0 24 24"><path d="M12 8c1.1 0 2-.9 2-2s-.9-2-2-2-2 .9-2 2 .9 2 2 2zm0 2c-1.1 0-2 .9-2 2s.9 2 2 2 2-.9 2-2-.9-2-2-2zm0 6c-1.1 0-2 .9-2 2s.9 2 2 2 2-.9 2-2-.9-2-2-2z"/></svg>
          </div>
        </div>
        <div class="wa-chat">
          <div class="wa-msg-v2 out">Ciao Marco! 👋 Ti ricordiamo il tuo appuntamento:<br><br>📅 Domani, Martedì 10 Giugno<br>🕐 Ore 15:00<br>📍 Via Roma 42, Firenze<br><br>Confermi la presenza?<span class="wa-meta-msg">09:00<span class="wa-ticks">✓✓</span></span></div>
          <div class="wa-msg-v2 in">Sì confermo, ci sarò!<span class="wa-meta-msg">09:12</span></div>
          <div class="wa-msg-v2 out">Perfetto Marco! ✅ Appuntamento confermato.<br><br>Se hai bisogno di spostarlo, scrivici qui. A domani!<span class="wa-meta-msg">09:12<span class="wa-ticks">✓✓</span></span></div>
          <div class="wa-msg-v2 out">Ciao Marco! 😊 Ti aspettiamo domani.<br>Per qualsiasi esigenza, scrivici qui!<span class="wa-meta-msg">09:30<span class="wa-ticks">✓✓</span></span></div>
        </div>
        <div class="wa-inputbar">
          <div class="wa-input-pill"><span class="wa-emoji">😊</span> Messaggio</div>
          <span class="wa-attach">📎</span>
          <span class="wa-mic">🎤</span>
        </div>
      </div>
```

- [ ] **Step 3: Verifica visiva slide 24**

`preview_eval`: `window.location.reload()`  
`preview_eval`: `goTo(24)`  
`preview_console_logs` → nessun errore  
`preview_snapshot` → verifica presenza nodi `.wa-statusbar`, `.wa-header`, `.wa-msg-v2.out`, `.wa-msg-v2.in`, `.wa-inputbar`  
`preview_screenshot` → guarda il telefono e verifica:
- Status bar nera in cima con "09:00" sx e icone dx
- Dynamic island al centro
- Header verde `#008069` con back-arrow + avatar S + "Studio Rossi" + "online" + 3 icone
- Bolle verdi (out) con tail in alto a destra
- Bolla bianca (in) con tail in alto a sinistra
- Spunte azzurre dentro le bolle out
- Bottom: barra input con 😊 + "Messaggio" + 📎 + 🎤

---

### Task 3: Sostituisci il telefono nella slide 25 con `.wa-phone-v2`

**Files:**
- Modify: `workshop-ivan.html:1361-1373` (blocco `.wa-phone` dentro s25)

- [ ] **Step 1: Sostituisci il blocco**

Trova il blocco che inizia con `<div class="wa-phone" style="margin:0 auto">` dentro `id="s25"` (riga 1361). Termina con `</div>` di chiusura prima della `<div class="card anim-up d4" ...>` (riga 1373).

Sostituisci con:

```html
        <div class="wa-phone-v2">
          <div class="wa-statusbar">
            <span class="wa-sb-time">10:00</span>
            <span class="wa-island"></span>
            <span class="wa-sb-icons">
              <svg viewBox="0 0 16 12"><path d="M0 11h2v1H0zm3-2h2v3H3zm3-3h2v6H6zm3-3h2v9H9zm3-3h2v12h-2z"/></svg>
              <svg viewBox="0 0 16 12"><path d="M8 1.5C5.5 1.5 3.2 2.4 1.4 4l1.4 1.4C4.2 4 6 3.5 8 3.5s3.8.5 5.2 1.9L14.6 4C12.8 2.4 10.5 1.5 8 1.5zm0 4C6.3 5.5 4.8 6.1 3.7 7.2l1.4 1.4C5.9 7.8 6.9 7.5 8 7.5s2.1.3 2.9 1.1l1.4-1.4C11.2 6.1 9.7 5.5 8 5.5zm0 4c-.8 0-1.5.3-2.1.9L8 12.5l2.1-2.1c-.6-.6-1.3-.9-2.1-.9z"/></svg>
              <svg class="wa-batt" viewBox="0 0 22 11"><rect x="0.5" y="0.5" width="19" height="10" rx="2" fill="none" stroke="#fff"/><rect x="2" y="2" width="16" height="7" rx="1"/><rect x="20" y="3.5" width="1.5" height="4" rx=".5"/></svg>
            </span>
          </div>
          <div class="wa-header">
            <span class="wa-back">‹</span>
            <div class="wa-av">S</div>
            <div class="wa-meta">
              <div class="wa-name">Studio Rossi</div>
              <div class="wa-status">online</div>
            </div>
            <div class="wa-actions">
              <svg viewBox="0 0 24 24"><path d="M17 10.5V7c0-.6-.4-1-1-1H4c-.6 0-1 .4-1 1v10c0 .6.4 1 1 1h12c.6 0 1-.4 1-1v-3.5l4 4v-11l-4 4z"/></svg>
              <svg viewBox="0 0 24 24"><path d="M20.01 15.38c-1.23 0-2.42-.2-3.53-.56-.35-.12-.74-.03-1.01.24l-1.57 1.97c-2.83-1.35-5.48-3.9-6.89-6.83l1.95-1.66c.27-.28.35-.67.24-1.02-.37-1.11-.56-2.3-.56-3.53 0-.54-.45-.99-.99-.99H4.19C3.65 3 3 3.24 3 3.99 3 13.28 10.73 21 20.01 21c.71 0 .99-.63.99-1.18v-3.45c0-.54-.45-.99-.99-.99z"/></svg>
              <svg viewBox="0 0 24 24"><path d="M12 8c1.1 0 2-.9 2-2s-.9-2-2-2-2 .9-2 2 .9 2 2 2zm0 2c-1.1 0-2 .9-2 2s.9 2 2 2 2-.9 2-2-.9-2-2-2zm0 6c-1.1 0-2 .9-2 2s.9 2 2 2 2-.9 2-2-.9-2-2-2z"/></svg>
            </div>
          </div>
          <div class="wa-chat">
            <div class="wa-msg-v2 out">Ciao Laura! 😊<br><br>Grazie per essere venuta martedì per la consulenza marketing.<br><br>Se hai 30 secondi, ci farebbe molto piacere una tua recensione su Google. Il tuo feedback ci aiuta tantissimo! ⭐<br><br>👉 [Lascia una recensione]<span class="wa-meta-msg">10:00<span class="wa-ticks">✓✓</span></span></div>
            <div class="wa-msg-v2 in">Certo! Mi sono trovata benissimo, la lascio subito 😊<span class="wa-meta-msg">10:23</span></div>
            <div class="wa-msg-v2 out">Grazie mille Laura! ❤️ La tua opinione vale tantissimo per noi.<span class="wa-meta-msg">10:23<span class="wa-ticks">✓✓</span></span></div>
          </div>
          <div class="wa-inputbar">
            <div class="wa-input-pill"><span class="wa-emoji">😊</span> Messaggio</div>
            <span class="wa-attach">📎</span>
            <span class="wa-mic">🎤</span>
          </div>
        </div>
```

- [ ] **Step 2: Verifica visiva slide 25**

`preview_eval`: `window.location.reload()`  
`preview_eval`: `goTo(25)`  
`preview_screenshot` → verifica:
- Stesso stile telefono moderno di s24
- Conversazione: messaggio recensione + "Certo! Mi sono trovata benissimo" + ringraziamento
- La card gialla `⭐⭐⭐⭐⭐` sotto deve restare visibile (era fuori dal blocco sostituito)

---

### Task 4: Inserisci le 5 nuove slide s26-s30 dopo s25

**Files:**
- Modify: `workshop-ivan.html` (inserisci dopo riga 1381 `</div>` di chiusura s25 attuale)

- [ ] **Step 1: Inserisci blocco di 5 nuove slide**

Trova nel file il blocco:

```html
</div>

<!-- ══ S22: TRANSITION, CONCLUSIONE ══ -->
<div class="slide" id="s26" style="background:radial-gradient(ellipse at center,#FFF0E5 0%,var(--bg) 70%)">
```

Sostituiscilo con (mantiene il `</div>` di chiusura s25 attuale, poi inserisce 5 nuove slide, poi continua con la transition conclusione che ora avrà id `s31`):

```html
</div>

<!-- ══ S26: PLUGIN REVEAL "IL MOTORE È GIÀ TUO" ══ -->
<div class="slide" id="s26" style="background:radial-gradient(ellipse at center,#EDFBF3 0%,var(--bg) 70%)">
  <div class="transition-slide">
    <div class="transition-icon anim-up">🎁</div>
    <h2 class="transition-h anim-up d1">Hai visto cosa si può fare.<br><span style="color:var(--wa-d)">Il motore è già tuo.</span></h2>
    <p class="transition-sub anim-up d2">Il plugin che orchestra tutte le automazioni WhatsApp è già installato in Claude Cowork insieme alla tua iscrizione al workshop.</p>
    <div class="anim-up d3" style="display:grid;grid-template-columns:repeat(3,1fr);gap:16px;max-width:760px;margin:24px auto 18px">
      <div class="card" style="text-align:center;padding:20px 16px"><div style="font-size:32px;margin-bottom:8px">✅</div><div style="font-size:14px;font-weight:700;color:var(--text);margin-bottom:4px">API ufficiale Meta</div><div style="font-size:12px;color:var(--muted);line-height:1.4">Zero rischio ban</div></div>
      <div class="card" style="text-align:center;padding:20px 16px"><div style="font-size:32px;margin-bottom:8px">🔓</div><div style="font-size:14px;font-weight:700;color:var(--text);margin-bottom:4px">Open source</div><div style="font-size:12px;color:var(--muted);line-height:1.4">Repo pubblica su GitHub</div></div>
      <div class="card" style="text-align:center;padding:20px 16px"><div style="font-size:32px;margin-bottom:8px">♾️</div><div style="font-size:14px;font-weight:700;color:var(--text);margin-bottom:4px">Multi-account</div><div style="font-size:12px;color:var(--muted);line-height:1.4">Tutti i tuoi WABA, un file</div></div>
    </div>
    <div class="transition-badges anim-up d4">
      <span class="badge g">🎓 Già installato per te</span>
      <span class="badge p">💬 Claude WhatsApp Broadcaster</span>
    </div>
  </div>
</div>

<!-- ══ S27: PLUGIN — TRE COSE FATTE BENE ══ -->
<div class="slide" id="s27">
  <div class="si">
    <div class="label green anim-up">💡 IL PLUGIN, IN BREVE</div>
    <h2 class="med anim-up d1" style="margin-bottom:14px">Tre cose,<br><span style="color:var(--wa-d)">fatte bene.</span></h2>
    <p class="sub anim-up d2" style="margin-bottom:24px">Non un tool generalista. Risolve tre problemi concreti di chi gestisce WhatsApp Business in modo professionale.</p>
    <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:18px;margin-bottom:18px">
      <div class="card anim-up d2"><div style="width:52px;height:52px;border-radius:14px;background:linear-gradient(135deg,var(--purple),#8E72FF);display:flex;align-items:center;justify-content:center;font-size:24px;margin-bottom:14px">🏢</div><div style="font-size:18px;font-weight:800;color:var(--text);margin-bottom:8px">Più WABA, un solo posto</div><div style="font-size:14px;color:var(--muted);line-height:1.55">Configuri ogni account WhatsApp Business in <code style="background:var(--light);padding:2px 6px;border-radius:4px;font-size:12px">waba_config.json</code> e li orchestri da Cowork con un comando.</div></div>
      <div class="card anim-up d3"><div style="width:52px;height:52px;border-radius:14px;background:linear-gradient(135deg,var(--wa-d),var(--wa));display:flex;align-items:center;justify-content:center;font-size:24px;margin-bottom:14px">📤</div><div style="font-size:18px;font-weight:800;color:var(--text);margin-bottom:8px">Campagne template da Excel</div><div style="font-size:14px;color:var(--muted);line-height:1.55">Carichi il foglio, scegli il template approvato, parte. 1,5s tra invii, log riga per riga nel foglio Excel.</div></div>
      <div class="card anim-up d4"><div style="width:52px;height:52px;border-radius:14px;background:linear-gradient(135deg,var(--orange),#FF8B5C);display:flex;align-items:center;justify-content:center;font-size:24px;margin-bottom:14px">📦</div><div style="font-size:18px;font-weight:800;color:var(--text);margin-bottom:8px">Liste grandi, invio a blocchi</div><div style="font-size:14px;color:var(--muted);line-height:1.55">Oltre il limite giornaliero? Il plugin spezza la lista e programma i blocchi via task Cowork giorno per giorno.</div></div>
    </div>
    <div class="anim-up d5" style="background:linear-gradient(90deg,var(--orange-l),var(--wa-l));border:1px solid var(--wa-m);border-radius:var(--r);padding:16px 24px;text-align:center;font-size:15px;color:var(--text)">Niente tool grigi. Niente rischio ban. <strong style="color:var(--wa-d)">Solo API ufficiali Meta.</strong></div>
  </div>
</div>

<!-- ══ S28: PLUGIN — TUTTE LE FUNZIONI ══ -->
<div class="slide" id="s28">
  <div class="si">
    <div class="label purple anim-up">⚙️ COSA TROVI DENTRO</div>
    <h2 class="med anim-up d1" style="margin-bottom:14px">Tutto quello che serve,<br><span style="color:var(--wa-d)">già pronto.</span></h2>
    <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:14px;margin-top:24px">
      <div class="card anim-up d2" style="padding:18px"><div style="display:flex;align-items:center;gap:10px;margin-bottom:8px"><span style="font-size:22px">🏢</span><strong style="font-size:15px">Multi-account WABA</strong></div><div style="font-size:13px;color:var(--muted);line-height:1.5">Più brand, più sedi, più clienti — un solo file di configurazione</div></div>
      <div class="card anim-up d2" style="padding:18px"><div style="display:flex;align-items:center;gap:10px;margin-bottom:8px"><span style="font-size:22px">📋</span><strong style="font-size:15px">Gestione template</strong></div><div style="font-size:13px;color:var(--muted);line-height:1.5">Crea, elenca, elimina i template direttamente dalla skill</div></div>
      <div class="card anim-up d2" style="padding:18px"><div style="display:flex;align-items:center;gap:10px;margin-bottom:8px"><span style="font-size:22px">📊</span><strong style="font-size:15px">Limiti e quality rating</strong></div><div style="font-size:13px;color:var(--muted);line-height:1.5">Tier giornaliero e qualità del numero sempre sotto controllo</div></div>
      <div class="card anim-up d3" style="padding:18px"><div style="display:flex;align-items:center;gap:10px;margin-bottom:8px"><span style="font-size:22px">✅</span><strong style="font-size:15px">Validazione E.164</strong></div><div style="font-size:13px;color:var(--muted);line-height:1.5">Numeri normalizzati automaticamente, ambigui segnalati prima</div></div>
      <div class="card anim-up d3" style="padding:18px"><div style="display:flex;align-items:center;gap:10px;margin-bottom:8px"><span style="font-size:22px">🧩</span><strong style="font-size:15px">Placeholder controllati</strong></div><div style="font-size:13px;color:var(--muted);line-height:1.5">Mapping verificato tra colonne Excel e segnaposto del template</div></div>
      <div class="card anim-up d3" style="padding:18px"><div style="display:flex;align-items:center;gap:10px;margin-bottom:8px"><span style="font-size:22px">📝</span><strong style="font-size:15px">Log riga per riga</strong></div><div style="font-size:13px;color:var(--muted);line-height:1.5">Stato, message_id, timestamp ed errore scritti nel foglio</div></div>
    </div>
    <div class="anim-up d5" style="margin-top:24px;text-align:center;background:var(--purple-l);border:1px solid var(--purple-m);border-radius:var(--r);padding:14px 24px;font-size:15px;color:var(--text)">Tu scrivi l'intento. <strong style="color:var(--purple)">Lui esegue.</strong></div>
  </div>
</div>

<!-- ══ S29: PLUGIN — INSTALLAZIONE 3 PASSI ══ -->
<div class="slide" id="s29">
  <div class="si">
    <div class="label anim-up">🚀 INSTALLAZIONE</div>
    <h2 class="med anim-up d1" style="margin-bottom:14px">Da zero a campagna,<br><span style="color:var(--orange)">in 2 minuti.</span></h2>
    <p class="sub anim-up d2" style="margin-bottom:28px">Nessuna configurazione tecnica complessa. Tre passi.</p>
    <div class="auto-flow-steps anim-up d2" style="max-width:900px;margin:0 auto">
      <div class="auto-step"><div class="auto-step-num" style="background:var(--purple)">1</div><div class="auto-step-body"><strong>Apri il plugin in Cowork</strong><span>Doppio click su <code style="background:var(--light);padding:2px 6px;border-radius:4px;font-size:12px">ivanlao-whatsapp-broadcaster.plugin</code>. Cowork lo riconosce e conferma l'installazione.</span></div></div>
      <div class="auto-step-connector"></div>
      <div class="auto-step"><div class="auto-step-num" style="background:var(--wa-d)">2</div><div class="auto-step-body"><strong>Installa le dipendenze Python</strong><span>Una sola volta: <code style="background:var(--light);padding:2px 6px;border-radius:4px;font-size:12px">pip install --break-system-packages requests openpyxl phonenumbers</code></span></div></div>
      <div class="auto-step-connector"></div>
      <div class="auto-step"><div class="auto-step-num" style="background:var(--orange)">3</div><div class="auto-step-body"><strong>Apri la cartella DEMO</strong><span>Scrivi "configura WABA". La skill si attiva, ti guida nell'inserimento dei dati e verifica le credenziali.</span></div></div>
    </div>
    <div class="anim-up d4" style="text-align:center;margin-top:28px"><span class="badge p" style="font-size:14px;padding:10px 20px">⏱ Tempo totale: ~2 minuti</span></div>
  </div>
</div>

<!-- ══ S30: PLUGIN — DEMO LIVE CAMPAGNA ══ -->
<div class="slide" id="s30">
  <div class="si">
    <div class="label green anim-up">🎬 DEMO LIVE</div>
    <h2 class="med anim-up d1" style="margin-bottom:18px">Excel + 1 comando =<br><span style="color:var(--wa-d)">campagna massiva.</span></h2>
    <div class="auto-flow-layout">
      <div class="wa-phone-v2 anim-up d2">
        <div class="wa-statusbar">
          <span class="wa-sb-time">09:14</span>
          <span class="wa-island"></span>
          <span class="wa-sb-icons">
            <svg viewBox="0 0 16 12"><path d="M0 11h2v1H0zm3-2h2v3H3zm3-3h2v6H6zm3-3h2v9H9zm3-3h2v12h-2z"/></svg>
            <svg viewBox="0 0 16 12"><path d="M8 1.5C5.5 1.5 3.2 2.4 1.4 4l1.4 1.4C4.2 4 6 3.5 8 3.5s3.8.5 5.2 1.9L14.6 4C12.8 2.4 10.5 1.5 8 1.5zm0 4C6.3 5.5 4.8 6.1 3.7 7.2l1.4 1.4C5.9 7.8 6.9 7.5 8 7.5s2.1.3 2.9 1.1l1.4-1.4C11.2 6.1 9.7 5.5 8 5.5zm0 4c-.8 0-1.5.3-2.1.9L8 12.5l2.1-2.1c-.6-.6-1.3-.9-2.1-.9z"/></svg>
            <svg class="wa-batt" viewBox="0 0 22 11"><rect x="0.5" y="0.5" width="19" height="10" rx="2" fill="none" stroke="#fff"/><rect x="2" y="2" width="16" height="7" rx="1"/><rect x="20" y="3.5" width="1.5" height="4" rx=".5"/></svg>
          </span>
        </div>
        <div class="wa-header">
          <span class="wa-back">‹</span>
          <div class="wa-av" style="background:linear-gradient(135deg,var(--orange),var(--purple))">IL</div>
          <div class="wa-meta">
            <div class="wa-name">Ivan Lao Marketing</div>
            <div class="wa-status">online</div>
          </div>
          <div class="wa-actions">
            <svg viewBox="0 0 24 24"><path d="M17 10.5V7c0-.6-.4-1-1-1H4c-.6 0-1 .4-1 1v10c0 .6.4 1 1 1h12c.6 0 1-.4 1-1v-3.5l4 4v-11l-4 4z"/></svg>
            <svg viewBox="0 0 24 24"><path d="M20.01 15.38c-1.23 0-2.42-.2-3.53-.56-.35-.12-.74-.03-1.01.24l-1.57 1.97c-2.83-1.35-5.48-3.9-6.89-6.83l1.95-1.66c.27-.28.35-.67.24-1.02-.37-1.11-.56-2.3-.56-3.53 0-.54-.45-.99-.99-.99H4.19C3.65 3 3 3.24 3 3.99 3 13.28 10.73 21 20.01 21c.71 0 .99-.63.99-1.18v-3.45c0-.54-.45-.99-.99-.99z"/></svg>
            <svg viewBox="0 0 24 24"><path d="M12 8c1.1 0 2-.9 2-2s-.9-2-2-2-2 .9-2 2 .9 2 2 2zm0 2c-1.1 0-2 .9-2 2s.9 2 2 2 2-.9 2-2-.9-2-2-2zm0 6c-1.1 0-2 .9-2 2s.9 2 2 2 2-.9 2-2-.9-2-2-2z"/></svg>
          </div>
        </div>
        <div class="wa-chat">
          <div class="wa-msg-v2 out">Ciao Marco! 🎉<br><br>Hai vinto un posto al <strong>Workshop AI</strong> del 9 giugno.<br><br>Conferma la presenza rispondendo a questo messaggio.<span class="wa-meta-msg">09:00<span class="wa-ticks">✓✓</span></span></div>
          <div class="wa-msg-v2 in">Confermo, grazie mille!<span class="wa-meta-msg">09:04</span></div>
          <div class="wa-msg-v2 out">Perfetto Marco ✅<br>Ci vediamo il 9 giugno alle 18:00. Ti mando il link la mattina stessa.<span class="wa-meta-msg">09:04<span class="wa-ticks">✓✓</span></span></div>
        </div>
        <div class="wa-inputbar">
          <div class="wa-input-pill"><span class="wa-emoji">😊</span> Messaggio</div>
          <span class="wa-attach">📎</span>
          <span class="wa-mic">🎤</span>
        </div>
      </div>
      <div>
        <div class="auto-flow-steps anim-up d2">
          <div class="auto-step"><div class="auto-step-num" style="background:var(--purple)">1</div><div class="auto-step-body"><strong>Scegli template + verifica limiti</strong><span>La skill elenca i template approvati, controlla tier giornaliero e quality rating</span></div></div>
          <div class="auto-step-connector"></div>
          <div class="auto-step"><div class="auto-step-num" style="background:var(--wa-d)">2</div><div class="auto-step-body"><strong>Valida numeri E.164 + duplicati</strong><span>Normalizza i telefoni, segnala ambigui e duplicati prima dell'invio</span></div></div>
          <div class="auto-step-connector"></div>
          <div class="auto-step"><div class="auto-step-num" style="background:var(--orange)">3</div><div class="auto-step-body"><strong>Mapping Excel ↔ placeholder</strong><span>Collega colonne foglio ai segnaposto {{1}}, {{2}}… del template</span></div></div>
          <div class="auto-step-connector"></div>
          <div class="auto-step"><div class="auto-step-num" style="background:var(--wa-d)">4</div><div class="auto-step-body"><strong>Conferma + invio (1,5s tra messaggi)</strong><span>Solo dopo il tuo sì esplicito. Log riga per riga scritto nell'Excel</span></div></div>
        </div>
        <div class="anim-up d3" style="margin-top:18px;background:#0F172A;color:#10B981;font-family:'Courier New',monospace;padding:14px 18px;border-radius:10px;font-size:13px;line-height:1.6;border:1px solid #1E293B;box-shadow:inset 0 1px 0 rgba(255,255,255,.04)">
          <div style="color:#94A3B8;font-size:11px;margin-bottom:6px">// console wab.py — invio in corso</div>
          <div>&gt; 234/500 inviati &middot; 12 duplicati &middot; 2 errori</div>
          <div>&gt; ETA fine campagna: 09:18</div>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- ══ S31: TRANSITION, CONCLUSIONE ══ -->
<div class="slide" id="s31" style="background:radial-gradient(ellipse at center,#FFF0E5 0%,var(--bg) 70%)">
```

(Notare: l'ultima riga sostituisce solo il `<div class="slide" id="s26"...>` originale con `id="s31"` per la transition conclusione.)

- [ ] **Step 2: Verifica visiva ognuna delle 5 nuove slide**

`preview_eval`: `window.location.reload()`  
Per ogni slide N in [26, 27, 28, 29, 30]:
- `preview_eval`: `goTo(${N})`
- `preview_snapshot` per verificare il contenuto principale
- `preview_screenshot` per controllare visivamente layout, colori, spacing

Note di verifica per slide:
- **s26**: gradient verde, emoji 🎁, 3 card stat orizzontali, 2 badge
- **s27**: 3 card colorate (purple/green/orange) con icone emoji 🏢/📤/📦, card chiusura gradient
- **s28**: 6 mini-card 3x2 con emoji e testo, footer purple
- **s29**: 3 step verticali numerati con codici inline, pill tempo totale
- **s30**: telefono moderno a sinistra con conversazione Ivan/Marco, 4 step + console nera a destra

`preview_console_logs` → nessun errore (in particolare se `goTo(30)` funziona, lo script non è andato fuori bounds — ma a questo punto TOTAL è ancora 32, quindi `goTo(30)` mostrerà la slide id `s30` come 30-esima nella sequenza, ma il counter mostrerà ancora "30 / 32". Risolveremo nel Task 6.)

---

### Task 5: Rinumera gli ID delle slide successive da s27 a s37

**Files:**
- Modify: `workshop-ivan.html` (id su righe ~1398, ~1436, ~1461, ~1508, ~1580, ~1641 — gli offset cambieranno dopo Task 4)

> ATTENZIONE: dopo Task 4 i numeri di riga si sono spostati di circa +175 righe. Usa `grep` per trovare gli ID, non i numeri di riga assoluti.

- [ ] **Step 1: Trova tutti gli ID slide attuali post-s30**

```bash
grep -n 'class="slide" id="s2[6-9]"\|class="slide" id="s3[0-2]"' "/Users/ivanlao/Documents/Vibe Coding/Claude Code/siti-ivan/workshop-ivan-09-06-26/workshop-ivan.html"
```

Atteso (dopo Task 4): gli id s26..s30 sono le NUOVE slide (non rinumerare). L'id s31 è già stato corretto in Task 4 (transition conclusione). Restano da rinumerare: s27→s32, s28→s33, s29→s34, s30→s35, s31→s36, s32→s37.

> ATTENZIONE: ci sono ora DUE blocchi con `id="s27"` (la nuova slide plugin e la vecchia testimonianze), DUE con `id="s28"`, ecc. Il grep ne identificherà tutti. Devi rinumerare SOLO le occorrenze più in basso nel file (quelle vecchie, post-transition s31).

- [ ] **Step 2: Rinumera i 6 id+commenti vecchi (s27-s32 originali → s32-s37)**

Lavorare **dal più alto al più basso** per evitare collisioni id intermedie. Usa `Edit` con il commento incluso per garantire unicità (i commenti vecchi sono univoci nel file).

**A) s32 → s37 (CTA finale):**
```
old_string: <!-- ══ S27: CTA ══ -->
<div class="slide" id="s32">
new_string: <!-- ══ S37: CTA ══ -->
<div class="slide" id="s37">
```

**B) s31 → s36 (Piani):**
```
old_string: <!-- ══ S26: PIANI ══ -->
<div class="slide" id="s31">
new_string: <!-- ══ S36: PIANI ══ -->
<div class="slide" id="s36">
```

**C) s30 → s35 (Testimonianze):**
```
old_string: <!-- ══ TESTIMONIANZE ══ -->
<div class="slide" id="s30">
new_string: <!-- ══ S35: TESTIMONIANZE ══ -->
<div class="slide" id="s35">
```

**D) s29 → s34 (Servizi):**
```
old_string: <!-- ══ S25: SERVIZI ══ -->
<div class="slide" id="s29">
new_string: <!-- ══ S34: SERVIZI ══ -->
<div class="slide" id="s34">
```

**E) s28 → s33 (Errori):**
```
old_string: <!-- ══ S24: ERRORI ══ -->
<div class="slide" id="s28">
new_string: <!-- ══ S33: ERRORI ══ -->
<div class="slide" id="s33">
```

**F) s27 → s32 (Piano d'azione):**
```
old_string: <!-- ══ S23: PIANO D'AZIONE ══ -->
<div class="slide" id="s27">
new_string: <!-- ══ S32: PIANO D'AZIONE ══ -->
<div class="slide" id="s32">
```

> Nota: il commento `<!-- ══ S22: TRANSITION, CONCLUSIONE ══ -->` con `id="s26"` è già stato gestito in Task 4 (sostituito con `S31` e `id="s31"`).

- [ ] **Step 3: Verifica che tutti gli id siano sequenziali e unici**

```bash
grep -oE 'id="s[0-9]+"' "/Users/ivanlao/Documents/Vibe Coding/Claude Code/siti-ivan/workshop-ivan-09-06-26/workshop-ivan.html" | sort -t s -k 2 -n | uniq -c
```

Atteso: 37 righe, ogni `id="sN"` con `N` da 1 a 37, count = 1.

- [ ] **Step 4: Verifica preview slide finali**

`preview_eval`: `window.location.reload()`  
`preview_eval`: `goTo(37)` → deve mostrare la slide thank-you finale (era s32 prima)  
`preview_eval`: `goTo(32)` → deve mostrare la slide testimonianze (era s27)  
`preview_console_logs` → nessun errore

---

### Task 6: Aggiorna TOTAL=37, counter, array timings, cap timer

**Files:**
- Modify: `workshop-ivan.html` (riga 594 counter, righe ~1674 TOTAL, ~1701 timings, ~1704 cap)

- [ ] **Step 1: Aggiorna counter HTML**

Usa `Edit`:
```
old_string: <div id="counter">1 / 32</div>
new_string: <div id="counter">1 / 37</div>
```

- [ ] **Step 2: Aggiorna `const TOTAL` nello script**

Usa `Edit`:
```
old_string: const TOTAL = 32;
new_string: const TOTAL = 37;
```

- [ ] **Step 3: Riscrivi l'array `timings`**

Usa `Edit`:
```
old_string: const timings = [0,1,1,1,3,4,5,3,3,1,4,6,4,1,3,4,3,4,3,6,1,3,6,4,4,4,1,4,4,3,3,3,4,3];
new_string: const timings = [0,1,1,2,2,2,3,1,1,1,2,3,2,1,2,2,1,2,1,3,1,2,2,2,2,2,2,2,1,3,1,1,2,1,1,1,1];
```

Verifica somma (37 entry, primo elemento 0):
```bash
python3 -c "print(sum([0,1,1,2,2,2,3,1,1,1,2,3,2,1,2,2,1,2,1,3,1,2,2,2,2,2,2,2,1,3,1,1,2,1,1,1,1]))"
```
Atteso: `60`

- [ ] **Step 4: Aggiorna il cap del timer da 75 a 60**

Usa `Edit`:
```
old_string: const rem = Math.max(0, 75 - elapsed);
new_string: const rem = Math.max(0, 60 - elapsed);
```

- [ ] **Step 5: Verifica counter e timer**

`preview_eval`: `window.location.reload()`  
`preview_eval`: `goTo(1)` → counter deve mostrare `1 / 37`, timer `⏱ ~60 min rimasti`  
`preview_eval`: `goTo(20)` → counter `20 / 37`, timer deve essere coerente con somma timings[0..19]. Calcolo: 0+1+1+2+2+2+3+1+1+1+2+3+2+1+2+2+1+2+1+3 = 33. Rem atteso: `~27 min rimasti`.  
`preview_eval`: `goTo(37)` → counter `37 / 37`, timer deve mostrare `⏱ ~0 min rimasti` (somma 60 = elapsed → rem 0)

Verifica numerica diretta:
`preview_eval`: `document.getElementById('timer-badge').textContent`  
A slide 37 → atteso: `⏱ ~0 min rimasti`

- [ ] **Step 6: Verifica navigation completa (estremi e mezzo)**

`preview_eval`: `goTo(1); document.getElementById('counter').textContent` → `1 / 37`  
`preview_eval`: `goTo(26); document.getElementById('counter').textContent` → `26 / 37`  
`preview_eval`: `goTo(30); document.getElementById('counter').textContent` → `30 / 37`  
`preview_eval`: `goTo(37); document.getElementById('counter').textContent` → `37 / 37`  
`preview_console_logs` → zero errori

---

### Task 7: Sincronizza `index.html` con `workshop-ivan.html`

**Files:**
- Modify: `index.html` (copia integrale da `workshop-ivan.html`)

- [ ] **Step 1: Copia il file**

```bash
cp "/Users/ivanlao/Documents/Vibe Coding/Claude Code/siti-ivan/workshop-ivan-09-06-26/workshop-ivan.html" "/Users/ivanlao/Documents/Vibe Coding/Claude Code/siti-ivan/workshop-ivan-09-06-26/index.html"
```

- [ ] **Step 2: Verifica che siano identici**

```bash
diff -q "/Users/ivanlao/Documents/Vibe Coding/Claude Code/siti-ivan/workshop-ivan-09-06-26/workshop-ivan.html" "/Users/ivanlao/Documents/Vibe Coding/Claude Code/siti-ivan/workshop-ivan-09-06-26/index.html"
```

Atteso: nessun output (file identici).

- [ ] **Step 3: Verifica preview index.html**

`preview_eval`: navigate to `http://localhost:8765/index.html`  
`preview_eval`: `goTo(28); document.getElementById('counter').textContent` → `28 / 37`  
Conferma che la versione root (index.html) si comporta identica a `workshop-ivan.html`.

---

### Task 8: Aggiorna `istruzioni-siti.md`

**Files:**
- Modify: `/Users/ivanlao/Documents/Vibe Coding/Claude Code/siti-ivan/istruzioni-siti.md` (aggiunta voce log)

- [ ] **Step 1: Trova la sezione del progetto workshop-ivan-09-06-26**

```bash
grep -n "workshop-ivan-09-06-26" "/Users/ivanlao/Documents/Vibe Coding/Claude Code/siti-ivan/istruzioni-siti.md"
```

Trova il blocco descrittivo (probabilmente con titolo `## workshop-ivan-09-06-26` o simile) e identifica il "log modifiche" se esiste, oppure la fine della sezione.

- [ ] **Step 2: Aggiungi voce log**

In coda alla sezione del progetto workshop-ivan-09-06-26, aggiungi una riga al log (formato coerente con voci esistenti):

```markdown
- 23/05/2026 — Aggiunte 5 slide (s26-s30) per presentazione plugin Claude WhatsApp Broadcaster (regalo iscrizione: reveal, 3 card, 6 funzioni, installazione, demo live). Modernizzato mockup telefono WhatsApp in stile 2025 (status bar iOS, Dynamic Island, header verde #008069 con icone moderne, wallpaper con pattern doodle, bolle con tail SVG, spunte azzurre, input bar). Ricalibrato timer da 75 min hardcoded e somma timings 107 a 60 min reali distribuiti su 37 slide totali.
```

Verifica con `grep` che la voce sia stata aggiunta:

```bash
grep "23/05/2026 — Aggiunte 5 slide" "/Users/ivanlao/Documents/Vibe Coding/Claude Code/siti-ivan/istruzioni-siti.md"
```

---

### Task 9: Verifica finale e checklist visiva

**Files:** nessuna modifica, solo verifica.

- [ ] **Step 1: Esegui la checklist completa di verifica**

| # | Check | Comando | Atteso |
|---|---|---|---|
| 1 | Counter slide 1 | `preview_eval: goTo(1); document.getElementById('counter').textContent` | `1 / 37` |
| 2 | Timer slide 1 | `preview_eval: document.getElementById('timer-badge').textContent` | `⏱ ~60 min rimasti` |
| 3 | Counter slide 37 | `preview_eval: goTo(37); document.getElementById('counter').textContent` | `37 / 37` |
| 4 | Timer slide 37 | `preview_eval: document.getElementById('timer-badge').textContent` | `⏱ ~0 min rimasti` |
| 5 | Telefono moderno s24 | `preview_eval: goTo(24); !!document.querySelector('#s24 .wa-phone-v2')` | `true` |
| 6 | Telefono moderno s25 | `preview_eval: goTo(25); !!document.querySelector('#s25 .wa-phone-v2')` | `true` |
| 7 | Telefono moderno s30 | `preview_eval: goTo(30); !!document.querySelector('#s30 .wa-phone-v2')` | `true` |
| 8 | Slide s26 esiste | `preview_eval: !!document.getElementById('s26')` | `true` |
| 9 | Slide s30 esiste | `preview_eval: !!document.getElementById('s30')` | `true` |
| 10 | Slide s37 esiste | `preview_eval: !!document.getElementById('s37')` | `true` |
| 11 | Nessun id duplicato | `bash: grep -oE 'id=\"s[0-9]+\"' workshop-ivan.html \| sort \| uniq -d` | nessun output |
| 12 | Files sync | `bash: diff -q workshop-ivan.html index.html` | nessun output |
| 13 | Console clean | `preview_console_logs` | nessun errore JS/CSS |
| 14 | Responsive mobile | `preview_resize: 375x812; goTo(30); preview_screenshot` | telefono full-width senza overflow |

- [ ] **Step 2: Cattura screenshot di tutte le 5 nuove slide + s24 + s25 + s30 per review user**

Per ogni slide N in [24, 25, 26, 27, 28, 29, 30]:
- `preview_eval: goTo(${N})`
- `preview_screenshot` (salva)

- [ ] **Step 3: Riassumi all'utente lo stato finale e attendi approvazione**

Output da fornire all'utente:
1. Conferma che tutti i check sopra sono passati
2. Mostra gli screenshot delle slide modificate
3. Stato `git status --short` per mostrare i 3 file modificati (workshop-ivan.html, index.html, istruzioni-siti.md)
4. **STOP qui. NESSUN commit, NESSUN push.** Chiedi all'utente: "Tutto OK visivamente? Posso committare e pushare per il deploy?"

> Il piano si ferma qui. Commit + push + verifica deploy Netlify saranno un task separato dopo OK dell'utente.

---

## Self-Review checklist (per chi implementa)

- [ ] Tutti i 9 task hanno step concreti con codice/comandi (no placeholder)
- [ ] La somma dell'array `timings` è esattamente 60 (verifica python3)
- [ ] Gli id sN sono unici e sequenziali da 1 a 37 dopo Task 5
- [ ] Il CSS `.wa-phone-v2` è caricato correttamente e non rompe altre slide
- [ ] `workshop-ivan.html` e `index.html` sono identici dopo Task 7
- [ ] `istruzioni-siti.md` ha la nuova voce log
- [ ] Console pulita su tutte le slide testate
- [ ] NESSUN commit eseguito automaticamente — l'utente deve approvare prima
