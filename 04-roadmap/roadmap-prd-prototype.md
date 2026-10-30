# Build a dynamic, clickable feature roadmap (Module 4 · Lab 1)

**Project path:** Scenario B · RouteLogic Velocity. Team: 2 engineers, 1 designer, 1 CS lead. 4-week pilot, 3 accounts.

## Pre-Work · Your strategic anchors
- **Persona (M2), who are you solving for?:** Drivers and Coordinators.
- **Primary success metric (M3), your leading indicator:** Speed of processing through transactions (in seconds), with high accuracy and system reliability.
- **Moment of misery (M2), the specific friction blocking the goal:** There is unnecessary complexity in the solution, and it's causing users to move away from it. They run the real work in WhatsApp groups, texts and spreadsheets instead.
- **Guardrail metric (M3), what must not drop or break:** Managerial reporting and financials functionality must stay relevant and keep delivering at current levels.

## Step 1 · Review
- **My instinctive "quick wins" before touching the AI (2 to 3 feature IDs + why):**
  - B1, One-Click Compliance Checklist. The compliance step takes 14.6 minutes, and cutting it is the most direct way to save coordinator time.
  - B4, Mobile-First Coordinator Dashboard. Getting dispatch, compliance and handoff down to 2 taps looked like the fastest way to make the product feel simpler.

  In hindsight B4 isn't a quick win. It's a full mobile rebuild, and the scoring showed that.

## Step 2 · Baseline
Ran the "Senior PM" scoring prompt with the anchors above and the 10-feature backlog. The AI baseline came out as:
- **Quick Wins:** B1 (V5/E3), B3 (V4/E2)
- **Major Project:** B4 (V4/E5)
- **Time Sinkers:** B2 (V3/E4), B8 (V1/E4)
- **Fill-Ins:** B5, B6, B7, B9, B10

## Step 3 · Override
- **Where did you override the AI? (feature + old vs. new score + why):**
  - **B2 Smart Daily Report Auto-Fill: AI gave V3/E4 (Time Sinker). I scored it V4/E3 (Quick Win) and put it in Now.** The AI marked it down because it assumed an AI model writing the report, and that accuracy can't be proven in 4 weeks. My version doesn't use AI. It fills fields with fixed rules from data RouteLogic already has (dispatch, compliance, handoff, timeclock), and leaves a field blank whenever it isn't sure. That takes away most of the accuracy risk and a good chunk of the effort. On value, my journey map shows the worst point of a coordinator's shift is re-typing all of that into the daily report at shift close. This feature removes that step, so it hits my seconds-per-transaction metric directly.
  - **B6 Driver Alert Notifications: AI gave V3/E2 (Fill-In). I scored it V4/E4 (Major Project). It stays in Later.** M2 research rated late route reassignments as critical: 8 to 15 minutes before a driver sees a change, and no push notification at all. That's worth more than a 3. But the same research says there is no push today, so we would be building that infrastructure from scratch, and effort goes up to 4. Higher value, still not this sprint.
  - **B3 Shift Handoff Wizard: no score change (V4/E2). Moved from Next to Now.** With B2 taking about 5 of our 8 engineer-weeks, there was room for one more small item, and B3 is the cheapest real quick win on the list.

- **Did the AI over-value a Sales/Eng request your M2 interviews don't support?:** No. The only Sales item, B8 Fleet Analytics Manager View, already came back as a Time Sinker (V1/E4). Nothing in M2 asks for a new exec dashboard, and the guardrail only needs manager reporting to stay where it is, not grow. The Legal/CS item B9 was also scored low, which matches the research. So there was nothing to pull back here.

- **Did it underweight something your M3 cohort/funnel data strongly supports?:** Yes, two things.
  - **B2.** The backlog row says "AI auto-populates," and the AI scored that wording, not the problem. The underlying need is backed by M3 data (the feature's origin) and by the journey map. Built with rules instead of AI, it's a stronger and safer bet than the AI's score suggested.
  - **B6.** The AI treated alerts as a nice-to-have. M2 called the reassignment delay critical, and it's the reason dispatchers keep a WhatsApp group as "the real system." I raised the value but kept it in Later because of the build effort.

## Step 4 · Generate
- **My "Now" lane (this sprint), the 2 to 3 quick wins I'll build first:**
  - **B2 Smart Daily Report Auto-Fill (V4/E3).** Removes the shift-close re-typing for coordinators. About 5 engineer-weeks.
  - **B3 Shift Handoff Wizard (V4/E2).** Guided handoff that saves about 6.8 minutes a day. About 1 engineer-week.

  Together that's roughly 6 of the 8 engineer-weeks in the pilot, leaving some room for fixes. B3 goes live for every pilot coordinator on day one, so it doesn't skew the B2 test.

- **What I cut, and the "no" I'm protecting the scope from:**
  - **Cut:** B8 Fleet Analytics Manager View and B10 In-App Coordinator Training.
  - **B8:** it's the wrong persona, and it came from Sales, not from users. Saying no to it is saying no to building for the buyer instead of the people doing the work.
  - **B10:** training people to cope with a complicated product just accepts the complexity, and the pilot users are experienced coordinators anyway.
  - **Also held back:** B4 (Next), because it can't ship in 4 weeks with 2 engineers. The designer prototypes it during the pilot instead.
  - **The main "no":** no new screens, dashboards or reports during this pilot. Every Now item has to take steps away from the coordinator, not add them.

- **Prototype/roadmap screenshot link (paste into your deliverables):** `roadmap-screenshot.png`, a screenshot of `routelogic-velocity-roadmap.html` showing B2 and B3 in Now, B1 and B4 in Next, B5, B6, B7 and B9 in Later, and B8 and B10 in the Cut List.
