# Spark for Southeast Asia — Partner Orchestration
### Product Brief · May 2026

---

## The Gap

Spark launched in the US connecting Instacart, OpenTable, and Canva — three single-purpose apps, three separate integration contracts, three distinct API schemas. As Google moves Spark into Southeast Asia, the first question no one has answered publicly is: **which partners do you connect first, and does the US playbook even apply?** In a region where daily digital life runs through a handful of super-apps, the US connector model may solve for the wrong unit of integration.

---

## The Insight

Southeast Asia's digital economy is organized around **super-apps**, not point solutions. Grab alone covers ride-hailing, food delivery, hotel booking, and payments — one connector for what would require four separate Spark integrations in the US. This changes the economics of the partner layer dramatically: fewer contracts, fewer schemas, but each partner carries far more behavioral surface area.

Layered on top are dimensions absent from the US version of Spark: **cross-border trips** (Singapore–Malaysia is a daily commute corridor, not an edge case), **multi-currency flows** (SGD/MYR/THB switching mid-trip), **halal requirements** as a first-class restaurant parameter (Malaysia is ~60% Muslim, Indonesia ~85%), and **code-switching** across Mandarin, English, Malay, and Thai within a single user utterance. A Spark trained primarily on English-first, single-market, secular consumer behavior will produce the wrong defaults in all four of these dimensions without explicit product choices to address them.

---

## The Prototype

To pressure-test these hypotheses, I built a multi-partner orchestration probe using Gemini function-calling, live on the day of I/O 2026. The demo orchestrates four mock partner tools — `search_flights` (AirAsia/Scoot/Jetstar), `book_ground_transport` (Grab), `find_restaurant`, and `get_fx_rate` — for a Singapore→Kuala Lumpur weekend trip. The agent resolves a dependency graph (flights must run first; transport and restaurant slots depend on arrival time; FX check runs last), and when the aggregated cost exceeds the SGD 600 budget, it **stops and surfaces the trade-off to the human** rather than making the value judgment autonomously. This reflects the Spark design principle of asking permission before sensitive actions. The user's choice (save on the flight vs. save on the restaurant) then propagates back through the affected downstream steps — demonstrating that state is maintained across the orchestration, not re-derived from scratch.

---

## What Would Need to Be True to Ship This

Four things, in priority order:

1. **Partner API standardization.** Grab, AirAsia, and their peers do not expose MCP-compatible schemas today. Google would need to negotiate schema standards — or invest in adapter layers — before Spark can call these tools reliably at scale. This is the highest-friction item and the one that requires the most internal cross-functional alignment.

2. **Cross-border legal and payments compliance.** Any agentic flow that crosses a national border and touches a financial transaction (FX conversion, Grab pre-booking) triggers at minimum two jurisdictions' data residency and payments regulations. A Singapore-booked, Malaysia-executed Grab ride is already a cross-border data flow under PDPA and PDPD frameworks. Compliance review needs to start before the API contracts close, not after.

3. **Halal and religious context as a product primitive.** `halal_required` cannot be an afterthought parameter added by a partner — it needs to be a first-class signal in Spark's restaurant tool schema, surfaced proactively based on destination context. Getting this wrong is a trust issue, not just a feature gap.

4. **Super-app commercial model.** Connecting Grab differently than you'd connect Uber — because Grab is ride + food + pay — requires a different revenue and attribution model. The usual Spark connector playbook (one tool, one action, one commission surface) may not map cleanly. The commercial structure needs to be figured out alongside the technical integration, not sequentially.

---

These are the questions any team taking Spark to SEA partners would need to answer first. This prototype is a first pass at what the answer looks like in practice — built to make the conversation concrete rather than abstract.

---

*Built using Gemini 3.5 Flash function-calling · I/O 2026 · [spark-sea-demo.html]*
