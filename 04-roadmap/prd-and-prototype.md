# B2 Smart Daily Report Auto-Fill - Simplified PRD (RouteLogic)

**Author:** Shravan · **Status:** Draft · **Target:** High-Fidelity Prototype · **Persona:** Coordinators (submit the report), Managers (use it)

## 1. The Big Picture
- **Vision:** The daily report fills itself in from what already happened during the shift, so coordinators just confirm it instead of re-typing everything.
- **Press release:** RouteLogic Launches Smart Daily Report Auto-Fill

MINNEAPOLIS, [Date] - RouteLogic today announced Smart Daily Report Auto-Fill, part of its Velocity initiative to make the product simpler for coordinators and drivers. Today coordinators spend the end of every shift re-entering data RouteLogic already has - dispatches, compliance checks, handoffs. With Auto-Fill, when the shift closes the daily report is already filled in from the system. The coordinator reviews it, fixes anything that's flagged and submits with one click. If the system isn't sure about a value it leaves it blank and highlights it instead of guessing. In the pilot with three accounts coordinators cut report time by [X]% and [Y]% of auto-filled fields were accepted without edits.

For managers and finance nothing changes except the reports are more reliable - same format, same timing, same dashboards. "Our coordinators told us they were spending the end of every shift repeating themselves to the software," said [Name], [Title] at RouteLogic. "Auto-Fill gives them that time back." Smart Daily Report Auto-Fill will be available to [all customers / GA accounts] starting [date].

(Numbers in brackets get filled in once the pilot data is in.)

- **Success metric:** Time to submit the daily report in seconds, from opening to submitting, compared against the same coordinators before the pilot. For the experiment this is tracked as % of reports submitted within 3 min (see M5). Ties back to the Velocity metric of transaction speed in seconds.
- **Guardrail:** Downstream reporting discrepancies, target is zero (any mismatch in schema, values or timing between auto-filled reports and what the manager/finance dashboards expect). On-time submission rate is tracked with it and can't drop below the current baseline. A breach on either one triggers the kill switch for that account.

## 2. The Details
### User stories
| # | Story | Acceptance criteria |
|---|---|---|
| US-1 | As a coordinator I want my daily report pre-filled from today's dispatch, compliance and handoff records so I dont have to re-type my shift | Report opens with a filled draft within 3s. Any field without exactly one valid value is blank and flagged |
| US-2 | As a coordinator I want to see which fields need my attention, fix them and submit in one click so closing my shift is fast | Flagged fields show first with a count. All fields editable with today's validation. Submit is enabled once required fields are valid |
| US-3 | As a manager I want reports in the same format and at the same time as today so my reporting doesnt change | Payload is the same schema as today and on-time rate is at or above baseline |

### Screens to build (3)
1. **Entry point - open daily report**
   - Header with shift date, time window and time zone
   - Loading state ("Building your draft from today's records…") showing which sources are being read
   - If the draft fails or takes more than 5s, the existing manual form loads with a banner "Auto-fill unavailable. Enter manually."
2. **Feature core - Report Review**
   - Summary bar, e.g. "15 of 18 filled, 3 need attention"
   - Needs attention fields pinned at the top, rest of the fields in the current report order
   - Field states - auto-filled (source shows on hover), needs attention (blank, with the reason), edited, error (fails today's validation, with a message)
   - Sticky submit button with a hint showing any required fields still left
3. **Success - submitted confirmation**
   - Existing confirmation screen plus "Submitted in [N]s"
   - Count of auto-filled values the coordinator edited

### Functional requirements (Must haves)
- **FR-1 Trigger:** Draft is generated when the coordinator opens the report. Only pulls records inside their actual shift window (clock-in to clock-out, account time zone).
- **FR-2 Fill rule:** Each field maps to one documented source and rule. Field only gets filled if the rule returns exactly one valid value. No value, multiple values, conflicting values, unmapped field or a value that fails validation - field stays blank and flagged with the reason.
- **FR-3 Submit gating:** Submit is disabled until every required field is valid. Optional flagged fields can be left blank.
- **FR-4 Editing:** Every field can be edited inline using the existing validation rules. User edit always wins over the auto-filled value.
- **FR-5 Submission:** Same endpoint, same payload schema, nothing added, renamed or reordered.
- **FR-6 Fallback:** If the draft fails or isn't ready in 5s, the manual form loads and the failure is logged. Coordinator is never blocked from submitting.
- **FR-7 Feature flag (two levels):** Account level is the master/kill switch, if its off nobody in that account gets auto-fill. Coordinator level puts each coordinator in an experiment arm (auto-fill or control), only applies while the account flag is on, stays the same for the whole test, and coordinators at the same depot get the same arm (see E6). Both take effect next time the report is opened, no data migration. Every report opened event logs the coordinators arm.
- **FR-8 Instrumentation:** Log report opened (with arm), autofill completed (latency, filled and flagged counts), field edited (field, before/after values), report submitted (duration in seconds), autofill failed.

### Smart behaviors
| Situation | What happens |
|---|---|
| Not sure about a value | Leave it blank and flagged, never show a guess |
| Two sources disagree (e.g. stop count in dispatch vs compliance) | Blank and flagged, show both values and where they came from, coordinator enters the right one |
| Some fields flagged | Flagged fields go to the top so review time depends on the problems, not the length of the form |
| Overnight shift crosses midnight | Use the actual clock-in/clock-out window, not the calendar day |
| Yesterday's report exists | Never copy from it, only today's records |
| Nothing flagged and required fields valid | One click submit |
| Free text shift notes | Never auto-filled, left empty for the coordinator |

### Technical constraints (what not to build)
- 2 engineers for 4 weeks. One new endpoint for draft generation, no new services or data stores.
- Dispatch, compliance and handoff data is read only.
- Drafts generate in under 3s at p95, hard timeout at 5s triggers fallback.
- Contract tests check the submission payload matches the current schema exactly and block deploy if they fail.
- Only RouteLogic internal data, tenant isolation enforced, no third party or external AI calls.
- Desktop web only, mobile depends on B4.
- Turning the account flag off goes straight back to the current experience, nothing left behind.
- Prototype - no external APIs, no login, in-memory state only, simulated records for 3 pilot accounts.

## 3. The Logistics
### Features out
- **Moved to Should (sprint 2 if there's capacity):** one-tap conflict picker, staleness refresh prompt, submit confirmation modal, manager "Auto-filled, Reviewed by" badge, per field audit table, more detailed field source info, save draft and resume
- **Won't have (now):** generative AI summary text, auto-submit at any confidence level, changes to report schema/template/fields, custom report types, weekly/incident or other report types, auto-fill on amendments, backfilling old reports, third party data not already in RouteLogic, per account learning or model tuning, shift end reminders, mobile submission, admin UI for flags and metrics, manager analytics, localization

### Edge cases & safety guard
| # | Unhappy path | Required behavior |
|---|---|---|
| E1 | Overnight shift crosses midnight | Use actual clock-in/clock-out window |
| E2 | Shift with zero activity | Show 0 only if the source confirms zero, otherwise blank and flagged |
| E3 | A source system is partly down | Fill what's available, flag the rest "Source unavailable". Full fallback only if every source fails |
| E5 | Duplicate source records | Dedupe on record ID, flag if they still conflict |
| E6 | Two coordinators share a shift or depot | Attribute by assigned coordinator, flag if unclear. Same depot = same experiment arm |
| E7 | Source value fails form validation | Dont fill it, flag "Source value invalid" |
| E8 | Account time zone differs from device | Shift window uses account time zone, times show the zone |
| E9 | Report opened in two tabs | Last submit wins (existing behavior), second attempt shows "Already submitted" |
| E10 | Account flag turned off mid session | Open draft can still be submitted, next open uses manual form |
| E11 | Coordinator reopens a submitted report | Existing view/amend flow, no auto-fill |
| E12 | Very high volume shift | Same 3s p95 applies, 5s timeout falls back |

**Safety guards:**
- Blank instead of guess - rules based fill only, no generated values
- Human review is required, nothing submits without the user clicking submit
- Contract tests block deploy on any payload schema change
- Account kill switch overrides every coordinator assignment
- Every source query is scoped by account ID, query without one fails
- Daily discrepancy check comparing submitted reports to what manager/finance dashboards pulled in
- Alarm if edit rate goes over 10% of auto-filled fields per account per day
- CS lead audits 10 reports per account per week against source records (to catch rubber stamping)

### Decision log
| # | Decision | Other option | Why | Status |
|---|---|---|---|---|
| D1 | Rules based fill, no generative AI in pilot | LLM extraction | Accuracy is part of the primary metric and we cant validate an LLM in 4 weeks | Decided |
| D2 | Leave blank instead of guessing | Best guess with confidence score | One wrong number in a finance report hurts trust more than 10 blanks | Decided |
| D3 | Human review required | Auto-submit above a threshold | Guardrail and accountability | Decided |
| D4 | Report schema frozen | Add an "auto-filled" field to the schema | Any schema change risks downstream reporting | Decided |
| D5 | Generate draft when report is opened | Batch at shift end | Fresher data, simpler, no scheduler | Decided |
| D6 | No carry forward from yesterday | Pre-fill static fields from yesterday | Would repeat stale data without anyone noticing | Decided |
| D7 | Desktop web only | Responsive/mobile | Capacity, and mobile depends on B4 | Decided |
| D8 | No admin UI | Simple CS dashboard | Config and analytics are enough for 3 accounts | Decided |
| D9 | Coordinator submits, manager consumes | Manager as only persona | Coordinator gets the time saving, manager owns the guardrail | Decided |
| D10 | Go ahead with B2 even though AI scored it Time Sinker | Pilot B1 or B3 instead | Accepted risk, covered by D1-D4, kill switch, contract tests and exit criteria | Decided |
| D11 | Two level flag (account kill switch + coordinator arm) | Account level only, some accounts test and some control | With only 3 accounts an account split compares accounts not the feature and is way underpowered. Splitting coordinators inside each account works, and grouping by depot limits contamination from shared handoffs | Decided |
| D12 | Keep all Should items out of sprint 1 | Build them with the Musts | They make review nicer but none are needed to get a correct report in faster. Keeps sprint 1 at ~5 of 8 engineer weeks with room for fixes | Decided |

### Evals
| Eval | How | Pass |
|---|---|---|
| Accuracy (wrong fills) | Replay last 30 days of source data for the 3 accounts and compare drafts to what was actually submitted. Manually check mismatches to separate system errors from human errors in the old reports | Under 1% of filled fields wrong, at least 70% of fields filled |
| Time on task | Report opened to report submitted, auto-fill arm vs control, during pilot | At least 35% submitted within 3 min (baseline assumed 20%, see M5) |
| Safety | Daily discrepancy check plus schema contract test on every deploy | Zero reporting discrepancies, exact schema match |

## MoSCoW scope
- **Must:**
  - Auto-populate only from data RouteLogic already has (dispatch, compliance, handoff), mapped with fixed rules. This is the core time saving and doesnt need any new data.
  - Leave a field blank and flag it instead of guessing. One wrong number in a finance report kills trust faster than 10 blank fields.
  - Human review with one click submit. Nothing goes in without the coordinator submitting, accountability stays with them.
  - Inline editing, user edit overrides the auto-filled value.
  - Output exactly the same as today's report - same fields, format and endpoint. This is the guardrail.
  - Fall back to the current manual form if auto-fill errors or times out.
  - Two level feature flag (account kill switch + coordinator arm) so the pilot can run as a 50/50 test.
  - Instrumentation for time to submit and edit rate per field, otherwise we cant prove accuracy.
- **Should:** one-tap conflict picker, staleness refresh prompt (never overwrites user edits), submit confirmation modal, manager "Auto-filled, Reviewed by" badge (metadata only), per field audit table, save draft and resume
- **Could:** AI generated summary text (only after structured accuracy is proven), highlighting values outside an account's normal range, learning from edits per account, auto drafted report and reminder at shift end, mobile submit (depends on B4)
- **Won't (now):** auto-submit without review, any change to report schema/fields/templates or downstream dashboards, new or custom report types, manager analytics (thats B8 and it was cut), outside data sources, per account model training, bulk or multi-day submission, backfilling, localization
