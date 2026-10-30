# Experimentation Plan (Module 5)

## Get your documents ready
- **From M3, your hypothesis sentence:** If we deliver a leaner product that strips out extra clicks and administrative overhead, CSAT scores across drivers and coordinators should improve significantly and system adoption should improve as well.

  This is the overall Velocity hypothesis. The experiment only tests one feature so the test hypothesis further down is narrower.
- **From M3, your primary success metric & guardrail metric:** Primary - driver/coordinator CSAT and adoption. Guardrail - Manager CSAT. These stay as the long term signals (3 month window) but they wont move in 14 days, so I'm using feature level metrics for the test.
- **From M4, the feature you scoped in your PRD (this is what you're testing):** B2 Smart Daily Report Auto-Fill. At the end of the shift the daily report opens as a draft that's pre-filled from the coordinators dispatch, compliance, handoff, timeclock and fuel records. Coordinator reviews it, fixes anything flagged and submits.

## Define your experiment parameters
- **Feature under test (from your M4 PRD):** B2 Smart Daily Report Auto-Fill, rules based, no generative AI
- **Persona (from your M2 persona):** Coordinators. They are the ones who file the daily report, and the CSAT heatmap flags them on Scheduling and Compliance which is where the report pulls its data from. Drivers are excluded since B2 is desktop only and drivers dont file daily reports.
- **Expected outcome (the behaviour change you expect, from your M3 hypothesis):** Coordinators close their shift faster because they are confirming data RouteLogic already has instead of re-typing it.
- **Primary success metric (the one number that defines success, from M3):** % of daily reports submitted within 3 min of opening. Measured from the report opened and report submitted events. Picked this because its specific to what B2 changes and it moves in days, CSAT doesnt.
- **Baseline rate (today's rate of your primary metric, from your M3 data):** 20% - this is an assumption. M3 data doesnt have report timing so this needs to be replaced with the real number from report logs before launch. The scheduling adoption numbers (15% drivers, 42% coordinators) are for a different module so I couldnt use those.
- **Guardrail metric & boundary (what must not break, and how far it can move before you investigate):**
  - Downstream reporting discrepancies (report vs what manager/finance dashboards pull in) - has to be 0, any discrepancy stops the test
  - On-time daily report submission - investigate if 1 pt below control, stop at 2 pts below

  Both of these protect the same thing as Manager CSAT but can actually be measured within 14 days. Will still check Manager CSAT after the pilot.
- **Minimum Detectable Effect (MDE) (the smallest improvement worth shipping, your floor):** +15 pts (20% to 35%)
- **Sample size per arm (calculator: baseline + MDE):** 138 reports per arm (20% baseline, +15 MDE, 80% power, p < 0.05 two sided). Randomizing by coordinator so each one stays in one arm for the whole test. ~10 reports per coordinator over 14 days = ~14 coordinators per arm, ~28 total, which fits in the 3 pilot accounts. Coordinators at the same depot go in the same arm since they share handoffs.
- **Traffic split & test duration (50/50 standard, cover ≥ 2 weekly cycles):** 50/50 by coordinator, 14 days (2 full weeks). Will look at week 1 and week 2 seperately to check for a novelty effect.
- **Significance threshold (p < 0.05 is standard, explain any deviation):** p < 0.05 two sided. No change since there is only one primary metric.

## Define your control and variant
- **Control (A) (the current experience, with your M2 moment of misery and M3 funnel/workflow data):** Current manual daily report form. Coordinator re-types numbers that are already in RouteLogic - routes dispatched, stops completed, inspections, HOS violations, handoff details, fuel, overtime. From M3: "Takes way too long to schedule dispatch and complete compliance checks in the system". Basic tasks take much longer than they should and coordinator CSAT is lowest on Scheduling and Compliance. Users keep the real record in spreadsheets, email and WhatsApp.
- **Variant (B) (your single change, with the relevant screens & functional requirements from your M4 PRD):** Same daily report but it opens as a pre-filled draft. Anything the system can't confirm is left blank and flagged instead of guessed. Coordinator reviews, fixes the flagged fields and submits in one click. This tests pre-fill and the new review layout together, would need a third arm to separate them and we dont have enough coordinators for that.

  Screens:
  - Report review screen - shift date/time header, summary bar ("15 of 18 filled, 3 need attention"), needs attention fields pinned at the top, rest of the fields in the current order, sticky submit button
  - Field states - auto-filled, needs attention, edited, conflict (shows both values to pick from)
  - Submit confirmation listing edited fields and blank optional fields (skipped if nothing was edited or left blank)
  - Fallback to the existing manual form with "Auto-fill unavailable. Enter manually."
  - Submitted screen with "Submitted in 48s"
  - Manager badge "Auto-filled, Reviewed by [name]" as metadata, no layout change

  Functional requirements:
  - Draft is generated when the coordinator opens the report, using records from their shift window
  - Each field maps to one documented source and rule, unmapped fields stay blank
  - Field only gets filled if the rule returns exactly one valid value, otherwise blank and flagged
  - Required fields must be valid before submit, all fields editable, same validation as today
  - Same endpoint and payload, no new or renamed fields
  - Audit record per field (auto-filled value, submitted value, source) stored outside the payload
  - If the draft fails or takes more than 5s, load the manual form and log it
  - Feature flag needs to work per coordinator (not just per account) for the randomization
  - Events - report opened, autofill completed, field edited, report submitted (with duration), autofill failed
- **Isolation check (what has NOT changed: everything identical between arms):** Everything else stays the same between both arms - app version, dispatch/compliance/handoff screens (B2 only reads from them), report schema and endpoint, manager and finance dashboards, notifications, training (no extra coaching for the variant group), account and shift mix, desktop web in both.

  Risks - coordinators at a shared depot could see each others screens, so whole depots go into one arm. Managers will see reports from both arms but thats ok since the guardrails are measured per report and not by survey.

## Formalize your hypothesis & shipping criteria
- **Your hypothesis (filled in):** If we pre-fill the daily report for coordinators using data RouteLogic already has (and leave blank anything it cant confirm), the % of reports submitted within 3 minutes will go from ~20% to at least 35% within 14 days, with zero downstream reporting discrepancies and no more than a 2 pt drop in on-time submission.
- **Your shipping criteria (filled in):**
  - SHIP if all of these are true at day 14 - variant beats control by 15+ pts at p < 0.05, week 2 lift is at least 70% of week 1 (so its not just novelty), zero reporting discrepancies, on-time submission no more than 2 pts below control, and coordinators edit 5% or less of the auto-filled fields.
  - ITERATE if the guardrails hold but the lift is under 15 pts or not significant, or the edit rate is between 5-10%. Fix the most flagged field mappings and rerun for 14 days.
  - KILL (turn off the flag for that account) if there is any reporting discrepancy, on-time submission is more than 2 pts below control, edit rate goes above 10% on any account-day, or the fallback rate is above 5%.
- **Hardest parameter to define, and did it change your hypothesis? (quick debrief):** The primary metric. In my first draft I just reused the M3 metrics (CSAT, adoption) and the scheduling adoption baseline, but none of that can tell you if auto-fill works in 14 days and the adoption number is for a different module anyway. Once I picked a metric only this feature could move, a few things changed. Persona narrowed to coordinators only since drivers never touch the daily report. The control changed from the driver pain points in M2 (taps to mark delivered, late reassignments, closed roads) to the coordinators report entry pain - the driver stuff is still valid but belongs in a seperate test for B4 and B6. And Manager CSAT turned into the two per-report guardrails.

Still need to get the real baseline from report logs and rerun the sample size before launch (it goes up to ~150-165 per arm if the baseline is 25-30%), and confirm engineering can do the per coordinator flag.
