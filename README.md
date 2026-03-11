# MyResAssist — Research Intelligence

**[→ Open Live App](https://abhishek-sinha-bgl.github.io/MyResAssist/)** · [Report a bug](https://github.com/abhishek-sinha-bgl/MyResAssist/issues)

---

<!-- SCREENSHOTS: Add a /screenshots folder to the repo root with these two files -->
<table>
<tr>
<td width="62%">
<img src="screenshots/desktop.png" alt="MyResAssist desktop — topic view with claims and research map" width="100%">
</td>
<td width="38%">
<img src="screenshots/mobile.png" alt="MyResAssist mobile — Key Points digest" width="100%">
</td>
</tr>
<tr>
<td align="center"><em>Desktop — claims panel, chat, and Research Map</em></td>
<td align="center"><em>Mobile — Key Points digest with follow-on chips</em></td>
</tr>
</table>

---

Most AI chat is disposable. You ask, it answers, you close the tab and it's gone. The next time you research the same topic you start from zero.

**MyResAssist is built around a different idea.** Every research session builds a persistent, structured knowledge base. Facts are extracted as discrete, citable *claims*. Each claim carries a confidence rating, a source with an authority tier, and a rationale for why it matters. Contradictions between claims are detected automatically. Gaps in the evidence are surfaced explicitly. Each follow-on question is answered in the context of what you already know — the model builds on accumulated research rather than restating it.

Runs as a single HTML file in any modern browser. No installation. No account. No server.

---

## Contents

- [Getting Started](#getting-started)
- [The Research Wall](#the-research-wall)
- [The Topic View](#the-topic-view)
- [How Research Works](#how-research-works)
- [Absorbing Claims](#absorbing-claims)
- [Contradictions](#contradictions)
- [The Research Map](#the-research-map)
- [Source Intelligence](#source-intelligence)
- [Analysing URLs & Documents](#analysing-urls--documents)
- [Validating Claims](#validating-claims)
- [Key Points Digest](#key-points-digest)
- [Research Gaps](#research-gaps)
- [Staleness Detection](#staleness-detection)
- [Sharing Findings](#sharing-findings)
- [Trusted Sources](#trusted-sources)
- [AI Provider Configuration](#ai-provider-configuration)
- [Themes](#themes)
- [Technical Notes](#technical-notes)
- [Quick Reference](#quick-reference)

---

## Getting Started

### Prerequisites

- An API key from one of: **Anthropic (Claude)**, **OpenAI**, **Google (Gemini)**, or any OpenAI-compatible custom provider (Groq, Cerebras, Together, Ollama, etc.)
- A modern browser — Chrome, Edge, Firefox, or Safari
- No server, no build step, no dependencies

### First Launch

1. Open `index.html` in your browser, or visit the [live app](https://abhishek-sinha-bgl.github.io/MyResAssist/)
2. Click **Settings** (top right) and enter your API key
3. Select your provider tab and confirm the model
4. Toggle **Web Search** on for live internet access (Claude with web search tool)
5. Click **Save** and close Settings
6. Click **+ New Topic** to begin

> **Name your topic as a thesis, not a subject area.** "Why AI impact is hard to model" gives focused, contradictable research. "AI economics" gives scatter. The new-topic dialog shows examples.

---

## The Research Wall

Your home screen — a grid of all your research topics. Each card shows the topic title, a top claim preview, claim count, average confidence bar, and contradiction count.

| Action | How |
|--------|-----|
| **Create** | Click the dashed card or **+ New Topic** |
| **Open** | Click any topic card |
| **Search** | Search bar below the header — searches titles and claim text |
| **Archive** | Hover card → ⋯ → Archive |
| **Delete** | Hover card → ⋯ → Delete |

---

## The Topic View

Split into two panels:

**Left — Claims panel.** Your growing knowledge base. Each claim card shows:
- Numbered header and confidence tier (High / Medium / Low / Contradiction)
- Claim text and one-sentence rationale
- Source citations with authority tier badges
- Expandable drawer with full source details and re-research options
- Age indicator if the claim is over 30 days old

**Right — Chat panel.** The research interface. Ask questions, paste URLs, attach PDFs. Responses are structured and conditioned on your existing knowledge base.

On mobile, the 💬 button opens the chat panel as a slide-up sheet.

---

## How Research Works

Every message — including follow-on questions and validations — is sent with your current knowledge base injected into the system context. The model:

1. Sees what claims are already in your KB, with confidence ratings
2. Is instructed to find **new** findings that the KB doesn't already cover
3. Produces a **self-critiqued** response: identifies its two weakest claims and rewrites them with epistemic hedging before responding
4. Appends a structured JSON block with new points, contradictions, and follow-on questions

The first message on a new topic produces 5 points and 3 contradictions. Each subsequent message produces 3 new points and up to 2 contradictions specifically targeting gaps in the existing KB.

**Parser robustness:** the structured JSON block is extracted via marker-based detection first (RESEARCH\_JSON\_START/END), then fence stripping, balanced-brace recovery, partial regex extraction for truncated responses (common with token-limited models like Cerebras), and finally prose-section extraction for models like Gemini and Groq that return rich prose with contradiction headers but no JSON.

---

## Absorbing Claims

After each AI response, an **↓ Absorb into knowledge base** panel appears:

- **✓ checked** — will be saved; uncheck any you don't want
- **↩ Already in knowledge base** — duplicates detected via word-overlap, pre-unchecked
- **◆ Contradiction — will be flagged** — carries a contradiction flag set directly from the structured response
- **Absorb selected** saves to your KB; **Skip** dismisses without saving
- **↓ Extract claims** on any previous response re-runs extraction if you skipped

---

## Contradictions

Contradictions are first-class objects. They are:

- **Extracted directly** from the structured JSON block when the model identifies genuine opposing evidence
- **Recovered from prose** when models use section headers like "Contradictory Perspectives" or "Conflicting Views"
- **Partially recovered** from truncated responses via field-level regex
- **Displayed distinctly** on claim cards with a ⚠ Contradiction badge and the opposing text inline
- **Visualised** on the Research Map in a dedicated sector (purple diamonds)
- **Counted** on the topic card and in the filter bar

Filter to contradictions only with the **Contradictions** filter chip.

---

## The Research Map

Click **◎ Map** in the claims toolbar.

The map plots all claims in a polar layout on a high-contrast dark background:

- **Radial distance** = confidence. High-confidence claims cluster at the centre; uncertain ones push outward.
- **Colour** = cyan (≥70%), amber (35–69%), red (<35%), purple (contradictions)
- **Shape** = circles for claims, diamonds for contradictions
- **Angular zone** = contradictions isolated in a dedicated 60° sector, labelled CONTRA

**Controls:**

| Action | How |
|--------|-----|
| See claim detail | Hover node (tooltip) |
| Jump to claim card | Click node |
| Maximise | ⤢ button — expands to near-fullscreen, re-renders at larger size |
| Zoom in/out | +/− buttons, scroll wheel (desktop), or pinch (mobile/trackpad) |
| Reset zoom | Reset button |
| Tap on mobile | Single tap shows tooltip with "→ Jump to claim" link |

The force-spread algorithm runs 3 passes pushing overlapping nodes apart, preventing cluster collapse with dense claim sets.

---

## Source Intelligence

Every cited URL is automatically classified into four authority tiers:

| Tier | Badge | Examples |
|------|-------|---------|
| Primary | 🟢 | Peer-reviewed journals, government statistics, central banks, UN/IMF/World Bank |
| Secondary | 🔵 | Major newswires (Reuters, AP, AFP, Bloomberg), established broadsheets |
| Tertiary | 🟡 | Industry reports, think tanks, specialist publications |
| Weak | 🔴 | Blogs, forums, unverified sources |

Classification uses ~40 domain pattern rules, your trusted sources list, and an LLM fallback pass. Results are cached per URL.

---

## Analysing URLs & Documents

**URLs:** Paste a URL alone in the chat and press Enter. Choose from:
- **Summarise** — key argument and main facts
- **Extract claims** — full claims extraction pipeline
- **Find counter-arguments** — what challenges the content
- **Full analysis** — all three in sequence

**PDFs:** Click 📎 or drag a file onto the chat panel. Same intent options. Claude handles PDFs natively; other providers receive extracted text.

---

## Validating Claims

Click **⊕ Validate** on any claim card. Fires immediately — no mode-selection popup — with a structured query covering:

1. Credibility: most authoritative sources bearing on the claim
2. Supporting data: statistics and empirical findings
3. Contrary evidence: counter-arguments, contradictions, important caveats
4. Verdict: well-supported, contested, or unsupported

Runs through the full pipeline: new claims and contradictions are offered for absorption. Your KB grows from validation just as it does from initial research.

---

## Key Points Digest

Click **⚡ Key Points** to generate an AI summary of your knowledge base:
- Overview paragraph synthesising strongest findings
- Top high-confidence claims
- Medium-confidence supporting claims
- Contradictions requiring resolution
- Identified research gaps

**↗ Share digest** copies to clipboard or opens the native share sheet on mobile.

Claim numbers in the digest are clickable — they scroll to and highlight the corresponding claim card.

---

## Research Gaps

From the Key Points digest, click **🔍 Research these gaps →** to fire a targeted query on the specific weaknesses and unanswered questions the digest identified. Runs through the full structured pipeline, generating new claims conditioned on your existing KB.

---

## Staleness Detection

Claims older than 30 days show a timestamp. An amber banner appears at the top of the claims panel with **↺ Validate stale claims**.

---

## Sharing Findings

| What | How |
|------|-----|
| Share an AI response | Hover response → ↗ Share |
| Share the Key Points digest | ⚡ Key Points → ↗ Share digest |
| Export a topic | Claims panel → ↓ Export (plain text or Markdown) |
| Export all data | Settings → Data → ↓ Export backup (JSON) |
| Import a backup | Settings → Data → ↑ Import backup |

---

## Trusted Sources

Add specialist publications or domain databases under **Settings → Trusted Sources**.

Enter any URL or domain (`epw.in`, `mospi.gov.in`, `data.rbi.org.in` — normalised automatically) and press **+ Add**. The domain is immediately classified as 🟢 Primary, and every AI prompt instructs the model to prioritise these sources.

---

## AI Provider Configuration

All API keys are stored locally — never proxied.

| Provider | Recommended model | Notes |
|----------|------------------|-------|
| **Claude** | `claude-sonnet-4-6` | Best overall. Native PDF, streaming, built-in web search. [Get key](https://console.anthropic.com) |
| **OpenAI** | `gpt-4o` | Full streaming. [Get key](https://platform.openai.com) |
| **Gemini** | `gemini-2.0-flash` | Fast. Returns rich prose; contradictions extracted from section headers. [Get key](https://aistudio.google.com) |
| **Custom** | Any OpenAI-compatible | Groq, Cerebras, Together, Ollama, LM Studio, etc. Auto-fetch of available models supported. |

---

## Themes

| Theme | Character |
|-------|-----------| 
| **Parchment** *(default)* | Warm cream, charcoal ink. Lora serif. |
| **Obsidian** | Near-black, electric cyan. Outfit sans. |
| **Slate & Bone** | Cool off-white, blue-grey. IBM Plex Mono. |
| **Forest Codex** | Deep green, aged gold. Crimson Pro italic. |

The Research Map always renders in high-contrast dark mode regardless of theme.

---

## Technical Notes

- **Single file** — entire application is one HTML file (~135 KB). No dependencies, no build step, no server.
- **Storage** — `localStorage` key `myresassist_v1`. Nothing leaves your device except API calls.
- **API calls** — made directly from the browser to provider APIs. Keys never proxied.
- **Offline** — UI loads offline. Sending messages requires a provider connection.
- **Mobile** — fully functional on iOS and Android. Chat opens as a slide-up sheet. Research Map supports pinch-to-zoom and tap-for-tooltip. All actions work on touch.
- **Privacy** — no analytics, no telemetry, no tracking.

---

## Quick Reference

| Task | How |
|------|-----|
| Create a topic | Dashed card on Research Wall, or **+ New Topic** |
| Search research | Search bar below wall header |
| Archive a topic | Hover card → ⋯ → Archive |
| Analyse a URL | Paste URL alone in chat → Enter → choose intent |
| Analyse a PDF | Click 📎 or drag file onto chat panel |
| Copy an AI response | Hover response → Copy |
| Share an AI response | Hover response → ↗ Share |
| Open Research Map | **◎ Map** in claims toolbar |
| Maximise Research Map | **⤢** button in map header |
| Zoom Research Map | +/− buttons, scroll wheel, or pinch |
| Validate a claim | Claim card → **⊕ Validate** |
| Find better sources | Open claim drawer → ↺ Find better sources |
| View Key Points digest | **⚡ Key Points** |
| Share research digest | ⚡ Key Points → **↗ Share digest** |
| Research identified gaps | ⚡ Key Points → **🔍 Research these gaps →** |
| Jump to a claim from digest | Click the claim number in digest text |
| Filter contradictions | **Contradictions** chip in toolbar |
| Validate stale claims | Amber banner → **↺ Validate stale claims** |
| Sort claims | Sort dropdown — Newest / Highest / Lowest |
| Export topic | Claims panel → **↓ Export** |
| Back up all data | Settings → Data → **↓ Export backup** |
| Restore a backup | Settings → Data → **↑ Import backup** |
| Add a trusted source | Settings → Trusted Sources → enter domain → **+ Add** |
| Change theme | Settings → Appearance |

---

*Knowledge accumulates · Claims persist · Contradictions surface · Research deepens*
