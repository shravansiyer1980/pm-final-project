# GTM Strategy & Success Dashboard

## Go-to-market summary
Launching B2 Smart Daily Report Auto-Fill to coordinators at existing RouteLogic accounts. Goal is engagement - getting coordinators to close every shift with the pre-filled report instead of re-typing it. It's an M (targeted) launch, no press or paid, rolled out in 3 cohorts over weeks 5-8 after the pilot, and each cohort only goes live if the previous one kept manager and finance reporting clean.

Channels are in-app guided first use, CS led rollout calls with each account, and pilot coordinators sharing their before/after results. The idea is to protect what customers buy RouteLogic for (reporting) while getting rid of the end of shift re-keying that pushes coordinators to WhatsApp and spreadsheets.

## Success dashboard

| KPI | Baseline | Target | Measurement window |
|---|---|---|---|
| Daily reports submitted within 3 min of opening | 20% (assumed, confirm from report logs) | 35%+ | Weekly per cohort, wk 5-12 |
| Coordinators still using auto-fill in their 4th week | New, no baseline | 90%+ of week 1 users | 4th week after each cohort goes live |
| Edit rate on auto-filled fields | New, no baseline | 5% or less | Weekly, wk 5-12 |
| Guardrail: reporting discrepancies | 0 | 0 | Daily from cohort 1 go-live |
| Guardrail: on-time daily report submission | Current rate per account (pull before wk 5) | No more than 2 pts below baseline | Weekly, wk 5-12 |

## Get your prior work ready
- **The feature you're launching from your M4 PRD, one feature, not a list:** B2 Smart Daily Report Auto-Fill. At end of shift the daily report opens as a draft built from dispatch, compliance, handoff and timeclock records. Fields it cant confirm stay blank and flagged, coordinator fixes those and submits.
- **What your M5 experiment told you, shipped / iterating, the evidence behind the launch:** The test runs inside the 4 week pilot (3 accounts, 50/50 by coordinator, 14 days). Launch only goes ahead if it meets the SHIP criteria from M5 (+15 pts at p < 0.05, week 2 holds 70% of week 1 lift, zero discrepancies, on-time submission within 2 pts, edit rate 5% or less). If it lands in ITERATE the launch waits till we fix the field mappings and rerun. Will update with the actual numbers once the pilot closes.
- **Your persona, pulled from your M2 persona, this anchors your audience:** Coordinator (dispatcher). Worst part of their day is end of shift - chasing driver updates on WhatsApp and texts because the board lags 20-60 min, then re-typing counts, compliance, fuel and overtime into the daily report.

## Set your goal, then your audience
- **Primary GTM goal, awareness · engagement · conversion, pick one:** Engagement
- **Why this goal? what makes this the right goal for this feature right now:** B2 turns on inside a product customers already pay for, so nobody needs to discover it or buy it. The risk is habit and trust - coordinators may keep re-typing out of habit, or stop trusting it after one wrong number on a report that goes to finance. M2 shows people already work around the product so winning back daily use is the main job. Retention and the at-risk renewal follow from that.
- **Target audience, the specific segment your goal implies, be precise:** Primary - coordinators/dispatchers at existing accounts who file the end of shift report on desktop (CSAT heatmap flags them on Scheduling and Compliance). Secondary - ops managers at those accounts who approve the rollout and read the reports, they need to know format, timing and finance feeds dont change. Not in scope - drivers (desktop only, they dont file it) and prospects.

## Size your launch
- **Launch tier, S (minimal) · M (targeted) · L (multi-channel) · XL (full GTM):** M, targeted
- **Justification, reach + revenue impact + what silence would risk:** Reach is existing accounts only. Revenue impact is indirect, it protects renewals, especially the enterprise account thats looking at the leaner competitor. Risk of silence is high - if a pre-filled report just shows up with no warning coordinators wont trust it and managers may escalate what looks like a changed report. Not L because press/events/paid would reach people who cant use it yet and we are a team of 4.
- **Is this a launch or a release? does it need go-to-market, or can it just ship?:** Technically a release (behind a flag, existing users, no pricing change) but it changes how a finance report gets filled in, so each account needs a heads up, a quick walkthrough and someone to call. So a small launch.

## Choose your top three channels and plan assets
- **Channel 1, owned / earned / paid, and why it reaches your audience:** Owned - in-app guided first use. First time a coordinator opens a pre-filled report they get a 3 step walkthrough (how to read Needs attention, fix a flagged field, submit). Submitted screen shows "Submitted in [N]s". Reaches them at the exact moment the new habit needs to form.
- **Channel 2, owned / earned / paid, and why:** Owned - CS led account rollout. Before the flag turns on for an account, CS lead does a 30 min call with the ops manager and lead coordinators, followed by an admin email with go-live date, one pager and who to call to turn it off. In B2B adoption goes through the account and managers need to hear "your reports dont change" from a person.
- **Channel 3, owned / earned / paid, and why:** Earned - pilot coordinator champions. Coordinators from the 3 pilot accounts share a quote and their before/after time, which goes in the release notes and one pager, and one champion joins each cohort's first call. Coordinators trust other coordinators more than they trust the vendor.
- **Enablement & assets, what Sales / CS / Support need, plus the assets to build:** Support - FAQ/troubleshooting guide (fallback to manual form, flagged/conflicting fields, flag off, late records), briefed a week before cohort 1. CS - rollout runbook with per account checklist, kill switch steps, link to the guardrail dashboard and a short enablement deck. AE for the at-risk account - one page brief on what changed for their coordinators with pilot numbers. Assets to build - in-app walkthrough, customer one pager, 90 sec demo video, release notes, admin go-live email, champion quote cards.

## Make it executable
- **Ownership, named owner per key activity, individual, not department:**

  | Activity | Owner |
  |---|---|
  | GTM plan, cohort go/no-go, post-launch decision | Product Manager (me) |
  | Feature flag cohorts, kill switch, guardrail monitoring | Engineering Lead |
  | In-app walkthrough and tracking | Engineer #2 |
  | Walkthrough design, one pager, enablement deck, quote cards | Product Designer |
  | Enablement calls, admin emails, champion program, support briefing, weekly guardrail check | CS Lead |
  | Renewal brief for the at-risk account | Account Executive for that account |

- **Budget & resource gaps, what costs extra, and any asset you can't currently build:** About $7,900 total, these are estimates.

  | Item | Cost |
  |---|---|
  | Freelance editor for the 90 sec demo video (nobody on the team does video) | $4,000 |
  | Contract writer for the one pager, release notes and emails | $3,000 |
  | Thank you gift cards for pilot champions (~9 coordinators x $100) | $900 |

  Everything else is existing tools or built in-house. Gap is that there is no product marketer so I own the messaging. Bottleneck is the CS lead's time, cohort size is capped at what one person can onboard in a week.
- **Timeline, Phase 1 beta → Phase 2 launch moment → Phase 3 post-launch:**
  - Phase 1 Beta (wk 1-4) - pilot on 3 accounts with the 14 day test inside it. B3 is live for all pilot coordinators from day one so both arms match. Collect champion quotes and before/after numbers, build the walkthrough and assets in parallel. End of wk 4 apply the M5 ship/iterate/kill rule.
  - Phase 2 Launch (wk 5-8) - support briefing in wk 4, cohort 1 in wk 5 (3 pilot accounts, the at-risk enterprise account and ~6 high volume accounts), cohort 2 in wk 6-7 (half the remaining), cohort 3 in wk 8 (all) plus public release notes. Each cohort only goes if guardrails held for the previous one.
  - Phase 3 Post-launch (wk 9-12) - track the dashboard weekly, CS lead audits 10 reports per account per week against the source records, collect feedback, make the decision in wk 12.

## Define how you'll know it worked
- **Success metrics, 2 to 3 metrics that match your GTM goal:**
  1. Reports submitted within 3 min - 35%+ (baseline 20%, assumed)
  2. Habit retention - 90%+ of week 1 users still using it in week 4
  3. Edit rate 5% or less. Low edits plus clean audits means they trust it because its actually right.

  Guardrails - zero discrepancies, on-time submission no more than 2 pts below baseline.
- **Bad signal to watch for:** Rubber stamping - reports get faster but the audit finds more than 1% wrong values, which means coordinators are just clicking submit without checking. Also if submit times drop but the WhatsApp group is still the real record, we fixed the report but not the workaround.
- **Most likely post-launch decision, double-down · iterate · pivot · deprioritize, and what would trigger it:** Double down. If all 3 metrics hit target and guardrails are clean through wk 12, reuse the same rules based pre-fill for B1 (compliance checklist) which is the next step in the shift close. If edit rate sits at 5-10% then iterate and fix the most flagged fields first. Any reporting discrepancy - kill switch for that cohort, find the cause and only resume after a full clean week.
