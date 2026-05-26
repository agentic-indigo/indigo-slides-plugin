# Indigo Slides Plugin

Crea presentazioni professionali con il design system di **indigo.ai** direttamente da Claude. Editor visuale online, condivisione via link, brand cliente opzionale.

Plugin ufficiale per [Claude Code](https://claude.ai/code). Il server di rendering vive su [indigo-slides.fly.dev](https://indigo-slides.fly.dev).

---

## Installazione (Claude Code Desktop — 1 minuto, zero terminale)

1. Apri **Claude Code Desktop**
2. Vai nelle **Settings** → **Plugins** (o usa il pannello plugins dalla sidebar)
3. Click **"Add marketplace"**
4. Incolla questo URL:
   ```
   https://github.com/agentic-indigo/indigo-slides-plugin
   ```
5. Trovi **indigo-slides** nella lista — click **Install**
6. **Quit e riapri** Claude Code Desktop

Dopo il riavvio, il plugin è attivo. Scrivi nel chat:

> Crea una presentazione di 10 slide per un pitch interno

...e Claude userà la skill `indigo-slides:create-presentation` automaticamente.

---

## Come ottenere gli aggiornamenti

Quando viene rilasciata una nuova versione del plugin (es. v1.3), Claude Code Desktop dovrebbe notificarti automaticamente se hai l'**Auto-update** attivo sul marketplace.

**Se non vedi l'aggiornamento:**

1. Apri **Settings** → **Plugins** → tab **Marketplaces**
2. Trova **indigo-slides** → click sui 3 puntini → **Remove**
3. Re-aggiungi il marketplace seguendo i passi di installazione qui sopra
4. Re-installa il plugin

Il fix per gli update senza remove+re-add è una feature di Claude Desktop attualmente in evoluzione.

---

## Cosa fa il plugin

- **Phase-based generation**: outline strategico → preview cover → estensione full deck
- **Design system Indigo**: Roobert + Suisse Intl + Slussen Mono, palette brand
- **Customer theming opzionale**: gradient + accent + logo cliente per proposte commerciali
- **12+ pattern enterprise**: cover-dark-hero, intro-context-stories, highlight-insight, numbered-vertical-steps, pricing-detail, service-team, section-divider-themed, closing-big, ecc.
- **Editor visuale**: ogni pres ha un link `/edit` per modifiche manuali post-generazione

---

## Skill disponibile

| Skill | Trigger | Cosa fa |
|---|---|---|
| `indigo-slides:create-presentation` | Chiedi a Claude di creare una pres | Genera deck HTML + carica su indigo-slides server + ritorna link condivisibile |

---

## Architettura

| Componente | Repo | Visibilità |
|---|---|---|
| **Plugin** (marketplace + skill + tool MCP definitions) | `agentic-indigo/indigo-slides-plugin` (questo repo) | Public |
| **Server di rendering** (Next.js + DB + asset storage) | `andreaindigo/indigo-slides` | Private |

Il plugin contatta il server via MCP (`https://indigo-slides.fly.dev/api/mcp`). Il server gestisce auth Clerk, slide DB su Neon, asset storage su volume fly, rendering HTML.

---

## Supporto

- Problemi tecnici / feature request: apri un'[issue](https://github.com/agentic-indigo/indigo-slides-plugin/issues)
- Per indigo.ai team: chiedi su Slack `#agentic-slides`

---

## Licenza

Plugin distribuito sotto licenza indigo.ai. Uso interno e per partner.
