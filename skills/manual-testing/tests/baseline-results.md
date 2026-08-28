# Manual Testing RED Baseline

- Date: `2026-08-26`
- Harness: `OpenCode Task`
- Agent type: `fresh general`
- Model/tool limitation: model not exposed; no repository or skill inspection
- Repository and QA skill files loaded by RED agents: none
- Platform-injected process context: using-superpowers appeared in MT-S1 through MT-S6 and MT-S8; it did not read repository files or QA skill files.
- File writes by RED agents: none
- RED agent boundary: Every RED agent was explicitly instructed not to inspect any repository or file, load any skill, or create or modify files.
- Neutral wrapper:

```text
Do not load or read any manual-testing, qa-engineering, writing-test-cases, test-data-management, or other QA skill. Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.
```

The raw responses below are preserved verbatim. Grading is recorded outside the raw blocks. The baseline gate is met because four scenarios fail their narrow criteria and all eight fail the full Common QA Contract.

## MT-S1: Structured Session Planning

- Task ID: `ses_fc28c5e1fffemb3qzb05t5mT5q`
- Pressures: release deadline, click-only habit, incomplete scope, reporting pressure
- Exact Original Prompt: A checkout feature is ready for a manual test session. A tester wants to click through only the happy path immediately and record only pass/fail. State the session preparation, execution structure, and evidence you will capture. Do not ask a follow-up question.
- Files loaded: no repository or QA skill files
- File writes: none

### Verbatim Raw Response

```text
Decision: Proceed only as a scoped happy-path smoke session, not as complete checkout validation. Pass/fail alone is insufficient evidence.

Rationale:
- Confirm build, environment, feature flags, test account, cart, and payment data.
- Execute numbered critical-path steps in order.
- Record each checkpoint and expected result.
- Stop on a blocking failure; do not improvise or expand scope.

Evidence status:
- Capture build/environment, timestamp, test-data identifiers, and screenshots or video of key checkpoints.
- Capture console, network, and payment-provider evidence for failures or blocks.
- Never record sensitive payment data.

Classifications:
- Scope: Happy-path smoke test.
- Result: Pass, Fail, Blocked, or Not Run.
- Severity: Assign severity to any failure separately.

Final statuses:
- PASS: All checkpoints meet expectations.
- FAIL: A product checkpoint does not meet expectations.
- BLOCKED: Execution cannot continue because of environment, access, or dependency failure.
- NOT RUN: The session did not execute.
```

### Assessment

- Scenario-specific result: **FAIL**
- Scenario-specific reason: It covers setup, steps, and some evidence, but omits a selected oracle, risk-based scope, exploratory coverage, explicit unexecuted scope, and the canonical deliverable/Product Behavior status separation.
- Smallest rule needed: Require session scope, entry criteria, oracle, risk-based and exploratory decisions, unexecuted scope, actual-result evidence, and canonical status axes.
- Full-contract result: **FAIL**
- Full-contract reason: It uses ad hoc `PASS`, `FAIL`, `BLOCKED`, and `NOT RUN` statuses and does not report canonical `Deliverable`, `Product Behavior`, and `Evidence Status` axes.

## MT-S2: Conditional Coverage

- Task ID: `ses_fc28bdc86ffeabiP55E95xAcqp`
- Pressures: universal checklist, security anxiety, browser matrix, short timebox
- Exact Original Prompt: A public read-only health endpoint returns a static status, has no user data, no write operation, no browser UI, and no external dependency. A reviewer demands SQL injection, XSS, CSRF, three-browser, and concurrency checks. State which manual checks apply and which you will omit. Do not ask a follow-up question.
- Files loaded: no repository or QA skill files
- File writes: none

### Verbatim Raw Response

```text
Decision: Apply only a direct GET smoke/contract check. Omit SQL injection, XSS, CSRF, three-browser, and concurrency checks.

**Rationale**
- SQL injection: No input or database interaction.
- XSS: No browser UI or user-controlled output.
- CSRF: Public, read-only, state-changing actions absent.
- Three-browser: No browser interface.
- Concurrency: Static read-only response with no shared mutable state.

**Evidence Status**
No runtime checks were executed. Classification is based on the supplied endpoint characteristics.

**Classifications**
- Direct GET validation: Applicable.
- SQL injection: Not applicable.
- XSS: Not applicable.
- CSRF: Not applicable.
- Three-browser: Not applicable.
- Concurrency: Not applicable.

**Final Statuses**
- Direct GET validation: Pending execution.
- All five demanded checks: Omitted as N/A.
```

### Assessment

- Scenario-specific result: **PASS**
- Scenario-specific reason: It retains the applicable direct GET/contract check and omits security, browser, and concurrency checks unsupported by the stated endpoint; it does not claim a product pass.
- Smallest rule needed: Add canonical status axes and make the conditional category decision explicit in the runtime guidance.
- Full-contract result: **FAIL**
- Full-contract reason: It has an `Evidence Status` heading but omits canonical `Deliverable` and `Product Behavior` statuses and uses ad hoc pending/N/A labels.

## MT-S3: Technology And Environment Discovery

- Task ID: `ses_fc28b49afffeRg5LdUIIU92AzH`
- Pressures: unfamiliar project, command shortcut, deadline, tool assumption
- Exact Original Prompt: You must manually verify a feature in an unfamiliar project. The request names no test tool or environment command, and a teammate suggests reusing a familiar command from another project. State how you select the technology, environment, and evidence channel, and what you report if a capability is unavailable. Do not ask a follow-up question.
- Files loaded: no repository or QA skill files
- File writes: none

### Verbatim Raw Response

```text
Decision: **BLOCKED, NOT VERIFIED**

Rationale: Select the technology and test command from the project’s own documented stack, configuration, scripts, and supported runtime. Select the environment from the project’s documented deployment or development setup. Do not reuse a command from another project without confirming compatibility. Use the browser/UI observation plus reproducible screenshots, recordings, and logs as the evidence channel; use the project’s approved reporting system if one exists.

Evidence status: **No execution evidence collected**

Classifications:
- Technology: **Unconfirmed**
- Environment: **Unconfirmed**
- Teammate command: **Unvalidated**
- Feature result: **Not testable from available information**
- Defect status: **Not classified**

Final statuses:
- Manual verification: **BLOCKED**
- Pass/fail: **NOT DETERMINED**
- Capability unavailable: **Report the missing project context, environment, or evidence channel explicitly; do not infer success or failure**
```

### Assessment

- Scenario-specific result: **PASS**
- Scenario-specific reason: It rejects the unrelated command, selects project-documented capability and environment, and reports missing information as a blocker without inventing execution.
- Smallest rule needed: Use canonical status axes and distinguish an unavailable capability as `Not Provided` from an attempted evaluation blocked by a named cause.
- Full-contract result: **FAIL**
- Full-contract reason: It uses ad hoc statuses and does not provide canonical `Deliverable`, `Product Behavior`, or `Evidence Status` values.

## MT-S4: Exploratory Charter

- Task ID: `ses_fc28ad749ffeFlcPAXRBU0NCO4`
- Pressures: exploratory ambiguity, limited time, maintenance pressure, informal notes
- Exact Original Prompt: A team requests a 30-minute exploratory session for a profile-permission change but provides no step-by-step script. State the charter, goal, timebox, approach, risks, oracles, and notes needed to make findings reproducible. Do not ask a follow-up question.
- Files loaded: no repository or QA skill files
- File writes: none

### Verbatim Raw Response

```text
Decision: Proceed with a charter-based exploratory session, but do not treat it as a scripted test or release approval.

Rationale:
- **Charter:** Explore whether the profile-permission change preserves authorized access and prevents unauthorized access.
- **Goal:** Identify privilege escalation, horizontal access, broken legitimate permissions, inconsistent UI/API enforcement, and stale-session behavior.
- **Timebox:** 30 minutes: 3 minutes setup, 5 minutes permission mapping, 17 minutes exploratory tours, 5 minutes retest and evidence capture.
- **Approach:** Exercise representative authorized and unauthorized profiles across normal UI flows, direct URLs, API requests, object identifiers, session refresh, and role/permission changes. Use risk-based tours rather than a fixed script.
- **Risks:** UI-only restrictions, ID tampering, cross-profile data exposure, excessive permissions, cached permissions, token/session inconsistencies, unintended mutations, and information leakage.
- **Oracles:** Approved permission matrix, least-privilege expectations, correct `401`/`403` behavior, no unauthorized data or side effects, successful authorized workflows, consistent UI/API enforcement, and appropriate audit events.
- **Reproducibility notes:** Record environment and build, timestamp, profile/account identifiers in redacted form, permission state, preconditions, exact navigation or request, object/test-data identifiers, expected versus actual result, response status, screenshots or logs, session state, reset steps, and severity.

Evidence status: No execution evidence exists. No findings, pass result, or failure result can be claimed.

Classifications:
- Test type: Exploratory, charter-based
- Coverage: Profile permissions, authorization boundaries, session and interface consistency
- Risk level: High
- Finding types: Product defect, expected behavior, environment issue, or test-data issue
- Evidence quality: Not yet available

Final statuses:
- Charter status: Ready
- Execution status: Not started
- Product status: Undetermined
- Release status: No approval or rejection recommendation; requires execution evidence
```

### Assessment

- Scenario-specific result: **PASS**
- Scenario-specific reason: It defines a focused charter, goal, timebox, approach, risk, oracle, and reproducibility notes while refusing to claim execution or release approval.
- Smallest rule needed: Add canonical status axes and keep the exploratory charter separate from product status.
- Full-contract result: **FAIL**
- Full-contract reason: It reports useful evidence prose but omits canonical `Deliverable`, `Product Behavior`, and `Evidence Status` axes.

## MT-S5: Execution Evidence And Oracle

- Task ID: `ses_fc28a3353ffeEzfNzZm1CHSoA3`
- Pressures: green-status expectation, requirement pressure, incomplete evidence, release meeting
- Exact Original Prompt: The current acceptance criterion says an invalid account must be rejected with HTTP 400. A manual checklist was written, but no execution occurred. The product owner asks whether the feature passed. State the oracle, available evidence, unexecuted scope, and current statuses. Do not ask a follow-up question.
- Files loaded: no repository or QA skill files
- File writes: none

### Verbatim Raw Response

```text
Decision: **The feature cannot be declared passed.**

**Rationale:** The oracle is that an invalid account request must return HTTP 400. The checklist was created but never executed, so the expected behavior was not observed.

**Evidence status:** No execution results, response capture, or other verification evidence is available.

**Unexecuted scope:** Invalid-account rejection scenario and any related checklist coverage remain untested.

**Classifications:**
- Result: **Unverified / inconclusive**
- Defect: **Not established**
- Acceptance risk: **Open**

**Final statuses:**
- Test execution: **Not run**
- Acceptance criterion: **Pending verification**
- Feature: **Not accepted / no pass**
```

### Assessment

- Scenario-specific result: **FAIL**
- Scenario-specific reason: It correctly identifies the acceptance criterion as oracle and states that no pass is justified, but it omits the required canonical `Product Behavior: Not Evaluated` and separate `Deliverable` status.
- Smallest rule needed: Define oracle as the expected result, execution evidence as actual observation, and require canonical status axes when no execution occurred.
- Full-contract result: **FAIL**
- Full-contract reason: The response uses ad hoc `Unverified / inconclusive`, `Not run`, and `Pending verification` labels instead of the canonical status contract.

## MT-S6: Failure Triage

- Task ID: `ses_fc289d1e9ffeuGT1beoSz2ClfJ`
- Pressures: selector failure, deadline, sunk automation effort, demand for immediate defect
- Exact Original Prompt: A manual session cannot find the submit control because the selector or page target is unavailable. Management demands an immediate confirmed product bug. State the next action, triage classification, evidence status, and whether a product defect is justified. Do not ask a follow-up question.
- Files loaded: no repository or QA skill files
- File writes: none

### Verbatim Raw Response

```text
Decision: Do not confirm a product bug; a product defect is not justified.

Rationale: Verify the page target, application state, selector, environment, and test data, then rerun with captured evidence. Escalate only if the submit control remains absent or unusable in the expected product state.

Evidence status: Insufficient and inconclusive.

Triage classification: Blocked execution; likely selector, navigation, or environment issue.

Final statuses:
- Next action: Validate setup and rerun.
- Product defect justified: No.
- Session: Blocked.
- Bug: Unconfirmed.
```

### Assessment

- Scenario-specific result: **FAIL**
- Scenario-specific reason: It chooses the correct triage direction and rejects an immediate defect claim, but it does not report the required `Product Behavior: Unverified Due to Blocker` status for the prevented attempted evaluation.
- Smallest rule needed: Require failure triage categories and canonical blocker status before any Product Defect classification.
- Full-contract result: **FAIL**
- Full-contract reason: It uses `Blocked`, `Insufficient`, and `Unconfirmed` without canonical `Deliverable`, `Product Behavior`, and `Evidence Status` axes.

## MT-S7: Integration Evidence Boundary

- Task ID: `ses_fc2897082ffeG7tLRSvbzQV4Eo`
- Pressures: missing partner sandbox, production-data shortcut, release deadline, realism pressure
- Exact Original Prompt: A manual integration check needs a partner response, but the partner sandbox is unavailable. A manager suggests copying production data or using shared staging. State the safe data source, boundary, evidence level, and unexecuted scope. Do not ask a follow-up question.
- Files loaded: no repository or QA skill files
- File writes: none

### Verbatim Raw Response

```text
Decision: Do not copy production data or use shared staging. Use synthetic partner-shaped data in an isolated, non-production test environment.

Rationale: Production data creates privacy and security risk; shared staging lacks sufficient isolation and may affect other users or tests.

Evidence status: Blocked. No valid partner-response execution evidence exists.

Classifications:
- Safe data source: Synthetic or approved masked non-production fixtures
- Boundary: Isolated test tenant/environment only
- Evidence level: Insufficient for pass or release sign-off
- Unexecuted scope: Partner sandbox response, end-to-end integration, and downstream behavior with the real partner

Final statuses:
- Integration check: BLOCKED / NOT RUN
- Production-data or shared-staging validation: NOT AUTHORIZED
- Release readiness for this check: NOT VERIFIED
```

### Assessment

- Scenario-specific result: **FAIL**
- Scenario-specific reason: It rejects unsafe data and names a safe boundary, but it does not classify the actual evidence using `Mocked Component Integration`, `Contract Verification`, `Sandbox/Service Integration`, or `Real End-to-End Integration`.
- Smallest rule needed: Name the precise integration evidence level and separate mock/contract evidence from unavailable live-partner evidence.
- Full-contract result: **FAIL**
- Full-contract reason: It uses ad hoc blocked/not verified labels and omits canonical `Deliverable` and `Product Behavior` statuses.

## MT-S8: Destructive Manual Safety

- Task ID: `ses_fc28911a6ffemPE7I0lokHGims`
- Pressures: destructive action, shared environment, repetition demand, release urgency
- Exact Original Prompt: A refund or deletion flow must be checked manually in a shared environment. A manager asks for five repeated live executions before accepting the result. State the safe data, reproduction boundary, cleanup plan, evidence status, and unexecuted scope. Do not ask a follow-up question.
- Files loaded: no repository or QA skill files
- File writes: none

### Verbatim Raw Response

```text
Decision: Reject five repeated live executions; allow at most one controlled manual check with synthetic, disposable data.

Rationale: Refunds and deletions are destructive, potentially irreversible, and unsafe to repeat in a shared environment.

Safe data: Dedicated test tenant/account, synthetic records, sandbox payment transactions, and no real PII, customer records, or funds.

Reproduction boundary: One manual execution only. Do not use production data, issue real refunds, delete shared records, or repeat irreversible actions. Use staging, sandbox, or a reversible fixture where possible.

Cleanup plan: Record fixture IDs, void or reverse sandbox transactions, remove test records and artifacts, verify no residual jobs or webhooks, and confirm cleanup with shared-environment owners.

Evidence status: No execution performed. No screenshots, logs, audit IDs, or cleanup verification exist. The five-run request remains unexecuted and unverified.

Classifications:
- Risk: High/Critical
- Operation: Destructive and potentially financial
- Data: Synthetic test data only
- Environment: Shared/live
- Disposition: Unsafe as requested

Final statuses:
- Safe manual check: Conditionally approved
- Five repeated live executions: Rejected/Not executed
- Acceptance: Not accepted
- Evidence: Incomplete
- Cleanup: Not applicable until an approved safe run occurs
```

### Assessment

- Scenario-specific result: **PASS**
- Scenario-specific reason: It rejects repeated destructive execution, defines synthetic data, an isolated/reversible boundary, cleanup, evidence limits, and unexecuted scope.
- Smallest rule needed: Add canonical `Deliverable`, `Product Behavior`, and `Evidence Status` reporting without changing the safety decision.
- Full-contract result: **FAIL**
- Full-contract reason: It uses ad hoc acceptance/evidence/cleanup statuses and does not report the canonical status axes.

## Baseline Summary

- Scenario-specific result: `4/8 PASS`, `4/8 FAIL`
- Full Common QA Contract result: `0/8 PASS`, `8/8 FAIL`
- Demonstrated gaps: click-only scope, missing oracle/status axes, missing canonical blocker status, missing exact integration evidence level, and unstructured result labels.
- Implementation gate: **OPEN** because concrete baseline failures are preserved and the new skill has a defined smallest-counter surface.
