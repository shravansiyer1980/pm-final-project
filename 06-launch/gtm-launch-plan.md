# GTM Launch Plan · RouteLogic (B2B)

| | |
|---|---|
| **Project path** | RouteLogic · B2B logistics |
| **Feature** | B2 Smart Daily Report Auto-Fill (M4 PRD) |
| **Goal** | Engagement |
| **Launch tier** | M, Targeted |

## Goal & Audience
**Goal:** Engagement. Get coordinators at existing customers to close every shift with the auto-filled daily report instead of re-typing it.

**Why this goal:**
- B2 isn't sold or discovered; it switches on inside a product the customer already pays for. The risk isn't that nobody hears about it. The risk is that coordinators keep re-keying out of habit, or stop trusting the draft after one wrong value.
- The M2 research shows users already route around RouteLogic (*"We keep a WhatsApp group as the real system"*). Winning daily use back is the job.
- The business outcome follows from that engagement: lower churn risk, including the enterprise account evaluating a leaner competitor (M2 synthesis).

**Target audience:**
- **Primary:** coordinators and dispatchers at existing RouteLogic customer accounts who file the end-of-shift daily report. They are the M2 persona, and the M3 CSAT heatmap flags them on Scheduling and Compliance.
- **Secondary:** the ops managers at those accounts who approve the rollout and consume the reports. They need to know that report format, timing and finance feeds are unchanged.

## Launch Tier
**M, Targeted.**
- **Reach:** limited to existing customer accounts with coordinators who file daily reports. There's no net-new market and no prospects in scope.
- **Revenue impact:** indirect but real. The feature protects renewals by removing a daily frustration, which matters most for the enterprise account evaluating a competitor.
- **Risk of silence:** high inside accounts, low outside them. If the feature switches on unannounced, coordinators may distrust a pre-filled finance-bound report and keep re-typing. Managers may also escalate a "changed" report.
- **Why not L:** press, events or paid channels would aim at prospects who can't use this feature yet, with a 4-person team carrying the launch.
- **Why not S:** silence risks breaking trust in a report that feeds manager and finance reporting.

**Rollout, staged by cohort, gated on the M5 guardrails:**
- **Cohort 1 (week 5):** the 3 pilot accounts go to 100%, plus the at-risk enterprise account and about 6 accounts with high report volume.
- **Cohort 2 (week 6–7):** 50% of remaining accounts.
- **Cohort 3 (week 8):** all accounts.

A cohort only advances if the previous one had zero reporting discrepancies and on-time submission no more than 2 points below baseline. The per-account kill switch (PRD FR-9) stays live throughout.

## Channels
**1 · Owned: in-app guided first use.**
- The first time a coordinator opens a pre-filled report, a 3-step walkthrough shows how to read "Needs attention," pick a conflict value, and submit.
- The submitted screen shows "Submitted in 48s," so the time saved is visible every shift.
- This channel is where the behavior happens, at the exact moment it matters.

**2 · Owned: CS-led account rollout.**
- The CS Lead runs a 30-minute enablement call with each account's ops manager and lead coordinators before the account's flag turns on.
- An admin email follows with the go-live date, a one-pager and the kill-switch contact.
- B2B adoption runs through the account, not the individual user, and managers need to hear "your reports don't change" from a person.

**3 · Earned: pilot coordinator champions.**
- Coordinators at the 3 pilot accounts share before-and-after results: a quote and their time-to-submit change. These go into release notes, the one-pager and the enablement calls.
- One champion joins each cohort's first enablement call.
- Coordinators trust other coordinators more than a vendor claim, especially about a report that feeds finance.

**Enablement & assets:**
- **Support:** an internal FAQ and troubleshooting guide covering fallback to the manual form, conflict fields, the flag being off, and late-arriving records. Support is briefed one week before Cohort 1 goes live.
- **CS:** a rollout runbook with the per-account checklist, kill-switch procedure and guardrail dashboard link, plus a 10-slide enablement deck.
- **Account management (at-risk enterprise account):** a short "what changed for your coordinators" brief with pilot numbers, used at the renewal conversation.
- **Assets to build:**
  - The in-app walkthrough (3 steps).
  - A customer one-pager on what auto-fill does, what it never does, and what managers see.
  - A 90-second demo video.
  - Release notes.
  - The admin go-live email template.
  - A champion quote card for each pilot account.

## Ownership, Budget & Timeline
**Ownership.** All owners are RouteLogic roles:

| Activity | Owner |
|---|---|
| GTM plan, cohort go/no-go, post-launch metrics and decision | RouteLogic Product Manager |
| Feature flag cohorts, kill switch, guardrail monitoring (discrepancy monitor, contract tests) | RouteLogic Engineering Lead |
| In-app walkthrough, telemetry for walkthrough completion | RouteLogic Engineer #2 |
| Walkthrough design, one-pager, enablement deck, quote cards | RouteLogic Product Designer |
| Enablement calls, admin emails, champion program, Support briefing, weekly guardrail check | RouteLogic CS Lead |
| Renewal brief for the at-risk enterprise account | RouteLogic Account Executive for that account (outside the core team) |

**Budget.** *These figures are assumptions; replace them with real numbers.*

| Item | Assumed cost | Why |
|---|---|---|
| 90-second demo video (freelance editor) | $4,000 | No video capability on the team |
| Contract product-marketing writer (one-pager, release notes, email copy) | $3,000 | Asset gap: no product marketer on a PM + 2 eng + designer + CS team |
| Champion thank-you (gift cards, 3 pilot accounts × ~3 coordinators × $100) | $900 | Recognition for quotes and call time |
| Webinar/meeting tooling, in-app walkthrough | $0 | Existing tools; built in-house |
| **Total** | **~$7,900** | |

**Asset gap to flag:** with no product marketer, the PM owns messaging. CS capacity is the main bottleneck, so cohort size is capped at what one CS Lead can enable in a week.

**Timeline:**
- **Phase 1, Beta (weeks 1–4):**
  - The pilot runs on 3 accounts, with the M5 14-day experiment inside it.
  - Collect champion quotes and before-and-after numbers.
  - Build the walkthrough and assets in parallel.
  - At the end of week 4, apply the M5 ship rule: SHIP, ITERATE or KILL.
- **Phase 2, Launch (weeks 5–8):**
  - Week 4: Support briefing.
  - Week 5: Cohort 1 goes live, with enablement calls, admin emails and the walkthrough on.
  - Weeks 6–7: Cohort 2.
  - Week 8: Cohort 3 (all accounts) and public release notes.
  - Each cohort advances only after a guardrail check.
- **Phase 3, Post-launch (weeks 9–12):**
  - Track weekly metrics and run the CS Lead's rubber-stamp audit (10 reports per account per week).
  - Collect coordinator feedback.
  - Make the post-launch decision in week 12.

## Success Metrics
These metrics measure engagement, not reach.

1. **Reports submitted within 3 minutes:** at least **35%** of daily reports across rolled-out accounts, holding the M5 result at scale (baseline 20%, assumed).
2. **Week-4 retention of the new habit:** at least **90%** of coordinators who submitted via auto-fill in their first week are still doing so in their fourth week, without asking for the flag to be turned off.
3. **Trust signal, edit rate:** coordinators edit **5% or fewer** of auto-filled fields. Low edits with passing audits means they accept the draft because it's right.

**Guardrails held from M5:** zero downstream reporting discrepancies, and on-time submission no more than 2 points below baseline.

**Bad signal to watch for:** submissions get faster but the weekly audit error rate rises above 1%. That means coordinators are rubber-stamping drafts without reviewing them, and the speed is coming from skipped checks, not saved work. A second bad signal is fast submits while the account's WhatsApp group stays the real record. That would mean the feature closed the report but not the workaround.

**Most likely post-launch decision: double down.**
- **Trigger:** all three metrics on target and guardrails clean through week 12.
- **Action:** reuse the same rules-based pre-fill engine for B1 (One-Click Compliance Checklist) and B3 (Shift Handoff Wizard), the next steps on the coordinator's shift-close journey.
- **Fallback:** if the edit rate sits between 5% and 10%, **iterate**: fix the most-flagged field mappings before expanding to B1.
