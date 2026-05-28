# BRAND — Indigo Design System

Riferimento autoritativo per il brand Indigo applicato alle presentazioni. Ogni decisione tipografica, cromatica e di layout in `PATTERNS.md`, `html-template.md` e `SKILL.md` parte da qui.

---

## Font system

Tre famiglie. Niente fallback `system-ui` come display, niente Inter/Roboto.

| Font | Uso | Pesi disponibili | CSS variable |
|---|---|---|---|
| **Roobert** | Display (headlines, titoli card, valori metric, prezzi, cover) | 300 / 400 / 500 / 600 / 700 | `--f-display` |
| **Suisse Intl** | Body (testo corrente, descrizioni, label, subtitle) | 300 / 400 / 500 / 600 / 700 | `--f-body` |
| **Slussen Mono** | Mono (counter slide, tagline header, date in cover, section-label, metric-label) | 400 / 500 / 700 | `--f-mono` |

**Pesi tipici:**
- Headlines (`headline-*`): Roobert 500 medium. **700 (bold)** solo su `headline-huge` cover/closing per impatto.
- Body text (`body-*`): Suisse Intl 400 regular. Body grandi 1.5em line-height.
- Card titles, shield-item h4, flow-step h4: Roobert 600.
- **Section label: Slussen Mono 500, UPPERCASE, letter-spacing 0.06em, colore INDIGO** (vedi sotto).
- Tag, metric-label: Slussen Mono 500.

**Regola d'oro:** una sola gerarchia bold per slide. Se la headline è 500, il body è 400. Non mettere bold su tutto.

---

## Color palette

### Neutri (sfondi e testo)

| Token | Hex | Uso |
|---|---|---|
| `--c-black` | `#000` | Sfondo slide dark, testo su slide light |
| `--c-white` | `#fff` | Testo su chat-bubble user, raramente come sfondo |
| `--c-cream` | `#F5F1EB` | Testo su slide dark/themed |
| `--c-warm-bg` | `#F9F8F5` | Sfondo slide light (warm cream, non bianco pieno) |
| `--c-dark-bg` | `#111` | Sfondo flow-arrow / chrome details su dark |

### Brand accent — INDIGO PURPLE

`--c-indigo: #6366F1` — è **l'accent operativo del design system**, anche nelle presentazioni customer.

**Dove appare l'accent indigo (sempre, anche su pres customer):**
- Section-label in mono uppercase (`SCELTI DAL SETTORE FINANZIARIO`, `COMPLIANCE & SECURITY`, `IL JOURNEY DEL CLIENTE`, `NEXT STEPS`)
- Numerini circle nei `numbered-vertical-steps` (1/2/3/4/5)
- Metric value più importante della slide (sempre **una** per slide)
- Highlight pill colorato in fondo a card (per "Sconto", "Scalabilità inclusa")
- Chat-bubble.user background (NON il customer primary)
- Flow-arrow color
- Highlight-card-dark background `#1F1F3A` (indigo dark)

### Customer brand — solo per slide themed

Il colore primario del cliente (es. Acme `#00AA88`) **NON è l'accent operativo**. Vive **come sfondo** in 1-2 slide themed (section-divider), e basta.

> **Big rule:** quando una pres ha `customer_theme`, l'accent dei section-label resta INDIGO, non diventa customer. Il customer brand è "atmosfera di sezione", non "ink ovunque".

### Accent semantici

| Token | Hex | Uso |
|---|---|---|
| `--c-green` | `#22C55E` | `metric-delta.up`, `metric-check`, `tag.green`, pill highlight "Sconto" |
| `--c-red` | `#EF4444` | `metric-delta.down`, `tag.red` |
| `--c-blue` | `#3B82F6` | `tag.blue`, pill highlight "Zero lock-in" |
| `--c-orange` | `#F97316` | `tag.orange` |
| `--c-purple` | `#8B5CF6` | `tag.purple` |
| `--c-amber` | `#F59E0B` | Stati custom |

**Regola "un accento operativo per slide":** una sola tinta indigo + eventualmente una sola tinta semantica per la pill highlight. Mai 3 colori vivi insieme.

---

## Variants

Tre varianti slide. Il renderer applica automaticamente palette, logo e dividers a seconda della variant.

| Variant | Background | Testo | Logo header | Quando usarla |
|---|---|---|---|---|
| `light` | `--c-warm-bg` (#F9F8F5) | `--c-black` | `logo-dark.svg` | Slide con icon-driven reassurance (compliance), slide con foto-driven evidence (mockup), pricing detail |
| `dark` | `--c-black` | `--c-cream` | `logo-light.svg` | **Default per proposte commerciali.** Cover hero, slide "rich content" (case studies, numbered steps, pricing, services, closing) |
| `themed` | `--c-client-gradient` | `--c-cream` | `logo-light.svg` | **ECCEZIONALE — max 2 slide per deck.** SOLO section-divider mid-deck per pres customer. **MAI cover, MAI closing, MAI consecutive.** |

### Distribuzione raccomandata per tipo di deck

| Tipo deck | Light | Dark | Themed |
|---|---|---|---|
| **Recap interno** (retro, team update) | 60-70% | 30-40% | **0%** (no customer) |
| **Pitch tecnico interno** | 30% | 70% | 0% |
| **Proposta commerciale customer (default)** | 25-30% | 60-65% | 5-10% (max 2 slide) |
| **Quick demo** (5-6 slide) | 20% | 80% | 0% (troppo corto) |

### Regole hard sulle variant

1. **Cover SEMPRE dark.** Mai light, mai themed. La cover è dark con hero image (foto contestuale) OPPURE dark con headline + ghost-text decorativo.
2. **Closing SEMPRE dark.** Una headline grande centrata + contatto sotto. Mai themed.
3. **Themed solo mid-deck.** Da slide 4 in poi, mai prima. Da slide N-2 in dietro, mai dopo (non come penultima/ultima).
4. **Themed mai consecutive.** Almeno 2 slide light/dark tra due themed.
5. **Themed solo per pres customer.** Se non c'è `customer_theme`, nessuna slide themed.

---

## Customer theme

Quando una presentazione ha `customer_theme` impostato, il renderer inietta un blocco `<style data-customer-theme>` che sovrascrive tre CSS variables:

```css
:root {
  --c-client-primary: #00AA88;        /* da customer_theme.primary */
  --c-client-primary-light: #D4F0E5;  /* da customer_theme.primaryLight */
  --c-client-gradient: linear-gradient(135deg, #00AA88 0%, #00755C 100%);
  /* da customer_theme.gradient, oppure derivato da primary */
}
```

**Cosa cambia automaticamente:**
- Sfondo slide `.themed` → gradient cliente
- **Niente altro.** Il resto della pres usa indigo accent (vedi sopra).

### Schema `customer_theme`

```json
{
  "clientName": "Acme Bank",
  "primary": "#00AA88",
  "primaryLight": "#D4F0E5",
  "gradient": "linear-gradient(135deg, #00AA88 0%, #00755C 100%)",
  "logoUrl": "https://indigo-slides.fly.dev/api/assets/abc123"
}
```

| Field | Obbligatorio | Note |
|---|---|---|
| `clientName` | Sì | Appare nel header tagline (`indigo.ai × <clientName>`) |
| `primary` | Sì | Hex colore principale brand cliente |
| `primaryLight` | Sì | Tinta chiara dello stesso colore |
| `gradient` | No | Se omesso, derivato automaticamente |
| `logoUrl` | No | Per `<img>` opzionale dentro slide (es. case study card), MAI nel header |

---

## Header chrome (generato automaticamente)

Il renderer aggiunge automaticamente `slide-header` sopra ogni `slide-content`:

- **Top-left**: `indigo.ai` (logo testuale o SVG) — sempre presente
- **Top-right**: tagline contestuale in mono — variabile per sezione (es. "Proposta commerciale", "Sicurezza e governance", "CA Scenario", "Prossimi passi")
- **Bottom-right**: numero pagina in mono uppercase ("01", "02", … "15")

**Cosa fa la skill:** suggerisce la tagline appropriata sezione-per-sezione. Es. nella metà tecnica del deck "Sicurezza e governance", nella metà commerciale "Proposta commerciale".

---

## Logo variants

Due file SVG. Il renderer sceglie automaticamente:

| Variant | Logo file |
|---|---|
| `light` | `logo-dark.svg` |
| `dark` | `logo-light.svg` |
| `themed` | `logo-light.svg` |

**Mai forzare un logo manualmente** dentro `html_content`.

---

## Spacing scale

Tutte le distanze interne usano la scala. Niente valori arbitrari.

| Token | Px | Uso tipico |
|---|---|---|
| `--s-xs` | 4 | Gap tra label e value piccoli |
| `--s-sm` | 8 | Gap intra-componente stretto |
| `--s-md` | 16 | Gap tra card in `grid-4`, margin interno default |
| `--s-lg` | 24 | Gap tra card in `grid-3`, margin tra headline e contenuto |
| `--s-xl` | 40 | Gap tra sezioni grandi della stessa slide |
| `--s-2xl` | 60 | Gap colonne in `two-col` |

---

## Padding slide

| Token | Px | Cosa controlla |
|---|---|---|
| `--slide-pad-x` | 44 | Margine sx/dx interno |
| `--slide-pad-y` | 44 | Margine bottom |
| `--header-h` | 68 | Altezza area header |

---

## Image guidelines

### Hero image su cover-dark-hero

**Quando usarla:** cover di proposte commerciali / pitch di sales. NON su deck interni.

**Specifiche:**
- Aspect ratio: 16:9 o 16:10 (mai verticale, mai 1:1)
- Risoluzione: ≥1920px sul lato lungo
- Soggetto: persone reali in contesto pertinente (clienti che usano l'app, banking, customer care). Mai stock asettico.
- Trattamento: full-bleed dietro l'header, **gradient overlay dal nero a trasparente da sinistra a destra** per leggibilità della headline (la headline vive sulla parte scura sx)

```html
<!-- Pattern: hero-image-cover -->
<section class="slide dark hero-bg" style="background-image: url('/api/assets/xyz')">
  <!-- slide-header generato automaticamente -->
  <!-- slide-content generato con headline a sinistra -->
</section>
```

Vedi `theme.css` § `.hero-bg` per il gradient overlay.

### Immagini generate (flora)

Due tecniche, scelte con `technique` in `generate_image`. Entrambe ritornano **subito** `{id, status:"pending", embed}` (non bloccante): incolli l'`embed` nel `html_content` dove va l'immagine, il deck è già visibile con uno skeleton lì e l'immagine compare da sola in ~60-90s. **Non aspettare, non fare polling.** Solo proposte/pitch clienti, mai deck interni. Cap: max 8 generate per presentazione (`remaining_generations` nel ritorno). **Mai** per logo, grafici o dashboard (quelli restano `upload_asset` o pattern dedicati).

**⚠️ Il posizionamento dipende dal tipo — regola rigida:**

**1. `lifestyle-photos` — foto di persone/lifestyle** (cover, hero, divisori). Soggetti reali in contesto (professionisti, customer care, banking), candid, luce naturale.
- **Posizionamento: SEMPRE full-bleed, oppure con dissolvenza/gradient sul background. MAI inline in un blocco.** L'`embed` ritornato usa già `gen-image gen-image-cover` (full-bleed): mettila come **prima** riga del `html_content`, poi il contenuto (`cover-content`, ecc.) sopra. Full-bleed + overlay di leggibilità sono nel CSS. (La `.hero-bg` con `background-image` vale solo per immagini **già caricate** via upload, non per le generate che partono `pending`.)

**2. `product-chat-generator` — mockup UI di chat** (slide demo "l'assistente in azione", es. l'esperienza in app). Genera l'immagine di uno screen di conversazione.
- **Posizionamento: SEMPRE inline dentro la slide, MAI come background full-bleed.** Usa il wrapper dedicato `<div class="two-col mockup-right">`: colonna **sinistra** = il testo della slide (section-label + headline + body + feature-list), colonna **destra** = la `figure` del mockup. Il titolo va **dentro** la colonna sinistra (non sopra il `two-col`), così il mockup occupa tutta l'altezza ed è grande. La classe `mockup-right` dimensiona tutto: **non** impostare `height`/`align-items` a mano. L'`embed` ritornato usa `gen-image gen-image-inline` (mostrato intero, non croppato).

**Prompt:** per le foto, specifico su soggetto, setting, mood, luce (candid, luce naturale, professionisti reali, ambienti moderni); per i mockup, descrivi lo scenario/contenuto della chat. Evita posa da stock, watermark, soggetti generici. (Le emoji eventualmente disegnate nel mockup vengono già soppresse lato generazione.)

### Logo cliente

| Posizione | Crop | Aspect target |
|---|---|---|
| Cover (raro, solo se richiesto esplicito) | Preserva | 3:1 ≤ ratio ≤ 1:1 |
| Case study card | Preserva o circle se quadrato | max-height 32px |
| Chat-avatar | Crop circle | 1:1 |
| Nel header | **MAI** — il header è chrome Indigo | — |

### Quando emoji vs Lucide vs logo cliente

| Contesto | Scelta |
|---|---|
| Slide interne / informali (recap team, retro) | Emoji OK |
| Slide customer-facing formali (proposte, pitch, demo) | Lucide SVG outline (`stroke-width="1.5"`) |
| Cover proposta cliente | Hero image se disponibile, altrimenti niente icon |
| Card-icon decorativo su slide light | Lucide piccolo (28px), color indigo o currentColor |
| Card-icon decorativo su slide dark | Lucide piccolo, color cream/currentColor |

**Mai mischiare** emoji e Lucide nella stessa griglia.

### Quando l'immagine non è usabile

Per il flusso image-evaluation (vedi `SKILL.md`), una immagine è **NOT USABLE** se:
- Aspect ratio fuori range (es. 1:5 verticale)
- Risoluzione < 1200px sul lato lungo (per hero) o < 600px (per card)
- Trasparenza assente quando serve (logo PNG con sfondo bianco su slide dark)
- Soggetto sbagliato (l'utente ha allegato uno screenshot del browser invece del prodotto)
- Watermark / "comp" / "draft" visibili

---

## Anti-pattern brand (cose da non fare)

1. **Mai `app.indigo.ai`** come URL piattaforma. È legacy pre-Pravda. Usa sempre `platform.indigo.ai`.
2. **Mai logo cliente nel header.** Il header è chrome Indigo, sempre.
3. **Mai accent customer al posto di indigo.** Section-label, numerini, metric chiave: sempre indigo `#6366F1`. Il customer brand vive solo come sfondo `.themed`.
4. **Mai cover/closing themed.** Cover sempre `dark` (con o senza hero image). Closing sempre `dark`.
5. **Mai tutte le slide themed.** Max 2 slide themed in tutto il deck, mai consecutive, mai prima della slide 4.
6. **Mai themed senza customer_theme.** Se non c'è cliente, niente themed (fallback non basta).
7. **Mai font diversi da Roobert/Suisse Intl/Slussen Mono.**
8. **Mai padding fissi diversi dai token** (no `padding: 17px`).
9. **Mai inline color** in hex per il testo. Usa CSS variables o classi.
10. **Mai più di un accent operativo per slide** (eccetto tabelle/griglie semantiche dove i colori hanno significato).
