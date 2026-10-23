# B2 Smart Daily Report Auto-Fill, Simplified PRD (RouteLogic)

**Author:** Me · **Status:** Draft · **Target:** High-Fidelity Prototype · **Persona:** Managers

## 1. The Big Picture
- **Vision:** The daily report writes itself from what already happened, so coordinators confirm the shift in seconds instead of re-typing it.
- **Press release:** RouteLogic Launches Smart Daily Report Auto-Fill: The End of Re-Typing Your Shift

MINNEAPOLIS, [Date] — RouteLogic today announced Smart Daily Report Auto-Fill, part of its Velocity initiative to remove complexity for coordinators and drivers. Every day, coordinators re-enter data RouteLogic already holds: dispatches run, compliance checks completed, handoffs made. Auto-Fill ends that. When the shift closes, the daily report is already populated from the system of record. The coordinator reviews it, fixes anything flagged, and submits with one click. Fields the system can’t confirm are left blank and highlighted rather than guessed. In pilots across three accounts, coordinators cut report time by [X]%, with [Y]% of auto-filled fields accepted without edits.

For managers and finance teams, nothing changes except reliability. Reports arrive on time, in the same format, feeding the same dashboards, now with a clear marker showing each was auto-filled and human-reviewed. “Our coordinators told us they were spending the end of every shift repeating themselves to software,” said [Name], [Title] at RouteLogic. “Auto-Fill gives them that time back without asking anyone downstream to trust a black box.” Smart Daily Report Auto-Fill is available to [all customers / GA accounts] starting [date].

The bracketed figures are placeholders until the pilot instrumentation produces real numbers.
- **Success metric:** median time-to-submit for the daily report, in seconds. It’s measured from opening the report to submitting it, against the pre-pilot baseline for the same coordinators across the 3 accounts. This ties directly to your anchor of transaction speed in seconds.
- **Guardrail:** downstream reporting discrepancies, with a target of zero. This counts any mismatch in schema, field values, or timing between auto-filled reports and what managerial and finance dashboards expect. Track it alongside on-time report submission rate, which must stay at or above the current baseline. A breach on either one triggers the per-account kill switch.

## 2. The Details
### User stories
- US-1	As a coordinator, I want my daily report pre-filled from today’s dispatch, compliance, and handoff records, so I don’t re-type my shift.	Opening the report shows a populated draft within 3s.
- US-2	As a coordinator, I want to see which fields need my attention, so I review only those.	Flagged fields appear first, along with a count.
- US-3	As a coordinator, I want to edit any field inline, so I can correct errors without leaving the screen.	Edits save in place and use the same validation as today’s form.
- US-4	As a coordinator, I want to submit in one click after review, so closing my shift is fast.	Submit is enabled once required fields are valid.
- US-5	As a coordinator, I want the normal form if auto-fill fails, so I’m never blocked.	On error or timeout, the manual form loads with a banner.
- US-6	As a manager, I want reports in the same format and at the same time as today, so my reporting doesn’t change.	The payload is byte-for-byte schema-identical, and on-time rate is at or above baseline.
- US-7	As a manager, I want to know a report was auto-filled and human-reviewed, so I can trust it.	A badge appears in the existing report view, carried as metadata only.
- US-8	As a CS lead, I want to switch the feature on or off per account and see pilot metrics, so I can run and stop the pilot safely.	A config flag takes effect immediately, and metrics show up in the analytics pipeline.
### Screens to build
- Report Review (the core screen).
- A header showing the shift date and time window.
- A summary bar, for example “19 of 22 filled · 3 need attention.”
- A “Needs attention” group pinned at the top, followed by the remaining fields in the current report’s section order.
- A sticky Submit button.
- Field state components (a design-system piece, not a page). Four visual states:
- Auto-filled: neutral styling, with source shown on hover.
- Needs attention: blank or conflicting, highlighted.
- Edited: marked as changed by the user.
- Conflict: shows the competing values with a pick action.
- Submit confirmation. A light modal listing what the coordinator edited and any optional fields left blank, with Confirm and Back buttons. Skip it entirely when there were zero edits and zero blanks.
- Fallback state. The existing manual form with a single banner: “Auto-fill unavailable. Enter manually.” It needs no new layout.
- Submitted state. The existing confirmation screen, plus “Submitted in 48s.”
- Manager badge. A small “Auto-filled · Reviewed by [name]” tag on the existing report view, with no layout change.
### Functional requirements
- FR-1 Trigger: A draft is generated on demand when the coordinator opens the report. It pulls records within that coordinator’s actual shift window.
- FR-2 Field mapping: A documented table maps each report field to its source and rule. A field with no mapping stays blank.
- FR-3 Fill rules: A field is filled only when the rule returns exactly one valid value. Zero values, multiple values, conflicting values, or stale data all leave it blank and flagged.
- FR-4 Submit gating: Required fields must be valid before submitting. Optional flagged fields can be left blank, but the confirmation modal lists them.
- FR-5 Editing: Every field is editable and uses the existing form’s validation rules unchanged.
- FR-6 Submission: Reports go to the existing endpoint with the existing payload schema, with no added or renamed fields.
- FR-7 Audit record: For each field, store the auto-filled value, the submitted value, and the source reference, in storage separate from the report payload.
- FR-8 Fallback: If the draft fails or isn’t ready within 5s, load the manual form and log the failure.
- FR-9 Feature flag: On or off per account, effective on the next report open, with no data migration either way.
- FR-10 Instrumentation: Log these events:
- report_opened
- autofill_completed, with latency and counts of filled and flagged fields
- field_edited
- report_submitted, with duration in seconds
- autofill_failed
- FR-11 Manager marker: The “auto-filled & reviewed” indicator is carried as metadata outside the report schema.
### Smart behaviors (Situation → Outcome)
- Blank beats guess. Any uncertainty produces a flagged blank, never a plausible-looking value.
- Attention-first ordering. Flagged fields float to the top so review time scales with problems, not with form length.
- Cross-source conflict detection. When two sources disagree, for example a stop count from dispatch versus compliance, the field shows both values and the coordinator picks one in a single tap.
- Staleness refresh. If a source record changes after the draft was generated, a prompt appears: “2 values updated, refresh?” Values the coordinator has already edited are never overwritten.
- Shift-window awareness. Uses actual clock-in and clock-out times and handles overnight shifts that cross midnight correctly.
- No carry-forward from yesterday. Values come only from today’s system records, never copied from the previous report. This prevents silent repetition of stale data.
- Zero-friction path. If nothing is flagged or edited, Submit goes straight through without the confirmation modal.
### Technical constraints
- Capacity: 2 engineers for 4 weeks. Add one draft-generation endpoint, no new services, and no new data stores beyond the audit table.
- Read-only sources: Dispatch, compliance, and handoff data are read and never written.
- Performance: Drafts must generate in under 3s at p95, with a hard timeout at 5s that triggers the fallback.
- Guardrail contract tests: Automated tests must assert that the submission payload matches the current schema exactly, and they block deploy on failure.
- Data boundaries: Use only RouteLogic internal data, with tenant isolation enforced. No third-party or external AI calls in the pilot.
- Platform: Desktop web only. Mobile depends on B4.
- Retention: The audit record follows existing report retention policy.
- Rollback: Turning the flag off restores the current experience immediately with zero residual state.

## 3. The Logistics
### Features out
- Generative AI narrative or summary text
- Auto-submit at any confidence level
- Report schema, template, or field changes
- Custom or configurable report types
- Weekly, incident, or other report types
- Auto-fill on amendments to submitted reports
- Backfilling historical reports
- Third-party data not already ingested by RouteLogic
- Per-account learning or model tuning
- Shift-end reminders and notifications
- Mobile submission
- Admin UI for flags and metrics
- Manager analytics or drill-down
- Localization
### Edge cases & safety guard
- Edge cases:
- 1	Overnight shift crosses midnight	Use the actual clock-in and clock-out window, not the calendar day.
- E2	Shift with zero activity	Show “0” only if the source confirms zero. If the data is missing, leave the field blank and flagged.
- E3	A source system is partially down	Fill what’s available and flag the rest as “Source unavailable.” Don’t trigger a full fallback unless every source fails.
- E4	Late-arriving records sync after the draft opens	Show a staleness prompt, and refresh only fields the coordinator hasn’t edited.
- E5	Duplicate source records	Deduplicate on record ID. If they still conflict, flag the field.
- E6	Two coordinators share a shift or depot	Attribute records by assigned coordinator. If attribution is ambiguous, flag rather than split.
- E7	A source value fails existing form validation	Don’t fill it. Flag it as “Source value invalid.”
- E8	Account time zone differs from the device time zone	The shift window uses the account time zone, and the displayed times show the zone.
- E9	Report opened in two tabs	The last submit wins, using existing behavior, and a second submit attempt shows “Already submitted.”
- E10	Feature flag turned off mid-session	The open draft can still be submitted. The next open uses the manual form.
- E11	Report already submitted, coordinator reopens it	Existing view or amend flow, with no auto-fill.
- E12	Very high-volume shift	The p95 under 3s still applies, and the 5s timeout falls back.
- E13	Coordinator edits a field, then refreshes the draft	User edits are never overwritten.
- Safety guards
- Contract tests block deploy. If the payload schema differs from production in any way, the build fails.
- Per-account kill switch. It takes effect on the next report open.
- Mandatory human review. No code path submits without a user action.
- Tenant isolation assertion. Every source query is scoped by account ID, and a query without an account ID fails closed.
- Daily discrepancy monitor. Auto-filled reports are compared against what manager and finance dashboards ingested, and any mismatch alerts the CS lead and engineering.
- Edit-rate alarm. If one account exceeds the threshold in a day, for example 10% of auto-filled fields edited, the CS lead is alerted and decides whether to switch the feature off.
- Rubber-stamp check. The CS lead manually audits a weekly sample of 10 reports per account against the source records. This catches errors that coordinators approved without noticing, which the edit rate can’t see.
- Audit trail. The auto-filled value, submitted value, and source reference are kept for every field.
### Decision log
- #	Decision	Alternatives considered	Rationale	Status
- D1	Rules-based fill, with no generative AI in the pilot	LLM extraction across sources	Accuracy is part of the Primary Metric, and an LLM can’t be validated in 4 weeks.	Decided
- D2	Leave a field blank rather than guess	Best-guess with a confidence score	One wrong value in a finance-bound report costs more trust than ten blanks.	Decided
- D3	Mandatory human review	Auto-submit above a confidence threshold	Guardrail and accountability.	Decided
- D4	Report schema frozen, and the manager badge carried as metadata	Add an “auto-filled” field to the schema	Any schema change risks downstream reporting.	Decided
- D5	Generate the draft on demand when the report opens	Batch-generate at shift end	It’s fresher, simpler, and needs no scheduler.	Decided
- D6	No carry-forward from the previous day’s report	Pre-fill static fields from yesterday	It silently repeats stale data.	Decided
- D7	Desktop web only	Responsive or mobile	Capacity, and mobile depends on B4.	Decided
- D8	No admin UI	A simple CS dashboard	Config and analytics are enough for 3 accounts.	Decided
- D9	The coordinator submits and the manager consumes	Treat the manager as the sole persona	The coordinator owns the time saving, and the manager owns the guardrail.	Decided
- D10	Proceed with B2 in the pilot despite its Time Sinker score	Pilot B1 or B3 instead	Accepted risk. Mitigated by D1–D4, the kill switch, contract tests, and the exit criteria.
### Evals
- Eval	Method	Pass gate
- False-fill rate	Replay the last 30 days of source data for the 3 accounts and compare drafts against what was actually submitted. Manually review mismatches to separate system errors from human errors in the historical reports.	Under 1% of filled fields wrong
- Coverage	Same replay: the share of fields filled versus flagged.	At least 70% filled. Below that, the time saving doesn’t materialize.
- Edge-case suite	Synthetic test cases for E1–E13.	100% pass
- Schema contract	Automated payload comparison against production.	Exact match
- Latency	Load test at pilot volume, including high-volume shifts.	p95 under 3s

## MoSCoW scope
- **Must:** Auto-populate from existing system-of-record data only. Pull from the dispatch, compliance, and handoff records RouteLogic already holds, and map structured fields deterministically. This is the core time saving with no new data dependencies.; Leave a field blank and flag it rather than guess. Any field that can’t be sourced with confidence stays empty and is highlighted. One silently wrong number in a finance-bound report destroys trust faster than ten blank fields.; Human review with a one-click submit. Nothing submits without explicit confirmation. Accountability stays with the coordinator, and this is the guardrail’s last line of defense.; Inline field editing. The reviewer corrects a value in place, and the edit overrides the auto-filled value. Without it, users work around the feature, which is your Moment of Misery all over again.; Output identical to today’s report. Same fields, formats, and submission endpoint, so managerial and finance consumers see no difference. This is the guardrail, and it’s non-negotiable.; Fail safe to the current manual form. If auto-fill errors or times out, the existing form loads empty and nothing is lost. The metric includes reliability, and a broken auto-fill can’t block reporting.; Per-account feature flag. On for the 3 pilot accounts, instantly reversible. You need a kill switch for a pilot touching finance-bound data.; Instrumentation. Capture time-to-submit in seconds and the edit rate per field. Without this, the pilot can’t prove accuracy, which was the reason B2 was scored a Time Sinker.
- **Should:** Field provenance on hover, such as “from Dispatch #4471.” It speeds up review and builds trust.; Audit log of auto-filled versus submitted values. It shows finance and compliance which numbers a human changed, and it serves as training data for later.; Manager-side “auto-filled & reviewed” marker. Managers know the report’s origin without any change to its format.; Draft save and resume. Coordinators get interrupted mid-shift.
- **Could:** AI-generated narrative and summary text. This is free text with the highest hallucination risk, so add it only after structured accuracy is proven.; Anomaly highlighting, flagging values outside each account’s normal range.; Learning from edits, where fields corrected often get tuned per account.; An auto-drafted report and reminder at shift end.; A mobile submit flow, which depends on B4.
- **Won't (now):** Auto-submit without review, under any confidence level.; Any change to report schema, fields, templates, or downstream manager and finance dashboards.; New or customizable report types.; Manager analytics or drill-down. That’s B8, and it was cut.; External or third-party data sources not already inside RouteLogic.; Per-account model training or fine-tuning.; Bulk or multi-day submission, backfilling past reports, and localization.

---
**Builder hook:** Build a working prototype based on this PRD. Use the User Story as the core flow, Functional Requirements as build constraints, and prioritize speed and clarity over visual complexity.
