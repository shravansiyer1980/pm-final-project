# GTM Launch Plan - RouteLogic (B2B)

| | |
|---|---|
| **Project path** | RouteLogic, B2B logistics |
| **Feature** | B2 Smart Daily Report Auto-Fill (from M4 PRD) |
| **Goal** | Engagement |
| **Launch tier** | M, Targeted |

## Goal & Audience
**Goal:** Engagement. Get coordinators at existing customers to close every shift with the auto-filled daily report instead of re-typing it.

**Why this goal:** B2 isn't something we sell or that people need to find, it just turns on inside a product the customer already pays for. So the risk is not that nobody hears about it, its that coordinators keep re-keying out of habit or stop trusting the draft after one wrong value. M2 research already shows users working around RouteLogic ("We keep a WhatsApp group as the real system") so getting the daily use back is the job. Lower churn risk follows from that, including the enterprise account that's looking at a leaner competitor.

**Target audience:**
- Primary - coordinators and dispatchers at existing accounts who file the end of shift daily report. This is the M2 persona and the M3 CSAT heatmap flags them on Scheduling and Compliance.
- Secondary - ops managers at those accounts who approve the rollout and use the reports. They need to know report format, timing and finance feeds aren't changing.

## Launch Tier
M, Targeted.
- Reach is limited to existing accounts with coordinators who file daily reports. No new market, no prospects.
- Revenue impact is indirect but real - it protects renewals by taking away a daily frustration, which matters most for the enterprise account evaluating a competitor.
- Risk of silence is high inside the accounts and low outside. If it turns on with no warning coordinators may not trust a pre-filled report that goes to finance and keep re-typing, and managers may escalate what looks like a changed report.
- Not L because press, events or paid would go to prospects who cant use this yet, and its a 4 person team running the launch.
- Not S because going silent risks breaking trust in a report that feeds manager and finance reporting.

**Rollout (staged by cohort, gated on the M5 guardrails):**
- Cohort 1 (wk 5) - the 3 pilot accounts go to 100%, plus the at-risk enterprise account and ~6 high report volume accounts
- Cohort 2 (wk 6-7) - half of the remaining accounts
- Cohort 3 (wk 8) - all accounts

A cohort only moves forward if the previous one had zero reporting discrepancies and on-time submission no more than 2 pts below baseline. The per account kill switch stays on the whole time.

## Channels
**1. Owned - in-app guided first use.** First time a coordinator opens a pre-filled report they get a 3 step walkthrough (reading Needs attention, picking a conflict value, submitting). The submitted screen shows "Submitted in 48s" so they see the time saved every shift. This is where the behavior actually happens so its the most important channel.

**2. Owned - CS led account rollout.** CS Lead does a 30 min enablement call with each account's ops manager and lead coordinators before the flag turns on, then an admin email with the go-live date, a one pager and who to contact to turn it off. In B2B adoption goes through the account not the individual user, and managers need to hear "your reports dont change" from a person.

**3. Earned - pilot coordinator champions.** Coordinators at the 3 pilot accounts share a quote and how much their time to submit changed. These go in the release notes, one pager and enablement calls, and one champion joins each cohort's first call. Coordinators trust other coordinators more than the vendor, especially on something that feeds finance.

**Enablement & assets:**
- Support - internal FAQ and troubleshooting guide (fallback to manual form, conflict fields, flag off, late records). Briefed one week before cohort 1.
- CS - rollout runbook with per account checklist, kill switch steps and link to the guardrail dashboard, plus a short enablement deck (~10 slides).
- Account management (at-risk account) - short brief on what changed for their coordinators with the pilot numbers, for the renewal conversation.
- Assets to build - 3 step in-app walkthrough, customer one pager (what auto-fill does, what it never does, what managers see), 90 sec demo video, release notes, admin go-live email template, champion quote card for each pilot account.

## Ownership, Budget & Timeline
**Ownership** (all RouteLogic roles):

| Activity | Owner |
|---|---|
| GTM plan, cohort go/no-go, post-launch metrics and decision | Product Manager |
| Feature flag cohorts, kill switch, guardrail monitoring | Engineering Lead |
| In-app walkthrough and tracking for it | Engineer #2 |
| Walkthrough design, one pager, enablement deck, quote cards | Product Designer |
| Enablement calls, admin emails, champion program, support briefing, weekly guardrail check | CS Lead |
| Renewal brief for the at-risk account | Account Executive for that account (outside the core team) |

**Budget** (estimates, need to be replaced with real numbers):

| Item | Cost | Why |
|---|---|---|
| 90 sec demo video (freelance editor) | $4,000 | Nobody on the team does video |
| Contract product marketing writer (one pager, release notes, emails) | $3,000 | No product marketer on the team |
| Champion thank you gift cards (3 accounts x ~3 coordinators x $100) | $900 | For their quotes and time on calls |
| Meeting tools, in-app walkthrough | $0 | Existing tools, built in-house |
| **Total** | **~$7,900** | |

No product marketer means I own the messaging. CS capacity is the main bottleneck so cohort size is capped at what one CS Lead can onboard in a week.

**Timeline:**
- Phase 1 Beta (wk 1-4) - pilot on 3 accounts with the 14 day M5 test inside it. Collect champion quotes and before/after numbers, build walkthrough and assets in parallel. End of wk 4 apply the M5 rule - ship, iterate or kill.
- Phase 2 Launch (wk 5-8) - support briefing wk 4, cohort 1 wk 5 (enablement calls, admin emails, walkthrough on), cohort 2 wk 6-7, cohort 3 wk 8 (all accounts) plus public release notes. Each cohort only moves after a guardrail check.
- Phase 3 Post-launch (wk 9-12) - track metrics weekly, CS Lead audits 10 reports per account per week, collect coordinator feedback, decision in wk 12.

## Success Metrics
1. Reports submitted within 3 min - at least 35% across rolled out accounts, basically holding the M5 result at scale (baseline 20%, assumed)
2. Habit retention - at least 90% of coordinators who used auto-fill in week 1 still using it in week 4 without asking to turn it off
3. Edit rate - coordinators edit 5% or less of auto-filled fields. Low edits with clean audits means they accept the draft because its right.

Guardrails carried over from M5 - zero reporting discrepancies, on-time submission no more than 2 pts below baseline.

**Bad signal to watch for:** submissions get faster but the weekly audit error rate goes above 1%. That would mean coordinators are rubber stamping without reviewing and the speed is coming from skipping checks, not saved work. The other one is fast submits while the account's WhatsApp group is still the real record - we fixed the report but not the workaround.

**Most likely post-launch decision - double down.** If all 3 metrics are on target and guardrails are clean through wk 12, reuse the same rules based pre-fill for B1 (One-Click Compliance Checklist), the next step in the coordinators shift close. If edit rate is between 5-10% then iterate - fix the most flagged field mappings first before moving on to B1.
