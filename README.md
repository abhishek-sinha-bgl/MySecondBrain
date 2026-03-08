# MyResAssist

**Research intelligence. Knowledge you can trust.**

[Live Demo](https://abhishek-sinha-bgl.github.io/MyResAssist/) · Single HTML file · No backend · No build tools · BYOK

---

## What it is

MyResAssist is a personal research assistant that treats knowledge as something worth keeping — and worth questioning. Unlike a chat interface where conversations disappear, MyResAssist extracts claims from every response, classifies the sources behind them, and builds a persistent knowledge base where every finding is traceable to its evidence.

The unit of value is a **claim**. The measure of a claim is its **evidence chain**.

---

## The problem it solves

A typical web search today returns SEO-optimised news stories, travel blogs, and listicles — content engineered to rank, not to inform. AI models trained on this content inherit its biases and will confidently cite weak sources. MyResAssist doesn't just surface findings; it makes the evidentiary basis of each finding visible so you can decide whether to trust it.

---

## How it works

**The Research Wall** is your home screen — a grid of topics you're actively researching, each showing its strongest finding, an average confidence level, and a flag if any claims contradict each other.

**Inside a topic**, your extracted knowledge lives on the left as a list of claims. Each claim carries two independent quality signals and a full source evidence chain. The research chat is on the right — a tool you reach for to dig deeper.

**After every AI response**, MyResAssist automatically extracts 2–4 key claims and presents them in an absorb panel. Before anything is saved, sources are classified through a two-pass pipeline. You choose which claims are worth keeping. If a new claim contradicts something you already know, it's flagged immediately.

---

## Source intelligence

This is the core of what makes MyResAssist different from a note-taking tool layered on top of a chat interface.

### Two confidence signals per claim

Every claim displays two independent scores side by side:

**LLM confidence** — the model's internal certainty about the claim, calibrated during extraction against the type of sources visible in the response. A claim explicitly backed by a named government dataset or peer-reviewed study scores higher than one inferred from a travel forum.

**Source quality** — derived from the actual sources retrieved, classified by authority tier. This is independent of what the model thinks. A model can be confidently wrong; source quality catches that.

The two signals can diverge, and that divergence is meaningful. High LLM confidence with low source quality is a warning sign. Low LLM confidence with strong primary sources means the claim deserves more research, not dismissal.

### Source authority tiers

Sources are classified into four tiers using a hybrid pipeline:

| Tier | Colour | What qualifies |
|---|---|---|
| **Primary** | 🟢 Green | Government agencies, academic institutions (`.gov`, `.edu`, `.ac.uk`), major newswires (Reuters, AP, AFP), intergovernmental bodies (WHO, UN, IMF, World Bank), peer-reviewed journals |
| **Secondary** | 🔵 Blue | Established newspapers and broadcasters (Guardian, NYT, BBC, FT, Bloomberg, The Hindu, Al Jazeera), named industry research bodies, think tanks |
| **Tertiary** | 🟡 Amber | Specialised or niche publications, travel and lifestyle sites, attributed blogs, forums with named contributors |
| **Weak** | 🔴 Red | Content without a byline, SEO-optimised listicles, aggregators with no primary citations |

### Hybrid classification pipeline

Classification happens in two passes before any claim is saved:

**Pass 1 — Rule-based (instant, free).** The source URL is matched against a pattern library covering ~40 domains and domain patterns. Known primary and secondary sources are classified in milliseconds with no API call.

**Pass 2 — LLM classification for ambiguous sources.** Anything not matched by rules — niche publications, unfamiliar domains, regional outlets — is sent to a fast model (Haiku / GPT-4o-mini / Gemini Flash) in a single batch. The model returns a tier and a one-sentence reason for each source. Results are cached on the claim object so the same URL is never reclassified.

### Expandable evidence drawer

Every claim card has a collapsed "Sources & evidence" section. Click to expand and see:

- Both confidence signals side by side with a colour-coded source quality bar
- Each source with its tier dot, live link, classification reason, and tier badge
- A **Find better sources** button that fires a targeted re-research query into the chat, specifically asking for primary sources to verify that claim
- A **Remove claim** button — now contextually motivated by what you can see

If you're dissatisfied with the sources, you have three options: re-research to find stronger ones, accept the claim at its rated quality, or remove it entirely.

---

## Features

### Knowledge base
- Claims persist across sessions — your research wall grows over time
- Two independent confidence signals per claim: LLM confidence and source quality
- Hybrid source classification: rule-based tier assignment + LLM for ambiguous cases
- Contradiction detection — new claims are automatically checked against existing knowledge when absorbed
- **⚡ Key Points** digest — one click shows top findings, contradictions, and uncertainties in a scannable summary panel
- Filter claims by All / High confidence / Contradictions / Uncertain
- Edit topic titles in place, delete or re-research individual claims

### Research
- Full streaming responses from Claude, OpenAI, Gemini, or any OpenAI-compatible provider
- Web search toggle — live results via Anthropic (Claude), Google (Gemini), or provider default
- **Research Deeper** — structured multi-angle deep dive: core findings, supporting evidence, counterarguments, implications, and further angles
- **Summarise** — concise executive brief of the current session against your existing claims
- Follow-up question suggestions after each response
- Export topic to Markdown (claims + full conversation)

### Themes
Four built-in themes, switchable live from Settings with no reload required. Your choice persists across sessions.

| Theme | Character |
|---|---|
| **Parchment** | Warm cream paper, charcoal ink, Lora serif. Editorial and readable — the default. |
| **Obsidian** | Near-black with electric cyan accents, Outfit typeface. Dark mode with precision energy. |
| **Slate & Bone** | Cool off-white, blue-grey ink, IBM Plex Mono display. Clinical, architectural, low noise. |
| **Forest Codex** | Deep forest green, aged gold accents, Crimson Pro italic. The richest and most distinctive. |

### Providers
- Claude (Anthropic) — with web search support
- OpenAI — with streaming
- Google Gemini — with Google Search grounding
- Any OpenAI-compatible provider: Groq, Mistral, Together AI, Ollama, LM Studio, Perplexity
- Custom providers: live model fetch from `/v1/models`, dropdown selection, edit/update saved providers

### Voice
- Voice input via Web Speech API (Chrome and Safari on Android/iOS)
- Hands-free: speak your question, response streams automatically

### Mobile
- Responsive layout — claims panel full-screen on mobile
- Chat opens as a slide-up panel via a floating button
- Non-streaming API calls on mobile for reliable CORS handling with custom providers

---

## Getting started

1. Open the [live demo](https://abhishek-sinha-bgl.github.io/MyResAssist/) or host `index.html` anywhere static
2. Click **Configure API key** in the top bar
3. Select your provider, enter your API key, choose a model, and save
4. Optionally pick a theme from the Appearance row at the top of Settings
5. Create your first research topic from the wall
6. Ask a question — absorb the findings — expand any claim to inspect its evidence chain

Your API keys and all research data are stored in your browser's localStorage. Nothing leaves your device except the API calls you make directly to your chosen provider.

---

## Understanding the confidence signals

**Why two signals?** Because they measure different things.

The LLM confidence score reflects how strongly the language model believes a claim is supported by the content it processed — but models are trained on the same SEO-saturated web that search engines index. A confidently stated claim from a travel blog is still just a travel blog.

The source quality score is grounded in the actual provenance of the sources retrieved. It doesn't care how confident the model sounds. It asks: what kind of outlet produced this, and how authoritative is it?

Used together, they let you make better decisions about which findings to act on and which to investigate further.

**A note on limitations.** Source classification is not infallible. A reputable domain can publish a poorly sourced article. An obscure journal can publish the most important paper in a field. Classification is a heuristic, not a verdict. Use it to prioritise your scrutiny, not to replace it.

---

## Deployment

This is a single `index.html` file. Drop it anywhere that serves static files:

- **GitHub Pages** — push to your repo, enable Pages, done
- **Netlify / Vercel** — drag and drop
- **Local** — open the file directly in your browser

No server, no database, no accounts.

---

## API key setup

| Provider | Where to get a key |
|---|---|
| Claude | [console.anthropic.com](https://console.anthropic.com) |
| OpenAI | [platform.openai.com](https://platform.openai.com) |
| Gemini | [aistudio.google.com](https://aistudio.google.com) |
| Groq | [console.groq.com](https://console.groq.com) |
| Mistral | [console.mistral.ai](https://console.mistral.ai) |

For Groq, the correct endpoint is `https://api.groq.com/openai/v1/chat/completions`. After entering the endpoint and key, click **Fetch models** to populate the model dropdown automatically.

---

## Roadmap

- Multi-step deep research with sequential independent searches
- URL and document analysis — paste a link or attach a PDF, have claims extracted with full source attribution
- Cross-device session sync via shareable URL or GitHub Gist
- Search within your knowledge base across topics
- Claim relationship mapping — visual graph of supporting and contradicting claims
- Source quality trends — track whether a topic's evidence base is strengthening or weakening as research deepens

---

## Built with

No frameworks, no bundler, no dependencies. Fonts via Google Fonts: Lora and Source Sans 3 (Parchment), Outfit and IBM Plex Mono (Obsidian), IBM Plex Mono (Slate), Crimson Pro (Forest). Everything else is vanilla HTML, CSS, and JavaScript.
