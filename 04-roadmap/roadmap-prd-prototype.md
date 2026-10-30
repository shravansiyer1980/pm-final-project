# Build a dynamic, clickable feature roadmap (Module 4 · Lab 1)

Scenario B - RouteLogic Velocity. Team of 2 engineers, 1 designer, 1 CS lead. 4 week pilot with 3 accounts.

## Pre-Work · Your strategic anchors
- **Persona (M2), who are you solving for?:** Drivers and Coordinators
- **Primary success metric (M3), your leading indicator:** Speed of processing transactions (in seconds) with high accuracy and system reliability
- **Moment of misery (M2), the specific friction blocking the goal:** Too much unnecessary complexity in the solution and its causing users to move away from it. The real work is happening in WhatsApp groups, texts and spreadsheets.
- **Guardrail metric (M3), what must not drop or break:** Managerial reporting and financials functionality must stay relevant and continue to deliver at current levels

## Step 1 · Review
- **My instinctive "quick wins" before touching the AI (2 to 3 feature IDs + why):**
  - B1 One-Click Compliance Checklist - compliance step takes 14.6 min, cutting that is the most direct way to save coordinator time
  - B4 Mobile-First Coordinator Dashboard - getting dispatch, compliance and handoff down to 2 taps seemed like the fastest way to make the product feel simpler

  Looking back B4 is not really a quick win, its a full mobile rebuild and the scoring showed that.

## Step 2 · Baseline
Ran the Senior PM scoring prompt with the anchors above and the 10 feature backlog. AI baseline:
- Quick Wins: B1 (V5/E3), B3 (V4/E2)
- Major Project: B4 (V4/E5)
- Time Sinkers: B2 (V3/E4), B8 (V1/E4)
- Fill-Ins: B5, B6, B7, B9, B10

## Step 3 · Override
- **Where did you override the AI? (feature + old vs. new score + why):**
  - B2 Smart Daily Report Auto-Fill - AI gave V3/E4 (Time Sinker), I changed it to V4/E3 (Quick Win) and put it in Now. The AI marked it down because it assumed an AI model writing the report and that the accuracy cant be proven in 4 weeks. My version doesn't use AI, it fills fields using fixed rules from data RouteLogic already has (dispatch, compliance, handoff, timeclock) and leaves the field blank if its not sure. That takes away most of the accuracy risk and some of the effort too. On the value side, the journey map shows the worst part of the coordinators shift is re-typing all this into the daily report, and this removes that step completely so it directly hits my seconds per transaction metric.
  - B6 Driver Alert Notifications - AI gave V3/E2 (Fill-In), I changed it to V4/E4 (Major Project), stays in Later. M2 research rated late route reassignments as critical (8-15 min before the driver sees it, no push notification). Thats worth more than a 3. But there is no push infrastructure today so we'd be building it from scratch, which is why effort goes up to 4. Higher value but not this sprint.
  - B3 Shift Handoff Wizard - no score change (V4/E2) but moved it from Next to Now. B2 takes about 5 of our 8 engineer weeks so there was room for one more small item and B3 is the cheapest real quick win.

- **Did the AI over-value a Sales/Eng request your M2 interviews don't support?:** No. The only sales item was B8 Fleet Analytics Manager View and the AI already scored it as a Time Sinker (V1/E4). Nothing in M2 asks for a new exec dashboard, the guardrail just needs manager reporting to stay where it is. B9 (Legal/CS) also scored low which matches the research. Nothing to pull back.

- **Did it underweight something your M3 cohort/funnel data strongly supports?:** Yes, B2 and B6. For B2 the backlog says "AI auto-populates" and the AI scored the wording, not the actual problem. The need is backed by the M3 data and the journey map, and with rules instead of AI its a safer bet than the score suggested. For B6 the AI treated alerts as a nice to have but M2 called the reassignment delay critical, its the reason dispatchers run a WhatsApp group. Raised the value but kept it in Later because of effort.

## Step 4 · Generate
- **My "Now" lane (this sprint), the 2 to 3 quick wins I'll build first:**
  - B2 Smart Daily Report Auto-Fill (V4/E3) - removes shift close re-typing for coordinators, about 5 engineer weeks
  - B3 Shift Handoff Wizard (V4/E2) - guided handoff, saves about 6.8 min a day, about 1 engineer week

  Thats about 6 of the 8 engineer weeks, leaves some room for fixes. B3 goes live for all pilot coordinators on day one so it doesn't skew the B2 test.

- **What I cut, and the "no" I'm protecting the scope from:** Cut B8 and B10. B8 is the wrong persona and it came from Sales not from users, it would be building for the buyer instead of the people actually doing the work. B10 is training people to deal with a complicated product which is just accepting the complexity, and the pilot users are experienced coordinators anyway. B4 pushed to Next since it cant ship in 4 weeks with 2 engineers, designer can prototype it during the pilot. Main "no" - no new screens, dashboards or reports during the pilot. Everything in Now has to take steps away from the coordinator, not add more.

- **Prototype/roadmap screenshot link (paste into your deliverables):** roadmap-screenshot.png in the repo (screenshot of the roadmap html). B2 and B3 in Now, B1 and B4 in Next, B5, B6, B7, B9 in Later, B8 and B10 cut.
