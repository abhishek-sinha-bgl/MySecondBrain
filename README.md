# MyResAssist — Research Intelligence

Most AI chat is disposable. You ask, it answers, you close the tab and it's gone. The next time you research the same topic you start from zero, re-establishing context, re-finding sources, re-checking what you already knew.

**MyResAssist is built around a different idea.** Every research session contributes to a persistent knowledge base. Facts are extracted as discrete, citable *claims*. Each claim carries a confidence rating, a source with an authority tier, and a rationale for why it matters. Contradictions between claims are detected automatically. Gaps in the evidence are surfaced explicitly. Over time, your knowledge on a topic doesn't just accumulate — it self-organises, self-critiques, and tells you what it doesn't yet know.

The result is less like chatting with an AI and more like building a living research dossier: one that deepens with every conversation, flags when its facts are getting stale, actively seeks better sources, and lets you share findings with a single tap.

It runs as a single HTML file in any modern browser. No installation. No account. No server. Just an API key for the AI provider of your choice.

**[Live App](https://abhishek-sinha-bgl.github.io/MyResAssist/)** · [Report a bug](https://github.com/abhishek-sinha-bgl/MyResAssist/issues)

---

## Contents

- [Getting Started](#getting-started)
- [The Research Wall](#the-research-wall)
- [The Topic View](#the-topic-view)
- [The Chat Panel](#the-chat-panel)
- [Absorbing Claims](#absorbing-claims)
- [Source Intelligence](#source-intelligence)
- [Analysing URLs & Documents](#analysing-urls--documents)
- [Research Gaps](#research-gaps)
- [Staleness & Validation](#staleness--validation)
- [Trusted Sources](#trusted-sources)
- [Themes](#themes)
- [AI Provider Configuration](#ai-provider-configuration)
- [Technical Notes](#technical-notes)
- [Quick Reference](#quick-reference)

---

## Getting Started

### Prerequisites

- An API key from one of: **Anthropic (Claude)**, **OpenAI**, **Google (Gemini)**, or any OpenAI-compatible custom provider (Groq, Together, Ollama, etc.)
- A modern browser — Chrome, Edge, Firefox, or Safari. No server, no build step.
- **Recommended:** Claude with web search enabled. PDF analysis, native document handling, and live web search all work best with Claude.

### First Launch

1. Open `index.html` in your browser (or visit the [live app](https://abhishek-sinha-bgl.github.io/MyResAssist/))
2. Click **Settings** (top right) and enter your API key
3. Select your provider tab (Claude / OpenAI / Gemini / Custom) and confirm the model
4. Toggle **Web Search** on for live internet access during research
5. Click **Save** and close Settings
6. Click **+ New Topic** to begin

---

## The Research Wall

Your home screen — a grid of all your research topics. Each card shows the topic title, a top claim preview, claim count, and an average confidence bar across all absorbed claims.

### Topic Cards

| Action | How |
|--------|-----|
| **Create** | Click the dashed "New Research Topic" card |
| **Open** | Click any card |
| **Menu (⋯)** | Hover a card to reveal options: Archive or Delete |
| **Archive** | Moves the topic to a collapsible "Archived (N)" section at the bottom of the wall |
| **Delete** | Removes immediately with a 6-second undo window |
| **Restore** | Expand the archived section → card menu → Restore |

### Search

The search bar searches across **all topic titles, claim text, and claim rationale** simultaneously. Results are grouped by topic with matches highlighted. Click any result to jump directly into that topic.

### Export & Import Backup

- **Export:** Settings → Data → ↓ Export backup. Downloads a complete JSON file with all topics, claims, source classifications, and archived state.
- **Import:** Settings → Data → ↑ Import backup. Merges new topics from a backup file without duplicating topics already present.

> **Storage note:** `localStorage` is capped at ~5 MB per browser origin. Typical usage (20–30 topics, 30 claims each) uses ~500–800 KB. Export backups periodically if you research intensively.

---

## The Topic View

Opening a topic splits the screen into two panels: **Claims** (left) and **Chat** (right). On mobile, a floating button opens the chat as a full-screen overlay.

### Claims Panel

Each claim card shows:
- Claim text and confidence classification (High / Medium / Low / Contradicted)
- A "Why it matters" rationale
- An age indicator for claims older than 30 days
- **Sources & evidence drawer** — click to expand for dual confidence display, source authority tier badges, and re-research actions

### Filter Chips

| Chip | Shows |
|------|-------|
| **All** | All claims in insertion or sort order |
| **High confidence** | Claims rated ≥70% confidence |
| **Contradictions** | Claims that contradict a previously absorbed claim — detected automatically at absorption time |
| **Uncertain** | Claims rated <35% confidence — anecdotal, forum-sourced, or model inference |
| **⚡ Key Points** | AI-generated research digest (see below) |

### ⚡ Key Points Digest

Selecting the Key Points filter generates a full **research digest** for the topic:

- **Research Digest paragraph** — summarises what is well-established, what is contested, and what the overall picture suggests, with references to specific claims by number
- **Research Gaps** — identifies what is missing, weakly sourced, or unresolved, and offers a direct button to research those gaps (see [Research Gaps](#research-gaps))
- **Claim groups** — Strongest findings / Supporting / Contradictions / Uncertain, each showing the top claims in that category

Claim numbers in the digest text are **clickable** — tapping a claim number switches to the All filter, scrolls to that exact card, and briefly highlights it.

The digest is generated once and cached per topic. It is automatically invalidated when new claims are absorbed.

### Sort

The **Sort** dropdown orders claims by:
- **Newest** — default insertion order
- **Highest confidence** — best-supported claims first
- **Lowest confidence** — use this to identify where to focus next research

### Staleness Banner

If any claim is older than 30 days, an amber banner appears above the claim list. Click **↺ Validate claims** to fire a targeted query asking whether those claims are still current. See [Staleness & Validation](#staleness--validation).

---

## The Chat Panel

Each conversation is per-session — closing and reopening a topic starts a fresh chat, though all absorbed claims persist.

### Input

| Element | Function |
|---------|----------|
| Text input | Type a research question and press Enter |
| **URL detection** | Paste a URL alone and press Enter → intent popup appears |
| **📎 Attach button** | Open a file picker for PDF, TXT, or MD files |
| **Drag and drop** | Drag any supported file onto the chat panel |
| **🎙 Voice** | Click to dictate your question |
| **Web search toggle** | Enable live internet access via the AI's built-in search tool |

### Chat Bubble Actions

Every AI response has an action bar (hover on desktop, always visible on mobile):

| Button | Action |
|--------|--------|
| **Copy** | Clean plain text — all markdown formatting stripped |
| **Copy MD** | Raw markdown, suitable for Notion, Obsidian, Bear, etc. |
| **↗ Share** | Native OS share sheet on mobile (WhatsApp, Mail, Notes, OneNote…). Falls back to clipboard copy on desktop. |

The Share button is the fastest path from a research finding to sharing it with colleagues — one tap from the analysis to any app on your phone.

### Research Action Buttons

| Button | Action |
|--------|--------|
| 🔬 **Research Deeper** | Multi-angle deep research prompt: core findings, supporting evidence, counterarguments, key implications, further angles to explore. Activates after the first message. |
| 📋 **Summarise** | Executive summary of the current session: summary paragraph, key findings as bullets, what remains uncertain. |
| ↓ **Export** | Downloads the topic's claims and conversation. A picker offers **Plain text (.txt)** for sharing with anyone, or **Markdown (.md)** for technical users. |

---

## Absorbing Claims

After each AI response, MyResAssist automatically extracts **2–4 key factual claims** and presents them in an Absorb panel below the response. Each proposed claim shows:

- The claim text and confidence rating
- A "Why it matters" rationale
- Source citations with authority tier classification

Use the checkboxes to select the claims worth keeping, then click **Save selected claims**. Contradictions with existing claims in the knowledge base are detected automatically at this stage.

> The claim extractor reads up to **6,000 characters** of the AI response, ensuring comprehensive extraction even from detailed URL analyses or multi-page document summaries.

---

## Source Intelligence

Every claim carries two independent confidence signals, both visible in the sources drawer:

| Signal | What it measures |
|--------|-----------------|
| **LLM Confidence** | The model's internal certainty rating, calibrated against the types of sources it cited |
| **Source Quality Score** | A weighted score derived from the actual authority tiers of cited URLs — independent of the model's opinion |

Having both matters: a model can be confidently wrong, and a well-sourced claim can still be overstated. Divergence between the two signals is itself informative.

### Source Authority Tiers

| Tier | Sources |
|------|---------|
| 🟢 **Primary** | `.gov`, `.edu`, `.ac.uk`, Reuters, AP, AFP, WHO, UN, IMF, World Bank, peer-reviewed journals. Also any domains you add to **Trusted Sources** in Settings. |
| 🔵 **Secondary** | Guardian, NYT, BBC, FT, Bloomberg, The Hindu, Al Jazeera, major think tanks |
| 🟡 **Tertiary** | Niche or lifestyle sites, attributed blogs, specialist forums |
| 🔴 **Weak** | No byline, SEO listicles, aggregators without primary citations |

Sources are classified first by a fast rule-based engine (~40 domain patterns). Ambiguous cases are resolved by a lightweight AI call. Results are cached — the same URL is never reclassified twice in a session.

### Find Better Sources

Each claim's sources drawer has a **↺ Find better sources** button. This fires a targeted re-research query instructing the AI to prioritise: peer-reviewed sources → government/official statistics → major newswires (Reuters, AP, AFP, Bloomberg) → established quality outlets, and to explicitly flag if a claim only has lower-tier backing.

---

## Analysing URLs & Documents

### URL Analysis

1. Paste a URL **by itself** into the chat input and press Enter
2. An intent popup appears with four options:

| Option | Does |
|--------|------|
| 📋 **Summarise** | Extract key points and the main argument |
| 🔍 **Extract & validate claims** | Pull out factual claims and assess their credibility |
| ⚖ **Find counter-arguments** | Identify what challenges or contradicts the content |
| ⚡ **Full analysis** | All three combined |

The AI retrieves the article, analyses it per your chosen intent, and presents findings in the chat with claims ready to absorb.

> URL retrieval depends on your AI provider's web access capability. Best results with Claude (web search enabled). Other providers may not retrieve all pages.

### PDF & Document Analysis

- Click the **📎 attach button** or **drag a file** onto the chat panel
- Accepts `.pdf`, `.txt`, and `.md` files
- The same intent popup appears — choose your analysis mode

> PDF analysis works natively with Claude (the document is sent directly to the API as a document block). For other providers, text is extracted and sent as a prompt — results may vary for complex layouts.

---

## Research Gaps

The ⚡ Key Points digest explicitly identifies what your knowledge base is missing — claims with weak sourcing, topics not yet researched, and questions the current evidence cannot answer.

A **🔍 Research these gaps →** button appears below the Research Gaps text. Clicking it:

1. Builds a targeted prompt quoting the exact gap text
2. Instructs the AI to prioritise peer-reviewed → official statistics → major newswires
3. Asks it to distinguish what can now be answered confidently from what remains genuinely uncertain
4. Fires it directly into the chat

The response generates new claims to absorb, closing the loop between identified gaps and new knowledge.

---

## Staleness & Validation

Research on fast-moving topics can become outdated. MyResAssist tracks when each claim was last validated and surfaces drift.

- **Age indicator** — claims older than 30 days show their age in the card header
- **Staleness banner** — an amber banner appears above the claims list when stale claims exist, showing a count
- **↺ Validate claims** — fires a query for up to 5 stale claims, asking the AI whether each is still accurate, has changed materially, or needs updating. Claims are marked as validated immediately. If the AI surfaces updated information, absorb the revised claims to replace the old ones.

---

## Trusted Sources

Not every authoritative source appears in the built-in classification rules. If you regularly rely on specialist publications, regional data portals, or industry databases that MyResAssist doesn't recognise, add them under **Settings → Trusted Sources**.

- Enter any URL or domain name (`epw.in`, `https://mospi.gov.in`, `data.rbi.org.in` — all normalised to clean hostname)
- Press Enter or click **+ Add**
- The domain appears in your list with a 🟢 **Primary** badge

**What changes when you add a trusted source:**

1. **Source classifier** — any URL from that domain (including subdomains) is immediately classified as Primary tier. The source quality score and confidence display on claim cards reflect this.

2. **Every AI prompt** — the system prompt tells the AI that you trust these sources and to prioritise them when relevant — in regular research, gap research, and "find better sources" queries.

Your trusted sources are saved with your other settings and persist across sessions.

---

## Themes

Four visual themes under **Settings → Appearance**:

| Theme | Character |
|-------|-----------|
| **Parchment** *(default)* | Warm cream background, charcoal ink. Lora serif headings. Designed for long reading sessions. |
| **Obsidian** | Near-black background, electric cyan accents. Outfit sans headings. High contrast for dark environments. |
| **Slate & Bone** | Cool off-white with blue-grey tones. IBM Plex Mono headings. Clean and minimal. |
| **Forest Codex** | Deep green background, aged gold accents. Crimson Pro italic headings. For those who prefer richness over utility. |

---

## AI Provider Configuration

All API keys are stored locally in your browser only — never sent anywhere except the provider's own API endpoint.

| Provider | Recommended model | Notes |
|----------|------------------|-------|
| **Claude** | `claude-sonnet-4-6` | Best overall. Native PDF support, real-time streaming, built-in web search. [Get key](https://console.anthropic.com) |
| **OpenAI** | `gpt-4o` | Full streaming support. [Get key](https://platform.openai.com) |
| **Gemini** | `gemini-2.0-flash` | Google Search grounding when web search is on. [Get key](https://aistudio.google.com) |
| **Custom** | Any OpenAI-compatible | Groq, Together, Ollama, LM Studio, etc. Enter endpoint URL, API key, and model name. Supports auto-fetch of available models from the provider's `/models` endpoint. |

Each first-party provider tab has a **↻ Fetch models** button that retrieves the current list of available models directly from the provider.

> **OSS / large model compatibility:** MyResAssist uses a robust five-step JSON parser that handles reasoning preambles, `<think>` tags, flat arrays, nested objects, and field-name aliases automatically — compatible with any OpenAI-format endpoint including local models via Ollama.

---

## Technical Notes

- **Single file** — the entire application is one HTML file (~120 KB). No dependencies, no build step, no server required. Download it and it works.
- **Storage** — all data (topics, claims, settings, trusted sources) is stored in `localStorage` under the key `myresassist_v1`. Nothing leaves your device except API calls to your chosen provider.
- **API calls** — made directly from the browser to provider APIs. Keys are never proxied through any intermediary.
- **Offline** — the app UI loads offline. Sending messages requires an active connection to reach your AI provider.
- **Data model** — topics contain claims; claims contain text, confidence, rationale, source citations with tier classifications, and timestamps. Exported backups are plain JSON and fully human-readable.
- **Privacy** — no analytics, no telemetry, no tracking of any kind. The only outbound connections are the API calls you initiate.

---

## Quick Reference

| Task | How |
|------|-----|
| Create a topic | Dashed card on the Research Wall |
| Search all research | Search bar below wall header |
| Archive a topic | Hover card → ⋯ → Archive |
| Analyse a URL | Paste URL alone in chat → Enter → choose intent |
| Analyse a PDF | Click 📎 or drag file onto chat panel → choose intent |
| Copy an AI response | Hover response → Copy |
| Share a response | Hover response → ↗ Share |
| Find better sources for a claim | Open claim drawer → ↺ Find better sources |
| Research identified gaps | ⚡ Key Points → 🔍 Research these gaps → |
| Jump to a claim from the digest | Click a claim number in the digest text |
| Validate old claims | Amber staleness banner → ↺ Validate claims |
| See research digest | Filter chips → ⚡ Key Points |
| Sort claims | Sort dropdown → Newest / Highest / Lowest |
| Export topic | ↓ Export → Plain text or Markdown |
| Back up all data | Settings → Data → ↓ Export backup |
| Restore a backup | Settings → Data → ↑ Import backup |
| Add a trusted source | Settings → Trusted Sources → enter domain → Add |
| Change theme | Settings → Appearance |

---

*Knowledge accumulates · Claims persist · Contradictions surface*
