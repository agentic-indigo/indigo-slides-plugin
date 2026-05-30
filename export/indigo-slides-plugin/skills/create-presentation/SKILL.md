---
name: create-presentation
description: Crea una presentazione professionale con il tema Indigo, opzionalmente brandizzata per un cliente (gradient + accent + logo). Genera HTML stile indigo.ai e la carica su indigo-slides per condivisione e editing visivo.
user_invocable: true
---

# Create Presentation

Crei presentazioni HTML professionali nel design system **Indigo** (Roobert + Suisse Intl + Slussen Mono, palette brand) e le carichi su indigo-slides via MCP, ritornando link di visualizzazione e di edit.

Lavori per **flusso a fasi**, non a colpo solo. Concordi la **scaletta** con l'utente prima di generare tutto il deck. Riproduci la **stratificazione densa** del deck Credit Agricole di riferimento (vedi `PATTERNS.md`): label + headline + body + 1-2 card grandi + footnote.

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

## Phase 1 — Capisci, poi decidi

Obiettivo: partire a costruire il prima possibile. **Inferisci dal brief, non interrogare.** Meno cerimonie.

### Cosa ti serve (inferisci se è già chiaro)
- **Tipo di deck** — decide tutto il resto:
  - **Commerciale** (proposta/pitch a cliente o prospect): narrativo, persuasivo, usa immagini, può avere 1-2 slide `themed` col brand cliente. Distribuzione variant: ~60% dark, ~30% light, ≤2 themed.
  - **Operativo** (interno: recap, analisi, dati per Solutions): denso di dati — tabelle, metriche. **Niente foto, niente themed.** ~60-70% light.
- **Cliente + brand** (solo se commerciale per un cliente): nome + colori. Il brand di un'azienda nota spesso lo conosci (es. Reale Mutua = rosso): usalo e **dichiaralo** invece di chiederlo.
- **Audience, lunghezza, contenuti chiave**: di solito impliciti nel brief.

### Quando chiedere (adattivo, non a numero fisso)
- **Brief completo** → non chiedere niente. Dichiara in 1 riga cosa hai capito ("Deck commerciale per X, ~12 slide, brandizzato Y, audience CTO") e **costruisci**.
- **Brief incompleto in punti che cambiano il deck** → fai **solo** le domande che colmano quei buchi, **batchate in un solo `AskUserQuestion`** (più domande nello stesso blocco, scelta multipla dove possibile). Buco vero = non capisci se commerciale/operativo; deck cliente ma brand non inferibile; il brief vuole case study/clienti reali ma non dice quali (non inventarli); taglio/lunghezza aperti che cambiano la struttura.
- **Mai** chiedere ciò che è inferibile o già nel brief. **Mai un form di domande di rito.** Per utenti non tecnici: poche, chiare, niente hex se il brand è inferibile.

### Raccolta materiale (limitata)
Due tipi di fatti, regole diverse:
- **Numeri/metriche** da mettere in slide: usa ciò che sai e il deck di riferimento, verifica **solo** quelli e in fretta. Non trasformare la creazione in una sessione di ricerca.
- **Clienti reali da citare come case study**: **non inventarli mai.** Se sono nel brief o li conosci con certezza (es. HYPE/Santander nel banking), usali. Se dovresti indovinare chi citare (settore che non conosci), **chiedi**: "quali clienti vuoi citare come riferimento?" — una domanda secca, non una ricerca.

**Mai interrogare la codebase per i fatti clienti/business.** Un tool tipo `ask_codebase` indicizza il **codice** (architettura, feature, integrazioni), non la mappa clienti-per-settore: non saprà dirti "che clienti energy abbiamo", e ha un timeout ~60s che ti blocca a vuoto. La lista clienti vive nel brief o dall'utente — chiedi a lui.

### Brand cliente → `customer_theme`
Solo se commerciale per un cliente. Costruisci:
```json
{ "clientName": "Reale Mutua", "primary": "#C8102E", "primaryLight": "#FBE2E5",
  "gradient": "linear-gradient(135deg, #C8102E 0%, #8F0B20 100%)", "logoUrl": "<da upload_asset, opz>" }
```
Se l'utente allega un logo/foto, `upload_asset` (serve `presentation_id` → crea prima con titolo placeholder se necessario). **Il `customer_theme` abilita le slide `themed` ma NON cambia l'accent**: indigo `#6366F1` resta su section-label, numerini, metric; il brand cliente vive solo come gradient di sfondo sulle 1-2 themed mid-deck.

### Lingua
Slide nella lingua dell'utente (IT → slide in IT). Eccezione: contenuto tecnico per audience internazionale resta in inglese.

---

## Phase 2 — Scaletta, poi approvazione

Prima di generare HTML, definisci la scaletta e **falla approvare dall'utente**. Due passi:

1. **Outline interno (per te):** pattern + variant + 1-liner di contenuto per ogni slide. È il tuo piano di build, non lo mostri così com'è.
2. **Scaletta per l'utente (gate):** la presenti in chiaro e **aspetti l'ok prima di generare** (vedi "Scaletta da approvare" sotto).

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

### Decisione immagini (auto)
Decidi qui se e dove generare immagini, in base al tipo:
- **Commerciale**: **1 cover hero** (`lifestyle-photos`, full-bleed) quasi sempre. Oltre la cover, una foto solo se una slide è divisore/contesto e il deck non è già denso. Un **mockup chat** (`product-chat-generator`, inline) sulla slide demo "assistente in azione", se prevista. **Mai** foto su slide con dati/tabelle/metriche. Mai troppe (≈1-3 in tutto il deck).
- **Operativo**: **zero immagini**, salvo richiesta esplicita.
- **Sempre** quando l'utente chiede esplicitamente un'immagine, a prescindere dal tipo.

Vedi `BRAND.md` § Immagini generate per il posizionamento per tipo.

### Scaletta da approvare (gate prima di generare)
Per un **deck nuovo** (da ~3-4 slide in su), prima di chiamare `create_presentation` mostra la scaletta all'utente e **aspetta l'ok**.

- **Formato leggibile**, non il tuo outline tecnico: una riga per slide, `N. Titolo: cosa contiene in una frase`. **Niente gergo** (no nomi-pattern, no variant, no nomi-classe): l'utente è spesso non-tecnico.
- Chiudi con una domanda secca: "Ti torna questa scaletta o cambio qualcosa?"
- L'utente **edita a parole** ("togli la slide pricing", "aggiungi un caso cliente energy", "inverti 4 e 5"): aggiorni la scaletta e **riconfermi**, senza generare.
- Solo sull'ok espandi in HTML (Phase 3). **Non generare il deck prima dell'approvazione.**

**Salta il gate** (genera diretto) quando l'utente chiede **una sola slide**, o un **edit di un deck già esistente** (lì usi `update_slide`/`add_slide`, non rifai la scaletta).

Esempio di scaletta mostrata all'utente:
```
1. Cover: "L'assistente AI nell'area clienti di Sorgenia"
2. Il contesto: perché le richieste sulle bollette intasano il call center
3. La soluzione: un assistente in-app che legge bolletta e consumi
4. Come funziona: esempio "perché la bolletta di marzo è più alta?"
5. Azioni in chat: autolettura e domiciliazione senza moduli
6. Risultati attesi: meno carico sul call center, risposte in tempo reale
7. Chiusura: prossimi passi
```

---

## Phase 3 — Costruisci in un colpo

**Solo dopo l'ok sulla scaletta (Phase 2).** Niente checkpoint a metà: costruisci il deck **intero**, poi la self-review (Phase 4), poi mostri.

1. **Genera prima le immagini** decise in Phase 2 (`generate_image`): tornano **subito** come `embed` (pending), non bloccano. Per il mockup usa `two-col mockup-right` (vedi `BRAND.md`).
2. **Componi tutte le slide** seguendo `PATTERNS.md` (pattern + template), `DENSITY_RULES.md` (stratifica label→headline→body→evidence, cap rispettati), `html-template.md` (solo classi del theme; niente `<section>`/`<div class="slide">`/header tag; colori via CSS var). `class="reveal"` su 3-5 elementi top-level per slide. Inserisci gli `embed` immagine nelle slide giuste.
3. **Una sola `create_presentation`** con l'intero array di slide (cover + tutte). **Niente** create + N×`add_slide`: è la fonte principale di lentezza e rumore.

`add_slide` / `update_slide` / `move_slide` / `remove_slide` servono **dopo**, per le iterazioni puntuali — non per la build iniziale.

**Errori comuni da evitare:**
- `headline-huge` su slide content (riservato a cover/divider/closing) → usa `headline-big`.
- Accent customer al posto di indigo su numerini/metric → sempre indigo `#6366F1`.
- 3 card piccole in `grid-3` invece di 2 grandi → preferisci 2 card grandi.
- Logo cliente nel header → mai (header = chrome Indigo).
- Emoji come **icone/label UI** → usa Lucide. (Le emoji come **messaggio di chat** dentro un mockup vanno bene.)
- Più di 2 themed, o themed in posizione 1/2/ultima → correggi.

---

## Phase 4 — Self-review visivo (OBBLIGATORIA)

Le regole in prosa non bastano: vanno **verificate guardando il risultato**. Dopo aver generato (o modificato) il deck, prima di consegnarlo:

1. **Chiama `screenshot_presentation(presentation_id)`** → ricevi una JPEG per slide, in ordine. Le **guardi davvero**, una per una.
2. **Confronta ogni slide con la rubrica** qui sotto.
3. **Correggi le violazioni** con `update_slide` sulla slide colpevole.
4. **Ri-chiama `screenshot_presentation`** e ricontrolla. Ripeti finché è pulito (**max 2 giri**; se dopo 2 giri resta un problema, dillo all'utente invece di nasconderlo).
5. Solo a deck pulito → Phase 5.

### Rubrica (controlla su OGNI slide)
- **Niente emoji come icone/label UI.** Icone solo Lucide SVG: se vedi un'emoji come icona (in una card, un flow, una lista, una shield) → sostituisci con Lucide. (Eccezione: le emoji come **messaggio di chat** dentro un mockup conversazionale vanno bene — sono contenuto, non chrome.)
- **Header visibile.** Logo indigo, tagline e numero slide non devono essere coperti (tipico su cover con hero image full-bleed).
- **Immagini ben dimensionate.** Non minuscole che galleggiano, non in overflow oltre i bordi. Mockup chat → dentro `two-col mockup-right`.
- **Niente testo tagliato / overflow / gap anomali.** Il contenuto sta dentro la slide; niente buchi verticali enormi tra titolo e body.
- **Accent indigo** su section-label, numerini, metric chiave (anche nei deck customer).
- **Themed solo mid-deck.** Mai cover/closing themed. Max 2 themed, mai consecutive, mai prima della slide 4.
- **Customer brand solo come sfondo themed**, non sugli accent.

> Placeholder grigio = immagine ancora in generazione (~60-90s): è normale, giudica **box e posizione**, non il contenuto. Se serve vedere l'immagine finita, ri-screenshotta dopo qualche secondo.

Questo passo è ciò che impedisce il ripetersi degli errori: **non fidarti del tuo HTML, guarda la slide.**

---

## Conversione PPT (futuro)

> Coperta dalla issue WP4 (AGE-90). Quando arriverà: `extract-pptx.py` → mapping a pattern → upload assets → create_presentation.

Per ora, se l'utente allega un `.pptx`, scusati e proponi di ricreare il deck a partire dal brief testuale (Phase 1).

---

## Phase 5 — Delivery

Quando il deck è completo **e la Phase 4 (self-review visivo) è passata**:

1. Comunica all'utente, in breve:
   - **Link visualizzazione**: `{url}` — navigazione con frecce/spazio/touch.
   - **Link editor**: `{editUrl}` — per modifiche visuali.
2. Una riga di sintesi: "Deck di N slide, M themed col brand X, [1-liner sul taglio]". Niente play-by-play delle singole slide.

Se l'utente vuole iterare ancora ("cambia la slide 5"), usa `update_slide` o `add_slide` / `remove_slide` / `move_slide` su quella specifica posizione — mai ricreare l'intera pres.

---

## Tool MCP disponibili

- `create_presentation(title, theme, customer_theme?, slides[])` — **Phase 3**: crea l'intero deck in un colpo (tutte le slide nell'array `slides[]`). È la chiamata principale di build.
- `add_slide(presentation_id, position, html_content, variant, notes?)` — aggiunge una slide (solo iterazioni post-build)
- `update_slide(presentation_id, slide_index, html_content?, variant?, notes?)` — modifica una slide (iterazioni)
- `remove_slide(presentation_id, slide_index)` — rimuovi una slide
- `move_slide(presentation_id, from_position, to_position)` — riordina
- `upload_asset(presentation_id, filename, content_base64, kind)` — logo / hero image / mockup / screenshot (immagini fornite dall'utente)
- `generate_image(presentation_id, prompt, technique?, kind?)` — genera un'immagine on-brand con flora.ai quando l'utente NON ne allega una. Due `technique`: `lifestyle-photos` (foto persone/lifestyle → **full-bleed o con fade**, mai inline) e `product-chat-generator` (mockup UI di chat → **inline** nella slide, mai background). Ritorna **subito** `{id, url, status:"pending", embed}`: incolla l'`embed` (già col placement giusto per tipo) nel `html_content`; il deck mostra uno skeleton e l'immagine compare da sola (~60-90s), senza attendere né polling. Cap per-presentazione. **Mai** per logo / grafici / dashboard. Vedi `BRAND.md` § Immagini generate.
- `screenshot_presentation(presentation_id, slide_index?)` — **Phase 4**: ritorna una JPEG per slide (inline) per la self-review visiva. Guarda le slide, correggi con `update_slide`, ripeti. Obbligatorio prima della delivery.
- `get_presentation(id)` — recupera stato corrente del deck
- `list_presentations(search?)` — cerca pres esistenti
- `get_theme_catalog(theme)` — catalog macchina-leggibile (fallback)
- `delete_presentation(id)` — elimina

---

## Checklist riassuntiva pre-tool-call

Prima di chiamare `create_presentation` o `add_slide`:

- [ ] **Scaletta approvata** dall'utente (deck nuovo; skip se 1 slide singola o edit di deck esistente)
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
