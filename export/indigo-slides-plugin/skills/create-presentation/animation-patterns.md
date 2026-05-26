# ANIMATION PATTERNS — Movimento brand Indigo

L'engine di `indigo-slides` v1.2+ adotta il pattern di `frontend-slides`: **scroll-snap + IntersectionObserver + `.reveal` stagger**. Una slide entra in viewport → riceve `.visible` → i child con classe `.reveal` si animano in sequenza (fade + rise).

Tono Indigo è **trattenuto**: 600ms easing out-expo, translateY 20px, stagger 0.12s. Niente neon, niente glitch, niente tilt 3D.

---

## Default automatico — `.reveal` su tutti gli elementi top-level

L'engine fa il lavoro: aggiunge `.visible` quando la slide è ≥50% in viewport. Il CSS `theme.css` ha già la regola:

```css
.reveal { opacity: 0; transform: translateY(20px); transition: opacity 0.6s var(--ease-out-expo), transform 0.6s var(--ease-out-expo); }
.slide.visible .reveal { opacity: 1; transform: translateY(0); }
.slide.visible .reveal:nth-child(1) { transition-delay: 0.05s; }
.slide.visible .reveal:nth-child(2) { transition-delay: 0.17s; }
.slide.visible .reveal:nth-child(3) { transition-delay: 0.29s; }
.slide.visible .reveal:nth-child(4) { transition-delay: 0.41s; }
.slide.visible .reveal:nth-child(5) { transition-delay: 0.53s; }
```

**Cosa fai nel `html_content`:** aggiungi `class="reveal"` agli elementi top-level che vuoi animare:

```html
<p class="section-label reveal">SCELTI DAL SETTORE FINANZIARIO</p>
<h2 class="headline-big mb-md reveal">La piattaforma per creare<br>AI Agents su Voce e Testo</h2>
<p class="body-large mb-xl reveal">Progettiamo, implementiamo e orchestriamo agenti AI Enterprise...</p>
<div class="case-study-grid reveal">...</div>
<div class="client-list reveal">...</div>
```

5 reveal max — lo stagger CSS ha 5 step. Oltre, gli extra elementi appaiono insieme all'ultimo.

---

## Quando NON usare `.reveal`

- **Elementi dentro card grandi** già rivelate (es. ogni `case-study-metric` dentro `case-study-card`) — non serve doppia animazione.
- **Header chrome** (logo, tagline, counter) — sempre visibile, mai animato.
- **Background image** della cover-hero — appare immediato.
- **Slide consecutive con stessa struttura** (es. due `pricing-detail` consecutive) — la seconda non ha bisogno del reveal stagger se il pattern è già "noto" all'utente.

---

## Pattern: hero entrance (cover)

Sulla cover, il reveal stagger sui 4-5 elementi del `cover-content` crea un'apertura cinematografica:

```html
<div class="cover-content cover-left">
  <p class="cover-eyebrow reveal">CRÉDIT AGRICOLE × INDIGO.AI</p>
  <h1 class="cover-title reveal">L'assistente AI<br>nella vostra app</h1>
  <p class="cover-subtitle reveal">Un'esperienza conversazionale per guidare i clienti Crédit Agricole...</p>
  <p class="cover-date reveal">25 maggio 2026 · Proposta commerciale</p>
</div>
```

4 elementi → 4 step di stagger. La hero image è già visibile (background), il `cover-content` "atterra" sopra.

---

## Pattern: section-divider drammatico

Sui section-divider themed, l'effetto è più "scenico". Usa una variante con `translateY` più ampio (40px) e duration leggermente più lunga (800ms). Inline nello `html_content`:

```html
<style>
  .slide.themed .reveal-hero {
    opacity: 0;
    transform: translateY(40px);
    transition: opacity 0.8s var(--ease-out-expo), transform 0.8s var(--ease-out-expo);
  }
  .slide.themed.visible .reveal-hero { opacity: 1; transform: translateY(0); }
  .slide.themed.visible .reveal-hero:nth-child(1) { transition-delay: 0.1s; }
  .slide.themed.visible .reveal-hero:nth-child(2) { transition-delay: 0.3s; }
  .slide.themed.visible .reveal-hero:nth-child(3) { transition-delay: 0.55s; }
</style>
<div class="slide-content centered">
  <p class="section-label highlight-eyebrow reveal-hero">LO SCENARIO</p>
  <h1 class="headline-huge reveal-hero">L'assistente AI<br>di Crédit Agricole</h1>
  <p class="body-large mt-md muted reveal-hero">Un agente conversazionale nell'app...</p>
</div>
```

Solo per section-divider e cover-themed. Non sulle slide di contenuto.

---

## Pattern: number count-in (KPI / results-dashboard)

Sui KPI tile, dare un fade-in stagger sui `metric-value`:

```html
<style>
  .slide.visible .metric-card { opacity: 0; transform: translateY(15px); animation: kpiIn 0.6s var(--ease-out-expo) forwards; }
  .slide.visible .metric-card:nth-child(1) { animation-delay: 0.15s; }
  .slide.visible .metric-card:nth-child(2) { animation-delay: 0.30s; }
  .slide.visible .metric-card:nth-child(3) { animation-delay: 0.45s; }
  .slide.visible .metric-card:nth-child(4) { animation-delay: 0.60s; }
  @keyframes kpiIn { to { opacity: 1; transform: translateY(0); } }
</style>
```

Override del default `.reveal` quando vuoi controllo specifico sui figli del grid.

---

## Pattern: highlight pulse (call to attention)

Su un singolo elemento di accent indigo (es. il `metric-value.accent` chiave o la `pill-highlight`), puoi aggiungere un sottile pulse di apertura:

```html
<style>
  .slide.visible .metric-value.accent {
    animation: indigoIn 1.2s var(--ease-out-expo) 0.6s both;
  }
  @keyframes indigoIn {
    0% { opacity: 0; transform: scale(0.92); }
    50% { opacity: 1; transform: scale(1.04); }
    100% { opacity: 1; transform: scale(1); }
  }
</style>
```

**Uso parsimonioso**: max una volta nel deck, sulla metric chiave più importante (es. "68%" sulla slide KPI).

---

## `prefers-reduced-motion`

**OBBLIGATORIO.** Il theme.css globalmente ha già:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { animation-duration: 0.01ms !important; transition-duration: 0.2s !important; }
  .reveal, .reveal-hero { opacity: 1 !important; transform: none !important; }
  html { scroll-behavior: auto; }
}
```

Se aggiungi animazioni custom inline (es. `kpiIn`, `indigoIn`), copia anche il guard nel tuo `<style>`:

```html
<style>
  /* ... le tue animazioni ... */
  @media (prefers-reduced-motion: reduce) {
    .slide.visible .metric-card,
    .slide.visible .metric-value.accent {
      animation: none !important;
      opacity: 1 !important;
      transform: none !important;
    }
  }
</style>
```

---

## Anti-pattern (cose da NON fare)

Animazioni associate ad altri brand — esplicitamente vietate:

| Effetto | Vietato perché |
|---|---|
| Neon glow / box-shadow pulsante | Cyber, non Indigo |
| Glitch / text scramble | Hacker, non Indigo |
| Tilt 3D su hover | "Landing page tech", troppo gimmick |
| Particle background (canvas) | Heavy, non brand-appropriate |
| Custom cursor con trail | Distrae, non funziona in fullscreen |
| Bounce / elastic easing | Playful, brand è "calm confidence" |
| Confetti / celebrations | Mai, anche su slide di success |
| Marquee / scroll infinito | Mai |
| Parallax tra layer | Distrae dalla content |
| **`.reveal` su 10+ elementi** | Disordine. Max 5 elementi top-level animati per slide. |
| **`.reveal` su slide consecutive con stesso pattern** | Diventa rumore. Lascia che il default scroll-snap basti. |

Se hai bisogno di "drama" su una slide, ottienilo con:
- Variant `dark` o `themed`
- Headline `headline-huge` su slide quasi-vuota (section-divider)
- `ghost-text` molto grande
- Una metric tile con `metric-value.accent` enorme

Il drama è **strutturale**, non animato.

---

## Quando inserire animazioni inline custom

**Default (90% dei casi):** non aggiungere `<style>` nel `html_content`. Usa solo `class="reveal"` sugli elementi top-level. Il default fa il lavoro.

**Aggiungi animazioni inline** solo se:
1. La slide è "hero" (cover, divider, closing, results-dashboard chiave)
2. L'utente ha esplicitamente chiesto "più dinamico"
3. Il deck nel suo insieme ha < 20% slide animate custom (per non saturare)

In dubbio: non aggiungere. Una slide statica ma ben composta è meglio di una mediocre animata.

---

## Check pre-tool-call

Se hai aggiunto animazioni inline:

- [ ] Duration ≤ 1.2s (1.2s solo per pulse, default 600ms)
- [ ] Easing `cubic-bezier(0.16, 1, 0.3, 1)` (out-expo) o `cubic-bezier(0.4, 0, 0.2, 1)` (standard)
- [ ] Translate ≤ 40px (40px solo per hero, default 20px)
- [ ] Stagger 0.10–0.20s tra elementi
- [ ] Max 5 `reveal` top-level per slide
- [ ] `prefers-reduced-motion` block presente per le animazioni custom
- [ ] Nessun effetto vietato (vedi anti-pattern)
- [ ] Slide non consecutiva a un'altra slide con custom animations
