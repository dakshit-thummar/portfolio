# Signal System — AI Trading Assistant

> Multi-provider AI trading signal generator and market opportunity scanner. React + Vite + Tailwind frontend. One env var switches between Anthropic, OpenAI, Perplexity, and Gemini — all with built-in web search.

🔒 *Source code is in a private repository. Available on request.*

---

## The problem

Retail traders make decisions on stale information. Most stock-research tools either show static yesterday-data dashboards, or require a paid subscription to a research firm whose recommendations are slow and one-size-fits-all. There's a real gap for a fast, on-demand "give me the current read on this ticker" tool that aggregates today's price action, today's news, and produces a structured signal that respects the user's actual trading style (intraday vs swing vs positional).

LLMs with built-in web search solve this almost perfectly — the model handles the synthesis, the search handles the freshness. The hard part is prompt engineering, output structuring, multi-provider support, and a UI that doesn't look like a 2005 prop-trading terminal.

## What it does

Two tools on one polished dark-themed dashboard:

### Trading Signal Agent
Type a ticker (e.g. `RELIANCE`, `AAPL`, `BTC`), pick a market (NSE / US / Crypto), pick a style (Intraday / Swing / Positional). The agent calls your chosen LLM with web search, then renders:

- **Direction** — BUY / SELL / HOLD with a confidence indicator
- **Entry / target / stop** levels with reasoning
- **Recent technicals** — support, resistance, key MAs, RSI, volume
- **News in the last 7 days** — earnings, regulatory, partnerships
- **Risk factors** — what could invalidate the signal

### Opportunity Scanner
Pick a market + style and the agent scans for tickers showing strong momentum right now. Returns 5–10 actionable setups with reasoning, entry levels, and risk callouts.

## Tech stack

| Layer | Tools |
|---|---|
| Frontend | React 18, Vite 5, Tailwind CSS 3, lucide-react icons |
| AI providers | Anthropic Claude, OpenAI Responses API, Perplexity Sonar, Google Gemini — all with built-in web search |
| Proxy | Vite dev-server middleware (`/api/signal`) — single function that dispatches by provider |
| DevOps | Multi-stage Dockerfile (node build + nginx serve), GitHub Actions CI |

## Architecture in one paragraph

The frontend is a two-tab React SPA. Each tab is its own component (`trading_signal_agent.jsx` and `opportunity_scanner.jsx`) that builds a structured prompt from form inputs, POSTs to `/api/signal`, and renders the markdown response with section parsing and icon decoration. The proxy is a Vite middleware that reads `PROVIDER` and the matching API key from `.env`, dispatches to the right provider's API (each has different request shapes for web-search-enabled calls), and returns text. This pattern keeps API keys out of the browser and makes provider swaps a one-line `.env` change. For production, the same ~80-line proxy function ports cleanly to Cloudflare Workers, Vercel Edge Functions, or Netlify Functions.

## Skills demonstrated

- **Multi-provider LLM integration** — four distinct APIs (Anthropic Messages, OpenAI Responses, Perplexity Chat Completions, Gemini generateContent) all unified behind a single internal interface
- **Web-search-enabled LLM calls** — each provider exposes search differently (web_search tool blocks, sonar models, google_search grounding); abstracted to one switch
- **Prompt engineering as code** — structured prompts with explicit sections (technical / news / risk / verdict) that consistently produce parseable output
- **Server-side secret handling** — API keys never reach the client, all calls go through the dev-server proxy
- **Modern React patterns** — functional components, hooks, conditional rendering, loading / error / empty states, dark-mode design
- **Tailwind utility-first design** — clean dark theme with semantic color usage (emerald for buy signals, amber for warnings, red for risks)
- **Vite custom middleware** — non-trivial Vite plugin use; same pattern can serve as a serverless-function shim
- **Multi-stage Dockerfile** — build with Node, serve with nginx, small final image

## What I deliver to a client

Full source, multi-provider proxy ready to drop into Cloudflare Workers / Vercel / Netlify for production, custom prompts tuned to the client's specific market or asset class, additional tabs for whatever screening / signal workflow they need, and a deployment guide. I can also extend the architecture to handle other "LLM + tool" patterns (research assistant, due-diligence brief generator, SEO content with citations, etc.).

## Variations I have built on top of this

- A news-monitoring agent that watches a ticker list and alerts on material news
- A portfolio-rebalancing recommender given current holdings and risk tolerance
- A backtest summarizer that ingests historical signal logs and reports hit-rate
- A research-brief generator that produces investor-grade one-pagers for stocks
- A cost-comparison harness that runs the same prompt across all four providers and reports latency, quality, and price per call

## Lessons / opinions

LLMs without web search are useless for any market-data task — their training cutoff is months old. Anthropic and Perplexity are currently the best at structured financial output; OpenAI's Responses API with web_search is improving fast; Gemini is the cheapest but needs more prompt scaffolding to stay structured. Don't ship the dev-server proxy to production — port the ~80-line provider switch to a serverless function, deploy the SPA to a CDN, and you have a $0 / month app that scales to thousands of users. Always include a disclaimer that the output is not financial advice — clients (and their lawyers) will care.

---

← [Back to portfolio](../README.md)
