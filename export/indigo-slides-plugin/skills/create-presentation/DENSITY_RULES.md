# DENSITY RULES — Quanto contenuto per slide

Le slide di reference Credit Agricole sono **dense ma stratificate**: 1 section-label + 1 headline + 1 body + 1-2 card grandi + (opzionale) pill highlight. La densità è alta, ma è organizzata in **strati narrativi** (label → claim → evidence → footnote), non in elementi piccoli accostati.

Questo file definisce il "quanto" è OK e quando splittare.

---

## Principio guida — stratificazione

Una slide tipica del reference ha **5-7 elementi** distribuiti in 3-4 livelli gerarchici:

1. **Eyebrow** (section-label uppercase mono indigo) — 1 elemento
2. **Claim** (headline-big, max 2 righe) — 1 elemento
3. **Narrative** (body-large, 2-4 righe) — 1 elemento
4. **Evidence** (card grandi, tabella, mockup, lista, metric grid) — 1-2 elementi
5. **Footnote** (pill highlight, body-small muted) — 0-1 elemento

Densità target: **70-85% dello spazio utile**. NON 50% (vuoto), NON 95% (soffocante).

Il pattern di errore più comune è:
- ❌ Slide che ha SOLO 3 card piccole in `grid-3` (vuota → vacua)
- ❌ Slide che ha 6 card + 2 tabelle + 3 metric tile (soffocata)

Il pattern giusto è:
- ✅ Slide con label + headline + body + 1 card grande + pill highlight (dense ma stratificate)
- ✅ Two-col asymmetric: narrative sx + evidence dx

---

## Cap globali per slide

| Elemento | Cap |
|---|---|
| Headline principale (h1 o h2) | **1** |
| Section-label uppercase indigo | **1** |
| Paragrafi `body-large` | **1-2** |
| Paragrafi `body-text` consecutivi | **≤ 3** |
| Card grandi (case-study, highlight, pricing, service) | **≤ 2** |
| Pill highlight colorate | **≤ 1** |
| Elementi con colore accento indigo (oltre il section-label) | **≤ 2** (es. 1 metric value + 1 numbered-step set) |

---

## Cap per pattern

### `cover-dark-hero` / `cover-dark`
| Elemento | Cap |
|---|---|
| Righe `cover-title` | ≤ 3 |
| Righe `cover-subtitle` | ≤ 3 |
| Cover-eyebrow (tagline cliente) | 1 riga UPPERCASE |
| `cover-date` | 1 riga |
| Elementi extra sotto subtitle | **0** (solo cover-date) |

### `intro-context-stories`
| Elemento | Cap |
|---|---|
| Case study card | **2** esatte (mai 3) |
| Metric per case study card | **3** (es. "30 giorni / +10K / 13%") |
| Caratteri per metric | ≤ 10 |
| Logo cliente nel case-study | 1, max-height 28-36px |
| Client list names | ≤ 12 nomi su 2 righe |

### `highlight-insight` / `next-steps-detail`
| Elemento | Cap |
|---|---|
| Card dark (`highlight-card-dark`) per slide | **1** |
| `highlight-eyebrow` | 1 |
| Body inside card dark | ≤ 4 righe |
| Lista `highlight-card-list` | ≤ 4 item |
| Card piccola "case study mini" affiancata | 1, max 4 righe |

### `text-and-chat`
| Elemento | Cap |
|---|---|
| Paragrafi colonna sinistra | 1 `body-large` + 1 `feature-list` (max 5 item) |
| `chat-bubble` totali | **≤ 4** |
| Caratteri per `chat-bubble` | ≤ 80 |
| Chat-mockup per slide | 1 (mai 2) |

### `text-and-mockup` / `mockup-pair`
| Elemento | Cap |
|---|---|
| Paragrafi colonna sinistra | 1 `body-large` + 1 `feature-list` (max 5) |
| Mockup phone affiancati | ≤ 2 |
| Screenshot dashboard | 1, aspect 16:9 o 16:10 |

### `compliance-shields`
| Elemento | Cap |
|---|---|
| `shield-item` totali | **4 / 6** (grid 2x2 o 3x2). Mai dispari oltre 1. |
| Caratteri `shield-item h4` | ≤ 28 |
| Righe `shield-item p` | ≤ 2 |

### `numbered-vertical-steps`
| Elemento | Cap |
|---|---|
| Step | **3 / 4 / 5** |
| Caratteri per `step h4` | ≤ 22 |
| Righe per `step p` | ≤ 2 |
| Cost-card affiancata sx | 1, opzionale |

### `process-flow` (orizzontale)
| Elemento | Cap |
|---|---|
| `flow-step` | **3-6** |
| Caratteri `flow-step h4` | ≤ 22 |
| Righe `flow-step p` | ≤ 2 |

### `results-dashboard`
| Elemento | Cap |
|---|---|
| `metric-card` | **3 / 4 / 6** (la griglia auto-fit) |
| `metric-value` accent indigo | 1 metric (la più importante) |
| Caratteri `metric-label` | ≤ 26 |
| Caratteri `metric-value` | ≤ 10 (es. "12.300", "78%", "0,42 €") |
| Caratteri `metric-delta` | ≤ 20 |

### `pricing-detail`
| Elemento | Cap |
|---|---|
| Card principali (pricing-card + addon-card) | 2 (sx e dx) |
| Righe in `pricing-includes` | ≤ 5 |
| Righe in `addon-table` | ≤ 6 |
| `pill-highlight` per card | ≤ 1 |
| Caratteri per voce | ≤ 60 |
| Caratteri per prezzo | ≤ 16 |

### `service-team`
| Elemento | Cap |
|---|---|
| Service-card-light (ipotesi) | 1 |
| Highlight-card-dark (cosa permette) | 1 |
| Service-block dentro card dark | **3** esatti |
| Righe per service-block p | ≤ 3 |

### `comparison-before-after`
| Elemento | Cap |
|---|---|
| Righe `compare-table` | ≤ 4 |
| Caratteri per cella | ≤ 60 |

### `section-divider-themed` / `demo-cta-themed`
| Elemento | Cap |
|---|---|
| Section-label / eyebrow | 1 |
| `headline-huge` | 1 |
| Body subtitle | 1-2 righe muted |
| `demo-meta` triplette | ≤ 3 (etichetta + valore) |
| Altri elementi | **0** |

### `closing-big`
| Elemento | Cap |
|---|---|
| `headline-huge` | 1 |
| Subtitle | ≤ 2 righe |
| Contatti | ≤ 2 (es. email + web) |
| Altri elementi | **0** (no checklist, no metric, no card) |

### `next-steps-detail`
| Elemento | Cap |
|---|---|
| `numbered-steps` | **3** step |
| Card dark a fianco | **2** (perché ora + cosa rende sicuro) |
| `check-list` dentro reassurance | ≤ 5 item |

---

## Quando il contenuto supera il cap

Tre opzioni, in ordine di preferenza:

1. **Splitta in più slide.** Aggiungi `(1/2)`, `(2/2)` al `section-label` per dare continuità.
2. **Tagli editoriali.** Riformula più conciso (una buona `pricing-detail` ha 4 voci nella `pricing-includes`, non 8).
3. **Cambio pattern.** Se hai 10 metriche, `results-dashboard` non è il pattern — diventa `pricing-detail` o `comparison-before-after`.

**Mai:**
- Ridurre la font size sotto il default del componente
- Ridurre il padding interno della slide
- Comprimere il `--header-h`
- Far overflowate la `slide-content`

---

## Controllo post-generazione

Prima di chiamare `create_presentation` / `add_slide`, per ogni slide controlla:

- [ ] **Stratificazione**: la slide ha label → headline → body → evidence → (footnote)? 5+ elementi gerarchicamente organizzati?
- [ ] **Densità ~70-85%**: la slide riempie lo spazio senza soffocare? Niente vuoti grossi a fondo né overflow?
- [ ] **Card grandi, non piccole**: se uso `grid-3` con card piccole, magari devo usare 2 card grandi affiancate?
- [ ] **Un solo elemento con accent**: 1 section-label indigo + (max) 1 metric accent indigo
- [ ] **Pattern density cap rispettato** (vedi tabella sopra)
- [ ] **No paragraph > 3 righe consecutive**
- [ ] **Two-col asymmetric** dove ha senso (narrative sx + evidence dx)
- [ ] **Pill highlight in fondo** se c'è una nota/footnote che merita evidenza

Se anche una checkbox fallisce: rifai la slide prima di chiamare il tool.

---

## Esempio: slide ben stratificata vs slide piatta

**❌ Piatta (vacua):**
```html
<p class="section-label">FEATURES</p>
<h2 class="headline-big">Cosa facciamo</h2>
<div class="grid-3">
  <div class="card"><h4>Voce</h4><p>...</p></div>
  <div class="card"><h4>Testo</h4><p>...</p></div>
  <div class="card"><h4>API</h4><p>...</p></div>
</div>
```
3 elementi gerarchici totali, niente evidence forte. Sembra incompleta.

**✅ Ben stratificata (densa ma chiara):**
```html
<p class="section-label">L'ESPERIENZA IN-APP</p>
<h2 class="headline-big mb-md">Voce + widget:<br>un'interazione naturale</h2>
<div class="two-col gap-xl">
  <div>
    <p class="body-large mb-lg">L'app parla con voce naturale e in parallelo mostra elementi visivi nel widget.</p>
    <ul class="feature-list">
      <li><b>Interazione vocale</b><br><span class="muted">Conversazione naturale.</span></li>
      <li><b>Elementi visivi</b><br><span class="muted">Grafici sincronizzati.</span></li>
      <li><b>Scheda CRM</b><br><span class="muted">Lead qualificato automatico.</span></li>
    </ul>
  </div>
  <div class="mockup-pair">
    <img src="..." class="phone-mockup">
    <img src="..." class="phone-mockup">
  </div>
</div>
```
5 livelli: label + claim + body + feature-list + mockup-pair. Stratificata, leggibile, densa al ~80%.
