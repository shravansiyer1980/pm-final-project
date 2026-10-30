# Experimentation Plan (Module 5)

## Get your documents ready
- **From M3, your hypothesis sentence:** If we deliver a leaner product that strips out extra clicks and administrative overhead, CSAT scores across drivers and coordinators should improve significantly, and system adoption metrics should improve as well.
  *M5 note: this is the initiative-level hypothesis for Velocity. This experiment tests one feature inside it, so the test hypothesis below is narrower and names a single metric.*
- **From M3, your primary success metric & guardrail metric:**
  - Primary (initiative signal): driver and coordinator CSAT, and module adoption.
  - Guardrail (product signal): Manager CSAT.
  *M5 note: CSAT and adoption stay as the initiative's long-run signals (M3 decision window: 3 months). Neither can show whether one feature worked within 14 days, so this test uses a feature-level metric and a feature-level guardrail derived from them (see below).*
- **From M4, the feature you scoped in your PRD (this is what you're testing):** B2 Smart Daily Report Auto-Fill. At shift end, the daily report opens as a draft pre-filled from the coordinator's own dispatch, compliance, handoff, timeclock and fuel records. The coordinator reviews it, fixes flagged fields and submits.

## Define your experiment parameters
- **Feature under test (from your M4 PRD):** B2 Smart Daily Report Auto-Fill, rules-based with no generative AI (PRD decision D1).
- **Persona (from your M2 persona):** Coordinators. They file the daily report, and the M3 CSAT heatmap flags coordinators on Scheduling and Compliance, the two areas the report pulls from. Drivers are excluded: B2 is desktop-only (PRD D7) and drivers don't file daily reports, so they can't be exposed to the change.
- **Expected outcome (the behaviour change you expect, from your M3 hypothesis):** Coordinators close their shift faster because they confirm data RouteLogic already holds instead of re-typing it. This is the "extra clicks and administrative overhead" from the M1 value proposition, applied to one task.
- **Primary success metric (the one number that defines success, from M3):** Share of daily reports submitted within 3 minutes of opening. Measured from the `report_opened` and `report_submitted` events (PRD FR-10).
  *Why this metric: M3's workflow data showed basic tasks take far longer in the tool than they should. This counts time saved on the specific task B2 changes, and it moves within days, unlike CSAT.*
- **Baseline rate (today's rate of your primary metric, from your M3 data):** **20%** of daily reports are submitted within 3 minutes today.
  *ASSUMPTION: M3 data has no daily-report timing. Replace 20% with the real figure from report logs before launch. The shift scheduling adoption figures (15% drivers, 42% coordinators) measure a different module and can't serve as this baseline.*
- **Guardrail metric & boundary (what must not break, and how far it can move before you investigate):**
  - **Downstream reporting discrepancies** (mismatches between submitted reports and what manager and finance dashboards ingest): boundary is **exactly 0**. Any discrepancy stops the test.
  - **On-time daily report submission rate:** investigate at **1 point** below control, stop the test at **2 points** below control.
  *These protect the same thing as M3's Manager CSAT guardrail (managers and finance keep getting correct, on-time reports) but are measurable per report and per arm within 14 days. Manager CSAT remains a post-pilot qualitative check.*
- **Minimum Detectable Effect (MDE) (the smallest improvement worth shipping, your floor):** **+15 percentage points** (20% → 35% of reports submitted within 3 minutes).
- **Sample size per arm (calculator: baseline + MDE):** **138 reports per arm** (baseline 20%, MDE +15 pts, 80% power, p < 0.05 two-sided).
  - Randomize **by coordinator**, so each coordinator stays in one arm for the whole test.
  - At ~10 reports per coordinator over 14 days, that is **~14 coordinators per arm (~28 total)**, which fits the 3 pilot accounts.
  - Coordinators at the same depot go into the same arm, because they share handoffs (PRD edge case E6).
- **Traffic split & test duration (50/50 standard, cover ≥ 2 weekly cycles):** 50/50 by coordinator, **14 days** (2 full weekly cycles). Report week 1 and week 2 separately to detect a novelty effect.
- **Significance threshold (p < 0.05 is standard, explain any deviation):** p < 0.05, two-sided. No deviation: there is a single primary metric, so no multiple-comparison correction is needed.

## Define your control and variant
- **Control (A) (the current experience, with your M2 moment of misery and M3 funnel/workflow data):** Today's manual daily report form. At shift end the coordinator re-types figures that already exist in RouteLogic: routes dispatched, stops completed, inspections, hours-of-service violations, handoff details, fuel and overtime.
  - **Moment of misery (M3 qualitative evidence):** *"Takes way too long to schedule dispatch and complete compliance checks in the system."*
  - **M3 workflow data:** basic tasks take far longer on average than they should, and coordinator CSAT is lowest on Scheduling and Compliance.
  - **M1/M2 workaround:** users keep the real record outside the tool, in spreadsheets, email and message groups, because the system costs them more effort than it saves.
- **Variant (B) (your single change, with the relevant screens & functional requirements from your M4 PRD):** The same daily report, opened as a pre-filled draft. Fields the system can't confirm are left blank and flagged rather than guessed. The coordinator reviews, fixes flagged fields and submits in one click.
  *Attribution note: this tests the B2 package as a whole, meaning pre-fill plus the new review layout. A single arm can't separate the two. A third arm (new layout, no pre-fill) would need roughly 50% more coordinators than the pilot has.*

  **Screens**
  - **Report Review (core screen):**
    - Header with shift date and time window.
    - Summary bar, e.g. "15 of 18 filled · 3 need attention."
    - "Needs attention" group pinned at the top, followed by the remaining fields in the current report's section order.
    - Sticky Submit button.
  - **Field states:**
    - Auto-filled: neutral styling, source shown on hover.
    - Needs attention: blank or conflicting, highlighted.
    - Edited: marked as changed by the user.
    - Conflict: shows the competing values with a pick action.
  - **Submit confirmation:** lists edited fields and optional fields left blank. Skipped entirely when there were zero edits and zero blanks.
  - **Fallback:** the existing manual form with the banner "Auto-fill unavailable. Enter manually."
  - **Submitted state:** the existing confirmation screen, plus "Submitted in 48s."
  - **Manager badge:** "Auto-filled · Reviewed by [name]", carried as metadata with no layout change.

  **Functional requirements**
  - **FR-1 Trigger:** a draft is generated when the coordinator opens the report, using records from that coordinator's actual shift window.
  - **FR-2 Field mapping:** each field maps to one documented source and rule. Unmapped fields stay blank.
  - **FR-3 Fill rules:** a field is filled only when its rule returns exactly one valid value. Zero, multiple, conflicting or stale values leave it blank and flagged.
  - **FR-4 Submit gating:** required fields must be valid before submitting.
  - **FR-5 Editing:** every field is editable, with today's validation rules unchanged.
  - **FR-6 Submission:** same endpoint, same payload schema, no added or renamed fields.
  - **FR-7 Audit record:** auto-filled value, submitted value and source reference stored per field, outside the payload.
  - **FR-8 Fallback:** if the draft fails or isn't ready in 5s, load the manual form and log the failure.
  - **FR-9 Feature flag:** extended for this test to work **per coordinator** as well as per account, so randomization matches the flag.
  - **FR-10 Instrumentation:** `report_opened`, `autofill_completed`, `field_edited`, `report_submitted` (with duration), `autofill_failed`.
  - **FR-11 Manager marker:** the "auto-filled & reviewed" indicator is metadata outside the report schema.
- **Isolation check (what has NOT changed: everything identical between arms):**
  - App version and release train.
  - Dispatch, compliance and handoff screens. B2 only reads their data (PRD constraint: read-only sources).
  - Report schema, fields, validation rules and submission endpoint (FR-5, FR-6, enforced by contract tests that block deploy).
  - Manager and finance dashboards and their layout (the badge is metadata only).
  - Notifications and reminders. Shift-end reminders are explicitly out of scope.
  - Training and onboarding: no extra coaching or announcements for the variant arm.
  - Account and shift mix: both arms are drawn from the same 3 accounts and the same mix of day and overnight shifts.
  - Platform: desktop web in both arms.

  **Risks to isolation:**
  - Coordinators at a shared depot could see each other's screen or reports. Mitigated by assigning whole depots to one arm.
  - Managers see reports from both arms. This is acceptable because the guardrails are measured per report, not by manager survey.

## Formalize your hypothesis & shipping criteria
- **Your hypothesis (filled in):**
  > We believe that **auto-filling the daily report** for **Coordinators** will increase **the share of reports submitted within 3 minutes from 20% to at least 35%** within **14 days**, while **downstream reporting discrepancies stay at zero** and **on-time submission stays within 2 points of control**.
- **Your shipping criteria (filled in):**
  - **SHIP to GA** only if all of these hold at day 14:
    1. The variant beats control on the primary metric by **≥ 15 points at p < 0.05**.
    2. The **week 2 lift is ≥ 70% of the week 1 lift**, which rules out a novelty effect.
    3. **Zero** downstream reporting discrepancies for the full 14 days.
    4. On-time submission is **no more than 2 points below control**.
    5. Coordinators edit **≤ 5%** of auto-filled fields (accuracy, from PRD evals).
  - **ITERATE** (no ship, no kill) if the guardrails hold but either of these is true:
    - The lift is under 15 points or not significant.
    - The edit rate is between 5% and 10%.
    In that case, fix the most-flagged field mappings and rerun for 14 days on the same accounts.
  - **KILL** immediately, switching the flag off for the affected account, on any of these:
    - Any reporting discrepancy.
    - On-time submission more than 2 points below control.
    - Edit rate above 10% on any account-day.
    - Auto-fill failure (fallback) rate above 5%.
- **Hardest parameter to define, and did it change your hypothesis? (quick debrief):**
  The primary metric. My first draft reused the M3 initiative metrics (CSAT and adoption) and the shift scheduling adoption baseline. None of those can show whether daily report auto-fill works within 14 days, and the adoption baseline measures a different module entirely.
  Choosing a metric that only this feature could move changed the hypothesis in three ways:
  - **Persona:** narrowed from drivers and coordinators to coordinators only, since drivers never touch a desktop daily report.
  - **Control:** I replaced the M2 driver moments of misery (three taps to mark delivered, late route reassignments, closed roads) with the coordinator's report-entry pain. Those driver pains are real and well evidenced, but they belong to a separate experiment for B4 (mobile, one-tap delivery) and B6 (push alerts on route changes).
  - **Guardrail:** Manager CSAT became two per-report guardrails that can be measured inside the test window.

---

### Before you submit
- [ ] Replace the **20% baseline** with real daily-report timing data, then rerun the calculator. At a 25% baseline the sample is 152 per arm; at 30% it is 163.
- [ ] Confirm each pilot account has enough coordinators for **~14 per arm**.
- [ ] Confirm engineering can add the **per-coordinator flag** (FR-9 extension) before launch.
