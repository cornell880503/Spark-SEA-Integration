# Spark for Southeast Asia — Multi-Partner Orchestration Probe

> A Gemini function-calling demo built for Google I/O 2026, exploring what Spark looks like when it runs on SEA super-apps instead of US point solutions.

**Live demo:** open `spark-sea-demo.html` in any browser · no server required  
**Product brief:** [`spark-sea-brief.md`](spark-sea-brief.md)

---

## What This Is

A single-file interactive demo that orchestrates three real-world SEA agent scenarios using Gemini 2.x/3.x function-calling. The model decides which tools to call, in what order, and with what arguments — all in real time when an API key is provided.

| Scenario | Partner tools | SEA-specific logic |
|---|---|---|
| ✈ **Travel** | `search_flights` · `book_hotel` · `book_ground_transport` · `find_restaurant` | Grab super-app (SEA-only) · halal as first-class param · cross-border SG↔KL corridor |
| 🛍 **Shopping** | `search_products` · `compare_prices` · `calculate_duties` · `checkout` | Shopee/Lazada (not Amazon) · cross-border GST arbitrage SG/MY/TH |
| 🍜 **Group Food** | `search_restaurants` · `get_menu` · `place_order` · `split_bill` | GrabFood + GrabPay split billing · halal + vegetarian constraint resolution |

---

## How to Use

1. Open `spark-sea-demo.html` in Chrome or Safari
2. *(Optional)* Paste a **Gemini API key** from [aistudio.google.com/apikey](https://aistudio.google.com/apikey) — without a key, the demo runs a scripted simulation
3. *(Optional)* Paste a **Google Places API key** to enable live restaurant search in the Travel scenario
4. Select a scenario tab, edit the user input (or use the default), click **▶ Run Orchestration**

The three-column layout shows:
- **Left** — dependency graph + budget tracker (updated by `set_budget_item` tool calls)
- **Centre** — orchestration timeline: each tool card appears as the model calls it
- **Right** — reasoning trace: what the model is thinking between tool calls

When total cost exceeds the user's budget, the agent calls `request_user_decision` and pauses for human input — demonstrating human-in-the-loop agentic design.

---

## Technical Highlights

- **Gemini function-calling via REST API** — `functionDeclarations`, tool responses fed back into multi-turn `history`, agentic loop up to 8 iterations
- **Model cascade** — tries `gemini-3.5-flash` → `gemini-2.5-flash` → `gemini-2.0-flash`, fails gracefully with actionable hints
- **Live Google Places API** — `find_restaurant` makes a real POST to `places.googleapis.com/v1/places:searchText`; results are passed back to Gemini as tool response so the model reasons over actual venues
- **UI-driving tools** — `set_budget_item` updates the budget bar in real time; `request_user_decision` renders a Gemini-authored conflict modal
- **Any destination** — live API mode uses Gemini's own knowledge for flights, hotels, and restaurants worldwide; not limited to a hardcoded city database
- **Code-switching input** — default prompts are in Mandarin/English mix, demonstrating native SEA user behaviour

---

## Why These Partners

| Partner | SEA rationale |
|---|---|
| **Grab** | One super-app = ride + food + pay. 3× integration surface vs. Uber + DoorDash + Venmo in US Spark |
| **Shopee / Lazada** | Dominant across 6 SEA countries; cross-border price arbitrage (SG/MY/TH) is a common user behaviour |
| **GrabPay** | Default split-bill mechanism for group dining; deeply embedded in daily SEA life |
| **Halal filter** | Malaysia ~60% Muslim, Indonesia ~85% — must be a first-class tool parameter, not an afterthought |

---

## Built With

- Gemini 3.5 Flash function-calling (Google AI Studio API)
- Google Places API v1 (New) for live restaurant data
- Vanilla HTML/CSS/JS — single file, zero dependencies
- Vibe-coded with Claude Code (Anthropic)
