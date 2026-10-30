# Competitive Analysis & Journey Map

## Responses
- **Role, who are you solving for?:** Coordinator (dispatcher) at a 3PL depot. They assign routes during the shift and at the end of the shift they file the daily report that managers and finance use.
- **Goal, what is this user ultimately trying to achieve?:** Close out the shift with an accurate record that managers, finance and the next coordinator can trust, and get back to dispatching. Basically the coordinator version of the M1 value prop - less admin overhead, more time on the actual work.
- **Friction, the main barrier (moment of misery) stopping them:** At the end of every shift the coordinator has to re-type data into the daily report that RouteLogic already has or should have (stops completed, compliance checks, fuel, overtime, handoff items etc..). Since the dispatch board lags, most of the live updates come in through WhatsApp and driver texts, so they have to piece the record together by hand first and then enter it again.
- **External tools the user is forced to use:** WhatsApp group (one coordinator called it "the real system"), SMS and phone calls from drivers, spreadsheets/notes to collect numbers, and email or WhatsApp for the shift handoff. Same tools I called out in M1 (spreadsheets, email) plus what came up in the research.
- **The process, the 3–5 manual steps they take:**
  1. Chase the real status - scroll through WhatsApp and driver texts because the board still shows finished stops as "in progress"
  2. Fix the board - match the messages to stops and correct the dispatch screen manually
  3. Pull compliance numbers from the compliance screens (inspections, HOS, open flags)
  4. Re-type everything into the daily report, field by field
  5. Handoff to the next coordinator over WhatsApp or email
- **Core frustration, where the process feels most "broken":** Step 4. They are typing in from memory and messages numbers that the system already recorded hours ago. No new information is being added, it happens at the end of a long shift, and any typo goes straight into manager and finance reports.
- **The evidence, a quote or behavior from the research:**
  - Coordinator: "I reassign a route and the driver doesn't see it for ten, fifteen minutes… We keep a WhatsApp group as the real system."
  - Driver: "I've started just texting my dispatcher instead." - every text like that is something the coordinator has to key in again later
  - Driver status updates take 20-60 min to show up on the dispatcher dashboard so dispatchers dont trust the board
  - Compliance step takes 14.6 min and the CSAT heatmap flags coordinators on Scheduling and Compliance (M3)
  - A structured shift handoff would save ~6.8 min a day (UX research)
- **Journey map, a shareable link or the map file you committed:** [competitive-and-journey-map.pptx](competitive-and-journey-map.pptx) - slide 1 is the journey map, slide 2 is the competitive snapshot.

## Competitive snapshot

| | RouteLogic today | Lean routing competitor | Workaround stack (WhatsApp, texts, spreadsheets, email) |
|---|---|---|---|
| **Where it wins** | Admin and reporting layer, which is the main reason customers buy it. Feeds manager and finance reporting | Focused on routing, which the research says is the deciding factor | Real time, no learning curve, works with one hand |
| **Where it loses** | Board lags 20-60 min, reassignments take 8-15 min with no push notification, drivers use maybe 5% of the product, core actions are 2-3 levels deep | Research doesnt really say how they do on reporting/compliance - gap | No audit trail, managers/finance cant see any of it, all has to be re-typed into RouteLogic at end of shift |
| **Shift-close effort** | Collect, reconcile and re-key everything into the daily report | Not in the research | Fast during the shift but creates the re-keying at the end |

**So what:** reporting is RouteLogic's moat so whatever we do has to keep manager and finance reporting intact. The threat is frontline speed, users have already moved the live work to WhatsApp and one enterprise account is looking at the leaner competitor. The opportunity is to build the shift record from data RouteLogic already has, keep the moat and get rid of the re-keying. This is what became B2 Daily Report Auto-Fill in M4 and M5.

Sources were the M2 synthesis and research quotes, my M1 problem hook, and the M3 data/CSAT heatmap. The competitor isn't named in the research so I'd want more discovery on them.
