# MyResAssist — Research Intelligence

> A single-file research intelligence application that runs entirely in your browser. Accumulate claims, track confidence, surface contradictions, and build a persistent knowledge base that deepens with every conversation. No installation. No account. Just an API key.

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
- [Staleness & Validation](#staleness--validation)
- [Themes](#themes)
- [AI Provider Configuration](#ai-provider-configuration)
- [Technical Notes](#technical-notes)
- [Quick Reference](#quick-reference)

---

## Getting Started

### Prerequisites

- An API key from one of: **Anthropic (Claude)**, **OpenAI**, **Google (Gemini)**, or any OpenAI-compatible custom provider (Groq, Together, Ollama, etc.)
- A modern browser — Chrome, Edge, Firefox, or Safari. No server, no build step.
- **Recommended:** Claude with web search enabled. PDF analysis and web search integration work best with Claude.

### First Launch

1. Open `index.html` in your browser (or visit the [live app](https://abhishek-sinha-bgl.github.io/MyResAssist/))
2. Click **Settings** (top right) and enter your API key
3. Select your provider tab (Claude / OpenAI / Gemini / Custom) and confirm the model
4. Toggle **Web Search** on if you want live internet access during research
5. Click **Save** and close Settings
6. Click **+ New Topic** to begin

---

## The Research Wall

Your home screen — a grid of all research topics. Each card shows the topic title, a top claim preview, claim count, and an average confidence bar.

### Topic Cards

| Action | How |
|--------|-----|
| **Create** | Click the dashed "New Research Topic" card |
| **Open** | Click any card |
| **Menu (⋯)** | Hover a card to reveal options: Archive or Delete |
| **Archive** | Moves the topic to a collapsible "Archived (N)" section at the bottom |
| **Delete** | Removes immediately with a 6-second undo window |
| **Restore** | Expand archived section → card menu → Restore |

### Search

The search bar below the wall header searches across **all topic titles, claim text, and claim rationale** simultaneously. Results are grouped by topic with matches highlighted. Click any result to jump directly into that topic.

### Export & Import Backup

- **Export:** Settings → Data → ↓ Export backup. Downloads a JSON file with all topics, claims, source classifications, and archived state.
- **Import:** Settings → Data → ↑ Import backup. Merges new topics from a backup file. Topics already present (by ID) are not duplicated.

> **Storage note:** `localStorage` is capped at ~5 MB per browser origin. Typical usage (20–30 topics, 30 claims each) uses ~500–800 KB. Export backups periodically if you research intensively.

---

## The Topic View

Opening a topic splits the screen into two panels: **Claims** (left) and **Chat** (right). On mobile, a floating button opens the chat as a full-screen overlay.

### Claims Panel

Each claim card shows:
- Claim text and confidence classification (High / Medium / Low / Contradicted)
- A "Why it matters" rationale
- An age indicator for claims older than 30 days
- **Sources & evidence drawer** — click to expand for dual confidence display, source authority tier badges, and action buttons

### Filter Chips

| Chip | Shows |
|------|-------|
| **All** | All claims in insertion or sort order |
| **High confidence** | Claims rated ≥70% confidence |
| **Contradictions** | Claims that contradict a previously absorbed claim (detected automatically) |
| **Uncertain** | Claims rated <35% confidence — anecdotal, forum-sourced, or model inference |
| **⚡ Key Points** | AI-generated digest (see below) |

### ⚡ Key Points Digest

The Key Points panel opens with an **AI-generated research digest**: a paragraph summarising what is well-established, what is contested, and what the overall picture suggests — followed by a **Research gaps** note identifying what is missing, weakly sourced, or unresolved. Below the digest, claims are grouped into: Strongest findings / Supporting / Contradictions / Uncertain.

The digest is generated once and cached per topic. It is automatically invalidated when new claims are absorbed.

### Sort

The **Sort** dropdown (top right of the toolbar) orders claims by:
- **Newest** — default insertion order
- **Highest confidence** — best-supported claims first
- **Lowest confidence** — use this to identify where to focus next

### Staleness Banner

If any claim is older than 30 days, an amber banner appears above the claim list. Click **↺ Validate claims** to fire a targeted query asking the AI whether those specific claims are still accurate. Claims are marked as validated and the banner clears.

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
| **Copy** | Clean plain text — all markdown stripped |
| **Copy MD** | Raw markdown, suitable for Notion, Obsidian, etc. |
| **↗ Share** | Native OS share sheet on mobile (WhatsApp, Mail, Notes, OneNote…). Falls back to clipboard copy on desktop. |

### Research Action Buttons

| Button | Action |
|--------|--------|
| 🔬 **Research Deeper** | Multi-angle deep research: core findings, supporting evidence, counterarguments, key implications, further angles. Activates after the first message. |
| 📋 **Summarise** | Executive summary of the session: summary paragraph, key findings as bullets, what remains uncertain. |
| ↓ **Export** | Downloads topic claims + conversation as **Plain text (.txt)** or **Markdown (.md)**. |

---

## Absorbing Claims

After each AI response, MyResAssist automatically extracts **2–4 key factual claims** and presents them in an Absorb panel below the response. Each proposed claim shows:

- The claim text and confidence rating
- A "Why it matters" rationale
- Source citations classified by authority tier

Use the checkboxes to select which claims to keep, then click **Save selected claims**. Contradictions with existing claims are detected automatically.

> The claim extractor reads up to **6,000 characters** of the AI response, ensuring comprehensive extraction even from detailed URL or document analyses.

---

## Source Intelligence

Every claim has two independent confidence signals displayed in the sources drawer:

| Signal | Description |
|--------|-------------|
| **LLM Confidence** | The model's internal certainty, calibrated against the types of sources it cited |
| **Source Quality** | A weighted score derived from the actual authority tiers of cited sources, independent of model opinion |

### Source Authority Tiers

| Tier | Sources |
|------|---------|
| 🟢 **Primary** | `.gov`, `.edu`, `.ac.uk`, Reuters, AP, AFP, WHO, UN, IMF, World Bank, peer-reviewed journals |
| 🔵 **Secondary** | Guardian, NYT, BBC, FT, Bloomberg, The Hindu, Al Jazeera, major think tanks |
| 🟡 **Tertiary** | Niche or lifestyle sites, attributed blogs, specialist forums |
| 🔴 **Weak** | No byline, SEO listicles, aggregators without primary citations |

Sources are classified first by a fast rule-based engine (~40 domain patterns), then ambiguous cases by a lightweight AI call. Results are cached — the same URL is never reclassified twice.

### Find Better Sources

Each claim's sources drawer has a **↺ Find better sources** button. This fires a targeted query instructing the AI to prioritise: peer-reviewed sources → government/official statistics → major newswires (Reuters, AP, AFP, Bloomberg) → established quality outlets, and to flag if a claim only has lower-tier backing.

---

## Analysing URLs & Documents

### URL Analysis

1. Paste a URL **by itself** into the chat input and press Enter
2. An intent popup appears with four options:

| Option | Does |
|--------|------|
| 📋 **Summarise** | Extract key points and the main argument |
| 🔍 **Extract & validate claims** | Pull out factual claims and assess credibility |
| ⚖ **Find counter-arguments** | Identify what challenges or contradicts the content |
| ⚡ **Full analysis** | All three combined |

> URL retrieval depends on your AI provider's web access capability. Best results with Claude (web search enabled). Other providers may not retrieve all pages.

### PDF & Document Analysis

- Click the **📎 attach button** or **drag a file** onto the chat panel
- Accepts `.pdf`, `.txt`, and `.md` files
- The same intent popup appears — choose your analysis mode

> PDF analysis works natively with Claude (document sent directly to the API). For other providers, text content is extracted and sent as a prompt — results may vary.

---

## Staleness & Validation

Research on fast-moving topics can become outdated. MyResAssist tracks claim age and surfaces drift.

- **Age indicator** — claims older than 30 days show their age in the card header
- **Staleness banner** — amber banner appears when stale claims exist
- **↺ Validate claims** — fires a query for up to 5 stale claims asking whether each is still accurate, has changed, or needs updating. If the AI surfaces updates, absorb the revised claims to replace the old ones.

---

## Themes

Four visual themes under **Settings → Appearance**:

| Theme | Character |
|-------|-----------|
| **Parchment** *(default)* | Warm cream background, charcoal ink, Lora serif headings |
| **Obsidian** | Near-black background, electric cyan accents, Outfit sans headings |
| **Slate & Bone** | Cool off-white, blue-grey tones, IBM Plex Mono headings |
| **Forest Codex** | Deep green background, aged gold accents, Crimson Pro italic headings |

---

## AI Provider Configuration

All keys are stored locally in your browser only — never sent anywhere except the provider's own API.

| Provider | Recommended model | Notes |
|----------|------------------|-------|
| **Claude** | `claude-sonnet-4-6` | Best overall. Native PDF support, streaming, web search. [Get key](https://console.anthropic.com) |
| **OpenAI** | `gpt-4o` | Full streaming. [Get key](https://platform.openai.com) |
| **Gemini** | `gemini-2.0-flash` | Google Search grounding. [Get key](https://aistudio.google.com) |
| **Custom** | Any OpenAI-compatible | Groq, Together, Ollama, etc. Enter endpoint, key, and model. Supports auto-fetch of available models. |

> **OSS / large model compatibility:** MyResAssist uses a robust five-step JSON parser that handles reasoning preambles, `<think>` tags, flat arrays, nested objects, and field-name aliases — compatible with any OpenAI-format endpoint.

---

## Technical Notes

- **Single file** — the entire application is one HTML file (~120 KB). No dependencies, no build step, no server.
- **Storage** — all data stored in `localStorage` under `myresassist_v1`. Nothing leaves your device except API calls to your provider.
- **API calls** — made directly from the browser. Keys are never proxied through any intermediary.
- **Offline** — the UI loads offline. Sending messages requires an internet connection.
- **Data model** — topics contain claims; claims contain text, confidence, rationale, source citations with tier classifications, and timestamps. Exported backups are plain JSON.

---

## Quick Reference

| Task | How |
|------|-----|
| Create a topic | Dashed card on the Research Wall |
| Search all research | Search bar below wall header |
| Archive a topic | Hover card → ⋯ → Archive |
| Analyse a URL | Paste URL alone in chat → Enter → choose intent |
| Analyse a PDF | Click 📎 or drag onto chat panel → choose intent |
| Copy an AI response | Hover response → Copy |
| Share a response | Hover response → ↗ Share |
| Find better sources | Open claim drawer → ↺ Find better sources |
| Validate old claims | Amber banner → ↺ Validate claims |
| See research digest | Filter chips → ⚡ Key Points |
| Sort claims | Sort dropdown → Newest / Highest / Lowest |
| Export topic | ↓ Export → Plain text or Markdown |
| Back up all data | Settings → Data → ↓ Export backup |
| Restore a backup | Settings → Data → ↑ Import backup |
| Change theme | Settings → Appearance |

---

*MyResAssist — open `index.html` and start researching.*
