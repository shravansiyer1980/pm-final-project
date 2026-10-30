# B2 Smart Daily Report Auto-Fill, Simplified PRD (RouteLogic)

**Author:** Me · **Status:** Draft · **Target:** High-Fidelity Prototype · **Persona:** Coordinators (submit the report) · Managers (consume it)

## 1. The Big Picture
- **Vision:** The daily report writes itself from what already happened, so coordinators confirm the shift in seconds instead of re-typing it.
- **Press release:** RouteLogic Launches Smart Daily Report Auto-Fill: The End of Re-Typing Your Shift

MINNEAPOLIS, [Date] — RouteLogic today announced Smart Daily Report Auto-Fill, part of its Velocity initiative to remove complexity for coordinators and drivers. Every day, coordinators re-enter data RouteLogic already holds: dispatches run, compliance checks completed, handoffs made. Auto-Fill ends that. When the shift closes, the daily report is already populated from the system of record. The coordinator reviews it, fixes anything flagged, and submits with one click. Fields the system can't confirm are left blank and highlighted rather than guessed. In pilots across three accounts, coordinators cut report time by [X]%, with [Y]% of auto-filled fields accepted without edits.

For managers and finance teams, nothing changes except reliability. Reports arrive on time, in the same format, feeding the same dashboards. "Our coordinators told us they were spending the end of every shift repeating themselves to software," said [Name], [Title] at RouteLogic. "Auto-Fill gives them that time back without asking anyone downstream to trust a black box." Smart Daily Report Auto-Fill is available to [all customers / GA accounts] starting [date].

The bracketed figures are placeholders until the pilot instrumentation produces real numbers.

- **Success metric:** Median time-to-submit for the daily report, in seconds, measured from opening the report to submitting it. It is compared against the pre-pilot baseline for the same coordinators across the 3 accounts. This ties directly to the Velocity anchor of transaction speed in seconds.
- **Guardrail:** Downstream reporting discrepancies, with a target of zero. This counts any mismatch in schema, field values or timing between auto-filled reports and what managerial and finance dashboards expect. On-time report submission rate is tracked alongside it and must stay at or above the current baseline. A breach on either one triggers the per-account kill switch.

## 2. The Details
### User stories
| # | Story | Acceptance criteria |
|---|---|---|
| US-1 | As a coordinator, I want my daily report pre-filled from today's dispatch, compliance and handoff records, so I don't re-type my shift. | Opening the report shows a populated draft within 3s. Any field without exactly one valid source value is blank and flagged. |
| US-2 | As a coordinator, I want to see which fields need my attention, fix them inline and submit in one click, so closing my shift is fast. | Flagged fields appear first with a count. Every field is editable using today's validation. Submit is enabled once all required fields are valid. |
| US-3 | As a manager, I want reports in the same format and at the same time as today, so my reporting doesn't change. | The payload is schema-identical to today's report, and the on-time rate is at or above baseline. |

### Screens to build (3)
1. **Entry point: Open daily report**
   - Header with shift date, time window and time zone.
   - Loading state ("Building your draft from today's records…") listing the sources being read.
   - If the draft fails or takes longer than 5s, the existing manual form loads with one banner: "Auto-fill unavailable. Enter manually."
2. **Feature core: Report Review**
   - Summary bar, e.g. "15 of 18 filled · 3 need attention."
   - "Needs attention" group pinned at the top, followed by the remaining fields in the current report's section order.
   - Field states:
     - **Auto-filled:** neutral styling, with the source shown on hover.
     - **Needs attention:** blank, with the reason stated.
     - **Edited:** marked as changed by the user.
     - **Error:** fails today's validation, with an inline fix message.
   - Sticky Submit button with a hint listing any required fields still to fix.
3. **Success: Submitted confirmation**
   - The existing confirmation screen, plus "Submitted in [N]s."
   - Count of auto-filled values the coordinator edited.

### Functional requirements (from MUST HAVE)
- **FR-1 Trigger:** A draft is generated when the coordinator opens the report. It pulls only records inside that coordinator's actual shift window (clock-in to clock-out, in the account's time zone).
- **FR-2 Fill rule:** Each report field maps to one documented source and rule. A field is filled only when its rule returns exactly one valid value. Zero values, multiple values, conflicting values, an unmapped field, or a value that fails validation all leave the field blank and flagged with the reason.
- **FR-3 Submit gating:** Submit stays disabled until every required field holds a valid value. Optional flagged fields may be left blank.
- **FR-4 Editing:** Every field is editable inline and uses the existing form's validation rules unchanged. A user edit always overrides the auto-filled value.
- **FR-5 Submission:** Reports go to the existing endpoint with the existing payload schema, with no fields added, renamed or reordered.
- **FR-6 Fallback:** If the draft fails or isn't ready within 5s, the manual form loads and the failure is logged. The coordinator is never blocked from submitting.
- **FR-7 Feature flag (two levels):**
  - **Account level:** the master switch and kill switch. When it is off, nobody in that account gets auto-fill.
  - **Coordinator level:** assigns each coordinator to an experiment arm (auto-fill or control). It is honoured only while the account flag is on. Assignment is sticky for the whole test, and all coordinators at the same depot get the same arm (see E6).
  - Both levels take effect on the next report open, with no data migration either way. Every `report_opened` event logs the coordinator's arm.
- **FR-8 Instrumentation:** These events are logged:
  - `report_opened`, with the coordinator's arm
  - `autofill_completed`, with latency and counts of filled and flagged fields
  - `field_edited`, with the field and its before and after values
  - `report_submitted`, with duration in seconds
  - `autofill_failed`

### Smart behaviors
| Situation | Outcome |
|---|---|
| Any uncertainty about a value | Leave the field blank and flagged. Never show a plausible-looking guess. |
| Two sources disagree (e.g. stop count in dispatch vs. compliance) | Leave the field blank and flagged, and state both values and their sources in the reason. The coordinator types the correct one. |
| Some fields are flagged | Flagged fields float to the top, so review time scales with problems, not with form length. |
| Overnight shift crosses midnight | Use the actual clock-in and clock-out window, not the calendar day. |
| Yesterday's report exists | Never copy values from it. Values come only from today's system records. |
| Nothing flagged and all required fields valid | Submit goes straight through in one click. |
| Free-text shift notes | Never auto-filled. Shown empty for the coordinator to write. |

### Technical constraints (what not to build)
- **Capacity:** 2 engineers for 4 weeks. Add one draft-generation endpoint, with no new services and no new data stores.
- **Read-only sources:** Dispatch, compliance and handoff data are read and never written.
- **Performance:** Drafts must generate in under 3s at p95, with a hard timeout at 5s that triggers the fallback.
- **Guardrail contract tests:** Automated tests assert that the submission payload matches the current schema exactly, and they block deploy on failure.
- **Data boundaries:** Use only RouteLogic internal data, with tenant isolation enforced. No third-party or external AI calls.
- **Platform:** Desktop web only. Mobile depends on B4.
- **Rollback:** Turning the account flag off restores the current experience immediately with zero residual state.
- **Prototype build:** No external APIs, no login, in-memory state only (useState or equivalent), and simulated records for 3 pilot accounts.

## 3. The Logistics
### Features out
- **Moved to Should (sprint 2 if capacity allows):**
  - One-tap conflict picker
  - Staleness refresh prompt
  - Submit confirmation modal
  - Manager "Auto-filled · Reviewed by" badge
  - Per-field audit table
  - Field provenance detail beyond the hover source
  - Draft save and resume
- **Won't have (now):**
  - Generative AI narrative or summary text
  - Auto-submit at any confidence level
  - Report schema, template or field changes
  - Custom or configurable report types
  - Weekly, incident or other report types
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
| # | Unhappy path | Required behavior |
|---|---|---|
| E1 | Overnight shift crosses midnight | Use the actual clock-in and clock-out window, not the calendar day. |
| E2 | Shift with zero activity | Show "0" only if the source confirms zero. If the data is missing, leave the field blank and flagged. |
| E3 | A source system is partially down | Fill what's available and flag the rest as "Source unavailable." Don't trigger a full fallback unless every source fails. |
| E5 | Duplicate source records | Deduplicate on record ID. If they still conflict, flag the field. |
| E6 | Two coordinators share a shift or depot | Attribute records by assigned coordinator. If attribution is ambiguous, flag rather than split. Same-depot coordinators share an experiment arm (FR-7). |
| E7 | A source value fails existing form validation | Don't fill it. Flag it as "Source value invalid." |
| E8 | Account time zone differs from the device time zone | The shift window uses the account time zone, and displayed times show the zone. |
| E9 | Report opened in two tabs | The last submit wins, using existing behavior, and a second submit attempt shows "Already submitted." |
| E10 | Account flag turned off mid-session | The open draft can still be submitted. The next open uses the manual form. |
| E11 | Report already submitted, coordinator reopens it | Existing view or amend flow, with no auto-fill. |
| E12 | Very high-volume shift | The p95 under 3s still applies, and the 5s timeout falls back. |

**Safety and hallucination guards:**
- **Blank beats guess (FR-2):** rules-based fill only; no generated values.
- **Mandatory human review:** no code path submits without a user action.
- **Contract tests block deploy:** any difference in payload schema from production fails the build.
- **Account-level kill switch:** overrides every coordinator assignment.
- **Tenant isolation:** every source query is scoped by account ID, and a query without one fails closed.
- **Daily discrepancy monitor:** compares submitted reports against what manager and finance dashboards ingested.
- **Edit-rate alarm:** fires above 10% of auto-filled fields per account per day.
- **Weekly rubber-stamp audit:** the CS lead audits 10 reports per account against source records.

### Decision log
| # | Decision | Alternatives considered | Rationale | Status |
|---|---|---|---|---|
| D1 | Rules-based fill, with no generative AI in the pilot | LLM extraction across sources | Accuracy is part of the Primary Metric, and an LLM can't be validated in 4 weeks. | Decided |
| D2 | Leave a field blank rather than guess | Best-guess with a confidence score | One wrong value in a finance-bound report costs more trust than ten blanks. | Decided |
| D3 | Mandatory human review | Auto-submit above a confidence threshold | Guardrail and accountability. | Decided |
| D4 | Report schema frozen | Add an "auto-filled" field to the schema | Any schema change risks downstream reporting. | Decided |
| D5 | Generate the draft on demand when the report opens | Batch-generate at shift end | It's fresher, simpler, and needs no scheduler. | Decided |
| D6 | No carry-forward from the previous day's report | Pre-fill static fields from yesterday | It silently repeats stale data. | Decided |
| D7 | Desktop web only | Responsive or mobile | Capacity, and mobile depends on B4. | Decided |
| D8 | No admin UI | A simple CS dashboard | Config and analytics are enough for 3 accounts. | Decided |
| D9 | The coordinator submits and the manager consumes | Treat the manager as the sole persona | The coordinator owns the time saving, and the manager owns the guardrail. | Decided |
| D10 | Proceed with B2 in the pilot despite its Time Sinker score | Pilot B1 or B3 instead | Accepted risk. Mitigated by D1–D4, the kill switch, contract tests and the exit criteria. | Decided |
| D11 | Two-level feature flag: account kill switch plus coordinator arm assignment | Per-account flag only, with some accounts as treatment and others as control | With only 3 pilot accounts, an account-level split compares accounts rather than the feature and is badly underpowered. A coordinator-level split inside each account gives a valid 50/50 test, and grouping by depot limits contamination through shared handoffs. | Decided |
| D12 | Hold all Should items (conflict picker, staleness refresh, confirmation modal, manager badge, audit table) out of sprint 1 | Build them alongside the Musts | Each one improves review comfort but none is needed for a coordinator to get a correct report submitted faster. Holding them keeps sprint 1 inside about 5 of 8 engineer-weeks, with room for hardening. | Decided |

### Evals
| Eval | Method | Pass gate |
|---|---|---|
| **Accuracy (false-fill rate)** | Replay the last 30 days of source data for the 3 accounts and compare drafts against what was actually submitted. Manually review mismatches to separate system errors from human errors in the historical reports. | **Under 1%** of filled fields wrong, with at least 70% of fields filled |
| **Time-on-task** | `report_opened` to `report_submitted`, auto-fill arm vs. control arm, during the pilot. | At least **35%** of reports submitted within 3 minutes (baseline assumed 20%; see M5) |
| **Safety trigger** | Daily discrepancy monitor plus automated schema contract test on every deploy. | **Zero** downstream reporting discrepancies; exact schema match |

## MoSCoW scope
- **Must:**
  - **Auto-populate from existing system-of-record data only.** Pull from the dispatch, compliance and handoff records RouteLogic already holds, and map structured fields deterministically. This is the core time saving with no new data dependencies.
  - **Leave a field blank and flag it rather than guess.** Any field that can't be sourced with confidence stays empty and is highlighted. One silently wrong number in a finance-bound report destroys trust faster than ten blank fields.
  - **Human review with a one-click submit.** Nothing submits without explicit user action. Accountability stays with the coordinator, and this is the guardrail's last line of defense.
  - **Inline field editing.** The reviewer corrects a value in place, and the edit overrides the auto-filled value.
  - **Output identical to today's report.** Same fields, formats and submission endpoint. This is the guardrail, and it's non-negotiable.
  - **Fail safe to the current manual form.** If auto-fill errors or times out, the existing form loads and nothing is lost.
  - **Two-level feature flag.** An account-level kill switch plus coordinator-level arm assignment, so the pilot can run as a controlled 50/50 experiment.
  - **Instrumentation.** Capture time-to-submit in seconds and the edit rate per field. Without this, the pilot can't prove accuracy.
- **Should:**
  - **One-tap conflict picker.** The two competing values become buttons instead of being typed.
  - **Staleness refresh prompt.** Shown when source records change after the draft is built, and never overwrites user edits.
  - **Submit confirmation modal.** Lists edits and blank optional fields.
  - **Manager "Auto-filled · Reviewed by" badge.** Carried as metadata only.
  - **Per-field audit table.** Stores the auto-filled value, the submitted value and the source.
  - **Draft save and resume.**
- **Could:**
  - **AI-generated narrative and summary text,** added only after structured accuracy is proven.
  - **Anomaly highlighting,** flagging values outside each account's normal range.
  - **Learning from edits,** tuning frequently corrected fields per account.
  - **An auto-drafted report and reminder at shift end.**
  - **A mobile submit flow,** which depends on B4.
- **Won't (now):**
  - Auto-submit without review, at any confidence level.
  - Any change to the report schema, fields, templates, or downstream manager and finance dashboards.
  - New or customizable report types.
  - Manager analytics or drill-down. That's B8, and it was cut.
  - External or third-party data sources not already inside RouteLogic.
  - Per-account model training or fine-tuning.
  - Bulk or multi-day submission, backfilling past reports, and localization.

---
**Builder hook:** Build a working prototype based on this PRD. Use the User Scenario as the core flow, Functional Requirements as build constraints, and prioritize speed and clarity over visual complexity.
