# A/B Experiment Brief, RouteLogic (B2B)

## Parameters
| Parameter | Decision |
|---|---|
| Feature under test | · |
| Persona | Drivers & Coordinators |
| Expected outcome | Driver adoption rate doubles, Coordinator adoption improves by 50% |
| Primary success metric | Adoption rate |
| Baseline rate | Shift scheduling module adoption - 15% for Drivers, 42% for coordinators |
| Guardrail metric | Manager CSAT and adoption across managerial reporting and finance modules |
| Guardrail boundary | Manager CSAT/adoption must stay flat |
| Second guardrail | · |
| Minimum Detectable Effect | +15 points |
| Sample size per arm | 121 |
| Traffic split | 50/50 |
| Test duration | 14 days |
| Significance threshold | p < 0.05% (95% confidence level) |

## Control vs. Variant
- **Control (A):** Moment of misery / red flag #1 (e.g., “user gave up after 3 tries”): To mark a stop delivered I tap through three screens. In the rain, at a doorstep, with a package in one hand. I've started just texting my dispatcher instead.
Moment of misery / red flag #2: I reassign a route and the driver doesn't see it for ten, fifteen minutes. By then they've driven the wrong way. We keep a WhatsApp group as the real system
Moment of misery / red flag #3: Route optimization sent me down a road that's been closed for months. It doesn't know about traffic or the loading dock round the back. I override it every day.
- **Variant (B):** Screens to build

Report Review (the core screen).
A header showing the shift date and time window.
A summary bar, for example “19 of 22 filled · 3 need attention.”
A “Needs attention” group pinned at the top, followed by the remaining fields in the current report’s section order.
A sticky Submit button.
Field state components (a design-system piece, not a page). Four visual states:
Auto-filled: neutral styling, with source shown on hover.
Needs attention: blank or conflicting, highlighted.
Edited: marked as changed by the user.
Conflict: shows the competing values with a pick action.
Submit confirmation. A light modal listing what the coordinator edited and any optional fields left blank, with Confirm and Back buttons. Skip it entirely when there were zero edits and zero blanks.
Fallback state. The existing manual form with a single banner: “Auto-fill unavailable. Enter manually.” It needs no new layout.
Submitted state. The existing confirmation screen, plus “Submitted in 48s.”
Manager badge. A small “Auto-filled · Reviewed by [name]” tag on the existing report view, with no layout change.
Functional requirements

FR-1 Trigger: A draft is generated on demand when the coordinator opens the report. It pulls records within that coordinator’s actual shift window.
FR-2 Field mapping: A documented table maps each report field to its source and rule. A field with no mapping stays blank.
FR-3 Fill rules: A field is filled only when the rule returns exactly one valid value. Zero values, multiple values, conflicting values, or stale data all leave it blank and flagged.
FR-4 Submit gating: Required fields must be valid before submitting. Optional flagged fields can be left blank, but the confirmation modal lists them.
FR-5 Editing: Every field is editable and uses the existing form’s validation rules unchanged.
FR-6 Submission: Reports go to the existing endpoint with the existing payload schema, with no added or renamed fields.
FR-7 Audit record: For each field, store the auto-filled value, the submitted value, and the source reference, in storage separate from the report payload.
FR-8 Fallback: If the draft fails or isn’t ready within 5s, load the manual form and log the failure.
FR-9 Feature flag: On or off per account, effective on the next report open, with no data migration either way.
FR-10 Instrumentation: Log these events:
report_opened
autofill_completed, with latency and counts of filled and flagged fields
field_edited
report_submitted, with duration in seconds
autofill_failed
FR-11 Manager marker: The “auto-filled & reviewed” indicator is carried as metadata outside the report schema.
- **Held constant (isolation check):** Read-only sources: Dispatch, compliance, and handoff data are read and never written.
Performance: Drafts must generate in under 3s at p95, with a hard timeout at 5s that triggers the fallback.
Guardrail contract tests: Automated tests must assert that the submission payload matches the current schema exactly, and they block deploy on failure.
Data boundaries: Use only RouteLogic internal data, with tenant isolation enforced. No third-party or external AI calls in the pilot.
Platform: Desktop web only. Mobile depends on B4.
Retention: The audit record follows existing report retention policy.
Rollback: Turning the flag off restores the current experience immediately with zero residual state.

## Hypothesis
> I believe that _(not filled in)_ for Drivers & Coordinators will result in Driver adoption rate doubles, Coordinator adoption improves by 50%, as measured by a +15 points change in Adoption rate within 14 days. We will protect Manager CSAT and adoption across managerial reporting and finance modules throughout the test.

## Shipping criteria
> We will **ship** if Adoption rate improves by ≥ +15 points at p < 0.05% (95% confidence level) and Manager CSAT and adoption across managerial reporting and finance modules does not reach Manager CSAT/adoption must stay flat after 14 days.
> We will **iterate** if direction is positive but lift is below the MDE.
> We will **kill** if the primary metric shows no improvement or moves negatively.
> The read date is fixed at the end of 14 days, no results reviewed before this date.
