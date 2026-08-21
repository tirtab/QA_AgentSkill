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
