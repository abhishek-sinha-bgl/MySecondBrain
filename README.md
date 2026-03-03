# 🧠 MySecondBrain

A personal AI research assistant that runs entirely in your browser. No backend, no server, no subscriptions — just bring your own API key and start researching.

**[Live Demo →](https://abhishek-sinha-bgl.github.io/MySecondBrain)** *(replace with your GitHub Pages URL)*

![MySecondBrain Screenshot](screenshot.png)

---

## What it does

MySecondBrain helps you research topics deeply and retain what matters. You chat with an AI that searches the web in real time, then automatically extracts key takeaways, suggests follow-up questions, and flags when new findings contradict earlier ones. Everything can be exported to Markdown, Obsidian, or NotebookLM.

**Core features:**

- **Live web search** — responses grounded in current information, not just training data
- **Streaming responses** — see answers appear token by token, no waiting
- **Key Takeaways panel** — auto-extracted after every response, with confidence ratings (High / Medium / Low) and contradiction detection
- **Follow-up chips** — 3 suggested next questions appear after each response
- **Citations** — inline superscript links with a sources bar showing titles and domains
- **Deep Research mode** — structured multi-angle report covering findings, perspectives, evidence, and implications
- **Session persistence** — all conversations saved locally, switchable from the left panel
- **Pinned Topics** — scope any session to a domain (Geopolitical, Technology, Regulatory, etc.) to keep responses focused
- **Export** — save as Markdown, Obsidian note (with YAML frontmatter), copy for NotebookLM, or print to PDF

---

## Supported AI Providers

MySecondBrain uses a **Bring Your Own Key (BYOK)** model. Your key is stored only in your browser's `localStorage` and sent directly to the provider — nothing passes through any server.

| Provider | Models | Web Search |
|----------|--------|------------|
| **Anthropic Claude** | Sonnet 4, Opus 4.5, Haiku 4.5, and more | ✅ Native (web_search tool) |
| **OpenAI** | GPT-4o, GPT-4o Mini, o1, o3-mini, and more | Extracts URLs from responses |
| **Google Gemini** | 2.0 Flash, 1.5 Flash, 1.5 Pro, 2.5 previews | ✅ Native (Google Search grounding) |
| **Custom (OpenAI-compatible)** | Any model — Groq, Mistral, Together AI, Ollama, LM Studio | Depends on provider |

The model dropdown fetches live models from each provider's API so you always see what's currently available.

---

## Getting Started

### 1. Get an API key

Pick any one provider to start:

- **Gemini (easiest/free tier)** → [aistudio.google.com](https://aistudio.google.com) — sign in with Google, grab a key instantly. Use the `gemini-1.5-flash` model for the free tier.
- **Claude** → [console.anthropic.com](https://console.anthropic.com) — pay-as-you-go, ~$5 of free credit on signup
- **OpenAI** → [platform.openai.com](https://platform.openai.com) — pay-as-you-go

### 2. Open the app

Either use the GitHub Pages URL, or just open `index.html` directly in your browser — it works as a local file too.

### 3. Configure your key

Click the 🔑 button in the bottom-left corner. Select your provider, paste your key, choose a model, and click **Save**. Your key is stored in your browser and won't need to be re-entered.

### 4. Start researching

Type a question or topic in the input bar and press Enter. Toggle **Live Web** on for real-time search results.

---

## Deployment on GitHub Pages

1. Fork or clone this repository
2. Make sure the HTML file is named `index.html` at the root
3. Go to **Settings → Pages**
4. Set source to **Deploy from branch → main → / (root)**
5. Click **Save** — your app will be live in ~60 seconds at `https://yourusername.github.io/reponame`

That's it. No build step, no dependencies, no Node.js required.

---

## Using Custom Providers

Any OpenAI-compatible endpoint works. In the provider config modal, click **+ Custom** and fill in:

| Field | Example |
|-------|---------|
| Provider Name | Groq |
| API Endpoint | `https://api.groq.com/openai/v1/chat/completions` |
| API Key | your Groq key |
| Model Name | `llama-3.3-70b-versatile` |

**Popular compatible providers:**

- **Groq** — extremely fast inference, generous free tier → [console.groq.com](https://console.groq.com)
- **Mistral** → [console.mistral.ai](https://console.mistral.ai)
- **Together AI** → [api.together.xyz](https://api.together.xyz)
- **Perplexity** → [perplexity.ai/settings/api](https://www.perplexity.ai/settings/api)
- **Ollama (local)** → endpoint: `http://localhost:11434/v1/chat/completions`, key: `ollama`
- **LM Studio (local)** → endpoint: `http://localhost:1234/v1/chat/completions`

---

## Privacy & Security

- **Your API keys never leave your browser.** They are stored in `localStorage` and used exclusively to make direct HTTP requests to the provider's API.
- **No analytics, no tracking, no ads.** This is a static HTML file with no external scripts beyond Google Fonts.
- **No data is sent to any intermediary server.** The app has no backend.
- Keys persist in your browser until you clear site data or enter a new key. They do not transfer to other browsers or devices — each needs its own key configured.

---

## Features In Detail

### Pinned Topics
Click any pinned topic in the left panel to activate it as a context scope. All subsequent responses will be framed through that lens (e.g. activating "Regulatory" will bias responses toward regulatory angles). Click again to deactivate. Add custom topics with the **+** button.

### Confidence Indicators
Each takeaway is rated High / Medium / Low based on how well-supported the claim is in the source material. The AI evaluates this during extraction — High means multiple corroborating sources, Low means speculative or single-source claims.

### Contradiction Detection
When new takeaways are generated, previous ones are passed as context. If a new finding conflicts with an earlier takeaway, the card is highlighted in red with a note showing the specific earlier claim it contradicts. Useful for fast-moving topics.

### Deep Research
Click **Deep Research** to send a structured prompt that requests a multi-angle report covering: overview, key findings with evidence, multiple stakeholder perspectives, specific data points, forward implications, and open questions. More thorough than a regular chat message.

### Sessions
All research sessions are saved automatically in your browser with a title derived from your first question. Switch between sessions from the left panel — full conversation history, takeaways, and topic scope are all restored.

### Export Options
| Option | Format | Best for |
|--------|--------|----------|
| Save Local | `.md` file download | Personal notes, local Obsidian vault |
| Obsidian | `.md` with YAML frontmatter | Direct import to Obsidian |
| NotebookLM | Clipboard copy | Paste as source in Google NotebookLM |
| Markdown | Clipboard copy | Pasting into any markdown editor |
| PDF | Browser print dialog | Sharing with others |

---

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `Enter` | Send message |
| `Shift + Enter` | New line in input |

---

## Roadmap Ideas

- [ ] Multi-step deep research with sequential web searches
- [ ] Image analysis (upload charts, screenshots, documents)
- [ ] Voice input
- [ ] Sync sessions across devices via GitHub Gist or similar
- [ ] Search within conversation history
- [ ] Dark/light theme toggle

---

## License

MIT — do whatever you want with it.

---

*Built as a single HTML file. No frameworks, no build tools, no dependencies.*
