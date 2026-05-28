# HTML TEMPLATE — Contratto tecnico per `html_content`

Come si scrive l'HTML di una slide, cosa è generato automaticamente dal renderer, quali classi usare, gotchas.

---

## Il contratto `slide-content`

Il `html_content` che passi a `create_presentation` / `update_slide` / `add_slide` viene wrappato automaticamente:

```html
<!-- Il renderer genera questo. NON includerlo nel html_content. -->
<section class="slide {variant}">
  <div class="slide-header">
    <img class="header-logo" src="...">
    <div class="header-tagline">indigo.ai{ × ClientName se customer_theme}</div>
    <div class="header-number">{n} / {total}</div>
  </div>
  <div class="slide-content">
    {IL TUO html_content VA QUI}
  </div>
</section>
```

**Cosa NON includere mai** nel `html_content`:
- `<section>`, `<div class="slide">`, `<div class="slide-header">`
- `<img class="header-logo">` (il renderer sceglie light/dark automaticamente)
- Tag `header-tagline` o `header-number`

**Cosa includere:** solo il contenuto interno della slide. La maggior parte dei pattern parte direttamente con:

```html
<p class="section-label reveal">UPPERCASE LABEL</p>
<h2 class="headline-big reveal">Headline su due<br>righe</h2>
<p class="body-large reveal">Body grosso sotto headline...</p>
<!-- evidence stratificata -->
```

---

## Engine v1.2 — scroll-snap + IntersectionObserver

L'engine adotta il pattern di `frontend-slides`:

- `html { scroll-snap-type: y mandatory }` + `.slide { scroll-snap-align: start; height: 100vh; height: 100dvh; }`
- IntersectionObserver: quando una slide è ≥50% in viewport, riceve classe `.visible`
- CSS `.reveal` su elementi → si animano in stagger quando il parent `.slide.visible` è attivo

**Cosa fai nel `html_content`:** aggiungi `class="reveal"` agli elementi top-level che vuoi animare (max 5 per slide). Il default è opacity 0 + translateY 20px → opacity 1 + translateY 0 con stagger 0.12s tra elementi.

```html
<p class="section-label reveal">DOVE SIAMO</p>
<h2 class="headline-big reveal">Headline animata seconda</h2>
<p class="body-large reveal">Body animato terzo</p>
<div class="grid-3 reveal">...</div>  <!-- la grid intera è il 4° reveal -->
```

Vedi `animation-patterns.md` per pattern avanzati.

---

## Eccezione: layout speciali

Alcuni pattern (`cover-dark-hero`, `cover-dark`, `cover-themed`, `section-divider-themed`, `closing-big`) usano un wrapper interno dentro `slide-content`:

```html
<!-- cover-dark-hero / cover-dark — wrapper cover-content -->
<div class="cover-content cover-left">
  <p class="cover-eyebrow reveal">TAGLINE UPPERCASE</p>
  <h1 class="cover-title reveal">Titolo<br>Cover</h1>
  <p class="cover-subtitle reveal">Sottotitolo discorsivo.</p>
  <p class="cover-date reveal">25 maggio 2026 · Proposta commerciale</p>
</div>

<!-- closing-big — centered + asciutta -->
<div class="slide-content centered closing-content">
  <h1 class="headline-huge reveal">Costruiamolo insieme.</h1>
  <p class="body-large muted mt-md reveal">Un pilota rapido...</p>
  <div class="contact-row mt-xl reveal">...</div>
</div>

<!-- section-divider-themed — centered nudo -->
<div class="slide-content centered">
  <p class="section-label highlight-eyebrow reveal-hero">LO SCENARIO</p>
  <h1 class="headline-huge reveal-hero">L'assistente AI<br>di Crédit Agricole</h1>
</div>
```

---

## Hero image cover (background)

Per `cover-dark-hero`, la `<section>` (generata dal renderer) deve avere `class="slide dark hero-bg"` + `style="background-image: url(...)"`. Il renderer accetta la classe extra `hero-bg` se passata via `variant` extra param — vedi `BRAND.md` Hero Image section.

**Per ora (workaround v1.2 senza variant change):** la skill genera la cover senza background-image automatico; aggiunge un wrapping `<div class="hero-bg" style="background-image: url('/api/assets/...')">` come prima riga del `html_content`, e il CSS `.hero-bg` posiziona absolute coprendo l'intera slide-content:

```html
<div class="hero-bg" style="background-image: url('/api/assets/abc123');"></div>
<div class="cover-content cover-left">
  <p class="cover-eyebrow reveal">CRÉDIT AGRICOLE × INDIGO.AI</p>
  <h1 class="cover-title reveal">L'assistente AI<br>nella vostra app</h1>
  <p class="cover-subtitle reveal">Un'esperienza conversazionale...</p>
  <p class="cover-date reveal">25 maggio 2026 · Proposta commerciale</p>
</div>
```

Il `.hero-bg` applica overlay gradient nero→trasparente per leggibilità.

---

## Variant → logo automatico

Il renderer sceglie il logo a runtime in base alla variant. Non passare URL del logo nel `html_content`.

| Variant | Logo iniettato |
|---|---|
| `light` | `themes/indigo/logo-dark.svg` |
| `dark` | `themes/indigo/logo-light.svg` |
| `themed` | `themes/indigo/logo-light.svg` |

---

## Customer theme injection

Se `customer_theme` è settato, il renderer inietta:

```html
<style data-customer-theme>
  :root {
    --c-client-primary: #00753E;
    --c-client-primary-light: #D4ECDE;
    --c-client-gradient: linear-gradient(135deg, #00753E 0%, #004F2A 100%);
  }
</style>
```

**Cosa cambia automaticamente:**
- `.slide.themed { background: var(--c-client-gradient) }` → gradient cliente
- `.progress-bar { background: var(--c-client-primary) }` → progress bar cliente (sottile, ok)
- **Niente altro.** L'accent operativo (section-label, numerini step, metric value) resta indigo.

---

## Classi disponibili

Tutte le classi sotto sono definite in `themes/indigo/theme.css`. **Mai inventarne di nuove** dentro `html_content`.

### Typography
`.headline-huge` `.headline-big` `.headline-medium` `.headline-small`
`.body-large` `.body-text` `.body-small`
`.section-label` (sempre UPPERCASE + mono + indigo)
`.cover-eyebrow` (variante section-label per cover)
`.cover-title` `.cover-subtitle` `.cover-date`
`.ghost-text`

### Layout primari
`.two-col` (+ `.narrow-right` / `.wide-left` / `.mockup-right`)
`.two-col.mockup-right` — testo a sx (titolo incluso), mockup generato a dx in colonna fissa dimensionata; non impostare height/align a mano
`.grid-2` `.grid-3` `.grid-4`
`.flow-container` `.flow-step` `.flow-arrow` `.flow-icon`
`.shield-grid` (+ `.shield-grid-3x2` per forzare 3 colonne)
`.shield-item` `.shield-icon`
`.analytics-grid` `.metric-card` `.metric-label` `.metric-value` (+ `.accent`) `.metric-delta` (+ `.up` / `.down` / `.neutral`)
`.cover-content` (+ `.cover-left`)
`.closing-content`

### Case study (NEW v1.2)
`.case-study-grid` (2 col) `.case-study-card`
`.case-study-logo` `.case-study-desc`
`.case-study-metrics` `.case-study-metric` `.metric-num` `.metric-cap`
`.case-study-mini` (versione piccola per slide highlight-insight)
`.client-list` `.client-list-names`

### Highlight-card-dark (NEW v1.2)
`.highlight-card-dark` (card scura grande)
`.highlight-card-sm` (versione più piccola)
`.highlight-eyebrow` (variante section-label per dentro card dark)
`.highlight-card-list` (bullet list)
`.closing-cards` (container per 2 card affiancate verticali)
`.reassurance-card` (light card con checklist)
`.check-list` (lista con checkmark verdi)

### Numbered steps (NEW v1.2)
`.numbered-steps` (ol) `.numbered-steps-sm` (variante piccola)
`.step-num` (cerchio indigo numerato)
`.step-body` (content next-to)

### Mockup
`.chat-mockup` `.chat-header` `.chat-avatar` `.chat-body` `.chat-bubble` (+ `.user` / `.assistant`)
`.mockup-pair` (flex row 2 phone)
`.phone-mockup` (img phone screenshot)
`.screenshot-frame` (img desktop screenshot con frame)

### Pricing (NEW v1.2 — sostituisce offer-table)
`.pricing-card` `.addon-card`
`.pricing-row` `.pricing-name` `.pricing-value` `.pricing-unit`
`.pricing-includes` (ul)
`.addon-title` `.addon-table`
`.pill-highlight` (+ modifier colore — **vedi regola sotto**)

**Regola pill-highlight (IMPORTANTE):**
| Modifier | Quando usarlo |
|---|---|
| `.pill-neutral` | **Default per disclaimer/note informative** ("Oltre 150K sessioni, tariffa scalare", "Validità offerta 30gg"). Grigio sobrio, non compete con l'accent. |
| `.pill-indigo` | Highlight di un **dato chiave** ("Totale anno €132.000"). |
| `.pill-green` | **SOLO risparmi/sconti reali** ("Sconto −22%", "−71% sul costo"). Mai per disclaimer. |
| `.pill-blue` / `.pill-amber` | Casi rari, stati custom. |

Mai mettere due pill colorate diverse (verde + viola) nella stessa slide: crea incoerenza. Disclaimer → sempre `pill-neutral`.
`.cost-card` (variante minore per costo onboarding)
`.cost-row` `.cost-amount` `.cost-note`

### Service team (NEW v1.2)
`.service-card-light` (card sx ipotesi)
`.service-row` `.service-name` `.service-value` `.service-unit` `.service-meta` `.service-detail`
`.service-list` (ul interna)
`.service-block` (blocco h4 + p dentro highlight-card-dark)

### Feature list
`.feature-list` (ul con `<b>` titolo + muted subtitle sotto)

### Tabelle
`.slide-table` (+ `.compare-table` per before/after)
`.highlight-row`
`.col-before` `.col-after`
`.num` (cella numerica destra) `.num.accent-green` (cella numerica verde)

### Tag
`.tag` (+ `.green` / `.red` / `.blue` / `.orange` / `.purple`)

### Stat
`.stat-big` `.stat-number` `.stat-label`

### Process step / metric row
`.process-step` `.process-dot`
`.metric-row` `.metric-check` `.metric-text`

### Card generica
`.card` `.card-icon` `.card-title` `.card-desc`

### Demo meta (NEW v1.2)
`.demo-meta` `.demo-label` `.demo-value`

### Contact row (closing)
`.contact-row` `.contact-label` `.contact-value` `.contact-divider`

### Opacità / weight
`.muted` (0.5) `.faded` (0.35) `.bold` (700)

### Spacing utilities
`.mt-xs` `.mt-sm` `.mt-md` `.mt-lg` `.mt-xl`
`.mb-sm` `.mb-md` `.mb-lg` `.mb-xl`
`.gap-sm` `.gap-md` `.gap-lg` `.gap-xl`

### Flex utilities
`.flex` `.flex-col` `.items-center` `.items-stretch` `.justify-between` `.flex-1`

### Width max
`.max-w-sm` (480px) `.max-w-md` (640px) `.max-w-lg` (800px)

### Alignment di slide-content
`.centered` (centra V+H + text-align center)
`.bottom` (allinea contenuto al fondo)
`.cover-left` (slide-content/cover-content allineato a sinistra)

### Background special
`.hero-bg` (cover background image con overlay gradient nero→trasparente)

### Reveal (animation)
`.reveal` (default fade+rise)
`.reveal-hero` (variante più drammatica per section-divider)

---

## Inline styles — quando OK, quando NO

### Inline OK
- `style="flex: 1"` per cards in flex
- `style="max-height: 65vh"` per immagini
- `style="margin: auto"` per centrare un blocco specifico
- `style="align-items: center"` o `style="align-items: stretch"` su `two-col`
- `style="text-align: right"` su cell di tabella
- `style="background-image: url(...)"` su `.hero-bg`

### Inline NO
- `style="color: #6366F1"` → usa `var(--c-client-primary)` o classi
- `style="font-family: ..."` → usa le classi `.headline-*` / `.body-*`
- `style="font-size: 18px"` → usa classi (sizing clamp-based)
- `style="padding: 17px"` → usa scala `--s-*` o utility
- `style="background: linear-gradient(...)"` su slide intera → usa variant `themed`

**Regola pratica:** layout (flex, position, max-width) → inline OK. Brand (colore, font, scale) → mai inline.

---

## Gotchas

### 1. Headers automatici — non duplicare

Il renderer inietta automaticamente `slide-header` con logo Indigo. **Non aggiungere mai un secondo logo Indigo** dentro `html_content`. Se vuoi un logo cliente in una case-study-card, usa `<img class="case-study-logo">`, non `<img class="header-logo">`.

### 2. Numero slide automatico

Il `header-number` mostra `{n} / {total}` calcolato a runtime. Non hardcodare numeri di slide nel `html_content`.

### 3. Tagline contestuale automatica

La tagline top-right `indigo.ai × <ClientName>` o `<TaglineSezione>` è gestita dal renderer. Per la tagline contestuale tipo "Proposta commerciale" / "Prossimi passi" / "Sicurezza e governance", per ora si imposta solo a livello pres (non per-slide). Future enhancement: per-slide tagline override.

### 4. Negazione di CSS functions

**SBAGLIATO** (silenziosamente ignorato):
```html
<div style="right: -clamp(28px, 3.5vw, 44px);">
```

**CORRETTO**:
```html
<div style="right: calc(-1 * clamp(28px, 3.5vw, 44px));">
```

### 5. SVG inline per Lucide icons

Usa inline SVG con `stroke="currentColor"`:

```html
<div class="card-icon">
  <svg xmlns="http://www.w3.org/2000/svg" width="28" height="28" viewBox="0 0 24 24"
       fill="none" stroke="currentColor" stroke-width="1.5"
       stroke-linecap="round" stroke-linejoin="round">
    <path d="..."/>
  </svg>
</div>
```

`currentColor` permette al colore di adattarsi alla variant. Mai hardcodare `stroke="#000"`.

### 6. Immagini esterne

Usa solo URL ritornati da `upload_asset` (es. `https://indigo-slides.fly.dev/api/assets/abc123`). Niente URL esterni.

### 7. Caratteri speciali in JSON

Quando passi `html_content` via tool MCP:
- Escape `"` come `\"`
- Escape newlines come `\n` (oppure usa template letterali HTML su una riga sola)
- `<br>` interpretato come HTML

### 8. Niente `<script>` né link CSS esterni

Il renderer aggiunge `engine.js` e `theme.css` automaticamente. Non includere `<script>` o `<link rel="stylesheet">` nel `html_content`. **Eccezione consentita:** `<style>` inline per animazioni custom puntuali (vedi `animation-patterns.md`).

### 9. Reveal su 5+ elementi

Lo stagger CSS ha 5 step nth-child. Se metti `reveal` su 8 elementi, dal 6° in poi appaiono insieme (delay massimo). Limita a 5 elementi top-level animati, o spezza in più container.

### 10. Section-label MAIUSCOLO

`.section-label` ha `text-transform: uppercase` nel CSS, ma il content che scrivi influenza lo stile. Scrivi sempre in maiuscolo per chiarezza (`SCELTI DAL SETTORE FINANZIARIO`, non `Scelti dal settore finanziario`).

---

## Validazione veloce pre-tool-call

Prima di chiamare `create_presentation` / `add_slide`, per ogni slide:

- [ ] `html_content` non inizia con `<section>` né `<div class="slide">`
- [ ] Solo classi presenti in questo file (controlla con Ctrl-F)
- [ ] Niente colori brand hardcodati — solo CSS vars o classi
- [ ] Niente `<script>` / `<link>` (`<style>` inline OK solo per animazioni custom)
- [ ] `class="reveal"` su 3-5 elementi top-level
- [ ] Section-label scritto MAIUSCOLO
- [ ] Caratteri speciali escaped correttamente
