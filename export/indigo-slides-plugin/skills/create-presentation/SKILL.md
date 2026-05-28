---
name: create-presentation
description: Crea una presentazione professionale con il tema Indigo, opzionalmente brandizzata per un cliente (gradient + accent + logo). Genera HTML stile indigo.ai e la carica su indigo-slides per condivisione e editing visivo.
user_invocable: true
---

# Create Presentation

Crei presentazioni HTML professionali nel design system **Indigo** (Roobert + Suisse Intl + Slussen Mono, palette brand) e le carichi su indigo-slides via MCP, ritornando link di visualizzazione e di edit.

Lavori per **flusso a fasi**, non a colpo solo. Mostri qualcosa di concreto prima di committarti su tutto il deck. Riproduci la **stratificazione densa** del deck Credit Agricole di riferimento (vedi `PATTERNS.md`): label + headline + body + 1-2 card grandi + footnote.

---

## File di riferimento

Leggili **prima** di generare. Sono il contratto da rispettare.

| File | Quando | Cosa contiene |
|---|---|---|
| [`BRAND.md`](BRAND.md) | Phase 1 sempre, e ogni volta che decidi colori/font/variant | Design tokens Indigo, customer_theme spec, **regole hard sulle variant** (cover dark, themed solo mid-deck), accent indigo logic, logo + image guidelines |
| [`PATTERNS.md`](PATTERNS.md) | Phase 2/3 sempre | I pattern di slide con HTML template denso e stratificato, when-to-use, anti-pattern. Sequenza tipica di un deck enterprise 12-17 slide. |
| [`DENSITY_RULES.md`](DENSITY_RULES.md) | Phase 3 ogni slide | Stratificazione (label→claim→body→evidence→footnote), cap per pattern, esempio "piatta vs ben stratificata" |
| [`html-template.md`](html-template.md) | Phase 3 (debug HTML) | Contratto `<div class="slide-content">`, classi disponibili (incluso le nuove: hero-bg, numbered-steps, highlight-card-dark, pill-highlight, case-study-card, service-card, pricing-card), gotchas |
| [`animation-patterns.md`](animation-patterns.md) | Phase 3 (reveal stagger) | Pattern `.reveal` con IntersectionObserver, quando aggiungere animazioni inline custom, prefers-reduced-motion |

Il catalog macchina-leggibile resta `get_theme_catalog(theme: "indigo")` — usalo come fallback se hai dubbi.

---

## Phase 0 — Detect mode

> **NOTA**: Phase 0 sarà espansa nella issue WP3 (AGE-89). Per ora supporto solo **Mode A — Nuova presentazione**.

Determina cosa vuole l'utente:

- **Mode A — Nuova presentazione**: l'utente vuole creare un deck da zero. **Default. Vai a Phase 1.**
- **Mode B — Enhance esistente** (post-WP3): l'utente cita un ID o titolo di pres esistente.
- **Mode C — Convert PPT** (post-WP4): l'utente allega un `.pptx`.

Per ora se l'utente menziona un ID o un PPT, scusati e segui Mode A creando una nuova pres.

---

## Phase 1 — Content discovery

Raccogli tutto in **un solo `AskUserQuestion`** (multi-question bundle).

Domande da bundlare:

1. **Argomento** (header `Topic`): cosa è la pres + obiettivo (es. "proposta voice AI per Revolut", "recap Q1 2026 per il team product").
2. **Tipo di deck** (header `Type`): scelte preimpostate — questa determina la **distribuzione variant** (vedi `BRAND.md`):
   - *Proposta commerciale customer* → 60-65% dark, 25-30% light, 5-10% themed (max 2 slide)
   - *Pitch tecnico interno* → 70% dark, 30% light, 0% themed
   - *Recap interno / team update* → 60-70% light, 30-40% dark, 0% themed
   - *Quick demo* (5-6 slide) → 80% dark, 20% light, 0% themed
3. **Cliente specifico?** (header `Brand`): *Sì, deck per cliente* (apre branch customer_theme + abilita section-divider themed) / *No, brand Indigo* (default, niente themed).
4. **Lunghezza** (header `Slides`): *Short 5–8* / *Medium 10–15* (recommended per proposte) / *Long 15+*.

Se l'utente menziona già queste info nel prompt iniziale ("proposta per Revolut, 12 slide"), **non chiedere di nuovo** ciò che è già esplicito.

### Branch customer_theme

Solo se l'utente ha scelto "Sì, deck per cliente" o cita un cliente esplicitamente:

1. Chiedi in **una sola domanda concisa**:
   > "Per il brand: passami il colore primario (hex), la tinta chiara per gli sfondi (hex), e se hai un logo (o foto hero per la cover) allegalo qui."

2. Se l'utente allega un logo o una foto hero, chiama subito `upload_asset` (richiede `presentation_id` — se la pres non esiste ancora, crea prima la pres con un titolo placeholder, poi upload, poi update slides).

3. Costruisci `customer_theme`:
   ```json
   {
     "clientName": "Crédit Agricole",
     "primary": "#00753E",
     "primaryLight": "#D4ECDE",
     "gradient": "linear-gradient(135deg, #00753E 0%, #004F2A 100%)",
     "logoUrl": "<URL dell'asset>"
   }
   ```
   `gradient` opzionale (vedi `BRAND.md`).

**Cruciale:** il `customer_theme` abilita le slide `themed` ma **non cambia l'accent operativo**. L'accent indigo `#6366F1` resta sui section-label, sui numerini step, sulle metric value. Il customer brand vive **solo come gradient di sfondo** sulle 1-2 section-divider mid-deck (vedi `BRAND.md`).

### Lingua

Scrivi le slide nella lingua dell'utente. Se parla italiano, le slide sono in italiano. Eccezione: contenuto tecnico per audience internazionale resta in inglese.

---

## Phase 2 — Outline strategico

Prima di generare HTML, butta giù un outline testuale veloce: **pattern + variant + 1-liner di contenuto per ogni slide**.

Esempio canonico per proposta commerciale 14 slide (dal reference Credit Agricole):

```
1.  cover-dark-hero          → "L'assistente AI nella vostra app" (foto hero)
2.  intro-context-stories    → dark / "La piattaforma per creare AI Agents" + case studies HYPE+Santander
3.  compliance-shields       → light / "Progettata per il settore finanziario"
4.  highlight-insight        → light / "Il banking sta diventando conversazionale" + card dark con opportunità
5.  text-and-mockup          → light / "Voce + widget: un'interazione naturale"
6.  process-flow             → light / "Da notifica push a lead qualificato"
7.  section-divider-themed   → themed (#1 themed) / "L'assistente AI di X"
8.  comparison-before-after  → dark / "Da processo manuale a 3 minuti"
9.  pricing-detail (pilota)  → dark / "6 mesi sull'assistente conversazionale"
10. numbered-vertical-steps  → dark / "Setup: da zero a live in 6–8 settimane"
11. pricing-detail (prodotto)→ dark / "La licenza Super copre tutto"
12. service-team             → dark / "I servizi: il team che fa funzionare l'AI"
13. next-steps-detail        → dark / "Come procedere"
14. closing-big              → dark / "Costruiamolo insieme."
```

Per deck più corti (6-8 slide), comprimi mantenendo gli elementi essenziali: cover-dark-hero + 1-2 evidence + 1 process + 1 pricing + closing-big.

### Regole hard di ordering

- **Slide 1**: sempre `cover-dark-hero` o `cover-dark`. **MAI themed.**
- **Slide 2**: tipicamente `intro-context-stories` o `compliance-shields` per stabilire credibilità.
- **Themed slide**: **MAI prima della slide 4**. **MAI come ultima o penultima.** **MAI 2 consecutive** (almeno 2 slide light/dark in mezzo). **Max 2 themed in tutto il deck**.
- **Slide N (ultima)**: sempre `closing-big` dark, headline grande + contatti. **MAI themed, MAI con checklist verbosa**.
- **Slide N-1 (penultima)** opzionale: `next-steps-detail` per deck enterprise.

### Decisione accent

Decidi qui anche **quale slide ha l'accent metric indigo** — una sola per deck, di solito su `results-dashboard` o `pricing-detail` per evidenziare il numero chiave.

---

## Phase 2.5 — Preview & approval

> **NOTA**: Phase 2.5 sarà espansa nella issue WP2 (AGE-88). Per ora il flusso è preview-light, da arricchire dopo.

Invece di generare tutto il deck in un colpo:

1. Genera **solo cover + 1 slide hero** (es. `cover-dark-hero` + `intro-context-stories` o + `highlight-insight`). Sono le due che definiscono il "look" del deck.
2. Chiama `create_presentation` con queste 2 slide.
3. Comunica all'utente il link di visualizzazione: *"Ecco le prime 2 slide qui: {url}. Procedo con il resto del deck?"*
4. Se l'utente chiede modifiche: iterazione tramite `update_slide` (mai rifare tutta la pres).
5. Quando l'utente approva, estendi il deck con `add_slide` per le slide successive (in ordine, inserendole nelle posizioni giuste — ricorda che le slide preview occupano posizione 0 e 1, e le aggiunte poi shiftano).

**Mai** generare tutte le slide e committare in unico tool call senza preview.

---

## Phase 3 — Generate full deck

Per ogni slide rimanente nell'outline:

1. **Apri `PATTERNS.md`**, trova il pattern, copia il template HTML.
2. **Adatta i contenuti** alla variant + lingua + brand cliente se attivo.
3. **Controlla `DENSITY_RULES.md`**: la slide è **stratificata** (label+headline+body+evidence+footnote)? I cap del pattern sono rispettati? Se no, splitta o trim.
4. **Aggiungi `class="reveal"`** agli elementi top-level che vuoi animare con stagger (vedi `animation-patterns.md`). Massimo 5 reveal per slide.
5. **Controlla `html-template.md`**: niente `<section>` né `<div class="slide">` nel tuo `html_content`, niente classi inventate, niente colori hex hardcodati.
6. **Chiama `add_slide`** con `presentation_id` esistente, posizione `n`, variant scelta, html_content.

**Errori comuni da evitare:**
- Headline `headline-huge` su slide content (riservato a cover + section-divider + closing) → usa `headline-big`.
- Section-label senza `class="section-label"` o senza UPPERCASE → la skill genererà casing sbagliato.
- Accent customer al posto di indigo sui numerini step / metric → sempre indigo `#6366F1`.
- 3 card piccole in `grid-3` invece di 2 card grandi → preferisci 2 card grandi (`case-study-grid`, `two-col + highlight-card-dark`).
- Logo cliente nel header → mai, il header è chrome Indigo.
- Slide content senza `class="reveal"` su nessun elemento → animation non triggered.
- Più di 2 slide `themed` → riduci.
- Themed in posizione 1, 2 o ultima → cambia variant.

---

## Phase 4 — PPT conversion

> **NOTA**: Phase 4 è coperta dalla issue WP4 (AGE-90). Quando arriverà, qui ci sarà il flusso `extract-pptx.py` → mapping a pattern → upload assets → create_presentation.

Per ora, se l'utente allega un `.pptx`, scusati e proponi di ricreare manualmente il deck a partire da brief testuale (Phase 1).

---

## Phase 5 — Delivery

Quando il deck è completo:

1. Verifica con `get_presentation(id)` che tutte le slide siano state caricate.
2. Comunica all'utente:
   - **Link visualizzazione**: `{url}` — scroll-snap navigation con frecce/spazio/touch.
   - **Link editor**: `{editUrl}` — per modifiche visuali.
3. Sintesi breve: "Deck di N slide, M con customer brand themed (slide X e Y), pattern usati: [...]"

Se l'utente vuole iterare ancora ("cambia la slide 5"), usa `update_slide` o `add_slide` / `remove_slide` / `move_slide` su quella specifica posizione — mai ricreare l'intera pres.

---

## Tool MCP disponibili

- `create_presentation(title, theme, customer_theme?, slides[])` — crea pres + slide iniziali (Phase 2.5 / 3)
- `add_slide(presentation_id, position, html_content, variant, notes?)` — aggiunge una slide (Phase 3)
- `update_slide(presentation_id, slide_index, html_content?, variant?, notes?)` — modifica una slide (iterazioni)
- `remove_slide(presentation_id, slide_index)` — rimuovi una slide
- `move_slide(presentation_id, from_position, to_position)` — riordina
- `upload_asset(presentation_id, filename, content_base64, kind)` — logo / hero image / mockup / screenshot (immagini fornite dall'utente)
- `generate_image(presentation_id, prompt, technique?, kind?)` — genera un'immagine on-brand con flora.ai quando l'utente NON ne allega una. Due `technique`: `lifestyle-photos` (foto persone/lifestyle → **full-bleed o con fade**, mai inline) e `product-chat-generator` (mockup UI di chat → **inline** nella slide, mai background). Ritorna **subito** `{id, url, status:"pending", embed}`: incolla l'`embed` (già col placement giusto per tipo) nel `html_content`; il deck mostra uno skeleton e l'immagine compare da sola (~60-90s), senza attendere né polling. Cap per-presentazione. **Mai** per logo / grafici / dashboard. Vedi `BRAND.md` § Immagini generate.
- `get_presentation(id)` — recupera stato corrente del deck
- `list_presentations(search?)` — cerca pres esistenti
- `get_theme_catalog(theme)` — catalog macchina-leggibile (fallback)
- `delete_presentation(id)` — elimina

---

## Checklist riassuntiva pre-tool-call

Prima di chiamare `create_presentation` o `add_slide`:

- [ ] **Pattern scelto** dal `PATTERNS.md` (no invenzione)
- [ ] **Variant coerente** col pattern e l'ordering (cover = dark, themed solo mid-deck, closing = dark)
- [ ] **Stratificazione**: la slide ha label → headline → body → evidence → (footnote) o equivalente?
- [ ] **Density cap rispettati** (vedi `DENSITY_RULES.md`)
- [ ] **Solo classi** presenti in `theme.css` (vedi `html-template.md`)
- [ ] **Niente `<section>` / `<div class="slide">` / header tag** nel `html_content`
- [ ] **Colori brand via CSS vars**, non hex hardcoded
- [ ] **Accent indigo `#6366F1`** su section-label, numerini step, metric chiave — anche su pres customer
- [ ] **Un solo elemento metric con accent** indigo per slide
- [ ] **`class="reveal"` su 3-5 elementi top-level** (per stagger animation)
- [ ] **Icone: SOLO Lucide SVG inline (`stroke-width="1.5"`), MAI emoji** su slide customer-facing — incluse flow-icon, shield-icon, card-icon. Regola rigida, non saltarla.
- [ ] **Mockup chat generato** → dentro `<div class="two-col mockup-right">`, titolo nella colonna sinistra (vedi `BRAND.md` § Immagini generate)
- [ ] **Lingua dell'utente**

Se anche uno solo fallisce: sistema prima del tool call.
