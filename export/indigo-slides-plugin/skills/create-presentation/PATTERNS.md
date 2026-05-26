# PATTERNS — Indigo slide patterns

I pattern coprono il ~95% dei casi d'uso reali. **Quando devi creare una slide, parti SEMPRE da un pattern.** Comporre da zero va bene solo se nessun pattern calza.

Ogni slide del reference Credit Agricole è **densa ma stratificata**: 1 section-label + 1 headline + 1 body + 1-2 card grandi + (opzionale) pill highlight. La densità è alta, ma è organizzata in **strati** (label → claim → evidence → footnote), non in elementi piccoli accostati.

I pattern qui sono organizzati in 6 famiglie:

1. **Cover** — apertura
2. **Content + Evidence** — slide narrative del corpo deck
3. **Process** — step di un journey o roadmap
4. **Detail tables** — pricing, comparison, compliance
5. **Section/Transition** — transizioni mid-deck
6. **Closing** — chiusura

---

## 1. Cover

### 1A. `cover-dark-hero` (DEFAULT per proposte commerciali)

**Variant:** `dark` + classe `hero-bg` con `background-image` inline.
**Quando:** prima slide di proposte commerciali, pitch di sales. Mostra una foto contestuale (cliente che usa l'app, banking, customer care, persona reale).

```html
<section class="slide dark hero-bg" style="background-image: url('/api/assets/<image-id>');">
  <!-- slide-header automatico -->
  <div class="slide-content">
    <div class="cover-content cover-left">
      <p class="cover-eyebrow">CRÉDIT AGRICOLE × INDIGO.AI</p>
      <h1 class="cover-title">L'assistente AI<br>nella vostra app</h1>
      <p class="cover-subtitle">Un'esperienza conversazionale per guidare i clienti Crédit Agricole nella scoperta e sottoscrizione della polizza salute. Voce e testo, direttamente in app.</p>
      <p class="cover-date">25 maggio 2026 · Proposta commerciale</p>
    </div>
  </div>
</section>
```

Il `.hero-bg` applica automaticamente un overlay gradient nero→trasparente da sinistra a destra per leggibilità.

**Anti-pattern:**
- Foto a destra mentre headline a sinistra dentro un `two-col` → NO, è hero-image full-bleed con overlay.
- Foto stock asettica → preferisci sempre persone reali in contesto.

### 1B. `cover-dark` (cover senza foto)

**Variant:** `dark`
**Quando:** deck interni, recap, demo internal, o proposte commerciali senza foto disponibile.

```html
<div class="cover-content">
  <p class="cover-eyebrow">PROPOSTA COMMERCIALE</p>
  <h1 class="cover-title">Indigo.ai × Acme<br>Voice AI per il Customer Care</h1>
  <p class="cover-subtitle">Una pres in 6 atti.</p>
  <p class="cover-date">25 maggio 2026</p>
</div>
<span class="ghost-text">2026</span>
```

### 1C. `cover-themed` (ECCEZIONALE — usare con cautela)

**Variant:** `themed`
**Quando:** **MAI come default.** Solo se il cliente lo richiede esplicitamente o se è un section-divider del deck "branded" (vedi 5A `section-divider-themed`).
**Regola:** la cover non DOVREBBE essere themed. Se hai dubbi, fai `cover-dark-hero`.

---

## 2. Content + Evidence

### 2A. `intro-context-stories` (case studies)

**Variant:** `dark`
**Quando:** seconda slide del deck — stabilisce credibilità. "Siamo il partner giusto perché..." + 2 case study + lista clienti.

```html
<p class="section-label">SCELTI DAL SETTORE FINANZIARIO</p>
<h2 class="headline-big mb-md">La piattaforma per creare<br>AI Agents su Voce e Testo</h2>
<p class="body-large mb-xl">Progettiamo, implementiamo e orchestriamo agenti AI Enterprise. Dal primo flusso conversazionale alle integrazioni coi sistemi aziendali, tutto sotto il completo controllo del cliente.</p>
<div class="case-study-grid">
  <div class="case-study-card">
    <img src="/api/assets/<hype-logo>" class="case-study-logo" alt="HYPE">
    <p class="case-study-desc">Touchpoint AI nelle sezioni Business e Privati: supporta gli utenti con le domande piu' frequenti e li accompagna alla soluzione giusta.</p>
    <div class="case-study-metrics">
      <div class="case-study-metric"><span class="metric-num">30 giorni</span><span class="metric-cap">Setup del progetto</span></div>
      <div class="case-study-metric"><span class="metric-num">+10K</span><span class="metric-cap">Utenti in 6 mesi</span></div>
      <div class="case-study-metric"><span class="metric-num">13%</span><span class="metric-cap">Click-through rate</span></div>
    </div>
  </div>
  <div class="case-study-card">
    <img src="/api/assets/<santander-logo>" class="case-study-logo" alt="Santander">
    <p class="case-study-desc">Assistente virtuale per il sito istituzionale: migliora l'esperienza utente e semplifica l'accesso alle informazioni.</p>
    <div class="case-study-metrics">
      <div class="case-study-metric"><span class="metric-num">+100K</span><span class="metric-cap">Messaggi in 5 mesi</span></div>
      <div class="case-study-metric"><span class="metric-num">+75%</span><span class="metric-cap">Richieste gestite dall'AI</span></div>
      <div class="case-study-metric"><span class="metric-num">+80%</span><span class="metric-cap">Utenti soddisfatti</span></div>
    </div>
  </div>
</div>
<div class="client-list">
  <p class="section-label muted">ALTRI CLIENTI NEL SETTORE FINANZIARIO (E NON SOLO)</p>
  <p class="client-list-names">Banco BPM · Banca Sella · Banca Ifis · Cofidis · Net Insurance · ITAS · Reale Mutua · Unipol · Satispay · Enel</p>
</div>
```

**Anti-pattern:**
- 3 case study card → 2 è il sweet spot. 3 diventa stretto.
- Metric senza unità → sempre "30 giorni" non "30".

### 2B. `highlight-insight` (slide light con card dark)

**Variant:** `light`
**Quando:** "il punto del deck" — un insight critico che merita evidenza. Testo narrativo a sinistra, card dark a destra che cattura l'occhio.

```html
<p class="section-label">IL MERCATO SI MUOVE</p>
<h2 class="headline-big mb-md">Il banking sta diventando<br>conversazionale</h2>
<div class="two-col gap-xl" style="align-items: stretch;">
  <div>
    <p class="body-large">Le banche che innovano stanno integrando AI generative nelle proprie app per trasformare l'interazione: da menu e form a conversazione naturale.</p>
    <div class="case-study-mini mt-xl">
      <p class="section-label muted">CASO WIIBIO — DKB</p>
      <p class="body-text">Banca WiiBio (Gruppo MPS) ha lanciato DKBdigo, la prima app bancaria italiana con AI generativa integrata.</p>
    </div>
  </div>
  <div class="highlight-card-dark">
    <p class="section-label highlight-eyebrow">L'OPPORTUNITÀ PER CRÉDIT AGRICOLE</p>
    <p class="body-text">Offrire ai vostri clienti un'esperienza conversazionale all'interno dell'app CA, partendo da un caso d'uso concreto e ad alto valore: la polizza salute.</p>
    <ul class="highlight-card-list">
      <li>Voce + testo + widget interattivi</li>
      <li>Lead qualificati dal CRM</li>
    </ul>
  </div>
</div>
```

**Anti-pattern:**
- Card dark senza section-label/eyebrow → manca la "firma" del messaggio.
- Card dark grande quanto la pagina → max 50% larghezza, 90% altezza disponibile.

### 2C. `text-and-chat` (esperienza voice/chat)

**Variant:** `light`
**Quando:** mostrare un caso d'uso conversazionale. Colonna sinistra spiega, destra fa vedere l'esperienza con chat-mockup.

```html
<p class="section-label">L'ESPERIENZA IN-APP</p>
<h2 class="headline-big mb-md">Voce + widget:<br>un'interazione naturale</h2>
<div class="two-col gap-xl" style="align-items: center;">
  <div>
    <p class="body-large mb-lg">Il cliente parla all'app. L'app risponde vocalmente e in parallelo mostra elementi grafici nel widget: statistiche di rischio, tabelle comparative tra piani, mappatura coperture sul profilo.</p>
    <ul class="feature-list">
      <li><b>Interazione vocale naturale</b><br><span class="muted">Il cliente parla come farebbe con un consulente.</span></li>
      <li><b>Elementi visivi in tempo reale</b><br><span class="muted">Grafici e tabelle compaiono in parallelo, sincronizzati alla risposta vocale.</span></li>
      <li><b>Questionario assicurativo vocale</b><br><span class="muted">Età, professione, trascorsi sanitari raccolti conversando.</span></li>
      <li><b>Scheda CRM automatica</b><br><span class="muted">Il profilo viene compilato e mandato al CRM. Lead qualificato.</span></li>
    </ul>
  </div>
  <div class="mockup-pair">
    <img src="/api/assets/<phone1>" class="phone-mockup" alt="App screen 1">
    <img src="/api/assets/<phone2>" class="phone-mockup" alt="App screen 2">
  </div>
</div>
```

Variante alternativa con chat-mockup invece dei phone-mockup: usa `<div class="chat-mockup">` (vedi `html-template.md`). Chat-bubble user **sempre indigo**, mai customer brand.

**Anti-pattern:** mai più di 5 feature nella `feature-list`. Mai chat-mockup + phone-mockup nella stessa slide.

### 2D. `text-and-mockup` (testo + screenshot/dashboard)

**Variant:** `light` o `dark`
**Quando:** quando hai 1 screenshot grande (dashboard, CRM, scheda lead). Colonna sinistra spiega, destra mostra.

Stessa struttura di `text-and-chat` ma con `<img src="..." class="screenshot-frame">` invece del chat-mockup.

---

## 3. Process

### 3A. `numbered-vertical-steps` (NEW — preferito)

**Variant:** `light` o `dark`
**Quando:** 3-5 step di un processo (Scoping → Build → Test → UAT → Go-live), una methodology, una roadmap. Colonna sinistra: headline + intro. Colonna destra: lista verticale numerata con cerchi indigo.

```html
<p class="section-label">COME SI PARTE</p>
<h2 class="headline-big mb-md">Setup:<br>da zero a live in 6–8 settimane</h2>
<div class="two-col gap-xl">
  <div>
    <p class="body-large">Un processo collaudato che abbiamo standardizzato su oltre 100 clienti enterprise. In 5 fasi vi portiamo dal kick-off al go-live del primo Agente AI in produzione.</p>
    <div class="cost-card mt-xl">
      <p class="section-label">COSTO ONBOARDING</p>
      <div class="cost-row">
        <span class="cost-amount">10.000 €</span>
        <span class="cost-note">Una tantum<br>Fino a 100 ore dedicate</span>
      </div>
    </div>
  </div>
  <ol class="numbered-steps">
    <li>
      <span class="step-num">1</span>
      <div class="step-body">
        <h4>Scoping</h4>
        <p>Raccolta requisiti, definizione use case, UX conversazionale e KPI di successo. Lavoriamo con il vostro team per disegnare l'esperienza.</p>
      </div>
    </li>
    <li>
      <span class="step-num">2</span>
      <div class="step-body">
        <h4>Build</h4>
        <p>Setup agente AI, workflow conversazionali, knowledge base, integrazioni con i vostri sistemi.</p>
      </div>
    </li>
    <li>
      <span class="step-num">3</span>
      <div class="step-body">
        <h4>Test</h4>
        <p>Validazione qualità, copertura scenari e robustezza delle risposte su dati reali.</p>
      </div>
    </li>
    <li>
      <span class="step-num">4</span>
      <div class="step-body">
        <h4>UAT</h4>
        <p>Test congiunto con il vostro team. Raccogliamo feedback, affiniamo l'agente, allineiamo i processi.</p>
      </div>
    </li>
    <li>
      <span class="step-num">5</span>
      <div class="step-body">
        <h4>Go-live</h4>
        <p>Attivazione in produzione nell'app CA. Monitoring attivo e supporto continuo dal nostro team.</p>
      </div>
    </li>
  </ol>
</div>
```

**Anti-pattern:**
- Più di 5 step → splitta o usa `process-flow` orizzontale.
- Step senza descrizione → il body è obbligatorio (1-2 righe).

### 3B. `process-flow` (orizzontale)

**Variant:** `light` o `dark`
**Quando:** alternativa a `numbered-vertical-steps` quando lo step è breve e il deck ha bisogno di "respiro orizzontale". 3-6 step.

```html
<p class="section-label">IL JOURNEY DEL CLIENTE</p>
<h2 class="headline-big mb-md">Da notifica push<br>a lead qualificato</h2>
<p class="body-large mb-xl">Il cliente riceve una notifica o naviga l'app normalmente. In una sezione vede la possibilità di parlare con l'app invece che interagire in modo tradizionale. Inizia una conversazione vocale che lo porta fino alla prenotazione di un appuntamento.</p>
<div class="flow-container">
  <div class="flow-step"><div class="flow-icon">🔔</div><h4>Notifica / Navigazione</h4><p>Push notification o accesso alla sezione prodotti.</p><div class="flow-arrow">→</div></div>
  <div class="flow-step"><div class="flow-icon">🎙</div><h4>Discovery vocale</h4><p>"Mi interessano coperture per fisioterapia vista la mia età..." — l'AI identifica l'interesse.</p><div class="flow-arrow">→</div></div>
  <div class="flow-step"><div class="flow-icon">📋</div><h4>Qualifica profilo</h4><p>Raccolta vocale di età, professione, trascorsi sanitari.</p><div class="flow-arrow">→</div></div>
  <div class="flow-step"><div class="flow-icon">📊</div><h4>Elementi visivi</h4><p>Tabelle comparative, mappatura coperture in parallelo.</p><div class="flow-arrow">→</div></div>
  <div class="flow-step"><div class="flow-icon">💾</div><h4>Scheda CRM</h4><p>Profilo compilato e inviato al CRM. Lead qualificato.</p><div class="flow-arrow">→</div></div>
  <div class="flow-step"><div class="flow-icon">📅</div><h4>Azione</h4><p>Il cliente sceglie: valutare coperture o fissare appuntamento.</p></div>
</div>
```

**Anti-pattern:** > 6 step → splitta in due slide. ≤ 2 step → usa `two-col` semplice.

---

## 4. Detail tables

### 4A. `compliance-shields`

**Variant:** `light`
**Quando:** reassurance enterprise (GDPR, ISO, AI Act, residency, audit trail). Grid 3x2 di shield-item.

```html
<p class="section-label">COMPLIANCE & SECURITY</p>
<h2 class="headline-big mb-md">Progettata per il settore<br>finanziario</h2>
<p class="body-large mb-xl">Sicurezza, controllo e conformità sono integrati nell'architettura della piattaforma, non aggiunti a posteriori.</p>
<div class="shield-grid shield-grid-3x2">
  <div class="shield-item"><div class="shield-icon">🔒</div><div><h4>ISO 27001 certificata</h4><p>Standard internazionale per la gestione della sicurezza delle informazioni.</p></div></div>
  <div class="shield-item"><div class="shield-icon">🛡</div><div><h4>Guardrails nativi</h4><p>Regole per controllare il comportamento dell'agente: topic boundaries, tone of voice, compliance. Anti-jailbreak integrato.</p></div></div>
  <div class="shield-item"><div class="shield-icon">📜</div><div><h4>GDPR, AI Act &amp; DORA ready</h4><p>Conforme alle normative europee su protezione dati, intelligenza artificiale e resilienza operativa digitale.</p></div></div>
  <div class="shield-item"><div class="shield-icon">🔍</div><div><h4>Audit trail completo</h4><p>Ogni conversazione tracciata, valutabile e esportabile. Conversation logs, evaluators e issue management integrati.</p></div></div>
  <div class="shield-item"><div class="shield-icon">🌐</div><div><h4>Isolamento dati</h4><p>Hosting conforme a normative regionali. Opzioni Private Cloud e On-Premise disponibili per massima sicurezza.</p></div></div>
  <div class="shield-item"><div class="shield-icon">🔧</div><div><h4>Controllo operativo</h4><p>Role manager, IP filtering, SSO. Governance fine-grained su chi accede a cosa e con quali permessi.</p></div></div>
</div>
```

**Anti-pattern:** > 6 shield. < 4 shield → usa un layout più semplice.

### 4B. `pricing-detail` (NEW — sostituisce pricing-offer)

**Variant:** `dark`
**Quando:** slide pricing dettagliata. Two-col: a sinistra struttura voci + total + highlight pill, a destra "cosa include" o tabella tariffaria.

```html
<p class="section-label">DOPO IL PILOTA — IL PRODOTTO</p>
<h2 class="headline-big mb-md">La licenza Super<br>copre tutto.</h2>
<p class="body-large mb-xl">Un unico canone annuale per tutti gli Agenti AI che attiverete su CA — voce, testo, multicanale. Quando il traffico cresce oltre la franchigia inclusa, si attiva l'add-on volumi a tariffa scalare.</p>
<div class="two-col gap-xl" style="align-items: stretch;">
  <div class="pricing-card">
    <p class="section-label">LICENZA PIATTAFORMA</p>
    <div class="pricing-row">
      <span class="pricing-name">Super</span>
      <span class="pricing-value">60.000 €<span class="pricing-unit">/anno</span></span>
    </div>
    <ul class="pricing-includes">
      <li>Tutti gli Agenti AI, tutti i canali</li>
      <li><b>120.000 sessioni/anno incluse</b> nella franchigia</li>
      <li>Voce e testo, integrazioni con i vostri sistemi</li>
      <li>Knowledge base, analytics, guardrails, audit trail</li>
    </ul>
    <p class="pill-highlight pill-green">Oltre le <b>120.000 sessioni</b> incluse, le sessioni aggiuntive si pagano a <b>tariffa scalare</b> — più volumi, meno costo per sessione.</p>
  </div>
  <div class="addon-card">
    <p class="section-label">ADD-ON VOLUMI</p>
    <h4 class="addon-title">Tariffa per sessione aggiuntiva</h4>
    <table class="addon-table">
      <thead><tr><th>Fascia</th><th>Volumi annui</th><th class="num">€ / sessione</th></tr></thead>
      <tbody>
        <tr><td>F1</td><td>fino a 240.000</td><td class="num">0,25 €</td></tr>
        <tr><td>F2</td><td>240K – 500K</td><td class="num">0,22 €</td></tr>
        <tr><td>F3</td><td>500K – 1M</td><td class="num">0,20 €</td></tr>
        <tr><td>F4</td><td>1M – 2M</td><td class="num accent-green">0,18 €</td></tr>
        <tr><td>F5</td><td>2M – 4M</td><td class="num accent-green">0,16 €</td></tr>
        <tr><td>F6</td><td>4M – 8M</td><td class="num accent-green">0,14 €</td></tr>
      </tbody>
    </table>
    <p class="pill-highlight pill-indigo">Tariffe applicate progressivamente: ogni fascia attiva lo sconto unitario sulla parte di volumi corrispondente.</p>
  </div>
</div>
```

**Anti-pattern:** prezzi senza unità (€, /anno). Total senza highlight visivo.

### 4C. `comparison-before-after`

**Variant:** `light`
**Quando:** mostrare il delta "oggi vs con Indigo".

```html
<p class="section-label">PRIMA E DOPO</p>
<h2 class="headline-big mb-xl">Da 8 strumenti a uno.</h2>
<table class="slide-table compare-table">
  <thead><tr><th>Prima</th><th>Dopo</th></tr></thead>
  <tbody>
    <tr><td class="col-before">CRM + ticketing + chat + IVR + 4 dashboard</td><td class="col-after">Piattaforma unica integrata</td></tr>
    <tr><td class="col-before">Time-to-resolve: 4h 30m</td><td class="col-after">Time-to-resolve: 12m</td></tr>
    <tr><td class="col-before">€ 1,40 per conversazione</td><td class="col-after">€ 0,42 per conversazione</td></tr>
  </tbody>
</table>
```

**Anti-pattern:** > 4 righe. Mai inversione (after a sinistra).

### 4D. `service-team` (NEW)

**Variant:** `dark`
**Quando:** spiegare il team di service / l'ipotesi di service-fee. Two-col: narrative + cost card a sx, dark card grande a dx con cosa il service include.

```html
<p class="section-label">DOPO IL PILOTA — I SERVIZI</p>
<h2 class="headline-big mb-md">I servizi:<br>il team che fa funzionare l'AI.</h2>
<p class="body-large mb-xl">A regime, l'ottimizzazione e l'evoluzione degli Agenti AI sono garantite da un team multidisciplinare dedicato: Customer Success, AI Implementation e Solution Engineering. La nostra ipotesi di partenza è <b>Service Enterprise</b>, ma il dimensionamento è flessibile.</p>
<div class="two-col gap-xl" style="align-items: stretch;">
  <div class="service-card-light">
    <p class="section-label">IPOTESI DI PARTENZA</p>
    <div class="service-row">
      <span class="service-name">Service Enterprise</span>
      <span class="service-value">48.000 €<span class="service-unit">/anno</span></span>
    </div>
    <p class="service-meta">+ In aggiunta al prodotto</p>
    <p class="service-detail"><b>400 ore/anno</b> di team multidisciplinare:</p>
    <ul class="service-list">
      <li>Customer Success</li>
      <li>AI Implementation</li>
      <li>Solution Engineering</li>
    </ul>
    <p class="body-small muted mt-md">Dimensionabile in più o in meno in base alle vostre effettive necessità.</p>
  </div>
  <div class="highlight-card-dark">
    <p class="section-label highlight-eyebrow">LA SERVICE FEE VI PERMETTE DI</p>
    <div class="service-block">
      <h4>Costruire nuovi Agenti AI</h4>
      <p>Il nostro team progetta e costruisce ogni nuovo use case all'interno delle ore dedicate. Nessun costo aggiuntivo di sviluppo.</p>
    </div>
    <div class="service-block">
      <h4>Ottimizzare quelli esistenti</h4>
      <p>Analisi performance, tuning conversazionale, aggiornamento knowledge base, evoluzione continua.</p>
    </div>
    <div class="service-block">
      <h4>Estendere senza limiti di licenza</h4>
      <p>Polizze, mutui, onboarding, customer care — ogni nuovo Agente AI si costruisce sulla stessa infrastruttura, senza costi di licenza aggiuntivi. Solo i volumi di traffico scalano col business.</p>
    </div>
  </div>
</div>
```

---

## 5. Section / Transition

### 5A. `section-divider-themed` (l'UNICO uso "themed")

**Variant:** `themed`
**Quando:** transizione mid-deck tra macro-sezioni (es. da "contesto" a "proposta", o da "tecnico" a "commerciale"). **Max 2 in tutto il deck, mai prima della slide 4, mai consecutivi, mai cover, mai closing.**

```html
<div class="slide-content centered">
  <p class="section-label highlight-eyebrow">LO SCENARIO</p>
  <h1 class="headline-huge">L'assistente AI<br>di Crédit Agricole</h1>
  <p class="body-large mt-md muted">Un agente conversazionale nell'app che guida il cliente nella scoperta del mondo Crédit Agricole. Voce, testo, elementi interattivi. Un'esperienza che semplifica la vita ai clienti e genera lead qualificati.</p>
  <p class="body-small mt-xl muted">Nella prossima slide, un approfondimento verticale sul mondo Polizze Persona &amp; Salute. La stessa esperienza è applicabile e modulabile manovrando sui mondi assicurativo, bancario e di credito.</p>
</div>
```

**Anti-pattern:** sezione divider con cards, tabelle, immagini. La forza è il vuoto.

### 5B. `demo-cta-themed` (raro)

**Variant:** `themed`
**Quando:** invitare a una demo a metà deck. Stessa formattazione di section-divider ma con CTA implicito.

```html
<div class="slide-content centered">
  <p class="section-label">DEMO LIVE</p>
  <h1 class="headline-huge">Vediamo<br>la demo?</h1>
  <p class="body-large mt-md muted">Una conversazione reale con l'assistente AI, costruita sui contenuti della polizza Protezione Persona &amp; Salute di CA Assicurazioni.</p>
  <div class="demo-meta mt-xl">
    <div><span class="demo-label">Knowledge base</span><span class="demo-value">Contenuti del sito CA Assicurazioni</span></div>
    <div><span class="demo-label">Interazione</span><span class="demo-value">Voce + bottoni dinamici</span></div>
    <div><span class="demo-label">Personalizzazione</span><span class="demo-value">Elementi grafici customizzati</span></div>
  </div>
</div>
```

Conta come "themed" — quindi è una delle 2 themed disponibili per il deck.

---

## 6. Closing

### 6A. `closing-big` (DEFAULT closing)

**Variant:** `dark`
**Quando:** ultima slide. Centrata, una headline grande + sotto subtitle breve + contatti.

```html
<div class="slide-content centered closing-content">
  <h1 class="headline-huge">Costruiamolo insieme.</h1>
  <p class="body-large muted mt-md">Un pilota rapido, un investimento contenuto, zero lock-in.<br>L'esperienza conversazionale per i vostri clienti parte da qui.</p>
  <div class="contact-row mt-xl">
    <div><span class="contact-label">Email</span><span class="contact-value">andrea@indigo.ai</span></div>
    <div class="contact-divider"></div>
    <div><span class="contact-label">Web</span><span class="contact-value">indigo.ai</span></div>
  </div>
</div>
```

**Anti-pattern:** lista verbosa di "prossimi passi". Più di 2 contatti. Una checklist con 4 voci → mai. La chiusura è asciutta, una sola frase di valore.

### 6B. `next-steps-detail` (penultima slide opzionale)

**Variant:** `dark`
**Quando:** slide PRIMA della closing-big. Listino di next step + 2 reassurance card.

```html
<p class="section-label">NEXT STEPS</p>
<h2 class="headline-big mb-xl">Come procedere</h2>
<div class="two-col gap-xl">
  <ol class="numbered-steps numbered-steps-sm">
    <li><span class="step-num">1</span><div class="step-body"><h4>Allineamento interno</h4><p>Condivisione della proposta con gli stakeholder interni per validare scope e tempistiche.</p></div></li>
    <li><span class="step-num">2</span><div class="step-body"><h4>Chiusura accordo</h4><p>Definizione del perimetro, firma e avvio del progetto pilota.</p></div></li>
    <li><span class="step-num">3</span><div class="step-body"><h4>Setup e go-live</h4><p>Build, test e lancio dell'assistente AI nell'app CA.</p></div></li>
  </ol>
  <div class="closing-cards">
    <div class="highlight-card-dark highlight-card-sm">
      <p class="section-label highlight-eyebrow">PERCHÉ ORA</p>
      <p class="body-text">Il mercato si muove. I competitor stanno integrando AI nelle app bancarie. Un pilota vi permette di testare l'esperienza con rischio minimo e posizionarvi come innovatori.</p>
    </div>
    <div class="reassurance-card">
      <p class="section-label highlight-eyebrow">COSA RENDE IL PILOTA SICURO</p>
      <ul class="check-list">
        <li>Pilota di 6 mesi con investimento definito</li>
        <li>Nessun vincolo contrattuale a lungo termine</li>
        <li>Risultati misurabili in settimane, non mesi</li>
        <li>Team dedicato indigo.ai per tutta la durata</li>
      </ul>
    </div>
  </div>
</div>
```

---

## Sequenza tipica di un deck enterprise (12-15 slide)

Riferimento concreto basato sul deck Credit Agricole.

| # | Pattern | Variant | Titolo esempio |
|---|---|---|---|
| 1 | `cover-dark-hero` | dark + hero | L'assistente AI nella vostra app |
| 2 | `intro-context-stories` | dark | La piattaforma per creare AI Agents su Voce e Testo |
| 3 | `compliance-shields` | light | Progettata per il settore finanziario |
| 4 | `highlight-insight` | light | Il banking sta diventando conversazionale |
| 5 | `text-and-mockup` | light | Voce + widget: un'interazione naturale |
| 6 | `process-flow` | light | Da notifica push a lead qualificato |
| **7** | **`section-divider-themed`** | **themed** | **L'assistente AI di Crédit Agricole** ← max 2 themed in tutto il deck |
| 8 | `comparison-before-after` | dark | Da processo manuale a lead qualificato in 3 minuti |
| 9 | `pricing-detail` (pilota) | dark | 6 mesi sull'assistente conversazionale |
| 10 | `numbered-vertical-steps` | dark | Setup: da zero a live in 6–8 settimane |
| 11 | `service-team` | dark | Un'infrastruttura AI su cui costruire |
| 12 | `pricing-detail` (prodotto) | dark | La licenza Super copre tutto |
| 13 | `service-team` | light | I servizi: il team che fa funzionare l'AI |
| 14 | `pricing-detail` (riepilogo) | dark | L'investimento annuale a regime |
| 15 | `next-steps-detail` | dark | Come procedere |
| **16** | **`demo-cta-themed`** | **themed** | **Vediamo la demo?** ← seconda e ultima themed |
| 17 | `closing-big` | dark | Costruiamolo insieme. |

Distribuzione: 1 cover (dark+hero) + 2 light + 12 dark + 2 themed (slide 7 e 16, ben separate).

---

## Quando NESSUN pattern calza

Capita raramente. In questi casi:
- Parti dal pattern più simile a quello che vuoi
- Riusa solo `section-label` + `headline-big` per gerarchia
- Compose con utility classes (`two-col`, `flex`, `grid-3`)
- Tieni density rules in mente (vedi `DENSITY_RULES.md`)
- **Salva la slide come potenziale nuovo pattern** se ti torna utile due volte

**Mai inventare nuove classi CSS** dentro `html_content`. Le classi disponibili sono solo quelle in `theme.css` (vedi `html-template.md`).
