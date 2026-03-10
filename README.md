# MyResAssist — Research Intelligence

**[→ Open Live App](https://abhishek-sinha-bgl.github.io/MyResAssist/)** · [Report a bug](https://github.com/abhishek-sinha-bgl/MyResAssist/issues)

---

<table>
<tr>
<td width="65%">
<img src="screenshots/desktop.png" alt="MyResAssist desktop view" width="100%">
</td>
<td width="35%">
<img src="screenshots/mobile.png" alt="MyResAssist mobile view" width="100%">
</td>
</tr>
<tr>
<td align="center"><em>Desktop — Topic view with claims and chat panel</em></td>
<td align="center"><em>Mobile — Key Points digest</em></td>
</tr>
</table>

---

Most AI chat is disposable. You ask, it answers, you close the tab and it's gone. The next time you research the same topic you start from zero — re-establishing context, re-finding sources, re-checking what you already knew.

**MyResAssist is built around a different idea.** Every research session contributes to a persistent knowledge base. Facts are extracted as discrete, citable *claims*. Each claim carries a confidence rating, a source with an authority tier, and a rationale for why it matters. Contradictions between claims are detected automatically. Gaps in the evidence are surfaced explicitly. Over time, your knowledge on a topic doesn't just accumulate — it self-organises, self-critiques, and tells you what it doesn't yet know.

It runs as a single HTML file in any modern browser. No installation. No account. No server. Just an API key for the AI provider of your choice.

---

## Contents

- [Getting Started](#getting-started)
- [The Research Wall](#the-research-wall)
- [The Topic View](#the-topic-view)
- [The Chat Panel](#the-chat-panel)
- [Absorbing Claims](#absorbing-claims)
- [Source Intelligence](#source-intelligence)
- [Analysing URLs & Documents](#analysing-urls--documents)
- [Validating Claims](#validating-claims)
- [Research Gaps](#research-gaps)
- [Staleness Detection](#staleness-detection)
- [Sharing Findings](#sharing-findings)
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

1. Open `index.html` in your browser, or visit the [live app](https://abhishek-sinha-bgl.github.io/MyResAssist/)
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
| **Menu (⋯)** | Hover a card to reveal Archive or Delete |
| **Archive** | Moves the topic to a collapsible "Archived (N)" section at the bottom of the wall |
| **Delete** | Removes immediately with a 6-second undo window |
| **Restore** | Expand the archived section → card menu → Restore |

### Search

The search bar searches across **all topic titles, claim text, and claim rationale** simultaneously. Results are grouped by topic with matches highlighted. Click any result to jump directly into that topic.

### Export & Import

- **Export:** Settings → Data → ↓ Export backup. Downloads a complete JSON file with all topics, claims, source classifications, and archived state.
- **Import:** Settings → Data → ↑ Import backup. Merges new topics from a backup file without duplicating existing ones.

> **Storage note:** `localStorage` is capped at ~5 MB per browser origin. Typical usage (20–30 topics, 30 claims each) uses ~500–800 KB. Export backups periodically if you research intensively.

---

## The Topic View

Opening a topic splits the screen into two panels: **Claims** (left) and **Chat** (right). On mobile, a floating **💬** button opens the chat as a full-screen overlay. Pressing the device back button from within a topic returns you to the Research Wall; pressing back again from the wall shows a confirmation before exiting the app.

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
| **Contradictions** | Claims that contradict a previously absorbed claim |
| **Uncertain** | Claims rated <35% confidence |
| **⚡ Key Points** | AI-generated research digest |

### ⚡ Key Points Digest

Selecting the Key Points filter generates a full research digest for the topic:

- **Research Digest** — summarises what is well-established, what is contested, and what the overall picture suggests. Claim numbers in the digest text are clickable — tapping a number switches to the All filter, scrolls to that exact card, and briefly highlights it.
- **Research Gaps** — identifies what is missing, weakly sourced, or unresolved, with a direct button to research those gaps
- **↗ Share digest** — shares the digest as plain text via the OS share sheet on mobile, or copies to clipboard on desktop
- **Claim groups** — Strongest findings / Supporting / Contradictions / Uncertain

The digest is generated once and cached per topic. It is automatically invalidated and regenerated when new claims are absorbed.

### Sort

The **Sort** dropdown orders claims by Newest (default), Highest confidence, or Lowest confidence.

### Staleness Banner

If any claim is older than 30 days, an amber banner appears above the claim list. Click **↺ Validate claims** to fire a targeted query checking whether those claims are still current.

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
| **↗ Share** | Native OS share sheet on mobile. Falls back to clipboard on desktop. |
| **↓ Extract claims** | Re-runs claim extraction on this response. If claims were already absorbed, shows "✓ Already absorbed". |

### Research Action Buttons

| Button | Action |
|--------|--------|
| 🔬 **Research Deeper** | Multi-angle deep research: core findings, counterarguments, key implications, further angles |
| 📋 **Summarise** | Executive summary of the current session |
| ↓ **Export** | Downloads topic claims and conversation as plain text or Markdown |

---

## Absorbing Claims

After each AI response, MyResAssist automatically extracts key factual claims and presents them in an **Absorb panel** below the response. Each proposed claim shows the claim text, confidence rating, rationale, and source citations.

**Duplicate detection** — claims already in your knowledge base are automatically identified, pre-unchecked, and labelled "↩ Already in knowledge base." You can still re-check and absorb them if needed.

Use the checkboxes to select the claims worth keeping, then click **Save N claims**. Contradictions with existing claims are detected automatically at this stage.

---

## Source Intelligence

Every claim carries two independent confidence signals, both visible in the sources drawer:

| Signal | What it measures |
|--------|-----------------|
| **LLM Confidence** | The model's internal certainty rating, calibrated against the types of sources it cited |
| **Source Quality Score** | A weighted score derived from the actual authority tiers of cited URLs — independent of the model's opinion |

### Source Authority Tiers

| Tier | Sources |
|------|---------|
| 🟢 **Primary** | `.gov`, `.edu`, `.ac.uk`, Reuters, AP, AFP, WHO, UN, IMF, World Bank, peer-reviewed journals. Also any domains you add to Trusted Sources. |
| 🔵 **Secondary** | Guardian, NYT, BBC, FT, Bloomberg, The Hindu, Al Jazeera, major think tanks |
| 🟡 **Tertiary** | Niche or lifestyle sites, attributed blogs, specialist forums |
| 🔴 **Weak** | No byline, SEO listicles, aggregators without primary citations |

Sources are classified first by a fast rule-based engine (~40 domain patterns). Ambiguous cases are resolved by a lightweight AI call. Results are cached per session.

---

## Analysing URLs & Documents

### URL Analysis

Paste a URL alone into the chat input and press Enter. An intent popup offers four options:

| Option | Does |
|--------|------|
| 📋 **Summarise** | Extract key points and the main argument |
| 🔍 **Extract & validate claims** | Pull out factual claims and assess their credibility |
| ⚖ **Find counter-arguments** | Identify what challenges or contradicts the content |
| ⚡ **Full analysis** | All three combined |

### PDF & Document Analysis

Click the **📎 attach button** or drag a file onto the chat panel. Accepts `.pdf`, `.txt`, and `.md` files. The same intent popup appears.

> PDF analysis works natively with Claude (sent as a document block). For other providers, text is extracted and sent as a prompt — results may vary for complex layouts.

---

## Validating Claims

Each claim card has a **⊕ Validate** button that opens a focused validation popup with four modes:

| Mode | Does |
|------|------|
| 🟢 **Find credible sources** | Searches for primary sources bearing on this specific claim |
| 📊 **Research supporting data** | Finds statistics and empirical data supporting the claim |
| ⚖ **Find contrary viewpoints** | Surfaces counter-arguments and contradictions |
| ⚡ **Full validation** | All three combined with a synthesis |

On mobile, selecting a validation mode automatically opens the chat panel before firing the query.

---

## Research Gaps

The ⚡ Key Points digest identifies what your knowledge base is missing — claims with weak sourcing, topics not yet researched, and questions the current evidence cannot answer.

A **🔍 Research these gaps →** button appears below the Research Gaps text. Clicking it builds a targeted prompt quoting the exact gap text, instructs the AI to prioritise peer-reviewed → official statistics → major newswires, and fires it directly into the chat. The response generates new claims to absorb, closing the loop between identified gaps and new knowledge.

---

## Staleness Detection

MyResAssist tracks when each claim was last validated and surfaces drift:

- **Age indicator** — claims older than 30 days show their age in the card header
- **Staleness banner** — an amber banner appears above the claims list when stale claims exist
- **↺ Validate claims** — fires a query for up to 5 stale claims, asking the AI whether each is still accurate or has changed materially. Claims are marked as validated immediately. Absorb any updated findings to replace the old ones.

---

## Sharing Findings

| What | Where | How |
|------|-------|-----|
| **A single AI response** | Chat bubble action bar | ↗ Share — native share sheet or clipboard |
| **Research digest** | ⚡ Key Points filter | ↗ Share digest button in the digest header |
| **Full topic export** | Chat action buttons | ↓ Export → Plain text or Markdown |

---

## Trusted Sources

Add specialist publications, regional data portals, or industry databases under **Settings → Trusted Sources**:

- Enter any URL or domain (`epw.in`, `mospi.gov.in`, `data.rbi.org.in` — all normalised automatically)
- Press Enter or click **+ Add**
- The domain appears with a 🟢 Primary badge

**What changes when you add a trusted source:**
1. Any URL from that domain (including subdomains) is immediately classified as Primary tier
2. Every AI prompt instructs the model to prioritise these sources when relevant

---

## Themes

Four visual themes under **Settings → Appearance**:

| Theme | Character |
|-------|-----------|
| **Parchment** *(default)* | Warm cream, charcoal ink. Lora serif headings. Designed for long reading sessions. |
| **Obsidian** | Near-black, electric cyan accents. Outfit sans headings. High contrast for dark environments. |
| **Slate & Bone** | Cool off-white, blue-grey tones. IBM Plex Mono headings. Clean and minimal. |
| **Forest Codex** | Deep green, aged gold accents. Crimson Pro italic headings. Rich and distinctive. |

---

## AI Provider Configuration

All API keys are stored locally in your browser only — never sent anywhere except the provider's own API endpoint.

| Provider | Recommended model | Notes |
|----------|------------------|-------|
| **Claude** | `claude-sonnet-4-6` | Best overall. Native PDF support, streaming, built-in web search. [Get key](https://console.anthropic.com) |
| **OpenAI** | `gpt-4o` | Full streaming support. [Get key](https://platform.openai.com) |
| **Gemini** | `gemini-2.0-flash` | Google Search grounding when web search is on. [Get key](https://aistudio.google.com) |
| **Custom** | Any OpenAI-compatible | Groq, Together, Ollama, LM Studio, etc. Enter endpoint URL, key, and model name. Auto-fetch of available models supported. |

> **OSS / large model compatibility:** MyResAssist uses a robust five-step JSON parser that handles reasoning preambles, `<think>` tags, flat arrays, nested objects, and field-name aliases — compatible with any OpenAI-format endpoint including local models via Ollama.

---

## Technical Notes

- **Single file** — the entire application is one HTML file (~130 KB). No dependencies, no build step, no server required.
- **Storage** — all data is stored in `localStorage` under the key `myresassist_v1`. Nothing leaves your device except API calls to your chosen provider.
- **API calls** — made directly from the browser to provider APIs. Keys are never proxied through any intermediary.
- **Offline** — the app UI loads offline. Sending messages requires a connection to your AI provider.
- **Data model** — topics contain claims; claims contain text, confidence, rationale, source citations with tier classifications, and timestamps. Exported backups are plain JSON and fully human-readable.
- **Privacy** — no analytics, no telemetry, no tracking of any kind.

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
| Share the research digest | ⚡ Key Points → ↗ Share digest |
| Validate a specific claim | Claim card → ⊕ Validate → choose mode |
| Find better sources for a claim | Open claim drawer → ↺ Find better sources |
| Research identified gaps | ⚡ Key Points → 🔍 Research these gaps → |
| Jump to a claim from the digest | Click a claim number in the digest text |
| Validate old claims | Amber staleness banner → ↺ Validate claims |
| Sort claims | Sort dropdown → Newest / Highest / Lowest |
| Export topic | ↓ Export → Plain text or Markdown |
| Back up all data | Settings → Data → ↓ Export backup |
| Restore a backup | Settings → Data → ↑ Import backup |
| Add a trusted source | Settings → Trusted Sources → enter domain → Add |
| Change theme | Settings → Appearance |

---

*Knowledge accumulates · Claims persist · Contradictions surface*
