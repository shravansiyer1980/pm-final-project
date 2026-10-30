# GTM Strategy & Success Dashboard

> **Module 6 · ★ Deliverable 6.** Repo file `06-launch/gtm-and-dashboard.md`. Becomes the GTM Strategy & Success Dashboard slide of the Module 6 final deck.

## Go-to-market summary
We're launching **B2 Smart Daily Report Auto-Fill** to coordinators at existing RouteLogic accounts. The goal is **engagement**: coordinators close every shift with the pre-filled report instead of re-typing it.

It's an **M (targeted) launch** with no press and no paid spend, rolled out in three cohorts over weeks 5–8 after the pilot. Each cohort only goes live if the previous one kept manager and finance reporting clean.

There are three channels:
- **In-app guided first use** (owned)
- **CS-led rollout calls with each account** (owned)
- **Pilot coordinators sharing their own before-and-after results** (earned)

The strategy is to protect what customers already buy RouteLogic for, which is reporting, while taking away the end-of-shift re-keying that pushes coordinators onto WhatsApp and spreadsheets.

## Success dashboard

| KPI | Baseline | Target | Measurement window |
|---|---|---|---|
| Daily reports submitted within 3 minutes of opening | 20% (assumed; confirm from report logs) | ≥ 35% | Weekly, per cohort, weeks 5–12 |
| Coordinators still using auto-fill in their 4th week (habit retention) | New metric, no baseline | ≥ 90% of week-1 users | 4th week after each cohort goes live |
| Edit rate on auto-filled fields (trust / accuracy) | New metric, no baseline | ≤ 5% of auto-filled fields | Weekly, weeks 5–12 |
| **Guardrail:** downstream reporting discrepancies | 0 | 0 | Daily, from cohort 1 go-live |
| **Guardrail:** on-time daily report submission | Current rate per account (pull before week 5) | No more than 2 points below baseline | Weekly, weeks 5–12 |

---

## Get your prior work ready
- **The feature you're launching from your M4 PRD, one feature, not a list:** B2 Smart Daily Report Auto-Fill. At shift end, the daily report opens as a draft built from the dispatch, compliance, handoff and timeclock records RouteLogic already has. Fields it can't confirm stay blank and flagged. The coordinator fixes those and submits in one click.
- **What your M5 experiment told you, shipped / iterating, the evidence behind the launch:** The M5 test runs inside the 4-week pilot: 3 accounts, 50/50 by coordinator, 14 days. This launch only goes ahead if it hits the SHIP rule:
  - At least +15 points on reports submitted within 3 minutes (20% → 35%), at p < 0.05
  - Week 2 holds at least 70% of week 1's lift
  - Zero reporting discrepancies
  - On-time submission no more than 2 points below control
  - Edit rate at or below 5%

  If it lands in ITERATE instead, the launch waits while we fix the most-flagged field mappings and rerun the test. The real numbers go here once the pilot closes.
- **Your persona, pulled from your M2 persona, this anchors your audience:** The Coordinator (dispatcher). Their worst moment is the end of the shift, when they chase driver updates on WhatsApp and texts because the board lags 20–60 minutes. They then re-type the counts, compliance figures, fuel and overtime into the daily report.

## Set your goal, then your audience
- **Primary GTM goal, awareness · engagement · conversion, pick one:** Engagement.
- **Why this goal? what makes this the right goal for this feature right now:**
  - B2 switches on inside a product our customers already pay for, so nobody needs to discover it or buy it.
  - The real risk is habit and trust. Coordinators may keep re-typing out of habit, or stop trusting the draft after one wrong number in a report that feeds finance.
  - M2 shows people already work around the product ("We keep a WhatsApp group as the real system"), so winning back daily use is the job.
  - Retention and the at-risk renewal come from that, not from awareness.
- **Target audience, the specific segment your goal implies, be precise:**
  - **Primary:** coordinators and dispatchers at existing RouteLogic accounts who file the end-of-shift daily report on desktop. The M3 CSAT heatmap flags this group on Scheduling and Compliance.
  - **Secondary:** the ops managers at those accounts, who approve the rollout and read the reports. They need to hear that report format, timing and finance feeds don't change.
  - **Not in scope:** drivers, since this is desktop only and they don't file the report, and prospects.

## Size your launch
- **Launch tier, S (minimal) · M (targeted) · L (multi-channel) · XL (full GTM):** M, targeted.
- **Justification, reach + revenue impact + what silence would risk:**
  - **Reach:** existing accounts only, the coordinators who file daily reports.
  - **Revenue impact:** indirect. It protects renewals by removing a daily frustration, which matters most for the enterprise account that's evaluating a leaner competitor.
  - **Risk of silence:** high inside the accounts. If a pre-filled report shows up with no warning, coordinators may not trust it and keep re-typing, and managers may escalate what looks like a changed report.
  - **Why not L:** press, events or paid channels would reach people who can't use this yet, and we're a team of 4.
- **Is this a launch or a release? does it need go-to-market, or can it just ship?:** A launch, but a small one. On paper it's a release, since the feature is behind a flag for existing users with no pricing or packaging change. But it changes how a finance-bound report gets filled in, so each account needs a heads-up, a quick walkthrough and someone to call. Shipping it silently would put the guardrail and adoption at risk.

## Choose your top three channels and plan assets
- **Channel 1, owned / earned / paid, and why it reaches your audience:** Owned: in-app guided first use.
  - The first time a coordinator opens a pre-filled report, a 3-step walkthrough shows how to read "Needs attention," fix a flagged field, and submit.
  - The submitted screen shows "Submitted in [N]s."
  - It reaches the coordinator at the exact moment the new habit has to form.
- **Channel 2, owned / earned / paid, and why:** Owned: CS-led account rollout.
  - Before an account's flag turns on, the CS lead holds a 30-minute call with the ops manager and lead coordinators.
  - An admin email follows with the go-live date, a one-pager and who to call to switch it off.
  - In B2B, adoption goes through the account, and managers need to hear "your reports don't change" from a person.
- **Channel 3, owned / earned / paid, and why:** Earned: pilot coordinator champions.
  - Coordinators from the 3 pilot accounts share a quote and their own before-and-after time. These go in the release notes and one-pager, and one champion joins each cohort's first call.
  - Coordinators trust other coordinators more than a vendor claim, especially about a report that feeds finance.
- **Enablement & assets, what Sales / CS / Support need, plus the assets to build:**
  - **Support:** an FAQ and troubleshooting guide covering fallback to the manual form, flagged and conflicting fields, the flag being off, and late-arriving records. Support is briefed a week before cohort 1.
  - **CS:** a rollout runbook with a per-account checklist, kill-switch steps and a link to the guardrail dashboard, plus a short enablement deck.
  - **Account Executive (at-risk enterprise account):** a one-page "what changed for your coordinators" brief with pilot numbers.
  - **Assets to build:**
    - The in-app walkthrough
    - A customer one-pager on what auto-fill does, what it never does, and what managers see
    - A 90-second demo video
    - Release notes
    - The admin go-live email
    - Champion quote cards

## Make it executable
- **Ownership, named owner per key activity, individual, not department:**

  | Activity | Owner |
  |---|---|
  | GTM plan, cohort go/no-go, post-launch metrics and decision | RouteLogic Product Manager (me) |
  | Feature-flag cohorts, kill switch, guardrail monitoring | RouteLogic Engineering Lead |
  | In-app walkthrough and its tracking | RouteLogic Engineer #2 |
  | Walkthrough design, one-pager, enablement deck, quote cards | RouteLogic Product Designer |
  | Enablement calls, admin emails, champion program, Support briefing, weekly guardrail check | RouteLogic CS Lead |
  | Renewal brief for the at-risk enterprise account | RouteLogic Account Executive for that account |

- **Budget & resource gaps, what costs extra, and any asset you can't currently build:** About $7,900 total. *These figures are assumptions.*

  | Item | Cost |
  |---|---|
  | 90-second demo video (freelance editor, no video skills on the team) | $4,000 |
  | Contract product-marketing writer for the one-pager, release notes and emails | $3,000 |
  | Thank-you gift cards for pilot champions (about 9 coordinators × $100) | $900 |

  Everything else uses existing tools or is built in-house.
  - **Gap:** there's no product marketer, so I own the messaging.
  - **Bottleneck:** the CS lead's time. Cohort size is capped at what one person can onboard in a week.
- **Timeline, Phase 1 beta → Phase 2 launch moment → Phase 3 post-launch:**
  - **Phase 1, Beta (weeks 1–4):**
    - The pilot runs on 3 accounts, with the 14-day M5 test inside it. B3, the Shift Handoff Wizard, goes live for every pilot coordinator on day one, so both test arms match.
    - Collect champion quotes and before-and-after numbers.
    - Build the walkthrough and assets in parallel.
    - At the end of week 4, apply the M5 rule: ship, iterate or kill.
  - **Phase 2, Launch (weeks 5–8):**
    - Week 4: Support briefing.
    - Week 5: cohort 1 (the 3 pilot accounts, the at-risk enterprise account and about 6 high-volume accounts).
    - Weeks 6–7: cohort 2 (half of the remaining accounts).
    - Week 8: cohort 3 (all accounts) and public release notes.
    - Each cohort goes ahead only if the guardrails held for the one before.
  - **Phase 3, Post-launch (weeks 9–12):**
    - Track the dashboard weekly.
    - The CS lead audits 10 reports per account per week against the source records.
    - Collect coordinator feedback.
    - Make the post-launch decision in week 12.

## Define how you'll know it worked
- **Success metrics, 2 to 3 metrics that match your GTM goal:**
  1. **Reports submitted within 3 minutes:** at least 35% across rolled-out accounts (baseline 20%, assumed).
  2. **Habit retention:** at least 90% of coordinators who used auto-fill in their first week are still using it in their fourth.
  3. **Edit rate:** at or below 5% of auto-filled fields. Low edits plus clean audits means they trust the draft because it's right.

  Guardrails: zero reporting discrepancies, and on-time submission no more than 2 points below baseline.
- **Bad signal to watch for:**
  - **Rubber-stamping.** Reports get faster but the weekly audit finds more than 1% wrong values, which means coordinators are clicking Submit without reviewing.
  - **Report fixed, workaround intact.** Submit times drop while the account's WhatsApp group is still the real record. That means we fixed the report but not the workaround.
- **Most likely post-launch decision, double-down · iterate · pivot · deprioritize, and what would trigger it:** Double down.
  - **Trigger:** all three metrics on target and the guardrails clean through week 12.
  - **What that means:** reuse the same rules-based pre-fill for B1 (One-Click Compliance Checklist), the next step in the coordinator's shift-close routine.
  - **If the edit rate sits between 5% and 10%:** iterate. Fix the most-flagged fields first, before expanding.
  - **If any reporting discrepancy shows up:** stop that cohort with the kill switch, find the cause, and only resume once a full week is clean.
