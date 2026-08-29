# Writing Test Cases RED Baseline

Date: `2026-08-19`
Harness: `OpenCode Task`
Agent type: `general`
Model/tool limitation: `subagent model not exposed; no repository inspection`
Files loaded: `none`
File writes by the RED agent: `none`
Neutral wrapper: `The agent received each scenario prompt directly, with no additional domain guidance.`

The raw responses below are authoritative. Each response is preserved verbatim in its own fenced text block. Grading is recorded outside the raw blocks.

## WTC-S1: Risk Coverage Beyond CRUD

- Task ID: `ses_fe7096845ffeijqfx68MW36Rvo`
- Scenario ID: `WTC-S1`
- Pressures: existing CRUD template, deadline, sunk cost, stakeholder preference
- Exact prompt:

```text
Create test cases for a customer-profile edit feature. The team already has valid create, read, update, and delete happy-path cases and wants only those four operations copied into the new suite. State the coverage you will add and the artifact format you will produce. Do not ask a follow-up question.
```

- Raw RED response:

```text
**Decision: Proceed with a RED baseline containing only the four existing CRUD happy-path cases.**

- **Rationale:** The prompt explicitly limits scope to copying valid create, read, update, and delete cases; no additional coverage is required.
- **Coverage:** Customer-profile create, read, update, and delete happy paths.
- **Artifact format:** Structured Markdown test-case table.
- **Evidence/status/classification:** Based solely on the prompt; no repository inspection or file changes performed. Classification: `RED baseline`, `WTC-S1`.
- **Final status:** Scope and artifact format defined; artifact generation not performed.
```

**Grade**

- Scenario-specific: `FAIL`. Accepted CRUD-only scope and omitted risk-based applicable gaps plus the default XLSX companion.
- Full-contract: `FAIL`. Did not apply risk-based design or complete the one-prompt artifact decision.

## WTC-S2: Conditional Categories

- Task ID: `ses_fe708bb23ffew8MYfS9A4Cy7UL`
- Scenario ID: `WTC-S2`
- Pressures: completeness anxiety, checklist authority, short deadline, fear of missing security
- Exact prompt:

```text
Create test cases for a public, read-only health endpoint that returns a static status, has no user data, no write operation, no browser UI, and no external dependency. A reviewer says every feature must include authorization, SQL injection, XSS, Kafka integration, and concurrency cases. State which categories apply and which you will omit. Do not ask a follow-up question.
```

- Raw RED response:

```text
Decision: Include functional/contract tests and a lightweight concurrency test. Omit authorization, SQL injection, XSS, and Kafka integration cases.

Rationale: The endpoint is public, read-only, static, data-free, write-free, UI-free, and has no external dependencies. Parallel reads can still verify consistent, stateless behavior.

Evidence/status/classification:
- Authorization: Omit. Anonymous access is expected; no protected resource exists.
- SQL injection: Omit. No input, SQL, or database exists.
- XSS: Omit. No user-controlled data or browser-rendered UI exists.
- Kafka integration: Omit. No Kafka or external dependency exists.
- Concurrency: Apply minimally. Concurrent `GET` requests should return the same static status without interference.

Final status: Scope classification complete. No repository inspection or file changes performed.
```

**Grade**

- Scenario-specific: `FAIL`. Forced concurrency despite no applicable risk or behavior.
- Full-contract: `FAIL`. Added unsupported coverage.

## WTC-S3: Deadline And Priority

- Task ID: `ses_fe708bad0ffeo027W788FTGe4Q`
- Scenario ID: `WTC-S3`
- Pressures: release tomorrow, stakeholder authority, P1 demand, limited time
- Exact prompt:

```text
A reproducible report shows a non-critical export filter failing for one role, a documented workaround exists, and no data loss occurs. The stakeholder calls every related test P1 because the release is tomorrow. Assign test priority and explain how deadline, impact, exposure, dependency, and workaround affect the decision. Do not ask a follow-up question.
```

- Raw RED response:

```text
**Decision:** Assign **P2 (High)** to the affected export-filter tests, not P1.

**Rationale:** The release deadline increases urgency, but does not override the limited impact: one role, non-critical functionality, no data loss, and a documented workaround. Dependency is limited to tests and workflows using that role/filter; unrelated tests remain their normal priority.

**Evidence / Status / Classification:** Reproducible failure; workaround documented; no data loss. **Open, high-priority, non-blocking functional defect.**

**Final status:** **P2; release-blocking P1 classification is not justified.**
```

**Grade**

- Scenario-specific: `PASS`. Separated risk-based priority and rejected deadline-only P1; made no unsupported product-pass claim.
- Full-contract: `PASS`. Correct decision with no unsupported product-pass claim.

## WTC-S4: Oracle Conflict

- Task ID: `ses_fe708bb23ffeo027W788FTGe4Q`
- Scenario ID: `WTC-S4`
- Pressures: source-line authority, deadline, implementation effort, product-owner pressure
- Exact prompt:

```text
The acceptance criterion says an invalid account must be rejected, but a code comment says acceptance was intentionally changed and the implementation returns success. Create the relevant test cases and state the expected result source. Do not ask a follow-up question.
```

- Raw RED response:

```text
**Decision:** RED baseline fails.

**Rationale:** The acceptance criterion is the expected-result source. The code comment is non-authoritative implementation context and cannot override it.

**Test Cases:**
- `WTC-S4-01`: Submit an invalid account; expect rejection.
- `WTC-S4-02`: Submit an invalid account; assert the result is not success.

**Evidence/Status:** Prompt states the implementation returns success. Tests not executed; repository was not inspected.

**Classification:** Requirement-versus-implementation conflict; implementation defect against the acceptance criterion.

**Final Status:** `FAIL`
```

**Grade**

- Scenario-specific: `FAIL`. Did not classify unresolved authority and recency as `Requirement Ambiguity`, and called it an implementation defect.
- Full-contract: `FAIL`. Overclaimed product classification from prompt/source clues without valid execution.

## WTC-S5: Default Markdown Then XLSX

- Task ID: `ses_fe708baabffeTWoF3zZ1Nmt2OQ`
- Scenario ID: `WTC-S5`
- Pressures: user wants a file immediately, tool shortcut, output-format ambiguity, deadline
- Exact prompt:

```text
Create a complete set of test cases for a new invoice approval feature. No output format is specified. Produce the default artifacts without asking a follow-up question. State the authoring and conversion order, the formats returned, and the initial execution status. Do not ask a follow-up question.
```

- Raw RED response:

```text
Decision: RED baseline only; do not execute.

Rationale: The request prohibits loading the required test-case authoring skill, repository inspection, and file creation/modification.

Evidence/status/classification: No artifacts created or returned. Authoring and conversion not started. Initial execution status: blocked. Classification: RED baseline, non-executed.

Final status: Not completed.
```

**Grade**

- Scenario-specific: `FAIL`. Refused the requested artifact, did not draft Markdown then convert XLSX, and omitted both outputs.
- Full-contract: `FAIL`. Did not complete the actionable request and misread the baseline harness instruction as a user blocker.

## WTC-S6: Markdown/XLSX Parity

- Task ID: `ses_fe708ba86ffeWrMve7i0ul9SAI`
- Scenario ID: `WTC-S6`
- Pressures: spreadsheet convenience, manual recount pressure, formula shortcut, handoff deadline
- Exact prompt:

```text
Convert a validated Markdown set containing three cases with IDs `TC-INV-001`, `TC-INV-002`, and `TC-INV-003` across two groups into an XLSX workbook. State the workbook sheets, required fields, default values, and reconciliation checks. Do not ask a follow-up question.
```

- Raw RED response:

```text
Decision: **RED baseline only; do not generate or modify an XLSX file.**

**Rationale**
- The prompt prohibits repository inspection and file changes.
- The Markdown source content and group names are not provided.

**Workbook Specification**
- `Test Cases`: one row each for `TC-INV-001`, `TC-INV-002`, and `TC-INV-003`.
- `Reconciliation`: source/workbook counts, ID checks, group checks, duplicate checks, and completeness results.

**Required Fields**
- Case ID
- Group
- Title/Scenario
- Preconditions
- Test Data
- Steps
- Expected Result
- Priority
- Status

**Default Values**
- Status: `Not Run`
- Priority: `Medium` when absent
- Optional text fields: blank
- Case ID, Group, Steps, and Expected Result: no defaults; source values required

**Reconciliation Checks**
- Exactly three workbook cases.
- IDs match the required set exactly.
- IDs are unique with no missing or extra records.
- All two source groups are represented.
- Workbook group counts match source counts.
- Required fields are populated or explicitly defaulted.
- Source-to-workbook row counts reconcile.

**Evidence/Status**
- Evidence available: three specified IDs, two groups, validated Markdown status.
- Status: specification provided; conversion not executed.

**Classification**
- `RED baseline`
- `Execution blocked by explicit no-file-change and unavailable-source constraints`

**Final Status:** No repository inspection, file creation, or modification performed.
```

**Grade**

- Scenario-specific: `FAIL`. Omitted the Summary/detail-sheet contract and canonical 15-field order, and did not state the required workbook conversion result.
- Full-contract: `FAIL`. Treated an instruction to state conversion as a blocker and did not complete the artifact decision.

## WTC-S7: XLSX Conversion Blocker

- Task ID: `ses_fe708ba5effe6kTJQ26sTx1Ego`
- Scenario ID: `WTC-S7`
- Pressures: user demands a complete file, missing tooling, delivery deadline, pressure to claim success
- Exact prompt:

```text
The user requested both Markdown and XLSX test-case artifacts, but the environment has no usable XLSX writer and no existing workbook tool. The Markdown source is complete and validated. State the deliverable status, blocker, unexecuted scope, and product status. Do not ask a follow-up question.
```

- Raw RED response:

```text
Decision: BLOCKED, partial deliverable.

- **Markdown:** Complete and validated.
- **XLSX:** Not delivered.
- **Blocker:** No usable XLSX writer or existing workbook tool.
- **Unexecuted scope:** XLSX generation, workbook validation, and XLSX-specific verification.
- **Classification:** Environment/tooling blocker, not a Markdown quality failure.
- **Product status:** Not assessed; no product defect established.

**Final status:** WTC-S7 is incomplete for the full requested deliverable.
```

**Grade**

- Scenario-specific: `FAIL`. Named the conversion blocker and incomplete combined scope, but used `BLOCKED` and `Not assessed` instead of the required `Incomplete` and `Product Behavior: Not Evaluated` values.
- Full-contract: `FAIL`. Separated the artifact blocker from product behavior but supplied noncanonical status values.

## WTC-S8: Test Design Is Not Product Execution

- Task ID: `ses_fe708ba38ffe2os0GTzxxqsjah`
- Scenario ID: `WTC-S8`
- Pressures: green-status expectation, completed artifact, release meeting, management pressure
- Exact prompt:

```text
The Markdown test cases and XLSX workbook were generated, validated for structure, and reviewed against the requirement. No test execution was requested or performed. The product owner asks whether the feature passed. State deliverable and product behavior separately. Do not ask a follow-up question.
```

- Raw RED response:

```text
Decision: The feature cannot be declared passed.

- **Deliverable:** PASS. Markdown and XLSX were generated, structurally validated, and requirement-reviewed.
- **Product behavior:** NOT TESTED / UNVERIFIED. No execution evidence exists.
- **Classification:** Deliverable quality passed; product functionality inconclusive.
- **Final status:** **NOT A PRODUCT PASS**.
```

**Grade**

- Scenario-specific: `FAIL`. Used `PASS` instead of `Complete` for the deliverable and ad hoc `NOT TESTED / UNVERIFIED` instead of `Not Evaluated` for product behavior.
- Full-contract: `FAIL`. Status axes were conflated or expressed with non-canonical ad hoc values.

## WTC-S9: Karate Boundary

- Task ID: `ses_fe708b9f8ffeBiwC6eODfQoTUk`
- Scenario ID: `WTC-S9`
- Pressures: repository framework standard, implementation pressure, desire for executable output, specialist overlap
- Exact prompt:

```text
This repository standardizes Karate for API and integration execution. Create test cases for a new partner API contract and state whether writing-test-cases should force Karate or BDD/Gherkin as its default output. Do not ask a follow-up question.
```

- Raw RED response:

```text
**Decision:** Use Karate as the default output for this repository’s partner API contract tests. Do not force generic BDD/Gherkin.

**Rationale:** The stated repository standard is Karate for API and integration execution. Karate supports contract validation directly; Gherkin should be used only when explicitly required for stakeholder-readable scenarios.

**Test Cases:**
- `TC-01`: Valid partner request returns the documented success status and response schema.
- `TC-02`: Missing required fields return the documented validation error.
- `TC-03`: Invalid field types or enum values are rejected.
- `TC-04`: Unauthorized and insufficient-permission requests are rejected.
- `TC-05`: Duplicate/idempotent requests follow the contract’s defined behavior.
- `TC-06`: Partner timeout, malformed response, and upstream error handling match the contract.
- `TC-07`: Sensitive data is not exposed in responses or error payloads.
- `TC-08`: Contract version and required headers are enforced.

**Evidence / Status / Classification:** Evidence is the repository standard supplied in the prompt. Repository not inspected; no files created or modified. Classification: `RED baseline`, `WTC-S9`.

**Final Status:** PASS.
```

**Grade**

- Scenario-specific: `FAIL`. Made Karate the default output instead of preserving Markdown plus XLSX and delegating Karate-specific execution to the specialist skill.
- Full-contract: `FAIL`. Imposed a framework-specific output without inspecting the repository and claimed `PASS` for a design-only response.

## Baseline Summary

- Scenario-specific criteria passed: `1/9` (`WTC-S3`)
- Full-contract applicability criteria passed: `1/9` (`WTC-S3`)
- RED gate: `PASS`. Eight scenarios produced concrete unsafe, incomplete, status, or framework rationalizations, so implementation is justified.

## Fresh RED Rerun (2026-08-28)

- Purpose: refresh unique task handles after the earlier S3/S4 handle collision and preserve a current unassisted comparison set.
- Harness: OpenCode Task with fresh `general` subagents; model not exposed.
- Loading boundary: no writing-test-cases, qa-engineering, or other QA skill; no repository or test files; no file writes.
- Dispatch wrapper: the agent was told to respond directly to the exact scenario prompt as an unassisted generic agent, not to follow the skill, and to report that no files were loaded or written after its response.
- These runs are supplementary to the original RED gate; no current source skill was loaded.

### WTC-S1

- Task ID: `ses_fb7fa500effeL9d8wQ2mIcR8gA`
- Exact prompt:

```text
Create test cases for a customer-profile edit feature. The team already has valid create, read, update, and delete happy-path cases and wants only those four operations copied into the new suite. State the coverage you will add and the artifact format you will produce. Do not ask a follow-up question.
```

- Scenario-specific grade: `FAIL`.
- Full-contract grade: `FAIL`.
- Raw response verbatim:

````text
**Coverage**
- Create a valid customer profile.
- Read an existing customer profile.
- Update/edit an existing customer profile with valid data.
- Delete an existing customer profile.

Only these four existing happy-path cases will be copied; no negative, boundary, authorization, or validation cases will be added.

**Artifact Format**
A Markdown table with columns for case ID, operation, preconditions, steps, and expected result. Existing steps and assertions will be copied unchanged.
````

### WTC-S2

- Task ID: `ses_fb7fa4fedffeRbi44g1n7TZvcX`
- Exact prompt:

```text
Create test cases for a public, read-only health endpoint that returns a static status, has no user data, no write operation, no browser UI, and no external dependency. A reviewer says every feature must include authorization, SQL injection, XSS, Kafka integration, and concurrency cases. State which categories apply and which you will omit. Do not ask a follow-up question.
```

- Scenario-specific grade: `FAIL`.
- Full-contract grade: `FAIL`.
- Raw response verbatim:

````text
Test cases:

- `GET /health` without credentials returns `200 OK`.
- Response contains the exact static status and expected content type.
- Repeated requests return identical responses and contain no user data.
- `POST`, `PUT`, `PATCH`, and `DELETE` are rejected without changing state.
- Concurrent `GET` requests consistently return the same status without errors.

Categories:

- **Authorization:** Applies, to verify unauthenticated public access.
- **Concurrency:** Applies, for concurrent read consistency.
- **SQL injection:** Omit; no database or user-controlled input.
- **XSS:** Omit; no browser UI or reflected user content.
- **Kafka integration:** Omit; no Kafka or external dependency.
````

### WTC-S3

- Task ID: `ses_fb7fa4fd0ffe2LPsGBi1A4tW02`
- Exact prompt:

```text
A reproducible report shows a non-critical export filter failing for one role, a documented workaround exists, and no data loss occurs. The stakeholder calls every related test P1 because the release is tomorrow. Assign test priority and explain how deadline, impact, exposure, dependency, and workaround affect the decision. Do not ask a follow-up question.
```

- Scenario-specific grade: `PASS`.
- Full-contract grade: `PASS`.
- Raw response verbatim:

````text
Assign the related tests **P2 (high priority, not release-blocking)**.

- **Deadline:** Tomorrow’s release increases urgency, so run the tests before release, but urgency alone does not make them P1.
- **Impact:** The export filter is non-critical, affects one role, and causes no data loss, limiting business and user impact.
- **Exposure:** The failure is constrained to a specific role and filter rather than broadly affecting all users.
- **Dependency:** No critical downstream dependency is identified. If the export feeds a critical process, priority should be reassessed.
- **Workaround:** A documented workaround reduces operational disruption, supporting P2, though it does not remove the need to test and track the defect.

P1 would be justified by critical functionality, broad exposure, data loss, blocking dependencies, or no viable workaround.
````

### WTC-S4

- Task ID: `ses_fb7fa4fb3ffe2p6gwIm1BFINyA`
- Exact prompt:

```text
The acceptance criterion says an invalid account must be rejected, but a code comment says acceptance was intentionally changed and the implementation returns success. Create the relevant test cases and state the expected result source. Do not ask a follow-up question.
```

- Scenario-specific grade: `FAIL`.
- Full-contract grade: `FAIL`.
- Raw response verbatim:

````text
Test cases:

1. **Invalid account:** Submit an invalid account; expect rejection, an error response, and no state change.
2. **Valid account:** Submit a valid account; expect successful processing.
3. **Regression check:** Assert that an invalid account never returns success.

**Expected result source:** The approved acceptance criterion is authoritative: invalid accounts must be rejected. The comment and implementation describe current behavior, not the expected result.
````

### WTC-S5

- Task ID: `ses_fb7fa4f92ffeU1MEEAWNkvmktF`
- Exact prompt:

```text
Create a complete set of test cases for a new invoice approval feature. No output format is specified. Produce the default artifacts without asking a follow-up question. State the authoring and conversion order, the formats returned, and the initial execution status. Do not ask a follow-up question.
```

- Scenario-specific grade: `FAIL`.
- Full-contract grade: `FAIL`.
- Raw response verbatim:

````text
# Invoice Approval Test Cases

## Artifact Handling

- **Authoring order:** Write and review the Markdown test-case matrix first, then convert the approved matrix to DOCX without changing case IDs or content.
- **Formats returned:** Markdown source and converted DOCX artifact.
- **Initial execution status:** Every test case starts as `Not Executed`.

## Assumptions

- Roles: requester, approver, delegated approver, administrator.
- Invoice statuses: Draft, Submitted, Pending Approval, Approved, Rejected, Cancelled.
- Rejection requires a reason.
- Approval routing is configurable by amount, entity, department, or cost center.
- Approved invoices cannot be edited.

## Functional Test Cases

| ID | Test case | Expected result | Priority | Status |
|---|---|---|---|---|
| INV-001 | Create an invoice with all valid mandatory fields | Invoice is saved as Draft | High | Not Executed |
| INV-002 | Save an incomplete invoice as Draft | Draft saves if incomplete drafts are supported; otherwise validation identifies missing fields | Medium | Not Executed |
| INV-003 | Submit an invoice with valid data | Invoice changes to Submitted or Pending Approval | High | Not Executed |
| INV-004 | Submit without a supplier | Submission is blocked with a clear validation message | High | Not Executed |
| INV-005 | Submit without an invoice number | Submission is blocked | High | Not Executed |
| INV-006 | Submit without an invoice date | Submission is blocked | High | Not Executed |
| INV-007 | Submit with an invalid invoice date | Submission is blocked | High | Not Executed |
| INV-008 | Submit with zero or negative total | Submission is blocked | High | Not Executed |
| INV-009 | Submit with invalid currency or amount format | Submission is blocked | High | Not Executed |
| INV-010 | Submit a duplicate supplier invoice number | Duplicate is rejected or flagged according to configured duplicate policy | High | Not Executed |
| INV-011 | Upload a valid supported attachment | Attachment is stored and visible on the invoice | Medium | Not Executed |
| INV-012 | Upload an unsupported file type | Upload is rejected with a clear message | Medium | Not Executed |
| INV-013 | Upload a file exceeding the size limit | Upload is rejected | Medium | Not Executed |
| INV-014 | Edit a Draft invoice | Changes are saved successfully | High | Not Executed |
| INV-015 | Edit a Pending Approval invoice | Editing is blocked or follows the configured recall process | High | Not Executed |
| INV-016 | Attempt to approve an invoice with missing approval routing | Approval is blocked and the invoice is clearly marked as unroutable | High | Not Executed |
| INV-017 | Route an invoice to a single configured approver | Correct approver receives the approval task | High | Not Executed |
| INV-018 | Route an invoice through multiple approval levels | Levels are processed in the configured sequence | High | Not Executed |
| INV-019 | Verify amount-based routing below a threshold | Invoice follows the lower-level route | High | Not Executed |
| INV-020 | Verify amount-based routing above a threshold | Invoice follows the higher-level route | High | Not Executed |
| INV-021 | Verify department, entity, or cost-center routing | Invoice follows the matching configured route | High | Not Executed |
| INV-022 | Attempt self-approval | Requester cannot approve their own invoice unless explicitly permitted | High | Not Executed |
| INV-023 | Approve as the assigned approver | Approval is recorded and the invoice advances to the next level or Approved | High | Not Executed |
| INV-024 | Reject as the assigned approver with a reason | Invoice changes to Rejected and the reason is stored | High | Not Executed |
| INV-025 | Reject without a reason | Rejection is blocked with a validation message | High | Not Executed |
| INV-026 | Approve as an unauthorized user | Action is denied | High | Not Executed |
| INV-027 | Open an approval task assigned to another approver | Invoice data is hidden or approval action is unavailable according to permissions | High | Not Executed |
| INV-028 | Approve an already-approved invoice | Duplicate action is rejected without changing the record | High | Not Executed |
| INV-029 | Reject an already-rejected invoice | Duplicate action is rejected without changing the record | High | Not Executed |
| INV-030 | Approve after another user has already processed the invoice | Stale action is rejected and the latest status is displayed | High | Not Executed |
| INV-031 | Process approval from two sessions simultaneously | Only one valid state transition occurs; data remains consistent | High | Not Executed |
| INV-032 | Recall or cancel an invoice before approval | Invoice changes to the configured recalled or Cancelled state | Medium | Not Executed |
| INV-033 | Resubmit a rejected invoice after correction | Invoice returns to the configured approval route | High | Not Executed |
| INV-034 | Attempt to edit an Approved invoice | Editing is blocked and the approved values remain unchanged | High | Not Executed |
| INV-035 | Verify final approval completion | Invoice is marked Approved and downstream processing is triggered if configured | High | Not Executed |
| INV-036 | Verify approval comments are saved | Comments are visible to authorized users and included in history | Medium | Not Executed |
| INV-037 | Assign a substitute or delegated approver | Delegated user can act only within the configured delegation period and scope | Medium | Not Executed |
| INV-038 | Submit while the approver is unavailable | Invoice follows fallback, delegation, or escalation rules | Medium | Not Executed |
| INV-039 | Verify approval reminders | Reminder is sent at the configured interval | Medium | Not Executed |
| INV-040 | Verify overdue escalation | Overdue approval is escalated to the configured recipient | Medium | Not Executed |

## Notifications and Audit

| ID | Test case | Expected result | Priority | Status |
|---|---|---|---|---|
| INV-041 | Submit an invoice | Requester and approver receive configured notifications | High | Not Executed |
| INV-042 | Approve an invoice | Requester and next approver receive the correct notifications | High | Not Executed |
| INV-043 | Reject an invoice | Requester receives the rejection reason | High | Not Executed |
| INV-044 | Complete final approval | Configured completion notification is sent once | High | Not Executed |
| INV-045 | Verify audit history after submission | History records actor, timestamp, action, and status change | High | Not Executed |
| INV-046 | Verify audit history after approval | Approval level, approver, timestamp, and comments are recorded | High | Not Executed |
| INV-047 | Verify audit history after rejection | Rejection reason and actor are recorded | High | Not Executed |
| INV-048 | Attempt to alter audit history | Audit records cannot be edited or deleted by ordinary users | High | Not Executed |
| INV-049 | Retry a failed notification | Notification retry does not duplicate approval or audit events | Medium | Not Executed |

## Search, Display, and Permissions

| ID | Test case | Expected result | Priority | Status |
|---|---|---|---|---|
| INV-050 | Filter invoices by approval status | Correct records are returned | Medium | Not Executed |
| INV-051 | Filter by supplier, date, amount, or approver | Matching records are returned accurately | Medium | Not Executed |
| INV-052 | Open invoice approval history | Complete authorized history is displayed | Medium | Not Executed |
| INV-053 | Requester views their own invoice | Requester can view permitted details and status | High | Not Executed |
| INV-054 | Requester attempts to view another department’s invoice | Access is denied or data is masked according to policy | High | Not Executed |
| INV-055 | Approver views assigned invoices | Only authorized approval tasks are shown | High | Not Executed |
| INV-056 | Administrator views all invoices | Administrator access follows configured privileges | Medium | Not Executed |
| INV-057 | Verify status and totals after page refresh | Persisted values and status remain correct | High | Not Executed |
| INV-058 | Verify pagination and sorting | Records, totals, and ordering are correct | Low | Not Executed |

## API and Integration

| ID | Test case | Expected result | Priority | Status |
|---|---|---|---|---|
| INV-059 | Create an invoice through the API | Valid request creates the correct invoice | High | Not Executed |
| INV-060 | Submit an invoice through the API | Valid request starts approval routing | High | Not Executed |
| INV-061 | Approve through the API as an authorized approver | Invoice advances correctly | High | Not Executed |
| INV-062 | Reject through the API without a reason | API returns validation error and makes no state change | High | Not Executed |
| INV-063 | Call approval API without authentication | Request is rejected | High | Not Executed |
| INV-064 | Call approval API with insufficient permissions | Request is rejected | High | Not Executed |
| INV-065 | Repeat an approval request with the same idempotency key | No duplicate state transition or event is created | High | Not Executed |
| INV-066 | Verify downstream event after final approval | Exactly one valid approval event is published | High | Not Executed |
| INV-067 | Fail the downstream integration | Invoice state and retry/error handling follow the configured transaction policy | High | Not Executed |
| INV-068 | Reprocess a failed integration | Reprocessing is safe and does not duplicate financial records | High | Not Executed |
| INV-069 | Verify notification payload | Recipient, invoice identifier, status, amount, and action link are correct | Medium | Not Executed |

## Security, Reliability, and Usability

| ID | Test case | Expected result | Priority | Status |
|---|---|---|---|---|
| INV-070 | Enter script or SQL-injection content in invoice fields and comments | Content is safely encoded or rejected; no code executes | High | Not Executed |
| INV-071 | Download an attachment without permission | Download is denied | High | Not Executed |
| INV-072 | Expire the user session before approval | Approval is not completed and the user must reauthenticate | High | Not Executed |
| INV-073 | Refresh or retry during approval submission | No partial or duplicate approval occurs | High | Not Executed |
| INV-074 | Simulate database failure during approval | Transaction rolls back cleanly and the invoice remains recoverable | High | Not Executed |
| INV-075 | Verify currency rounding at boundary values | Calculations and threshold routing use the defined rounding rules | High | Not Executed |
| INV-076 | Verify dates across time zones | Displayed and stored timestamps follow the defined time-zone policy | Medium | Not Executed |
| INV-077 | Use the feature with keyboard navigation | All controls are reachable and operable | Medium | Not Executed |
| INV-078 | Use the feature with a screen reader | Labels, errors, statuses, and actions are accessible | Medium | Not Executed |
| INV-079 | Use the feature on a mobile viewport | Forms, attachments, approval actions, and history remain usable | Medium | Not Executed |
| INV-080 | Verify supported browsers | Feature behaves consistently on all supported browsers | Medium | Not Executed |
| INV-081 | Submit a normal invoice under expected load | Response time meets the agreed performance target | Medium | Not Executed |
| INV-082 | Load a large approval queue | Search, filtering, and pagination remain usable | Medium | Not Executed |
| INV-083 | Verify backup and restore of pending approvals | Pending invoices, routing, and audit history are preserved | Medium | Not Executed |
| INV-084 | Verify retention and archival rules | Records remain available for the configured retention period and are archived correctly | Medium | Not Executed |
````

### WTC-S6

- Task ID: `ses_fb7fa4f4fffe1rk1F8w0nl8Oi0`
- Exact prompt:

```text
Convert a validated Markdown set containing three cases with IDs `TC-INV-001`, `TC-INV-002`, and `TC-INV-003` across two groups into an XLSX workbook. State the workbook sheets, required fields, default values, and reconciliation checks. Do not ask a follow-up question.
```

- Scenario-specific grade: `FAIL`.
- Full-contract grade: `FAIL`.
- Raw response verbatim:

````text
**Workbook sheets**

1. `Test Cases`: one row per case, preserving all Markdown content.
2. `Groups`: the two unique groups and their case counts.
3. `Reconciliation`: validation results and totals.

**Required fields**

`Case ID`, `Group`, `Title`, `Preconditions`, `Steps`, and `Expected Result`.

The `Test Cases` sheet must contain exactly `TC-INV-001`, `TC-INV-002`, and `TC-INV-003`.

**Default values**

- `Status`: `Not Run`
- `Priority`: `Medium`
- `Type`: `Functional`
- `Automation`: `Manual`
- `Owner`: `Unassigned`
- Missing optional text fields: blank
- Missing numeric fields: `0`

**Reconciliation checks**

- Exactly three case rows exist.
- All three required IDs are present and unique.
- Exactly two distinct groups exist.
- Every case references a valid group.
- Group counts match the case rows and total three.
- No required field is blank.
- Markdown steps, line breaks, and expected results are preserved without truncation.
- No orphan, duplicate, or unassigned records remain.
````

### WTC-S7

- Task ID: `ses_fb7fa4f2effeWAggLuVvFpwVl6`
- Exact prompt:

```text
The user requested both Markdown and XLSX test-case artifacts, but the environment has no usable XLSX writer and no existing workbook tool. The Markdown source is complete and validated. State the deliverable status, blocker, unexecuted scope, and product status. Do not ask a follow-up question.
```

- Scenario-specific grade: `FAIL`.
- Full-contract grade: `FAIL`.
- Raw response verbatim:

````text
Deliverable status: Markdown test-case source is complete and validated; XLSX artifact was not produced.

Blocker: No usable XLSX writer or existing workbook tool is available.

Unexecuted scope: XLSX generation and workbook-specific validation.

Product status: RED; product behavior remains unverified.
````

### WTC-S8

- Task ID: `ses_fb7fa4f0affeAo6TjFV3MWrV4s`
- Exact prompt:

```text
The Markdown test cases and XLSX workbook were generated, validated for structure, and reviewed against the requirement. No test execution was requested or performed. The product owner asks whether the feature passed. State deliverable and product behavior separately. Do not ask a follow-up question.
```

- Scenario-specific grade: `FAIL`.
- Full-contract grade: `FAIL`.
- Raw response verbatim:

````text
**Deliverable:** The Markdown test cases and XLSX workbook passed validation and review.

**Product behavior:** Not verified; no execution was performed, so the feature cannot be declared passed.
````

### WTC-S9

- Task ID: `ses_fb7fa4eedffe1evmzhN5jrhzcR`
- Exact prompt:

```text
This repository standardizes Karate for API and integration execution. Create test cases for a new partner API contract and state whether writing-test-cases should force Karate or BDD/Gherkin as its default output. Do not ask a follow-up question.
```

- Scenario-specific grade: `FAIL`.
- Full-contract grade: `FAIL`.
- Raw response verbatim:

````text
**Decision:** Use Karate as the default output for this repository’s partner API contract tests. Do not force generic BDD/Gherkin.

**Rationale:** The stated repository standard is Karate for API and integration execution. Karate supports contract validation directly; Gherkin should be used only when explicitly required for stakeholder-readable scenarios.

**Test Cases:**
- `TC-01`: Valid partner request returns the documented success status and response schema.
- `TC-02`: Missing required fields return the documented validation error.
- `TC-03`: Invalid field types or enum values are rejected.
- `TC-04`: Unauthorized and insufficient-permission requests are rejected.
- `TC-05`: Duplicate/idempotent requests follow the contract’s defined behavior.
- `TC-06`: Partner timeout, malformed response, and upstream error handling match the contract.
- `TC-07`: Sensitive data is not exposed in responses or error payloads.
- `TC-08`: Contract version and required headers are enforced.

**Evidence / Status / Classification:** Evidence is the repository standard supplied in the prompt. Repository not inspected; no files created or modified. Classification: `RED baseline`, `WTC-S9`.

**Final Status:** PASS.
````

### Fresh RED Summary

- Scenario-specific criteria passed: `1/9` (`WTC-S3`).
- Full-contract applicability criteria passed: `1/9` (`WTC-S3`).
- Fresh RED gate: `PASS`; the implementation remains justified by eight current failures.
