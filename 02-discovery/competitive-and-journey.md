# Competitive Analysis & Journey Map

> Module 2 · Lab 2 — ★ Deliverable 2. Repo file `02-discovery/competitive-and-journey.md`. Builds on `ai-synthesis.md` and Module 1 `problem-hook.md`. Becomes the Competitive Analysis & Journey Map slide of the Module 6 final deck.

## Responses
- **Role, who are you solving for?:** The Coordinator (dispatcher) at a 3PL depot. They assign routes during the shift and, at shift close, file the daily report that managers and finance rely on.
- **Goal, what is this user ultimately trying to achieve?:** Close the shift with an accurate record that managers, finance and the incoming coordinator can trust, then get back to dispatching. This is the coordinator's version of the M1 value proposition: restore the core value and strip out administrative overhead.
- **Friction, the main barrier (moment of misery) stopping them:** At the end of every shift the coordinator re-types into the daily report data that RouteLogic already holds or should hold: stops completed, compliance checks, fuel, overtime, handoff items. Live updates arrived through WhatsApp and driver texts because the dispatch board lags, so the record has to be rebuilt by hand before it can be entered twice.
- **External tools the user is forced to use:** A WhatsApp group (*"the real system"*), SMS and phone calls from drivers, spreadsheets or notes to collect figures, and email or WhatsApp for the shift handoff. These are the same off-platform tools named in the M1 problem hook (spreadsheets, email) and the M2 synthesis (WhatsApp groups, texts).
- **The process, the 3–5 manual steps they take:**
  1. **Chase the real status.** Scroll WhatsApp and driver texts, because the board still shows finished stops as "in progress."
  2. **Reconcile the board.** Match those messages to stops and correct the dispatch screen by hand.
  3. **Pull compliance numbers.** Work through the compliance screens for inspections, hours-of-service and open flags.
  4. **Re-type it all into the daily report.** Key the same counts, fuel and overtime into the report form, field by field.
  5. **Hand off off-platform.** Send the incoming coordinator the open items by WhatsApp or email.
- **Core frustration, where the process feels most "broken":** Step 4. The coordinator types, from memory and messages, figures the system recorded hours earlier. It is pure duplication with no new information, it happens at the end of a long shift, and any typo lands directly in manager and finance reporting.
- **The evidence, a quote or behavior from the research:**
  - *"I reassign a route and the driver doesn't see it for ten, fifteen minutes… We keep a WhatsApp group as the real system."* — coordinator (M2 research)
  - *"I've started just texting my dispatcher instead."* — driver (M2 research). Every texted update is data the coordinator has to key in again.
  - Driver status changes take **20–60 minutes** to reach the dispatcher dashboard, so completed stops still show "in progress." Dispatchers say they can't trust the board. (M2 AI synthesis, Platform Sync)
  - The compliance step takes **14.6 minutes**, and the CSAT heatmap flags coordinators on Scheduling and Compliance. (M3 data)
  - A structured shift handoff would save **~6.8 minutes a day**. (M2 UX research)
- **Journey map, a shareable link or the map file you committed:** [`02-discovery/competitive-and-journey-map.pptx`](competitive-and-journey-map.pptx). Slide 2 is the coordinator shift-close journey map and slide 3 is the competitive snapshot.

## Competitive snapshot

| | RouteLogic today | Lean routing competitor | Workaround stack (WhatsApp, texts, spreadsheets, email) |
|---|---|---|---|
| **Where it wins** | Admin and reporting layer, the main reason customers buy. Feeds manager and finance reporting. | Focused on routing, which the research names as the deciding factor. A leaner, routing-first product. | Real time. Zero learning curve. Works with one free hand. |
| **Where it loses** | Board lags 20–60 min; reassignments take 8–15 min with no push notification. Feature bloat: drivers use ~5% of the product. Core actions sit 2–3 levels deep. | *Not covered in the research:* reporting and compliance depth. | No audit trail. Invisible to managers and finance. Everything has to be re-typed into RouteLogic at shift end. |
| **Shift-close effort** | Collect, reconcile and re-key into the daily report. | *Not covered in the research.* | Fast during the shift, but it creates the re-keying at the end. |

**So what:**
- **Defend:** reporting is RouteLogic's moat. Any fix has to keep manager and finance reporting intact.
- **Threat:** frontline speed. Users have already moved the live work to WhatsApp, and an enterprise account is evaluating a leaner routing competitor.
- **Opportunity:** build the shift record from data RouteLogic already has, so the moat stays and the re-keying goes. This is B2 Daily Report Auto-Fill (M4 PRD, M5 experiment).

*Sources: M2 `ai-synthesis.md` (Product Health & Insights Summary and research quotes), M1 `problem-hook.md`, M3 data and CSAT heatmap. The competitor is unnamed in the research; italic cells are gaps to fill with further discovery.*
