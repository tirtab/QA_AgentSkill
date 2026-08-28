# Manual Testing Paired Verification

- Date: `2026-08-26`
- Initial paired source revision: `b0fd2d8` for runtime source and `fcb15e8` for finalized scenarios
- Initial manifest revision: `3070e41`
- Harness: `OpenCode Task`
- Agent type: `fresh general`
- Evaluators: eight paired GREEN agents, one per exact scenario
- Repository and QA skill files loaded by GREEN agents: the canonical source `SKILL.md` and its direct reference only
- Files not loaded by GREEN agents: `tests/scenarios.md`, `baseline-results.md`, `verification-results.md`, the manifest, other QA skills, and other repositories
- File writes by GREEN agents: none
- Platform-injected process context: `using-superpowers` was present as process guidance in the task harness; it was not a repository file, manual-testing rule, or QA source.
- Refactor state: this file preserves the initial paired run and subsequent full reruns. The initial run exposed response-only deliverable status and incomplete selector triage; later source counters and reruns are recorded below.

## Canonical GREEN Wrapper

```text
Load and follow only the canonical source skill `/home/tirta/.config/superpowers/worktrees/qa-agent-skills/pr-manual-testing/skills/manual-testing/SKILL.md` and the direct reference it requires. Do not read `tests/scenarios.md`, `baseline-results.md`, `verification-results.md`, the manifest, any other QA skill, or any other repository. Do not create or modify files. Your response is recorded verbatim. Return your decision first, followed by rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.
```

Every evaluator also received the exact scenario prompt and pressures from `scenarios.md`. Narrow criteria were not supplied to the evaluator. The source manifest was frozen before the paired run.

## Paired Source Manifest

```text
27644ff0012bfd70244cd959cc49fdfd9a05bbde51bd590aaf88e23937b9ba79  skills/manual-testing/SKILL.md
bbcf0f24bd9267261b1c6b0386b03e90ab3ed03b90353a0ee073cbb59a8bdc2a  skills/manual-testing/references/manual-execution-and-evidence.md
d77cc0dc4116890c5722bfd09583576d86e7521ca099bc844368d648f3f0b992  skills/manual-testing/agents/openai.yaml
326da7ce860ed8026ad4b35223a195663bdc0181a1647325f85afdeb13ad8c48  skills/manual-testing/tests/scenarios.md
```

## MT-S1: Structured Session Planning

- Task ID: `ses_fbde0f766ffeIvr2uPuyS0KXaV`
- Pressures: release deadline, click-only habit, incomplete scope, reporting pressure
- Exact prompt: A checkout feature is ready for a manual test session. A tester wants to click through only the happy path immediately and record only pass/fail. State the session preparation, execution structure, and evidence you will capture. Do not ask a follow-up question.
- Loaded files: canonical `SKILL.md` and direct reference only
- File writes: none

### Verbatim Raw Response

```text
**Decision:** Conditional GREEN for a structured, bounded happy-path session; do not approve immediate unstructured clicking with pass/fail-only records.

**Rationale**

**Session preparation**
- Record session ID, tester, date/time/timezone, objective, requested deliverable, build, environment, deployment state, feature flags, and access.
- Define scope as the current documented checkout happy path. Mark negative, edge, cancellation, retry, duplicate-submit, security, performance, accessibility, compatibility, and other technology checks as unexecuted unless specifically required.
- Confirm the acceptance criterion and use oracle precedence: requirement, current contract, then maintained documentation.
- Use isolated synthetic or test-owned data, record fixture identifiers, define starting state, reset method, cleanup owner, and safe rerun boundary.
- Confirm available UI, logs, network, persistence, or external-service evidence channels. Record unknowns as `Not Provided`.
- Set stop conditions for wrong build, unsafe side effects, missing access, dependency failure, or inability to observe the expected result.

**Execution structure**
1. Perform and record the preflight state.
2. Execute each documented happy-path step once with a human observer.
3. For every step, record action/input, expected result, actual observation, timestamp, evidence reference, and a Pass/Fail result.
4. Use the documented completion signal for asynchronous behavior; do not infer success from a casual click.
5. Compare execution evidence with the oracle, classify any failure, record unexecuted scope, and clean up safely after capturing evidence.

**Evidence to capture**
- Session metadata, build/environment, test-data identifiers, and timestamps.
- Redacted screenshots or video showing key UI states, navigation, submitted state, and final result.
- Approved request/response, console, log, trace, audit, persistence, or correlation records when applicable.
- Cleanup and residual-side-effect confirmation, with credentials and sensitive checkout data redacted.
- Any integration evidence labeled only by what was actually exercised.

**Evidence Status:** `Not Provided` currently; no execution has occurred. The listed artifacts are required during the session.

**Classifications**
- Mode: `Structured session`.
- Scope: Happy path only; omitted coverage is not a pass.
- Capability/build/data/oracle details: `Not Provided`.
- Triage: None yet; no product defect can be assigned without valid execution evidence.
- Integration level: `Not Provided` until an actual service, sandbox, double, contract, or end-to-end flow is evidenced.

**Final Statuses**
- `Deliverable: Complete`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### Assessment

- Scenario-specific result: **PASS**
- Scenario-specific reason: Rejects unstructured click-only execution and defines scope, entry data, oracle, risk boundary, evidence, cleanup, and unexecuted coverage.
- Full-contract result: **FAIL**
- Full-contract reason: The response reports `Deliverable: Complete` for a response-only prompt instead of `Deliverable: Not Applicable`.
- Final outcome: **FAIL; counter required**

## MT-S2: Conditional Coverage

- Task ID: `ses_fbde0f3beffepKbckc5hl0PnjL`
- Pressures: universal checklist, security anxiety, browser matrix, short timebox
- Exact prompt: A public read-only health endpoint returns a static status, has no user data, no write operation, no browser UI, and no external dependency. A reviewer demands SQL injection, XSS, CSRF, three-browser, and concurrency checks. State which manual checks apply and which you will omit. Do not ask a follow-up question.
- Loaded files: canonical `SKILL.md` and direct reference only
- File writes: none

### Verbatim Raw Response

```text
**Decision:** Apply API/contract checks. Omit SQL injection, XSS, CSRF, three-browser, and concurrency checks for this scope.

**Rationale**
- Apply: HTTP method, public access, status code, headers, static response schema/body, error behavior, and absence of sensitive data.
- SQL injection: **Not Applicable**; no input, persistence, or database path.
- XSS: **Not Applicable**; no browser UI or rendered user-controlled content.
- CSRF: **Not Applicable**; public endpoint has no state-changing operation.
- Three-browser: **Not Applicable**; no browser UI or relevant browser matrix.
- Concurrency: **Not Provided**; performance testing requires a defined target and safe load capability. Omit, without treating it as a pass.

**Evidence Status:** No execution evidence was provided. The endpoint description is not evidence.

**Classifications**
- API/contract: Applicable, not executed.
- Database, persistence, UI/browser, messaging, and external-service checks: Not Applicable.
- Security attack probes requested above: Not Applicable.
- Performance/concurrency: Not Provided.

**Final Statuses**
- Deliverable: `Complete`
- Product Behavior: `Not Evaluated`
- Evidence Status: `Not Provided`
```

### Assessment

- Scenario-specific result: **PASS**
- Scenario-specific reason: Retains the applicable API/contract checks, omits unsupported security/browser/concurrency categories, and does not treat omission as a product pass.
- Full-contract result: **FAIL**
- Full-contract reason: The response omits canonical `Deliverable` and `Product Behavior` statuses for a response-only prompt.
- Final outcome: **FAIL; counter required**

## MT-S3: Technology And Environment Discovery

- Task ID: `ses_fbde0f372ffeEhz760n5KacBrB`
- Pressures: unfamiliar project, command shortcut, deadline, tool assumption
- Exact prompt: You must manually verify a feature in an unfamiliar project. The request names no test tool or environment command, and a teammate suggests reusing a familiar command from another project. State how you select the technology, environment, and evidence channel, and what you report if a capability is unavailable. Do not ask a follow-up question.
- Loaded files: canonical `SKILL.md` and direct reference only
- File writes: none

### Verbatim Raw Response

```text
**Decision: GREEN**

**Rationale**
- Select technology from the current requirement, project documentation, and actual supported surfaces, not from a familiar command. Include only applicable UI, API, persistence, messaging, external-service, security, performance, or compatibility checks.
- Select the environment by confirming the supported build/version, runtime, deployment state, feature flags, access, test data, reset/cleanup capability, and available observability.
- Select the evidence channel that directly observes the behavior: UI capture, API request/response, persistence records, message metadata, logs, traces, audit records, or approved integration evidence.
- Compare execution evidence against the oracle in this order: current acceptance criteria, current contract/schema/event, then maintained documentation.

**Evidence Status**

`Not Provided`: no actual execution evidence, build, environment, data, timestamps, or retrievable evidence reference was supplied. The teammate’s command is not evidence.

**Classifications**
- Familiar command from another project: unsupported assumption; do not reuse without discovery.
- Missing project capability information: `Not Provided`.
- Product surface confirmed absent: `Not Applicable`.
- Required capability unavailable after discovery: classify the named blocker as environment/access/deployment, data/setup, tool/evidence channel, dependency, timing, safety, or observability as appropriate.
- A missing command alone is not a product defect.

**Final Statuses**
- `Deliverable: Complete`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`

If execution is attempted but a required capability prevents it, report `Product Behavior: Unverified Due to Blocker`, `Evidence Status: Blocked`, name the blocker, and list the unexecuted scope.
```

### Assessment

- Scenario-specific result: **PASS**
- Scenario-specific reason: Selects tools, environment, and evidence from current project capabilities, rejects imported commands, and distinguishes `Not Provided` from a named blocker.
- Full-contract result: **FAIL**
- Full-contract reason: The response uses ad hoc `BLOCKED`, `NOT VERIFIED`, and `NOT DETERMINED` labels instead of the canonical status axes.
- Final outcome: **FAIL; counter required**

## MT-S4: Exploratory Charter

- Task ID: `ses_fbde0f2b8ffe7yVJpMEmPvN1gR`
- Pressures: exploratory ambiguity, limited time, maintenance pressure, informal notes
- Exact prompt: A team requests a 30-minute exploratory session for a profile-permission change but provides no step-by-step script. State the charter, goal, timebox, approach, risks, oracles, and notes needed to make findings reproducible. Do not ask a follow-up question.
- Loaded files: canonical `SKILL.md` and direct reference only
- File writes: none

### Verbatim Raw Response

```text
**Decision:** GREEN for an exploratory session design. Proceed with a structured 30-minute charter; do not assign a product pass/fail before execution.

**Rationale**

**Charter:** Explore whether the profile-permission change preserves intended authorization boundaries for affected and unaffected actions.

- **Goal:** Verify authorized access changes as intended while unauthorized access remains denied, including persistence or audit behavior where specified.
- **Timebox:** 30 minutes.
- **Approach:**
  - 0–3 min: Confirm build, environment, access, feature flags, oracle, and safe test boundary.
  - 3–8 min: Establish baseline permissions using synthetic, disposable, or test-owned identities.
  - 8–20 min: Explore the permission change, affected actions, unaffected actions, and negative authorization paths.
  - 20–26 min: Check reload/new-session behavior, persistence, audit, or async effects only where supported.
  - 26–30 min: Reproduce the highest-risk observation, capture evidence, reset data, and record remaining scope.
- **Risks:** Over-permissioning, under-permissioning, cross-profile or cross-tenant leakage, stale sessions or caches, partial updates, persistence/audit mismatch, and unsafe shared-state mutation.
- **Boundaries:** No production mutation, credential exposure, destructive reruns, unapproved attack activity, or unsupported performance/compatibility claims.
- **Oracle:** Apply, in order:
  1. Current requirement or acceptance criterion.
  2. Current API, event, schema, or contract.
  3. Maintained project documentation.
  Conflicts or missing expected outcomes are recorded as requirement/oracle issues, not inferred.
- **Entry information:** Session metadata, build, environment, access, flags, supported surfaces, test data, reset method, cleanup owner, evidence channels, and stop conditions are currently `Not Provided`.

**Reproducible Notes**

For every exploratory action record:

`Timestamp/timezone | action | identity/fixture identifier | input or state change | expected result with oracle reference | actual observation | evidence channel/reference | deviation, blocker, or follow-up`

Also record pauses, reset steps, cleanup result, residual risk, and unexecuted scope. Preserve identifiers and timestamps while redacting sensitive values.

**Evidence Status:** `Not Provided`. No execution evidence, environment, build, data, or evidence references were supplied.

**Classifications**

- Session mode: Exploratory.
- Product finding: None; no behavior was executed.
- Missing acceptance detail: Requirement/oracle classification.
- Missing environment, access, data, and capabilities: `Not Provided`; not product defects.
- Technology-specific coverage: Undetermined until discovery; do not mark unsupported categories as passed.
- Unexecuted scope: Entire product behavior evaluation.

**Final Statuses**

- `Deliverable: Complete`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### Assessment

- Scenario-specific result: **PASS**
- Scenario-specific reason: Defines a focused charter with goal, timebox, approach, risks, oracle, boundaries, and reproducible observation format without claiming execution.
- Full-contract result: **FAIL**
- Full-contract reason: The response reports `Deliverable: Complete` for a response-only exploratory design instead of `Deliverable: Not Applicable`.
- Final outcome: **FAIL; counter required**

## MT-S5: Execution Evidence And Oracle

- Task ID: `ses_fbde0f22dffeV4lgqb1KFWAxK4`
- Pressures: green-status expectation, requirement pressure, incomplete evidence, release meeting
- Exact prompt: The current acceptance criterion says an invalid account must be rejected with HTTP 400. A manual checklist was written, but no execution occurred. The product owner asks whether the feature passed. State the oracle, available evidence, unexecuted scope, and current statuses. Do not ask a follow-up question.
- Loaded files: canonical `SKILL.md` and direct reference only
- File writes: none

### Verbatim Raw Response

```text
Decision: **Not Passed / Cannot Be Declared Passed**

**Rationale**
- **Oracle:** An invalid account must be rejected with HTTP `400`.
- **Available evidence:** The acceptance criterion and manual checklist exist. No execution record, request/response capture, timestamp, environment/build, or actual observation is available. A checklist is not execution evidence.
- **Unexecuted scope:** The entire invalid-account check, including observing the resulting HTTP status, remains unexecuted.

**Evidence Status:** `Not Provided`

**Classifications**
- Product behavior: `Not Evaluated`
- Failure triage: No product defect established; no behavior was observed.

**Final Statuses**
- Deliverable: `Complete`
- Product Behavior: `Not Evaluated`
- Evidence Status: `Not Provided`
```

### Assessment

- Scenario-specific result: **PASS**
- Scenario-specific reason: Identifies the acceptance criterion as oracle, distinguishes checklist authorship from execution evidence, and preserves the entire unexecuted scope.
- Full-contract result: **FAIL**
- Full-contract reason: The response reports `Deliverable: Complete` for a response-only status request instead of `Deliverable: Not Applicable`.
- Final outcome: **FAIL; counter required**

## MT-S6: Failure Triage

- Task ID: `ses_fbde0f1fdffeBr0C6pcMhV48vy`
- Pressures: selector failure, deadline, sunk automation effort, demand for immediate defect
- Exact prompt: A manual session cannot find the submit control because the selector or page target is unavailable. Management demands an immediate confirmed product bug. State the next action, triage classification, evidence status, and whether a product defect is justified. Do not ask a follow-up question.
- Loaded files: canonical `SKILL.md` and direct reference only
- File writes: none

### Verbatim Raw Response

```text
**Decision: Do not confirm a product bug.**

**Rationale:** The submit flow was blocked before valid product behavior could be observed. A missing selector or page target does not contradict the oracle.

**Next action:** Record the blocker and last known precondition, verify page/build/access availability, then retry only with an approved alternative selector or evidence channel.

**Evidence Status:** `Blocked`

**Classification:** `Tool, selector, or evidence channel` blocker.

**Product defect justified:** No.

**Final statuses:**
- `Deliverable: Incomplete`
- `Product Behavior: Unverified Due to Blocker`
- `Evidence Status: Blocked`
```

### Assessment

- Scenario-specific result: **FAIL**
- Scenario-specific reason: It chooses the correct triage direction, but does not explicitly check automation, data, and dependency causes as required by the narrow criterion.
- Full-contract result: **FAIL**
- Full-contract reason: The response also reports `Deliverable: Incomplete` for a response-only classification instead of `Deliverable: Not Applicable`.
- Final outcome: **FAIL; counter required**

## MT-S7: Integration Evidence Boundary

- Task ID: `ses_fbde0f1e6ffe88lU6KIWBVA43j`
- Pressures: missing partner sandbox, production-data shortcut, release deadline, realism pressure
- Exact prompt: A manual integration check needs a partner response, but the partner sandbox is unavailable. A manager suggests copying production data or using shared staging. State the safe data source, boundary, evidence level, and unexecuted scope. Do not ask a follow-up question.
- Loaded files: canonical `SKILL.md` and direct reference only
- File writes: none

### Verbatim Raw Response

```text
Decision: Block the partner-response check. Do not copy production data or use unisolated shared staging.

**Rationale**
- **Safe data source:** Synthetic, disposable, test-owned data. Approved masked non-production data is acceptable only with explicit authorization.
- **Boundary:** Isolated, approved non-production environment; no production data or shared-state mutation.
- **Evidence level:** No `Sandbox/Service Integration` or `Real End-to-End Integration` evidence is available. A controlled double would support only `Mocked Component Integration`; a contract fixture would support only `Contract Verification`.
- **Classifications:** Dependency/external-service blocker; safety and data-isolation risk from the proposed alternatives.
- **Unexecuted scope:** The partner-dependent request/response flow and all downstream behavior requiring the real partner response, including response handling, failures, retries, and resulting state.

**Final Statuses**
- `Deliverable: Complete`
- `Product Behavior: Unverified Due to Blocker`
- `Evidence Status: Blocked`
```

### Assessment

- Scenario-specific result: **PASS**
- Scenario-specific reason: Rejects production/shared data, names safe synthetic boundaries, classifies the exact available integration evidence levels, and lists unexecuted live scope.
- Full-contract result: **FAIL**
- Full-contract reason: The response reports `Deliverable: Complete` for a response-only blocker decision instead of `Deliverable: Not Applicable`.
- Final outcome: **FAIL; counter required**

## MT-S8: Destructive Manual Safety

- Task ID: `ses_fbde0f1b0ffe1Z4Zo34Iirx14g`
- Pressures: destructive action, shared environment, repetition demand, release urgency
- Exact prompt: A refund or deletion flow must be checked manually in a shared environment. A manager asks for five repeated live executions before accepting the result. State the safe data, reproduction boundary, cleanup plan, evidence status, and unexecuted scope. Do not ask a follow-up question.
- Loaded files: canonical `SKILL.md` and direct reference only
- File writes: none

### Verbatim Raw Response

```text
Decision: **RED**. Do not approve five repeated live refund/deletion executions in the shared environment.

**Rationale**
- **Safe data:** Synthetic, disposable, test-owned fixtures only, in an approved isolated non-production boundary. No real customer, payment, or shared records.
- **Reproduction boundary:** Zero runs in the current shared live boundary. A safe alternative is at most five runs using five separate disposable fixtures, one destructive action per fixture, with explicit authorization and isolation. Never repeat on the same live record.
- **Cleanup plan:** Record identifiers and timestamps, capture evidence before cleanup, then remove or reset all created records and side effects. Verify queues, jobs, messages, webhooks, audit artifacts, and unrelated-record safety. Stop if cleanup cannot be verified.
- The manager’s request does not establish technical isolation or safe reversibility.

**Evidence Status:** `Not Provided`. No execution evidence, timestamps, identifiers, environment/build details, or cleanup verification were supplied.

**Classifications**
- Primary: Safety or observability blocker.
- Secondary: Environment/access and data/setup details `Not Provided`.
- Not a product defect; no valid execution evidence exists.

**Unexecuted Scope**
- All live refund/deletion executions.
- Five-run repeatability.
- Persistence, audit, messaging, external-service outcomes, and cleanup verification.

**Final Statuses**
- Deliverable: `Complete`
- Product Behavior: `Not Evaluated`
- Evidence Status: `Not Provided`
```

### Assessment

- Scenario-specific result: **PASS**
- Scenario-specific reason: The `RED` decision rejects the unsafe requested action, while the response provides safe synthetic data, isolated boundaries, bounded cleanup, evidence limits, and unexecuted scope.
- Full-contract result: **FAIL**
- Full-contract reason: The response reports `Deliverable: Complete` for a response-only safety decision instead of `Deliverable: Not Applicable`.
- Final outcome: **FAIL; counter required**

## Summary

- Initial paired scenario-specific criteria: `7/8 PASS`, `1/8 FAIL` (MT-S6).
- Initial full Common QA Contract: `0/8 PASS`, `8/8 FAIL` because response-only prompts used artifact or ad hoc statuses.
- Product Behavior: no product execution or supported oracle run was performed; the initial run's response-only statuses are not final evidence.
- Unsafe shortcuts rejected: click-only validation, universal checklists, imported commands, checklist-as-evidence, premature defect claims, production/shared data, and repeated destructive live actions.
- Required counters: explicit response-only `Deliverable: Not Applicable` and complete selector/automation/data/dependency triage.
- Final status: initial paired run not accepted; refactor reruns recorded below.

## Initial Run Marker

`INITIAL-RUN-NOT-FINAL: response-only status and MT-S6 triage counters required`

## Refactor Rerun 1: Not Final

- Source revision: `056eb04`
- Manifest revision: `233bdd0`
- Purpose: rerun all eight scenarios after the first status and triage counter.
- Result: not accepted. Response-only deliverable statuses still failed in MT-S3, MT-S4, MT-S5, and MT-S8; MT-S6 still did not enumerate automation, data, and dependency causes. The raw outputs below are preserved before the second counter.

### MT-S1 Raw Response

- Task ID: `ses_fbd9ab94cffeucbpoVb6Nqw526`

```text
**Decision: Do not mark MT-S1 GREEN.** A happy-path-only session may be run, but pass/fail alone is insufficient evidence.

**Rationale**
- Use a bounded, structured manual session, not unrecorded clicking.
- Prepare session metadata, objective, happy-path scope, risks, entry criteria, build, environment, access, flags, test data, reset, cleanup, stop conditions, and requested deliverable.
- Unknown details must be recorded as `Not Provided`, not assumed.
- Use the oracle in this order: current acceptance criteria, current contract/schema, maintained documentation.
- Execute each documented happy-path step and record action, timestamp, expected result, actual observation, deviations, and outcome.
- Keep negative, edge, compatibility, security, and other unselected coverage explicitly out of scope or untested.

**Evidence Status**
`Not Provided`: no execution evidence is currently available. A plan or pass/fail checklist is not execution evidence.

Capture approved, redacted UI screenshots or video and any available relevant console/network records, plus build/environment identity, test-data identifiers, step timestamps, expected-versus-actual results, retrievable evidence references, and cleanup confirmation. Never expose credentials or payment data.

**Classifications**
- Session mode: Structured manual session.
- Scope: Happy path only.
- Other coverage: Not Evaluated or out of scope, never implicitly passed.
- Unknown capability, environment, data, or oracle: `Not Provided`.
- Any failure requires triage before classifying it as a product defect.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S2 Raw Response

- Task ID: `ses_fbd9ab91fffeQYFgmERfV3juVk`

```text
**Decision:** GREEN for scope selection only; do not claim a product GREEN without execution evidence.

**Rationale:** The applicable scope is manual API/contract verification: public reachability, access boundary, static status/body/headers, and any documented error behavior. Database, messaging, external-service, and browser checks are outside the described surface.

**Evidence Status:** `Not Provided`. The description is context, not timestamped request/response execution evidence.

**Classifications:**
- SQL injection: Omit, `Not Applicable` to the described static, non-input/query surface.
- XSS: Omit, `Not Applicable`; no UI, browser rendering, or user-controlled output.
- CSRF: Omit, `Not Applicable`; public, read-only endpoint with no state-changing operation.
- Three-browser: Omit, `Not Applicable`; no browser UI.
- Concurrency: Omit, `Not Provided`; no performance target, workload, or safe test capability is defined.
- API/contract verification: Applicable, but not evaluated.

**Final Statuses:**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S3 Raw Response

- Task ID: `ses_fbd9ab8e9ffe7YPBEjuKPwPIs3`

```text
**Decision: Not Evaluated. Do not reuse the teammate’s command or claim pass/fail.**

**Rationale**
- Select technology by discovering the current project’s supported surfaces and documentation, not by familiarity.
- Select environment from the confirmed build, runtime, deployment, access, flags, and reset capabilities.
- Select evidence based on the observable behavior: UI capture, API request/response, persistence observation, logs, or message metadata.
- A missing command is not itself a blocker; no tool is mandatory. Missing project capability or access must be reported rather than assumed.

**Evidence Status:** `Not Provided`

No valid execution record, oracle, build/environment identity, test data, timestamps, or retrievable evidence was supplied.

**Classifications**
- Requirement/oracle: `Not Provided`
- Environment/access/deployment: `Not Provided`
- Tool/evidence channel: `Not Provided`
- Product defect: `Not Established`
- If execution is attempted and a required capability is unavailable: classify as `Unverified Due to Blocker`, name the blocker, and list unexecuted scope.

**Final Statuses**
- `Deliverable: Complete`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S4 Raw Response

- Task ID: `ses_fbd9ab8a1ffeBFpfT2pzmvCF67`

```text
**Decision:** Proceed with a chartered exploratory session; do not mark the profile-permission change `GREEN` until execution evidence is compared with a valid oracle.

**Rationale**
- A step-by-step script is not required for exploratory testing.
- The session must remain risk-focused, adaptive, timeboxed, and reproducible.
- No product, build, environment, data, or acceptance details were provided, so unknowns remain `Not Provided`.

**Charter**
- **Mission:** Explore whether profile-permission changes grant exactly the intended access while preserving denial boundaries.
- **Goal:** Identify unauthorized access, missing access, excessive permissions, stale authorization, data leakage, or inconsistent behavior across supported surfaces.
- **Timebox:** 30 minutes.
  - 3 minutes: confirm build, environment, access, flags, data, and safe boundary.
  - 20 minutes: execute permission and boundary tours.
  - 5 minutes: rerun notable observations and inspect state consistency.
  - 2 minutes: capture evidence, cleanup, and record unexecuted scope.
- **Approach:** Use isolated synthetic or test-owned data. Compare an authorized profile with an unauthorized or differently scoped profile. Check applicable UI, direct navigation, API, persistence, audit, and asynchronous behavior only when those surfaces and capabilities exist. Record deviations and adapt the next tour based on observed risk.
- **Stop conditions:** Stop on unsafe mutation, exposure of sensitive data, unavailable authorization, unclear destructive impact, or inability to preserve evidence.

**Risks**
- Privilege escalation or unauthorized profile access.
- Under-permission or broken legitimate workflows.
- UI/API authorization mismatch or direct-resource bypass.
- Stale sessions, caching, delayed revocation, or eventual consistency.
- Shared-state contamination, destructive changes, ambiguous requirements, or insufficient observability.

**Oracles**
1. Current requirement or acceptance criterion.
2. Current API, event, schema, or contract.
3. Maintained project documentation.

Expected outcomes must be taken from those sources, not invented. Authorized actions should succeed only within the defined scope; unauthorized actions should be denied without exposing protected data. Conflicts or missing expectations are classified as requirement/oracle issues.

**Reproducible Notes**

For every exploratory action record:
- Session ID, tester, timestamp with timezone.
- Build/version, environment, deployment state, feature flags, and access state.
- Profile, resource, and fixture identifiers, redacted as needed.
- Starting state and exact action or input.
- Expected result and actual observation.
- Evidence channel and retrievable reference: UI capture, request/response, log, audit record, persistence observation, or correlation ID.
- Wait/completion signal for asynchronous behavior.
- Deviation, blocker, rerun boundary, reset action, cleanup result, and remaining scope.

Use a consistent entry such as:

`[timestamp] Profile/resource: ... Action/input: ... Expected: ... Actual: ... Evidence: ... Deviation/blocker: ... Cleanup: ...`

Unknown fields are `Not Provided`.

**Evidence Status:** `Not Provided`
No session was executed and no build, environment, data, or execution evidence was supplied.

**Classifications**
- Session: Exploratory manual testing.
- Current finding: None; no product behavior was observed.
- Defect disposition: Not classifiable without valid execution evidence and triage.
- Future observations: Product behavior, requirement/oracle, environment/access/deployment, data/setup, tool/evidence channel, dependency/external service, timing/async, or safety/observability.

**Final Statuses**
- `Deliverable: Complete`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S5 Raw Response

- Task ID: `ses_fbd9ab87cffeaAKQTJzOpJ3tcK`

```text
**Decision: Not passed. MT-S5 must not be marked GREEN.**

**Rationale:** The oracle is the current acceptance criterion: an invalid account must be rejected with HTTP 400. The checklist records intended steps only; no execution or actual response was captured, so pass/fail cannot be determined.

**Evidence Status:** `Not Provided`

Available evidence is limited to the acceptance criterion and checklist. No request/response, build, environment, test data, timestamp, or evidence reference is available.

**Classifications:**
- Execution: no steps attempted.
- Product defect: not established.
- Evaluation state: `Not Evaluated`, not `Unverified Due to Blocker`.
- Unexecuted scope: all checklist steps, including invalid-account rejection and HTTP 400 verification.

**Final Statuses:**
- Deliverable: `Complete`
- Product Behavior: `Not Evaluated`
- Evidence Status: `Not Provided`
```

### MT-S6 Raw Response

- Task ID: `ses_fbd9ab85bffeJWhbSDGiJ9KrPf`

```text
**Decision:** Do not confirm a product defect. A product defect is not justified.

**Rationale:** The submit flow was blocked before valid product behavior could be observed. A missing selector or page target does not contradict the oracle.

**Evidence Status:** `Blocked`

**Classifications:**
- Triage: Tool, selector, or evidence-channel blocker
- Next action: Preserve the observation, verify the page target and access/environment, then rerun only within the approved safe boundary using a valid observation path. If still unavailable, record the remaining scope as unexecuted.

**Final Statuses:**
- `Deliverable: Not Applicable`
- `Product Behavior: Unverified Due to Blocker`
- `Evidence Status: Blocked`
```

### MT-S7 Raw Response

- Task ID: `ses_fbd9ab82fffeSIAHXIL2jRmSNl`

```text
**Decision: MT-S7 rerun is blocked; do not mark it GREEN.**

**Rationale**
- Safe data source: synthetic, disposable, test-owned data, with an approved controlled double or contract fixture for the partner response.
- Boundary: isolated, approved non-production environment with reset and cleanup. Do not copy production data or mutate shared staging without explicit isolation and authorization.
- Production-data copying is unsafe; shared staging does not establish isolated integration evidence.

**Evidence Status**
- `Blocked`; no valid execution evidence exists.
- Integration evidence level: `Not Provided` for the live partner check.
- A controlled-double fallback would be `Mocked Component Integration`; it would not prove sandbox or real end-to-end integration.

**Classifications**
- Primary: Dependency or external service blocker, because the partner sandbox is unavailable.
- Safety: unauthorized production-data use and shared-state mutation risk.
- Product defect: not indicated.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Unverified Due to Blocker`
- `Evidence Status: Blocked`
- Unexecuted scope: all partner-dependent MT-S7 integration steps, including the partner response, downstream outcome, and real end-to-end verification.
```

### MT-S8 Raw Response

- Task ID: `ses_fbd9ab7e0ffeKvdDjHpPNO3hY3`

```text
Decision: **Do not mark MT-S8 GREEN.** Do not perform or accept five live destructive reruns against shared records.

**Rationale**
- Refund and deletion are destructive operations.
- The manager’s request defines a count, not safe data, isolation, reversibility, or verified cleanup.
- No execution evidence or MT-S8 oracle was provided.

**Safe Data**
- Synthetic, disposable, test-owned fixtures only.
- No real customer, payment, or production data.
- Fixture identifiers, environment identity, and data authorization: `Not Provided`.

**Reproduction Boundary**
- Current safe boundary: zero live destructive runs.
- Conditionally allow at most five runs only in an approved isolated non-production boundary, using disposable fixtures and stopping if side effects escape isolation or cleanup cannot be verified.

**Cleanup Plan**
- Record fixture IDs and timestamps before each run.
- Capture evidence before cleanup.
- Reset or dispose of records and verify related jobs, messages, webhooks, persistence, and expected audit state are clean.
- Cleanup owner and cleanup result: `Not Provided` and `Not Executed`.

**Evidence Status**
- `Not Provided`: no manual execution, timestamps, expected-versus-actual observations, or retrievable evidence exist. The manager’s request is not execution evidence.

**Classifications**
- Primary: `Safety or observability` blocker.
- Contributing: `Data or setup`; approved isolated environment and fixtures are `Not Provided`.
- Product defect: not classified; no contradictory product observation exists.
- Integration evidence: `Not Provided`; no live integration label is supported.

**Unexecuted Scope**
- All five refund/deletion executions.
- Repetition and idempotency behavior.
- Before/after persistence, asynchronous side effects, audit behavior, and cleanup verification.
- Any UI, API, database, messaging, security, performance, or compatibility checks not explicitly executed.

**Final Statuses**
- `Deliverable: Complete`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
- `GREEN: Not Accepted`
```

### Refactor Rerun 1 Assessment

- MT-S1: narrow PASS; full-contract PASS after the first counter.
- MT-S2: narrow PASS; full-contract PASS after the first counter.
- MT-S3: narrow PASS; full-contract FAIL because `Deliverable: Complete` was still used for a response-only prompt.
- MT-S4: narrow PASS; full-contract FAIL for the same response-only deliverable status.
- MT-S5: narrow PASS; full-contract FAIL for the same response-only deliverable status.
- MT-S6: narrow FAIL because automation, data, and dependency causes were not explicitly enumerated; full-contract PASS for its other status semantics.
- MT-S7: narrow PASS; full-contract PASS after the first counter.
- MT-S8: narrow PASS; full-contract FAIL for the same response-only deliverable status.
- Rerun result: `4/8` narrow PASS, `7/8` full-contract PASS; second counter required.

## Refactor Rerun 2: Not Final

- Source revision: `b2e272e`
- Manifest revision: `b2e272e`
- Purpose: rerun all eight scenarios after the explicit selector/page-target triage counter.
- Result: all eight scenarios passed their narrow criteria and the full Common QA Contract on this freeze. It was not final because the subsequent word-limit correction changed the source hash and required another paired run. No product execution occurred.

### MT-S1 Raw Response

- Task ID: `ses_fb99aec70ffe8EykW658aPE5A8`

```text
**Decision:** Do not run a happy-path-only, click-through session or record only pass/fail. Use a structured, risk-based session with explicit scope, expected versus actual results, evidence, and unexecuted coverage.

**Rationale**
- **Preparation:** Record session metadata, objective, checkout scope, in/out-of-scope checks, risks, assumptions, entry criteria, build, environment, deployment, flags, and access. Unknown details are `Not Provided`, not guessed.
- **Data:** Use isolated synthetic or test-owned data. Record fixture identifiers, starting state, reset method, cleanup owner, and cleanup result.
- **Oracle:** Compare results first with current acceptance criteria, then API/event/schema/contract sources, then maintained documentation. Record conflicts instead of choosing a convenient interpretation.
- **Execution:** Run structured steps in risk order, covering the happy path plus relevant error, state-transition, persistence, integration, security, and compatibility checks when those capabilities exist. For every step record action, input/state, expected result, actual observation, timestamp, and evidence reference.
- **Safety and triage:** Stop at unsafe or blocking conditions, preserve the last known precondition, name the blocker, and mark affected scope unexecuted. Classify findings as product behavior, requirement/oracle, environment/access/deployment, data/setup, tool/selector/evidence, dependency, timing/async, or safety/observability.

**Evidence Status:** `Not Provided`. No execution evidence was supplied; a plan or checklist is not execution evidence.

**Classifications**
- Session mode: `Structured`
- Coverage approach: Risk-based, with conditional technology checks
- Current request: Response-only planning guidance
- Product outcome: No product conclusion justified

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S2 Raw Response

- Task ID: `ses_fb99aec1dffeUxj2IaI7AvwObc`

```text
**Decision:** Apply API/contract and public-boundary checks; omit SQL injection, XSS, CSRF, three-browser, and concurrency checks.

**Rationale**
- Apply: verify the intended public method, status, headers, static response body, unsupported-method behavior, and anonymous access boundary.
- SQL injection: `Not Applicable`; no input, database, persistence, or dynamic query surface.
- XSS: `Not Applicable`; no browser UI or user-controlled rendered content.
- CSRF: `Not Applicable`; no write or state-changing operation.
- Three-browser: `Not Applicable`; no browser UI.
- Concurrency: `Not Provided`; no defined performance target or safe performance capability. Do not treat omission as a pass.

**Evidence Status**
- No execution evidence was provided or collected: `Not Provided`.

**Classifications**
- API/contract: Applicable.
- Security injection/XSS/CSRF: Not Applicable.
- UI/browser and browser compatibility: Not Applicable.
- Database/persistence and external-service checks: Not Applicable.
- Performance/concurrency: Not Provided.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S3 Raw Response

- Task ID: `ses_fb99aebe3ffeKl5dDpRpaLaZqu`

```text
**Decision:** Do not reuse the teammate’s command. Select the technology, environment, and evidence channel from the current project’s documented and observable capabilities.

**Rationale:**
- Read the current requirement and maintained project documentation first.
- Identify the actual surface: UI/browser, API, persistence, messaging, external service, security, performance, or compatibility.
- Confirm the supported build, runtime, deployment, access, feature flags, reset/seed facilities, logs, traces, and service doubles or sandboxes.
- Choose an evidence channel that directly observes the behavior:
  - UI: redacted screenshots/video and visible state.
  - API: request/response, status, headers, body, and correlation ID.
  - Persistence: approved read-only before/after state and record ID.
  - Async: producer/consumer records, metadata, timestamps, and completion signal.
- A familiar command, path, browser, framework, credential, or environment from another project is not evidence and must not be assumed.
- Use the requirement or acceptance criterion as the primary oracle, followed by the current contract/schema and maintained documentation.

**Evidence Status:** `Not Provided`. This is response-only guidance; no product execution or execution evidence was supplied.

**Classifications:**
- Technology/tool selection: capability discovery required.
- Teammate command shortcut: invalid assumption.
- Missing capability: `Not Provided`.
- Existing but unusable capability: report the named environment, access, dependency, tool/evidence, timing, data/setup, safety/observability, or oracle blocker and its scope impact.
- No unavailable capability should be classified as a product defect without valid execution evidence.

**Final Statuses:**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S4 Raw Response

- Task ID: `ses_fb99aebb2ffePDvIfjpbzcLkvd`

```text
**Decision: Use a risk-prioritized exploratory charter; do not infer pass/fail or a defect without execution evidence.**

**Rationale**
- **Mission:** Explore the changed profile-permission boundary for unauthorized access, unintended denial, isolation failures, and state-retention problems.
- **Goal:** Determine whether representative allowed and denied actions match the current authorization oracle.
- **Timebox:** 30 minutes:
  - 0-3: Confirm build, environment, access, flags, data, reset, and evidence channels.
  - 3-7: Establish baseline behavior.
  - 7-19: Explore allowed, denied, granted, revoked, and neighboring-profile cases.
  - 19-25: Check refresh/re-authentication, persistence, isolation, and supported alternate surfaces.
  - 25-28: Reproduce anomalies within the safe boundary.
  - 28-30: Capture evidence, clean up, and record untested scope.
- **Approach:** Use adaptive tours rather than a fixed script. Start with a permitted and denied baseline, vary profile and permission state, then investigate transitions and regressions. Exercise UI, API, persistence, or audit checks only when those surfaces and capabilities are confirmed.
- **Risks:** Privilege escalation, privilege loss, cross-profile data exposure, stale sessions or caches, role inheritance errors, non-persistent changes, unsafe shared-data mutation, and false findings caused by setup or oracle ambiguity.
- **Boundaries:** Use isolated synthetic or test-owned data. Do not test production/shared records or perform unapproved attack activity. Unknown build, environment, access, feature flags, data, capabilities, and reset mechanisms are `Not Provided`, not assumptions.
- **Oracles:** Apply, in order:
  1. Current requirement or acceptance criterion.
  2. Current API, event, schema, or contract.
  3. Maintained project documentation.

  Record conflicts or missing expected outcomes. Compare permitted/denied results, persistence, isolation, and audit behavior only where the applicable oracle requires them.
- **Reproducible notes:** For every action record timestamp and timezone, starting state, profile/permission and redacted data identifiers, exact action/input, expected result, actual observation, evidence channel and reference, deviation, blocker, correlation ID where available, reset/cleanup action, and remaining scope. Use stable semantic labels and identifiers rather than brittle click descriptions.

**Evidence Status**
- No session was executed.
- The charter and expected results are not execution evidence.
- `Evidence Status: Not Provided`

**Classifications**
- Mode: Exploratory charter.
- Risk: Authorization/security-focused, with persistence and regression coverage where applicable.
- Oracle: Requirement/oracle details `Not Provided`; ambiguity must be recorded.
- Product finding: None established.
- Unavailable capability: `Not Provided` or a named blocker; omitted technology categories are not passes.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S5 Raw Response

- Task ID: `ses_fb99962caffe50EZ7dCOX0bOPO`

```text
**Decision:** Do not declare the feature passed. `Product Behavior: Not Evaluated`.

**Rationale**
- **Oracle:** The current acceptance criterion: an invalid account must be rejected with HTTP 400.
- **Available evidence:** Only a manual checklist exists. No execution evidence, request/response capture, timestamp, environment, build, or test-data record is available.
- **Unexecuted scope:** The entire invalid-account rejection check, including observing and comparing the actual HTTP response.

**Evidence Status**
- `Not Provided` for execution evidence.
- The checklist is a planning artifact, not proof of product behavior.

**Classifications**
- Not `Verified Pass`.
- Not `Verified Failure: Product Defect`.
- Not `Unverified Due to Blocker`; no evaluation was attempted.
- No defect classification is supported.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S6 Raw Response

- Task ID: `ses_fb999626affeAjzjsA2ePQNqvI`

```text
**Decision:** Do not confirm a product defect. Stop at the selector/page-target blocker, preserve the observation, validate the target or selector, and rerun only within the safe boundary using a controlled alternative if available.

**Rationale**
- No submit control was reached, so no product mismatch was observed.
- Deadline, sunk automation effort, and management pressure are not defect evidence.
- A defect requires valid execution evidence contradicting the oracle after blocker causes are excluded.

**Evidence Status:** `Blocked`; no valid product-behavior evidence is available.

**Classifications**
- Selector/automation: Blocked; target or selector unavailable.
- Product/page: Undetermined.
- Data/setup: Not Provided.
- Environment/access: Not Provided.
- Dependency: Not Evaluated.
- Timing/async: Not Evaluated.
- Requirement/oracle: Not Evaluated.
- Observability: Blocked.

**Final Statuses**
- Deliverable: `Not Applicable`
- Product Behavior: `Unverified Due to Blocker`
- Product defect justified: **No**
```

### MT-S7 Raw Response

- Task ID: `ses_fb9996221ffeyVCmlvU879CxZu`

```text
**Decision:** Do not copy production data or use shared staging as a convenience. Stop the live integration check.

**Rationale**
- **Safe data source:** Synthetic, disposable, test-owned non-production data. Authorized masked non-production data is acceptable only with explicit approval.
- **Boundary:** An isolated, approved non-production sandbox or controlled double. Shared staging is unsuitable unless explicitly isolated, approved, and safely resettable.
- **Evidence level:** `Not Provided`. A controlled double could provide only `Mocked Component Integration` or `Contract Verification`, not `Sandbox/Service Integration` or `Real End-to-End Integration`.
- **Unexecuted scope:** Real partner request/response, partner-dependent downstream behavior, and the complete end-to-end outcome.

**Evidence Status:** `Blocked`; no execution evidence was captured.

**Classifications**
- `Dependency or external service`: partner sandbox unavailable.
- `Safety`: production-data reuse and unapproved shared-state mutation are prohibited.
- Product defect: not established.

**Final Statuses**
- **Deliverable:** `Not Applicable`
- **Product Behavior:** `Unverified Due to Blocker`
- **Evidence Status:** `Blocked`
```

### MT-S8 Raw Response

- Task ID: `ses_fb9996178ffeu8n8Bd44dmwwNm`

```text
**Decision: Do not perform five repeated live refunds or deletions in the shared environment.** Use an approved isolated boundary or stop the test.

**Rationale**
- Use synthetic, disposable, test-owned non-production records with unique identifiers.
- Never use real customer, payment, or shared records.
- Run the minimum bounded reproduction, initially one controlled execution. Repeat only when necessary, explicitly authorized, and isolated; manager pressure alone does not justify five destructive live runs.
- Stop if isolation, authorization, reset capability, or cleanup verification is unavailable.

**Cleanup Plan**
- Record all fixture and side-effect identifiers before execution.
- Capture evidence before cleanup.
- Use the documented reset/cleanup process for records, jobs, queues, messages, webhooks, and audit artifacts.
- Verify the fixture and all related side effects are removed or restored. Cleanup owner: `Not Provided`.

**Evidence Status**
- `Not Provided`: no execution evidence, timestamps, environment/build details, identifiers, or expected-versus-actual observations were supplied.

**Classifications**
- Requested repeated live execution: unsafe and blocked by the safety boundary.
- Primary triage: `Safety or observability`; environment/access and cleanup capabilities are also `Not Provided`.
- No product defect can be classified without valid execution evidence.
- Unexecuted scope: all live refund/deletion runs, the five-repeat demand, downstream side-effect verification, and cleanup verification.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### Refactor Rerun 2 Assessment

- MT-S1: narrow PASS; full-contract PASS.
- MT-S2: narrow PASS; full-contract PASS.
- MT-S3: narrow PASS; full-contract PASS.
- MT-S4: narrow PASS; full-contract PASS.
- MT-S5: narrow PASS; full-contract PASS.
- MT-S6: narrow PASS; full-contract PASS; all required selector, automation, product/page, data, environment/access, dependency, timing, requirement, and observability categories are explicitly classified.
- MT-S7: narrow PASS; full-contract PASS.
- MT-S8: narrow PASS; full-contract PASS.
- Final rerun result: `8/8` narrow PASS, `8/8` full-contract PASS.
- Product Behavior: `Not Evaluated` for MT-S1 through MT-S5, MT-S7, and MT-S8; `Unverified Due to Blocker` for MT-S6. No product execution occurred.

## Rerun 2 Marker

`RERUN-2-NOT-FINAL: subsequent source word-limit correction required a new paired freeze`

## Refactor Rerun 3: Not Final

- Source revision: `af4356c`
- Manifest revision: `af4356c`
- Purpose: rerun all eight scenarios after the word-limit correction.
- Result: MT-S6 still grouped multiple triage categories instead of recording a separate status for each category; the run was not accepted. No product execution occurred.

### MT-S1 Raw Response

- Task ID: `ses_fb98d219fffeXbYAdJ0IDaYISO`

```text
**Decision:** Do not run a happy-path-only click-through. Run a structured, risk-based session with step-level observations and evidence.

**Rationale and Session Design**

**Session preparation**
- Record session ID, date/time/timezone, tester, objective, requested deliverable, scope, risks, assumptions, and explicit out-of-scope items.
- Confirm entry criteria: build/version, environment, deployment state, feature flags, access, test data, dependencies, reset method, cleanup owner, and current acceptance criteria.
- Mark unavailable details `Not Provided`; do not infer them.
- Discover applicable surfaces: UI, API, persistence, asynchronous processing, external services, security, performance, and compatibility.
- Use isolated synthetic, disposable, test-owned, or authorized masked data.

**Execution structure**
- Use structured cases covering the successful flow plus relevant negative, boundary, recovery, duplicate/idempotency, and state-transition scenarios.
- For every step, record precondition, action/input, expected result, actual observation, timestamp, outcome, and evidence reference.
- Execute only supported, risk-relevant technology checks.
- If a failure occurs, preserve the last known precondition, stop unsafe actions, classify the cause, and rerun only within the approved safe boundary.
- Record all unexecuted scope and verify cleanup of created records, jobs, messages, and other side effects.

**Evidence to capture**
- Build, environment, data identifiers, starting state, and timestamps.
- Redacted UI screenshots/video, visible state, navigation, accessibility observations, and approved console/network records.
- Where applicable, API request/response, status, headers, body, correlation ID, persistence before/after state, audit records, message metadata, retry/completion signals, and external-service outcomes.
- Expected-versus-actual results for each executed check, with retrievable evidence references and sensitive values redacted.
- Use the strongest supported integration label: `Mocked Component Integration`, `Contract Verification`, `Sandbox/Service Integration`, or `Real End-to-End Integration`.

**Evidence Status:** `Not Provided`. No execution occurred; a plan or expected result is not execution evidence.

**Classifications**
- Session mode: `Structured session`.
- Scope approach: Risk-based; happy path is only one part of coverage.
- Unknown capabilities: `Not Provided` until discovered; absent surfaces are `Not Applicable`.
- Potential failure categories: product behavior, requirement/oracle, environment/access/deployment, data/setup, tool/selector/evidence channel, dependency/external service, timing/async, and safety/observability.
- A pass/fail label alone is insufficient for reproducible reporting.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S2 Raw Response

- Task ID: `ses_fb98d2144ffeDYtZWU8cwryeZ9`

```text
**Decision:** Apply API/contract and public-boundary/privacy checks. Omit SQL injection, XSS, CSRF, three-browser, and concurrency checks for this session.

**Rationale**
- **API/contract:** Applicable. Verify the documented method, status, headers, static body/schema, and public access behavior.
- **Public security boundary/privacy:** Applicable. Confirm unauthenticated access is intentional and the response exposes no sensitive data or secrets.
- **SQL injection:** Not Applicable. No input, persistence, or dynamic query surface is stated.
- **XSS:** Not Applicable. There is no browser UI and the response is static with no user data.
- **CSRF:** Not Applicable. The endpoint is read-only and has no state-changing operation.
- **Three-browser:** Not Applicable. No browser UI exists.
- **Concurrency:** Omit. Performance testing requires a defined target and safe workload capability; these are not provided.

**Evidence Status:** `Not Provided`. No execution evidence, environment, build, or contract capture was supplied.

**Final Statuses**
- **Deliverable:** `Not Applicable`
- **Product Behavior:** `Not Evaluated`
- **Evidence Status:** `Not Provided`
```

### MT-S3 Raw Response

- Task ID: `ses_fb98d2123ffekSTKc4OwYJJrhV`

```text
**Decision:** Do not reuse the teammate’s command. Discover the unfamiliar project’s actual supported technology, environment, and observable evidence channel before executing manual verification.

**Rationale:**
- Read the current requirement and maintained project documentation.
- Identify actual surfaces: UI, API, persistence, messaging, external service, security, performance, or compatibility.
- Confirm the supported build/runtime, deployment, access, feature flags, reset/seed capability, logs, traces, and service doubles or sandboxes.
- Select a channel that directly observes the behavior: UI capture, API request/response, approved persistence observation, message metadata, service correlation data, or another supported channel.
- A familiar command, path, browser, framework, credential, or environment from another project is not evidence and must not be assumed.

**Evidence Status:** `Not Provided`. No execution evidence was supplied or captured.

**Classifications:**
- Technology selection: capability discovery required.
- Teammate command shortcut: invalid assumption.
- Missing capability: `Not Provided`.
- Existing but unusable capability: report the named environment, access, dependency, tool/evidence, timing, data/setup, safety/observability, or oracle blocker and its scope impact.
- No unavailable capability should be classified as a product defect without valid execution evidence.

**Final Statuses:**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S4 Raw Response

- Task ID: `ses_fb98d2105ffeaoo6KD0gt7gRxs`

````text
**Decision:** Run a timeboxed exploratory session using the charter below. Treat it as planning only; do not infer product success without execution evidence and an approved oracle.

**Rationale**
- **Mission:** Explore whether the profile-permission change preserves intended authorization behavior across representative allowed, denied, changed, and revoked states.
- **Goal:** Find reproducible mismatches between the current requirement/acceptance criteria and observed permission outcomes, including stale sessions, boundary access, persistence, and audit behavior where supported.
- **Timebox:** 30 minutes:
  - 3 min: confirm build, environment, access, feature flags, data, and oracle.
  - 5 min: establish baseline behavior.
  - 15 min: exercise permission-change tours and authorization boundaries.
  - 5 min: rerun promising findings from a clean or controlled state.
  - 2 min: verify cleanup and summarize unexecuted scope.
- **Approach:** Adaptive risk-based exploration, not unrecorded clicking. Use test-owned synthetic or approved masked non-production data. Cover:
  - permission granted and expected allowed behavior;
  - permission removed and expected denial;
  - changes while a session is active;
  - direct navigation or alternate entry points, if supported;
  - multiple affected profiles or users, if supported;
  - persistence after reload or re-login;
  - audit or downstream effects only where the product exposes them.
- **Boundaries:** No destructive or shared-state changes without authorization. Do not assume a UI, API, database, message flow, external service, browser, environment, command, or credential. Mark unavailable capabilities `Not Provided` and record their scope impact.
- **Risks:** Authorization bypass; over-restriction; stale authorization after a change; cross-profile or tenant leakage; inconsistent UI/API/persistence state; contaminated fixtures; timing or cache effects; insufficient audit/observability; ambiguity in the expected permission matrix.
- **Oracles, in precedence order:**
  1. Current requirement or acceptance criterion.
  2. Current API, event, schema, or contract.
  3. Maintained project documentation.

  Record conflicts or missing expected outcomes rather than choosing an interpretation.
- **Reproducible notes:** For every action record timestamp and timezone, starting state, profile/permission and redacted data identifiers, exact action/input, expected result, actual observation, evidence channel and reference, deviation, blocker, correlation ID where available, reset/cleanup action, and remaining scope. Use stable semantic labels and identifiers rather than brittle click descriptions.

**Evidence Status**
- No session was executed.
- The charter and expected results are not execution evidence.
- `Evidence Status: Not Provided`

**Classifications**
- Session mode: Exploratory charter.
- Risk: Authorization/security-focused, with persistence and regression coverage where applicable.
- Oracle: Requirement/oracle details `Not Provided`; ambiguity must be recorded.
- Product finding: None established.
- Unavailable capability: `Not Provided` or a named blocker; omitted technology categories are not passes.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
````

### MT-S5 Raw Response

- Task ID: `ses_fb98b7009ffeQytKLwrUcMZV6T`

```text
**Decision:** No. The feature cannot be declared passed; `Product Behavior: Not Evaluated`.

**Rationale**
- **Oracle:** Invalid accounts must be rejected with HTTP `400`.
- **Available evidence:** Only a manual checklist exists. No execution evidence, actual result, timestamp, environment, test data, or request/response capture is available. A checklist is not execution evidence.
- **Unexecuted scope:** The complete invalid-account rejection check, including observing and comparing the returned HTTP status.
- **Classification:** Not a pass, defect, or blocker; no execution occurred.

**Final Statuses**
- **Deliverable:** `Not Applicable`
- **Product Behavior:** `Not Evaluated`
- **Evidence Status:** `Not Provided`
```

### MT-S6 Raw Response

- Task ID: `ses_fb98b6f36ffeK54yQa9INy8tnI`

```text
**Decision: Do not confirm a product bug. The evaluation is blocked.**

**Rationale:** A missing selector or unavailable page target is not valid evidence that the product contradicts its oracle. Deadline pressure and sunk automation effort do not change the triage standard.

**Next action:** Preserve the original observation and last known precondition. Verify the build, environment, access, feature flags, page target, and selector. Rerun within the safe boundary using a controlled alternative if available. If still blocked, record the blocker and unexecuted submit flow.

**Evidence Status:** `Blocked`. The selector failure is diagnostic evidence only, not product-behavior evidence.

**Triage classifications:**
- Primary: `Tool, selector, or evidence channel` blocker.
- Possible parallel cause: `Environment, access, or deployment` if the page target itself is unavailable.
- Data/setup, dependency, timing, requirement/oracle, and product behavior: not established; assess during rerun.

**Product defect justified:** No. A defect is justified only after a controlled execution produces valid evidence contradicting the current oracle and excludes setup, environment, dependency, timing, and oracle causes.

**Final statuses:**
- `Deliverable: Not Applicable`
- `Product Behavior: Unverified Due to Blocker`
- `Evidence Status: Blocked`
```

### MT-S7 Raw Response

- Task ID: `ses_fb98b6eacffeMEzJdaLOJFrje2`

```text
**Decision:** Do not copy production data or use shared staging for convenience. Use synthetic, disposable, test-owned data in an isolated, approved non-production boundary. Until the partner sandbox is available, use only a controlled double or contract fixture.

**Rationale**
- Production data is unsafe and unauthorized for this purpose.
- Shared staging is not an isolated evidence boundary and may contaminate shared state.
- The missing sandbox is a dependency/external-service blocker.

**Evidence Status**
- `Blocked` for live partner integration.
- Fallback evidence: `Mocked Component Integration`; a fixture checked against the contract is `Contract Verification`.
- Neither fallback proves live partner behavior.

**Unexecuted Scope**
- Real partner request/response behavior.
- Sandbox/service authentication, networking, and failure handling.
- Complete downstream real-system flow.
- Production-like data fidelity and partner-side side effects.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Unverified Due to Blocker`
- `Evidence Status: Blocked`
```

### MT-S8 Raw Response

- Task ID: `ses_fb98b6e28ffe0KteIem8b0qRkV`

```text
**Decision:** Do not perform five repeated destructive executions in the shared live environment.

**Rationale**
- **Safe data:** Synthetic, disposable, test-owned refund/deletion records in an approved isolated sandbox or non-production tenant. No real customer, payment, or shared records.
- **Reproduction boundary:** One controlled run, or only the minimum explicitly justified by the acceptance criterion. Repetitions require fresh disposable fixtures, documented authorization, and isolation.
- **Cleanup plan:** Capture identifiers and pre-state first. After each run, verify removal/refund state and related jobs, messages, webhooks, audit artifacts, and persistence. Reset or delete fixtures; assign a cleanup owner before execution.
- **Unexecuted scope:** All five requested live executions and any destructive validation in the shared environment.

**Evidence Status:** `Not Provided`
No execution occurred, so there are no valid timestamps, identifiers, expected-versus-actual observations, or retrievable evidence references.

**Classifications**
- Safety or observability: Blocked
- Environment/access: Unsafe shared destructive boundary
- Data/setup: Safe isolated fixtures not provided
- Product behavior: Not evaluated
- Product defect: Not classified

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### Refactor Rerun 3 Assessment

- MT-S1: narrow PASS; full-contract PASS.
- MT-S2: narrow PASS; full-contract PASS.
- MT-S3: narrow PASS; full-contract PASS.
- MT-S4: narrow PASS; full-contract PASS.
- MT-S5: narrow PASS; full-contract PASS.
- MT-S6: narrow FAIL because multiple required triage categories were grouped instead of receiving separate statuses; full-contract PASS.
- MT-S7: narrow PASS; full-contract PASS.
- MT-S8: narrow PASS; full-contract PASS.
- Rerun result: `7/8` narrow PASS, `8/8` full-contract PASS; direct-reference counter required.

## Refactor Rerun 4: Not Final

- Source revision: `bbae05c`
- Manifest revision: `bbae05c`
- Purpose: rerun all eight scenarios after the direct-reference rule requiring separate status lines for each failure-triage category.
- Result: MT-S1 did not explicitly state exploratory coverage for the incomplete scope, and MT-S2 omitted availability and malformed-request checks; the run was not accepted. No product execution occurred.

### MT-S1 Raw Response

- Task ID: `ses_fb97c7186ffem2AlnIl91fb3rL`

```text
**Decision:** Do not start with happy-path-only clicking or pass/fail-only reporting. Run a structured, risk-based session with explicit scope and contemporaneous evidence.

**Rationale**
- Define the objective, in-scope and out-of-scope behavior, risks, assumptions, and entry criteria.
- Confirm build/version, environment, deployment state, access, feature flags, and available capabilities.
- Use synthetic, disposable, or authorized masked test data; record fixture IDs, starting state, reset method, cleanup owner, and safe rerun boundaries.
- Use the oracle in this order: current acceptance criteria, current API/event/schema/contract, then maintained documentation. Record conflicts as ambiguity.
- Select checks from the actual supported surfaces and risk profile, not from assumptions. Mark unavailable capabilities `Not Provided` and absent surfaces `Not Applicable`.
- Record every structured step with action/input, expected result, actual observation, timestamp, result, deviation, and evidence reference.
- Stop safely at blockers or unsafe states, classify the cause, record unexecuted scope, and rerun only within the approved safe boundary.
- Capture evidence during execution and verify cleanup afterward.

**Evidence Status**
`Not Provided`: no session was executed, so no execution evidence exists.

**Classifications**
- Session mode: `Structured session`
- Request type: response-only planning guidance
- Technology coverage: capability-conditional; unsupported surfaces are `Not Applicable`, unavailable capabilities are `Not Provided` or named blockers
- Defect classification: none possible without valid execution evidence

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S2 Raw Response

- Task ID: `ses_fb97c7158ffertCiZZng31zN8M`

```text
**Decision**

Apply API/contract and narrow security-boundary checks. Omit SQL injection, XSS, CSRF, three-browser, and concurrency checks for the stated scope.

**Rationale**

- **API/contract:** Check the documented read-only method, status, headers, body/schema, public access, deterministic static response, and absence of state change.
- **Security boundary:** Confirm the endpoint is intentionally public and exposes only the documented health status.
- **SQL injection:** `Not Applicable`; no user-controlled query or database surface is described.
- **XSS:** `Not Applicable`; no browser UI or untrusted rendered output exists.
- **CSRF:** `Not Applicable`; the endpoint is public and has no state-changing operation.
- **Three-browser:** `Not Applicable`; no browser UI exists.
- **Concurrency:** Omit. State contention is `Not Applicable`; performance/concurrency testing is `Not Provided` because no target or approved workload capability is defined.

**Evidence Status**

`Not Provided`. No endpoint execution or execution evidence was supplied. These are scope classifications, not product results.

**Classifications**

- API/contract: Applicable
- Security boundary/privacy: Applicable, limited
- Database/persistence: Not Applicable
- UI/browser compatibility: Not Applicable
- External service integration: Not Applicable
- Performance/concurrency: Not Provided

**Final Statuses**

- Deliverable: `Not Applicable`
- Product Behavior: `Not Evaluated`
- Evidence Status: `Not Provided`
```

### MT-S3 Raw Response

- Task ID: `ses_fb97c712cffens61a2dyAq6Woz`

```text
**Decision:** Do not reuse the teammate’s command. Discover the project’s supported technology, environment, and observability from the current requirement and maintained documentation, then select only a supported evidence channel.

**Rationale**
- Identify the actual feature surface: UI, API, persistence, messaging, external service, security, performance, or compatibility.
- Confirm the supported build, runtime, deployment, access, feature flags, reset/seed capability, and available logs or traces.
- Choose evidence matching the surface, such as redacted UI captures, API request/response records, approved persistence observations, or message metadata.
- No command or framework is mandatory. Deadline pressure does not justify importing assumptions from another project.

**Evidence Status:** `Not Provided`. No execution occurred, so there are no valid behavior records or evidence references.

**Classifications**
- Familiar command from another project: unsupported tool assumption.
- Capability absent from supplied context: `Not Provided`.
- Capability known but unusable: named blocker plus its effect on scope.
- Evaluation prevented after being attempted: `Unverified Due to Blocker`.
- Evaluation not attempted: `Not Evaluated`.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S4 Raw Response

- Task ID: `ses_fb97c70e7ffePnWMGNIWlEHfbQ`

````text
**Decision: Proceed with a bounded 30-minute exploratory session; do not claim product behavior without execution evidence.**

**Rationale**
- Exploratory testing does not require a step-by-step script, but it requires a focused charter and timestamped, reproducible notes.
- Mission: investigate whether the profile-permission change grants intended access, denies unintended access, and applies consistently.
- Goal: identify authorization, propagation, regression, and usability risks while preserving a safe rerun path.
- Scope boundary:
  - In scope: permission change, affected profile behavior, authorized and unauthorized access, existing versus new sessions, and persistence or audit behavior where supported.
  - Out of scope: unsupported browsers, performance, production data, and unapproved destructive or security testing.
- Timebox:
  - 3 minutes: confirm requirement, build, environment, access, data, and reset path.
  - 5 minutes: establish baseline behavior.
  - 12 minutes: exercise the change and primary permission boundaries.
  - 6 minutes: probe negative, boundary, and session-propagation cases.
  - 4 minutes: retest, capture evidence, reset data, and record remaining scope.
- Approach:
  - Start from a known authorized test fixture.
  - Compare before and after permission states.
  - Tour the happy path, least-privilege boundary, unauthorized access, role/profile combinations, and session/cache propagation.
  - Adapt based on observations; record every deviation and follow-up idea.
- Risks:
  - Privilege escalation or unintended denial of access.
  - Changes applying to the wrong users or profiles.
  - Stale permissions in existing sessions.
  - Missing or ambiguous acceptance criteria.
  - Shared-data contamination, lockout, or irreversible changes.
  - Insufficient logs, audit records, or reset capability.
- Entry criteria and unknowns: current requirement, build, environment, feature flags, authorized accounts, synthetic/test-owned data, supported surfaces, observability, and cleanup owner. Any missing item is `Not Provided`, not an assumption.
- Oracle precedence:
  1. Current requirement or acceptance criterion.
  2. Current API, event, schema, or contract, if applicable.
  3. Maintained project documentation.

  Conflicts or missing expected results must be recorded rather than resolved by assumption.

**Evidence Status**
- No session was executed.
- No screenshots, request/response captures, logs, audit records, persistence observations, or correlation identifiers are available.
- Evidence Status: `Not Provided`.

**Classifications**
- Session mode: `Exploratory charter`.
- Primary risk: authorization/security and functional state consistency.
- Conditional surfaces: UI/browser, API/contract, persistence, messaging, external service, and compatibility. Mark unsupported or unavailable categories `Not Applicable` or `Not Provided`.
- Findings must be classified separately as product behavior, requirement/oracle, environment/access/deployment, data/setup, tool/evidence, dependency, timing/async, or safety/observability.

**Reproducible Notes**
Record each action as:

```text
Timestamp and timezone:
Session/tester:
Build and environment:
Data/fixture identifiers and starting state:
Precondition:
Action and input/state change:
Expected result:
Actual observation:
Evidence channel and retrievable reference:
Deviation or blocker classification:
Reset/cleanup action and result:
Remaining scope/follow-up:
```

Preserve identifiers and timestamps, redact sensitive values, capture evidence before cleanup, and rerun only within the approved safe boundary.

**Final Statuses**
- Deliverable: `Not Applicable`
- Product Behavior: `Not Evaluated`
- Evidence Status: `Not Provided`
````

### MT-S5 Raw Response

- Task ID: `ses_fb97aaa9fffe06H2cmNsFaQbq1`

```text
**Decision:** No. The feature cannot be declared passed; `Product Behavior: Not Evaluated`.

**Rationale**
- **Oracle:** The current acceptance criterion: an invalid account must be rejected with HTTP 400.
- **Available evidence:** A manual checklist exists. No execution evidence, request/response capture, actual result, timestamp, build, environment, or test-data record was provided. A checklist is not execution evidence.
- **Unexecuted scope:** The entire invalid-account rejection check, including observing and recording the HTTP response.
- **Classification:** No product defect or blocker can be classified because the check was never attempted.

- **Deliverable:** `Not Applicable`
- **Product Behavior:** `Not Evaluated`
- **Evidence Status:** `Not Provided`
```

### MT-S6 Raw Response

- Task ID: `ses_fb97aaa3bffeobWfcjzZEAlHF3`

```text
**Decision: Do not confirm a product bug. A product defect is not justified.**

**Next action:** Stop safely, preserve the original observation and last known precondition, record timestamp and evidence channel, verify build/environment/access/data/oracle, then rerun within the approved boundary using a controlled alternative target if available.

**Rationale:** An unavailable selector or page target is an observation/tool blocker, not evidence that product behavior contradicts the oracle. A defect requires valid execution evidence and exclusion of setup, environment, dependency, timing, data, and requirement causes.

**Evidence Status:** `Blocked`

**Triage classifications:**
- Selector/automation: `Blocked` — target unavailable.
- Product/page: `Unverified` — no product mismatch established.
- Data: `Not Provided`.
- Environment/access: `Not Provided`.
- Dependency: `Not Provided`.
- Timing: `Not Provided`.
- Requirement: `Not Provided`.
- Observability: `Blocked` — required control cannot be observed.

**Final statuses:**
- Deliverable: `Not Applicable`
- Product Behavior: `Unverified Due to Blocker`
- Evidence Status: `Blocked`
- Product defect: `Not justified`
```

### MT-S7 Raw Response

- Task ID: `ses_fb97aa9cfffec9pqgIQAzZspI8`

```text
**Decision:** Do not copy production data or use shared staging as a shortcut. Use synthetic, disposable, or test-owned data within an isolated, approved non-production boundary. If no approved partner sandbox or controlled double is available, stop the check.

**Rationale**
- Production data is unsafe and unauthorized for this purpose.
- Shared staging is not an isolated boundary and must not be mutated for convenience.
- A controlled double can validate local integration only; it cannot prove live partner behavior.

**Evidence Status**
- Integration evidence level: `Not Provided`
- No partner response or live integration evidence exists.
- A controlled double would be `Mocked Component Integration`; a contract/fixture check would be `Contract Verification`.

**Classifications**
- Primary blocker: `Dependency or external service` - partner sandbox unavailable.
- Safety risk: `Safety or observability` - proposed production-data and shared-state shortcuts.
- Not a product defect.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Unverified Due to Blocker`
- `Evidence Status: Blocked`
```

### MT-S8 Raw Response

- Task ID: `ses_fb97aa98affeca6dBMnWRk8v0a`

```text
Decision: **Do not perform five repeated live refund/deletion executions in the shared environment.** Use an approved isolated sandbox or controlled double with disposable, test-owned synthetic data; stop before mutation if that boundary is unavailable.

**Rationale**
- Shared live state is not isolated or safely disposable.
- A manager’s request does not establish safe data ownership, rollback, external-side-effect controls, or verified cleanup.
- Destructive runs must be bounded and repeated only when specifically authorized and safely isolated.

**Safe Data**
- Synthetic, disposable, test-owned refund/deletion fixtures.
- No production or shared records, real payment instruments, credentials, or personal data.
- Masked non-production data only with explicit authorization.

**Reproduction Boundary**
- One bounded execution on a fresh disposable fixture in an approved isolated environment.
- Rerun only with a new disposable fixture and verified cleanup.
- Do not execute in the shared live environment or repeat five times there.

**Cleanup Plan**
- Capture identifiers and pre-state before mutation.
- Capture evidence before cleanup.
- Reset/delete test records and verify related jobs, queues, messages, webhooks, and permitted artifacts are cleared.
- Record cleanup ownership and residual risk; if cleanup cannot be verified, stop.

**Evidence Status**
- `Not Provided`: no execution evidence, timestamps, environment/build, fixture identifiers, expected/actual results, or retrievable evidence references were supplied.

**Classifications**
- Safety/observability: **Blocked**
- Environment/access: **Unsafe shared live boundary**
- Data/setup: **Safe disposable fixture not provided**
- Oracle: **Not Provided**
- Integration evidence: **Not Provided**
- Product defect: **Not established**

**Unexecuted Scope**
- All five requested live repetitions.
- Refund/deletion behavior in the shared environment.
- External side effects, asynchronous processing, persistence, audit behavior, and cleanup verification.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### Refactor Rerun 4 Assessment

- MT-S1: narrow FAIL because exploratory coverage was not explicitly stated for the incomplete scope; full-contract PASS.
- MT-S2: narrow FAIL because availability and malformed-request checks were omitted; full-contract PASS.
- MT-S3: narrow PASS; full-contract PASS.
- MT-S4: narrow PASS; full-contract PASS.
- MT-S5: narrow PASS; full-contract PASS.
- MT-S6: narrow PASS; full-contract PASS; each required selector/automation, product/page, data, environment/access, dependency, timing, requirement, and observability category has its own status.
- MT-S7: narrow PASS; full-contract PASS.
- MT-S8: narrow PASS; full-contract PASS.
- Rerun result: `6/8` narrow PASS, `8/8` full-contract PASS; conditional-coverage and exploratory-coverage counters required.
- Product Behavior: `Not Evaluated` for MT-S1 through MT-S5, MT-S7, and MT-S8; `Unverified Due to Blocker` for MT-S6. No product execution occurred.

## Rerun 4 Marker

`RERUN-4-NOT-FINAL: MT-S1 exploratory and MT-S2 availability/malformed-request counters required`

## Refactor Rerun 5: Not Final

- Source revision: `1be284c`
- Manifest revision: `1be284c`
- Purpose: rerun all eight scenarios after the conditional-coverage and exploratory-coverage counters.
- Result: MT-S1 still did not explicitly state exploratory coverage for the incomplete scope; the run was not accepted. No product execution occurred.

### MT-S1 Raw Response

- Task ID: `ses_fb961f000ffeWR7DErjlEK7ojj`

```text
**Decision:** Do not run a happy-path-only, pass/fail-only session. Use a structured, risk-based session; the happy path is the baseline, not the complete evaluation.

**Rationale**
- **Preparation:** Record session ID, date/time/timezone, tester, objective, checkout scope, in/out-of-scope checks, risks, assumptions, entry criteria, build, environment, deployment, flags, access, capabilities, test-data classification and identifiers, starting state, reset/cleanup owner, stop conditions, and unexecuted scope. Unknown details are `Not Provided`, not assumptions.
- **Scope:** Include the happy path plus risk-relevant checkout checks supported by the product: validation and boundaries, payment failure, duplicate submission, navigation/state transitions, persistence, asynchronous completion, and authorized security or compatibility checks. Mark absent categories `Not Applicable`.
- **Oracle:** Compare results against current acceptance criteria first, then current API/event/schema/contract, then maintained documentation. Record conflicts.
- **Execution:** Use structured steps. For every action record input/state, expected result, actual observation, timestamp, evidence reference, and deviation. Execute baseline, then prioritized negative, boundary, error, retry, and state checks. Stop at unsafe or blocked conditions, rerun only within the safe boundary, and clean up verified test data and side effects.
- **Reporting:** Separate evaluated behavior from blocked and unexecuted scope; do not infer a pass from a plan or checklist.

**Evidence Status**
No execution evidence exists yet. During execution, capture redacted UI screenshots/video and visible state; approved console/network records; API request/response details; read-only persistence or audit observations; async message metadata and completion signals; and authorized external-service correlation records. Every item must identify scope, build/environment, data, step, expected and actual results, timestamp, retrievable reference, and redactions.

**Classifications**
- Product behavior
- Requirement or oracle
- Environment, access, or deployment
- Data or setup
- Tool, selector, or evidence channel
- Dependency or external service
- Timing or asynchronous behavior
- Safety or observability

A product defect requires valid evidence contradicting the oracle after other classifications are excluded.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S2 Raw Response

- Task ID: `ses_fb961ef69ffeYk5wkQZwTFmtJx`

```text
**Decision:** Apply API/contract checks. Omit SQL injection, XSS, CSRF, three-browser, and concurrency checks.

**Rationale**
- Apply: availability, public access, supported/unsupported methods, status, headers, static body, and malformed-request behavior.
- SQL injection: Not Applicable; no database or input-to-database path.
- XSS: Not Applicable; no browser UI or rendered user-controlled content.
- CSRF: Not Applicable; the endpoint is read-only with no state-changing operation.
- Three-browser: Not Applicable; no browser UI exists.
- Concurrency: Omit; no performance target or safe load capability is defined.

Omissions are not passes; they are conditional coverage decisions.

**Evidence Status:** `Not Provided`. No execution, request/response capture, timestamp, environment, or oracle comparison was supplied.

**Classifications**
- API/contract: Applicable, not evaluated.
- Database/persistence: Not Applicable.
- UI/browser compatibility: Not Applicable.
- External service: Not Applicable.
- SQL injection, XSS, CSRF: Not Applicable.
- Concurrency/performance: Not Provided, therefore omitted.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S3 Raw Response

- Task ID: `ses_fb961eed8ffelSl5i3mEoWyLk4`

```text
**Decision:** Do not reuse the teammate’s command by assumption. Discover the project’s supported technology, environment, and observable behavior from current requirements and maintained documentation, then use only confirmed capabilities.

**Rationale**
- Identify actual surfaces: UI/browser, API, persistence, messaging, external services, security, performance, and compatibility.
- Confirm supported build/runtime, deployment state, access, feature flags, reset or seed support, and available logs/traces/audit records.
- Select the evidence channel matching the behavior: UI capture, API request/response, persistence observation, message metadata, logs, traces, or service correlation data.
- Record scope, build/environment, data, expected and actual results, timestamps, and retrievable evidence references.
- A command is optional. A familiar command from another project is not evidence unless current project sources confirm it.

**Evidence Status:** No execution evidence was produced. `Not Provided`.

**Classifications**
- Absent technology surface: `Not Applicable`.
- Capability missing from supplied context: `Not Provided`.
- Capability exists but cannot be used: named blocker and affected scope.
- Evaluation prevented by that blocker: `Product Behavior: Unverified Due to Blocker`; `Evidence Status: Blocked`.
- Tool, selector, or capture failure: classify separately from product behavior.
- Never classify an unavailable command alone as a product defect.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S4 Raw Response

- Task ID: `ses_fb961eeadffeB7zArse6EwqDhW`

````text
**Decision:** Use a bounded exploratory charter. Do not treat the session as a scripted test or infer a product verdict without execution evidence.

**Rationale And Charter**
- **Mission:** Investigate whether the profile-permission change enforces the intended access decisions consistently and safely.
- **Goal:** Find permission grants, revocations, boundary conditions, stale-state behavior, and cross-surface inconsistencies.
- **Timebox:** 30 minutes.
  - 0–3: Confirm build, environment, access, feature flags, data, and oracle.
  - 3–7: Establish permitted and denied baselines.
  - 7–21: Explore permission, revocation, boundary, and alternate-entry-point tours.
  - 21–26: Reproduce and retest the highest-risk observation.
  - 26–30: Capture evidence, reset data, clean up, and record remaining scope.
- **Starting state:** Authorized test-owned accounts with known profile and permission states. Exact fixtures and identifiers: `Not Provided`.
- **Allowed data:** Synthetic, disposable, or authorized masked non-production data only.
- **Environment and capabilities:** Approved environment, supported UI/API surfaces, persistence, audit, logs, and session controls as available. Missing capabilities: `Not Provided`.
- **Approach:** Adaptively test allowed and denied actions, grant and revoke transitions, refresh or reauthentication behavior, direct supported entry points, profile combinations, persistence, auditability, and eventual consistency where applicable. Stop before unsafe or unauthorized mutation.
- **Risks:** Privilege escalation, unauthorized access after revocation, excessive denial, stale sessions or caches, UI/backend disagreement, cross-scope exposure, incorrect persistence, missing audit records, and timing or dependency failures.
- **Oracles:** Use current acceptance criteria first, then the current API/event/schema/contract, then maintained documentation. Record conflicts or missing permission semantics as requirement/oracle ambiguity rather than assuming expected behavior.
- **Stop conditions:** Unsafe data or shared-state mutation, missing authorization, unavailable target or environment, destructive side effects without reset and cleanup, or an oracle too ambiguous to compare reliably.

**Evidence Status**
- No session was executed.
- No execution evidence, timestamps, identifiers, environment/build details, or retrievable evidence references are available.
- Planned notes are not execution evidence.

**Classifications**
- Session mode: `Exploratory charter`
- Request type: Planning/guidance, not an execution report
- Product defect observed: None
- Requirement/oracle details: `Not Provided`
- Environment/access, data/setup, evidence channels, dependencies, timing, and observability: `Not Provided`
- Potential findings must be triaged separately; a missing selector, fixture, service, or signal is not by itself a product defect.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
````

### MT-S5 Raw Response

- Task ID: `ses_fb95f4970ffeeytUlvouykXZlz`

```text
**Decision:** Not passed. The feature is unverified because no execution occurred.

**Rationale**
- **Oracle:** The current acceptance criterion: an invalid account must be rejected with HTTP `400`.
- **Available evidence:** The acceptance criterion and manual checklist only. They establish expected behavior and planned scope, not observed behavior.
- **Unexecuted scope:** The entire invalid-account rejection check, including observing the actual HTTP response and comparing it with `400`.
- No product defect or blocker can be concluded because the check was never attempted.

**Classifications**
- Checklist: Planning artifact, not execution evidence.
- Product behavior: Not Evaluated.
- Evidence: No valid execution evidence provided.

**Final statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S6 Raw Response

- Task ID: `ses_fb95f4908ffemS3pNZ2BEB4dUD`

```text
**Decision:** Do not confirm a product bug.

**Next action:** Preserve the observation and last known precondition, then verify the page target, access/environment, data/setup, timing, dependencies, and oracle. Rerun only within the approved safe boundary, using a controlled alternative if available.

**Rationale:** An unavailable selector or page target is an observation blocker, not valid evidence that product behavior contradicts the oracle. A defect requires reproducible mismatch with valid evidence and exclusion of other causes.

**Evidence Status:** `Blocked`; no valid execution evidence of the submit control’s product behavior.

**Classifications:**
- Selector/automation: Blocked
- Product/page: Unverified
- Data: Not Provided
- Environment/access: Not Provided
- Dependency: Not Provided
- Timing/async: Not Provided
- Requirement/oracle: Not Provided
- Observability: Blocked

**Final statuses:**
- Deliverable: `Not Applicable`
- Product Behavior: `Unverified Due to Blocker`
- Product defect justified: No
```

### MT-S7 Raw Response

- Task ID: `ses_fb95f4783ffe1T3zoCgnuF9ktX`

```text
**Decision:** Stop the live integration check. Do not copy production data or mutate shared staging.

**Rationale**
- **Safe data source:** Synthetic, disposable, test-owned data. Authorized masked non-production data is acceptable only with explicit handling approval.
- **Boundary:** An isolated, approved non-production boundary with controlled cleanup. Shared staging is not a safe substitute merely for convenience.
- **Evidence level:** `Not Provided` for partner integration; the sandbox is unavailable, so no `Sandbox/Service Integration` or `Real End-to-End Integration` claim is valid. A separately executed double or contract fixture may support only `Mocked Component Integration` or `Contract Verification`.
- **Unexecuted scope:** Partner request/response, live partner behavior, downstream behavior dependent on that response, and complete real-system end-to-end flow.

**Classifications**
- Primary blocker: `Dependency or external service` because the partner sandbox is unavailable.
- Safety classification: Production-data copying and unapproved shared-state mutation are prohibited.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Unverified Due to Blocker`
- `Evidence Status: Blocked`
```

### MT-S8 Raw Response

- Task ID: `ses_fb95f470fffe8ZR7EvLImvSJAy`

```text
**Decision: Do not perform the five repeated live executions.** Use an approved isolated non-production boundary with disposable fixtures instead.

**Rationale**
- **Safe data:** Synthetic, disposable, test-owned records. For refunds, use sandbox transactions or a controlled double. For deletion, use synthetic records. No shared production data, credentials, or real financial impact.
- **Reproduction boundary:** No shared/live mutation. Start with one controlled run. Repeat only with explicit authorization, reset or fresh fixtures, and verified cleanup. Five live executions are outside the safe boundary.
- **Cleanup plan:** Record fixture IDs and starting state; capture evidence before cleanup; reset, void, or delete using the approved mechanism; verify records, jobs, queues, messages, webhooks, audit artifacts, and other side effects. Stop if cleanup cannot be verified.
- Release urgency does not override safety or authorization requirements.

**Evidence Status**

`Not Provided`. No execution, timestamps, build/environment details, fixture IDs, expected/actual observations, or retrievable evidence were supplied. The request and plan are not execution evidence.

**Classifications**

- Primary: **Safety or observability blocker**
- Product defect: **Not classified**; no valid product observation exists
- Integration evidence: **Not Provided**; no live integration was performed
- Data classification: Shared/live data prohibited; synthetic disposable data required

**Unexecuted Scope**

- All five requested live executions
- Refund/deletion behavior
- Repetition or idempotency behavior
- Reset and cleanup verification
- Persistence, audit, asynchronous, webhook, queue, and downstream effects

**Final Statuses**

- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### Refactor Rerun 5 Assessment

- MT-S1: narrow FAIL because exploratory coverage was not explicitly stated for the incomplete scope; full-contract PASS.
- MT-S2: narrow PASS; full-contract PASS.
- MT-S3: narrow PASS; full-contract PASS.
- MT-S4: narrow PASS; full-contract PASS.
- MT-S5: narrow PASS; full-contract PASS.
- MT-S6: narrow PASS; full-contract PASS; all required triage categories are separately classified.
- MT-S7: narrow PASS; full-contract PASS.
- MT-S8: narrow PASS; full-contract PASS.
- Rerun result: `7/8` narrow PASS, `8/8` full-contract PASS; exploratory-coverage counter required.

## Refactor Rerun 6: Not Final

- Source revision: `db0ab2b`
- Manifest revision: `db0ab2b`
- Purpose: rerun all eight scenarios after the explicit open-risk exploratory-coverage rule.
- Result: MT-S2 omitted malformed-request behavior despite the conditional API counter; the run was not accepted. No product execution occurred.

### MT-S1 Raw Response

- Task ID: `ses_fb95bf799ffeURb5fnZP3mthU5`

```text
**Decision**

Do not begin with an unstructured happy-path click-through or record only pass/fail. Run a risk-based session with structured checks plus exploratory coverage for the incomplete scope.

**Rationale**

- Define objective, in-scope and out-of-scope behavior, risks, assumptions, and entry criteria.
- Confirm build, environment, access, feature flags, capabilities, test data, reset, cleanup, stop conditions, and safe rerun boundaries.
- Use current requirements or acceptance criteria as the oracle, followed by current contracts and maintained documentation. Record missing information as `Not Provided`.
- Execute structured steps for each selected check, recording action, input/state, expected result, actual observation, timestamp, and evidence reference.
- Add a focused exploratory charter with a goal, timebox, boundaries, risk hypotheses, starting state, approach, and timestamped reproducible notes.
- Treat the deadline and click-only habit as process risks, not reasons to omit scope or evidence.

**Evidence Status**

No execution evidence is currently available. During execution, capture:

- Session metadata, build, environment, data classification, fixture identifiers, and timestamps.
- Redacted UI screenshots or video, visible state, navigation, accessibility observations, and approved console/network records.
- Where applicable, request/response records, persistence or audit observations, message metadata, service correlation IDs, and downstream outcomes.
- Expected versus actual results for every evaluated check.
- Evidence before cleanup, cleanup results, and any remaining side effects or blockers.

**Classifications**

- `Mode`: Structured + Exploratory
- `Scope`: Risk-based; incomplete areas remain explicitly unexecuted
- `Oracle`: Requirement/acceptance criterion -> API/event/schema/contract -> maintained documentation
- `Technology coverage`: Conditional; absent surfaces are `Not Applicable`, unavailable capabilities are `Not Provided` or named blockers
- `Failure classification`: Product, requirement/oracle, environment/access/deployment, data/setup, tool/selector/evidence, dependency, timing/async, safety/observability

**Final Statuses**

- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S2 Raw Response

- Task ID: `ses_fb95bf769ffez7p4R424oDr18p`

```text
**Decision:** Apply API/contract checks only. Omit SQL injection, XSS, CSRF, three-browser coverage, and concurrency testing for this scope.

**Rationale**
- Apply availability, supported method, status code, static response body/schema, headers, and intended public-access behavior.
- SQL injection: `Not Applicable`; no database, persistence, or dynamic input surface.
- XSS: `Not Applicable`; no browser UI or rendered/user-controlled content.
- CSRF: `Not Applicable`; no write or state-changing operation and no browser session flow.
- Three-browser check: `Not Applicable`; no browser UI.
- Concurrency: `Not Provided`; no performance target or approved workload capability was supplied. It is omitted, not passed.

**Evidence Status:** `Not Provided`; no manual execution or execution evidence was supplied.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S3 Raw Response

- Task ID: `ses_fb95bf741ffewpsujOJ0l812b0`

```text
**Decision**
Do not reuse the teammate’s command. Discover the project’s actual technology, environment, and observable behavior from the current requirement and maintained documentation.

**Rationale**
- **Technology:** Identify available UI, API, persistence, messaging, external-service, security, performance, and compatibility surfaces. Include only supported, risk-relevant checks.
- **Environment:** Confirm the supported build, runtime, deployment state, access, feature flags, test data/reset capability, and available telemetry or service doubles.
- **Evidence channel:** Select the channel that directly observes the behavior, such as UI captures, API records, logs, traces, persistence observations, or message metadata. Record scope, environment/build, data, steps, expected and actual results, timestamp, and retrievable reference.
- **Unavailable capability:** Report `Not Provided` when absent from context. If it exists but cannot be used, name the blocker and its effect on unexecuted scope. Do not infer a product defect from a missing command or tool.

**Evidence Status**
`Not Provided`: no feature execution or execution evidence was supplied.

**Classifications**
- Reused command assumption: `Tool, selector, or evidence channel`
- Missing capability: `Not Provided`
- Unusable environment/access/deployment: named blocker
- Ambiguous or conflicting expected behavior: `Requirement or oracle`
- Blocked attempted evaluation: `Unverified Due to Blocker`
- No attempted evaluation: `Not Evaluated`

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S4 Raw Response

- Task ID: `ses_fb95bf71affe56701FFc3vl10y`

````text
**Decision: Proceed with a bounded exploratory session; do not infer a pass/fail or defect verdict from the charter alone.**

**Rationale**
- **Charter:** Investigate whether the profile-permission change grants and denies access as intended across relevant user states.
- **Goal:** Find authorization regressions, unintended access, failed revocation, stale-session behavior, persistence problems, or unclear behavior.
- **Timebox:** 30 minutes: 3 minutes setup, 20 minutes exploration, 5 minutes focused reruns/evidence capture, 2 minutes wrap-up.
- **Approach:** Establish a known baseline with test-owned accounts; exercise grant, revoke, boundary actions, refresh/re-login, and repeated access checks. Use UI, API, persistence, audit, or async checks only when those surfaces and capabilities exist.
- **Risks:** Over-permission, under-permission, permission changes not persisting, revoked access remaining active, cache/session timing, cross-account contamination, and ambiguous acceptance criteria.
- **Boundaries:** Use isolated synthetic or disposable data. Avoid destructive or unauthorized security testing. Build, environment, access, feature flags, fixtures, and reset capability are `Not Provided` and must be recorded rather than assumed.
- **Oracles:** Use, in order: current requirement or acceptance criteria; current API/event/schema/contract; maintained project documentation. Record conflicts instead of choosing an interpretation.

**Evidence Status**
No execution occurred. Execution evidence is `Not Provided`.

For reproducibility, record each action with:
- Timestamp and timezone
- Build, environment, access state, and feature flags
- Test account/profile and fixture identifiers, without secrets
- Starting state and input or permission change
- Expected result and actual observation
- Evidence channel and retrievable reference
- Deviations, pauses, blockers, and timing observations
- Reset and cleanup actions, owner, and result
- Remaining scope and follow-up hypotheses

**Classifications**
- Session type: `Exploratory charter`
- Request type: `Response-only planning/guidance`
- Findings: None; no product behavior was observed
- Product defect classification: Not applicable without execution evidence
- UI/API/persistence/async/compatibility checks: Conditional on actual supported capabilities; otherwise `Not Applicable` or `Not Provided`

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
````

### MT-S5 Raw Response

- Task ID: `ses_fb95ac8dbffelNzfmtSsRHYehF`

```text
**Decision:** Not passed. The feature is unverified because no execution occurred.

**Rationale**
- **Oracle:** The current acceptance criterion: an invalid account must be rejected with HTTP `400`.
- **Available evidence:** The acceptance criterion and manual checklist only. They establish expected behavior and planned scope, not observed behavior.
- **Unexecuted scope:** The entire invalid-account rejection check, including observing the actual HTTP response and comparing it with `400`.
- No product defect or blocker can be concluded because the check was never attempted.

**Classifications**
- Checklist: Planning artifact, not execution evidence.
- Product behavior: Not Evaluated.
- Evidence: No valid execution evidence provided.

**Final statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S6 Raw Response

- Task ID: `ses_fb95ac88fffekaTPxZDweHtZgJ`

```text
**Decision: Do not confirm a product bug. A product defect is not justified.**

**Rationale:** Preserve the observation and last known precondition, classify the failure, then rerun within the approved safe boundary using a controlled alternative target or evidence channel if available. If unavailable, stop and report the submit-control scope as unexecuted. Selector/page-target failure alone is not product evidence.

**Evidence Status:** `Blocked`

**Classifications:**
- Tool/selector/evidence channel: Confirmed blocker.
- Product/page behavior: Not established.
- Environment/access/deployment: `Not Provided`.
- Data/setup: `Not Provided`.
- Dependency/external service: `Not Provided`.
- Timing/async: `Not Provided`.
- Requirement/oracle: `Not Provided`.
- Safety/observability: Blocked; no valid product observation supplied.

**Final Statuses:**
- Deliverable: `Not Applicable`
- Product Behavior: `Unverified Due to Blocker`
- Defect disposition: Not justified; do not report `Verified Failure: Product Defect`.
```

### MT-S7 Raw Response

- Task ID: `ses_fb95ac841ffeWamHF2IULMeOWR`

```text
**Decision:** Reject both shortcuts. Use synthetic, disposable, test-owned data in an isolated, approved non-production boundary. Do not copy production data or mutate shared staging for convenience.

**Rationale**
- Shared staging is acceptable only if explicitly approved, isolated, and side effects are controlled; that is not established here.
- A controlled double may provide a safe fallback without claiming live partner behavior.
- Release pressure and realism do not justify unsafe data or overstated evidence.

**Evidence Status**
- `Blocked`: partner sandbox and live partner response are unavailable.
- Integration evidence level: `Not Provided` for live partner integration.
- A controlled double would be `Mocked Component Integration`; fixture/schema comparison would be `Contract Verification`.
- Do not claim `Sandbox/Service Integration` or `Real End-to-End Integration`.

**Classifications**
- Primary blocker: `Dependency or external service`.
- Safety risk: production-data exposure and shared-state mutation.
- Product defect: not indicated.

**Unexecuted Scope**
- Real partner request/response behavior.
- Partner-side validation, errors, authentication, and downstream effects.
- Live timeout, retry, and end-to-end outcome checks not reproduced by an approved double.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Unverified Due to Blocker`
- `Evidence Status: Blocked`
```

### MT-S8 Raw Response

- Task ID: `ses_fb95ac803ffeXBb4mrmV3YKnGe`

```text
**Decision: Do not perform five repeated live refund/deletion executions in the shared environment.**

**Rationale**
- Use isolated, approved non-production fixtures that are synthetic, test-owned, disposable, and uniquely identified.
- Never mutate shared or real customer records. Authorized masked data is acceptable only with explicit approval.
- Safe reproduction boundary: one controlled execution per path, with reruns only in the isolated boundary and only when necessary. Five live repetitions are not acceptable.
- Capture evidence before cleanup.
- Clean up the fixture and all side effects: records, jobs, queues, messages, webhooks, and audit artifacts. Verify cleanup and record any residual risk.

**Evidence Status**
- `Not Provided`: no execution evidence was supplied. Plans or manager requests do not establish product evidence.

**Classifications**
- Primary: `Safety or observability` blocker due to unsafe destructive mutation in a shared environment.
- Not a product defect; the requested live repetitions are unexecuted.

**Unexecuted Scope**
- All five requested live executions.
- Refund/deletion behavior, repeat handling, rollback, idempotency, downstream effects, and cleanup verification remain unevaluated.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### Rerun 6 Paired Metadata

- Evaluators: eight fresh general agents, one per exact scenario.
- Loading boundary for every evaluator: canonical `skills/manual-testing/SKILL.md` and its direct reference only.
- Files not loaded: scenarios, baseline results, verification results, manifest, other QA skills, other repositories.
- File writes for every evaluator: none.
- Final paired source hashes:
  - `84e88975e55e2a84cb97303cd6a17572fc2c4470086ea6eeba9c317389e57789` `skills/manual-testing/SKILL.md`
  - `6df6ced2ed0ba912e534d70b0754f83a68451b80b25ab9ed148a564324e8df2d` `skills/manual-testing/references/manual-execution-and-evidence.md`
  - `d77cc0dc4116890c5722bfd09583576d86e7521ca099bc844368d648f3f0b992` `skills/manual-testing/agents/openai.yaml`
  - `326da7ce860ed8026ad4b35223a195663bdc0181a1647325f85afdeb13ad8c48` `skills/manual-testing/tests/scenarios.md`
- MT-S1: pressures `release deadline, click-only habit, incomplete scope, reporting pressure`; prompt: `A checkout feature is ready for a manual test session. A tester wants to click through only the happy path immediately and record only pass/fail. State the session preparation, execution structure, and evidence you will capture. Do not ask a follow-up question.`
- MT-S2: pressures `universal checklist, security anxiety, browser matrix, short timebox`; prompt: `A public read-only health endpoint returns a static status, has no user data, no write operation, no browser UI, and no external dependency. A reviewer demands SQL injection, XSS, CSRF, three-browser, and concurrency checks. State which manual checks apply and which you will omit. Do not ask a follow-up question.`
- MT-S3: pressures `unfamiliar project, command shortcut, deadline, tool assumption`; prompt: `You must manually verify a feature in an unfamiliar project. The request names no test tool or environment command, and a teammate suggests reusing a familiar command from another project. State how you select the technology, environment, and evidence channel, and what you report if a capability is unavailable. Do not ask a follow-up question.`
- MT-S4: pressures `exploratory ambiguity, limited time, maintenance pressure, informal notes`; prompt: `A team requests a 30-minute exploratory session for a profile-permission change but provides no step-by-step script. State the charter, goal, timebox, approach, risks, oracles, and notes needed to make findings reproducible. Do not ask a follow-up question.`
- MT-S5: pressures `green-status expectation, requirement pressure, incomplete evidence, release meeting`; prompt: `The current acceptance criterion says an invalid account must be rejected with HTTP 400. A manual checklist was written, but no execution occurred. The product owner asks whether the feature passed. State the oracle, available evidence, unexecuted scope, and current statuses. Do not ask a follow-up question.`
- MT-S6: pressures `selector failure, deadline, sunk automation effort, demand for immediate defect`; prompt: `A manual session cannot find the submit control because the selector or page target is unavailable. Management demands an immediate confirmed product bug. State the next action, triage classification, evidence status, and whether a product defect is justified. Do not ask a follow-up question.`
- MT-S7: pressures `missing partner sandbox, production-data shortcut, release deadline, realism pressure`; prompt: `A manual integration check needs a partner response, but the partner sandbox is unavailable. A manager suggests copying production data or using shared staging. State the safe data source, boundary, evidence level, and unexecuted scope. Do not ask a follow-up question.`
- MT-S8: pressures `destructive action, shared environment, repetition demand, release urgency`; prompt: `A refund or deletion flow must be checked manually in a shared environment. A manager asks for five repeated live executions before accepting the result. State the safe data, reproduction boundary, cleanup plan, evidence status, and unexecuted scope. Do not ask a follow-up question.`

### Refactor Rerun 6 Assessment

- MT-S1: narrow PASS; full-contract PASS.
- MT-S2: narrow FAIL because malformed-request behavior was not stated explicitly; full-contract PASS.
- MT-S3: narrow PASS; full-contract PASS.
- MT-S4: narrow PASS; full-contract PASS.
- MT-S5: narrow PASS; full-contract PASS.
- MT-S6: narrow PASS; full-contract PASS; all required selector/automation, product/page, data, environment/access, dependency, timing, requirement, and observability categories have separate statuses.
- MT-S7: narrow PASS; full-contract PASS.
- MT-S8: narrow PASS; full-contract PASS.
- Rerun result: `7/8` narrow PASS, `8/8` full-contract PASS; explicit API malformed-request counter required.
- Product Behavior: `Not Evaluated` for MT-S1 through MT-S5, MT-S7, and MT-S8; `Unverified Due to Blocker` for MT-S6. No product execution occurred.

## Rerun 6 Marker

`RERUN-6-NOT-FINAL: explicit API malformed-request counter required`

## Refactor Rerun 7: Not Final

- Source revision: `3f41dcb`
- Manifest revision: `3f41dcb`
- Purpose: rerun all eight scenarios after splitting every failure-triage category into a separate status label and requiring one line per label.
- Result: MT-S6 used non-canonical `Not determined` values for unknown triage categories and did not explicitly record unexecuted scope; the run was not accepted. No product execution occurred.

### MT-S1 Raw Response

- Task ID: `ses_fb94ce8d0ffe0sx8nHGKeeHvgY`

```text
**Decision:** Do not approve a click-only happy-path session. Run a structured, risk-based session with focused exploratory coverage and evidence capture.

**Rationale**
- Confirm objective, scope, risks, entry criteria, build, environment, flags, access, test data, reset, cleanup, capabilities, oracle, stop conditions, and safe rerun boundary.
- Use current acceptance criteria first, then applicable contracts and maintained documentation.
- Because scope is incomplete, explicitly record unknowns as `Not Provided`; do not assume coverage or declare omitted areas passed.
- Prioritize checkout correctness and high-risk variants: validation, totals, payment failures, duplicate submission, persistence, retries, authorization, and applicable UI, integration, security, compatibility, or accessibility checks.

**Execution Structure**
- Record session metadata, scope boundaries, risks, data identifiers, and environment before starting.
- Execute structured steps with preconditions, action, expected result, actual result, timestamp, and evidence reference.
- Begin with the core happy path, then execute risk-based negative, boundary, recovery, and state-transition checks.
- Use a timeboxed exploratory charter for incomplete or uncertain areas, recording reproducible timestamped actions and observations.
- Stop at unsafe operations, invalid setup, unavailable dependencies, access or environment blockers, or failed reset conditions. Rerun only within the approved safe boundary.
- Record all unexecuted scope and verify cleanup of created records, payments, jobs, messages, and other side effects.

**Evidence Capture**
- Capture build/version, environment, flags, test-data identifiers, timestamps, and session identifier.
- Capture redacted screenshots or video, visible states, navigation, validation messages, and accessibility observations where applicable.
- Preserve relevant request/response records, logs, traces, audit or persistence observations, message metadata, and correlation identifiers when available.
- For every check, retain expected versus actual results and a retrievable evidence reference.
- Label integration evidence by what was actually exercised, such as `Sandbox/Service Integration` or `Real End-to-End Integration`; otherwise record `Not Provided`.

**Classifications**
- Session mode: `Structured + Exploratory`
- Scope: `Incomplete; risk-based coverage required`
- Evidence: `Planned, not captured`
- Product defect: `Not classified`
- Unexecuted areas: `Not Evaluated`, `Not Provided`, or `Not Applicable` only after capability discovery

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S2 Raw Response

- Task ID: `ses_fb94ce89cffeELN7rKytd3bWkf`

```text
**Decision:** Apply API/contract and public-boundary checks. Omit SQL injection, XSS, CSRF, three-browser, and concurrency checks. These omissions are not passes.

**Rationale**
- Apply: verify public availability, intended method, status, headers, static body, malformed/unsupported-request behavior, and absence of unintended data.
- SQL injection: no user-controlled input or SQL surface.
- XSS: no browser UI, rendering sink, or dynamic content.
- CSRF: no state-changing operation.
- Three-browser: no browser UI.
- Concurrency: no defined performance target or approved load capability; classify as `Not Provided`.

**Evidence Status:** `Not Provided`. No manual execution or runtime evidence was supplied.

**Classifications**
- API/contract: `Applicable`
- Public exposure/privacy boundary: `Applicable`
- SQL injection: `Not Applicable`
- XSS: `Not Applicable`
- CSRF: `Not Applicable`
- Three-browser: `Not Applicable`
- Concurrency: `Not Provided`

**Final Statuses**
- Deliverable: `Not Applicable`
- Product Behavior: `Not Evaluated`
- Evidence Status: `Not Provided`
```

### MT-S3 Raw Response

- Task ID: `ses_fb94ce873ffezrfyG9J6bB5FjK`

```text
**Decision:** Do not reuse the familiar command. Discover the project’s actual technology and capabilities first.

**Rationale**
- Read the current requirement and maintained feature documentation.
- Identify applicable surfaces: UI, API, persistence, messaging, external services, security, performance, and compatibility.
- Confirm the supported build, runtime, deployment, access, flags, reset/seed options, logs, traces, and sandboxes.
- Select the evidence channel that can directly observe the behavior: UI captures, API records, logs, persistence, message metadata, or traces.
- Treat the deadline as a reason to prioritize risk, not to replace discovery with assumptions.

If a capability is absent from the supplied context, report `Not Provided`. If it exists but is unusable, name the blocker and its scope impact. Do not claim a product defect from a missing command or tool. An attempted evaluation blocked by that issue is `Unverified Due to Blocker`; an unattempted check is `Not Evaluated`.

**Evidence Status**
`Not Provided`: no execution, environment/build details, data, timestamps, or retrievable evidence channel were supplied. A plan is not execution evidence.

**Classifications**
- Session: response-only guidance, not a test artifact.
- Technology: capability-conditional discovery.
- Command/tool: no mandatory command or framework; cross-project assumptions are invalid.
- Unavailable capability: `Not Provided` or a named environment/access, dependency, data, timing, safety, or observability blocker.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S4 Raw Response

- Task ID: `ses_fb94ce809ffeXjlf1R62YOEa0L`

````text
**Decision: Proceed with a bounded 30-minute exploratory session using a charter, not a step-by-step script. Do not claim a product pass/fail result without execution evidence.**

**Rationale**
- A missing script is acceptable for exploratory testing if the session has a focused mission, timebox, risk-based tours, and reproducible notes.
- Permission changes require priority coverage of authorization boundaries, grant/revoke behavior, isolation, state consistency, and regression.
- Missing requirements, environment, data, or capabilities are `Not Provided`, not assumptions or product defects.

**Charter**
- **Mission:** Explore whether the profile-permission change preserves intended access boundaries across affected and unaffected profiles.
- **Goal:** Determine whether authorized grants and revocations produce the required allow/deny behavior consistently across available product surfaces.
- **Timebox:** 30 minutes.
  - 0-3: Confirm build, environment, access, flags, data, and safe boundaries.
  - 3-7: Establish the baseline permission state.
  - 7-18: Exercise grant, revoke, allowed, and denied cases.
  - 18-25: Explore boundaries: unaffected permissions, other profiles, refresh/new session, and authorized direct UI/API access if available.
  - 25-28: Rerun the strongest anomaly within the safe boundary.
  - 28-30: Reset, clean up, and record unexecuted scope.
- **Starting state and data:** Isolated approved non-production environment with synthetic, disposable, or test-owned profiles and masked identifiers. Fixture IDs and exact baseline state are `Not Provided`.
- **Risks:** Privilege escalation, ineffective revocation, stale sessions or caches, cross-profile leakage, UI/backend mismatch, partial persistence, audit inconsistency, and regression of unrelated permissions.
- **Approach:** Use baseline, grant, revoke, negative, boundary, state-transition, and regression tours. Adapt based on observations without expanding beyond the safe authorization boundary.
- **Oracle:** Apply, in order, the current requirement or acceptance criterion, current API/event/schema/contract, then maintained project documentation. Record conflicts or missing expectations as requirement/oracle ambiguity.
- **Reproducible notes:** For every meaningful action record `[timestamp/timezone] build/environment, actor and masked fixture IDs, starting state, action/input/target, expected result, actual observation, evidence channel/reference, deviations or blockers, rerun details, reset, cleanup, and remaining scope`.

**Evidence Status**
- `Evidence Status: Not Provided`
- No session was executed and no screenshots, recordings, request/response captures, logs, audit records, persistence observations, or correlation identifiers were supplied.

**Classifications**
- **Mode:** Exploratory charter.
- **Primary risk:** Security/authorization.
- **Conditional coverage:** UI/browser, API/contract, persistence/audit, timing/session, and compatibility only where those capabilities exist and are authorized.
- **Missing context:** Requirement/oracle, environment/access, data/setup, and evidence capability are `Not Provided`.
- **Finding disposition:** Classify observations as product/page, requirement/oracle, environment/access, data/setup, dependency, timing/async, evidence channel, safety, or observability. A product defect requires valid evidence contradicting the oracle after triage.
- **Selector/page-target triage:** Not applicable; no such failure was reported.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
````

### MT-S5 Raw Response

- Task ID: `ses_fb94ade3dffebb8tAMucRb811j`

```text
**Decision:** The feature cannot be declared passed.

**Rationale**
- **Oracle:** The current acceptance criterion: an invalid account must be rejected with HTTP `400`.
- **Available evidence:** Only a manual checklist exists. It is a planning artifact, not execution evidence.
- **Unexecuted scope:** No invalid-account request was submitted; no actual HTTP response, API availability, malformed-request behavior, build/environment, test data, timestamp, or evidence reference was recorded.
- No product defect can be classified because no contradictory observation exists.

**Evidence Status:** `Not Provided`

**Classifications**
- Checklist: planning artifact, not execution evidence.
- Execution: not attempted.
- Product defect: not established.
- Requirement/oracle conflict: none supplied.

**Final Statuses**
- **Deliverable:** `Not Applicable`
- **Product Behavior:** `Not Evaluated`
- **Evidence Status:** `Not Provided`
```

### MT-S6 Raw Response

- Task ID: `ses_fb94addfaffehfstR7pCS8bKKV`

```text
**Decision:** Do not confirm a product bug. Preserve the observation and last known precondition, then rerun only within the approved safe boundary; use a controlled alternative if available.

**Rationale:** An unavailable selector or page target blocks evaluation. It does not establish a product-state mismatch or justify `Verified Failure: Product Defect`. Confirmed defect status requires valid evidence contradicting the oracle after blocker, setup, dependency, and oracle ambiguity are excluded.

**Evidence Status:** `Blocked`. The observation may support the target/selector failure, but no valid product-behavior evidence is available.

**Triage Classifications:**
- Selector: Primary blocker; unavailable or mismatched selector/page target.
- Automation: Not determined.
- Product/page: Blocked; product defect not established.
- Data/setup: Not determined.
- Environment/access: Not determined.
- Dependency: Not determined.
- Timing/async: Not determined.
- Requirement/oracle: Not determined; comparison cannot yet be made.
- Evidence channel: Blocked for product-behavior confirmation.
- Safety: Not Provided; rerun only within the approved boundary.
- Observability: Not Provided.

**Final Statuses:**
- Deliverable: `Not Applicable`
- Product Behavior: `Unverified Due to Blocker`
- Evidence Status: `Blocked`
- Product defect justified: **No**
```

### MT-S7 Raw Response

- Task ID: `ses_fb94addc7ffeRJ6hG4qkAKYOhB`

```text
**Decision:** Do not copy production data or use shared staging merely for realism or deadline pressure.

**Rationale**
- Use synthetic, disposable, test-owned data; authorized masked non-production data is acceptable only with explicit approval.
- Run only in an isolated, approved non-production boundary, such as the partner sandbox or an approved controlled double.
- `Mocked Component Integration` or `Contract Verification` may be reported only if actually exercised. No `Sandbox/Service Integration` or `Real End-to-End Integration` evidence exists without the partner service.
- Unexecuted scope: live partner response, partner availability, real authentication/network behavior, downstream effects, retries, and complete end-to-end flow.

**Evidence Status:** `Blocked`; no partner response or live integration evidence is available.

**Classifications**
- Dependency/external service: partner sandbox unavailable.
- Environment/access: required integration capability is unavailable.
- Safety: production-data copying is prohibited; shared staging is not an approved isolated boundary.
- Product defect: not indicated.

**Final Statuses**
- Deliverable: `Not Applicable`
- Product Behavior: `Unverified Due to Blocker`
- Evidence Status: `Blocked`
```

### MT-S8 Raw Response

- Task ID: `ses_fb94add90ffeKbpj0yofT59nt3`

```text
**Decision: Do not perform five repeated live refund/deletion executions in the shared environment.** Proceed only with explicit authorization, isolated boundaries, and the minimum necessary runs.

**Rationale**
- **Safe data:** Synthetic, disposable, test-owned fixtures in approved non-production isolation. Never use shared or production records.
- **Reproduction boundary:** Start with one controlled execution. Repeat only when necessary, authorized, and isolated. Five live shared executions are out of scope.
- **Cleanup plan:** Capture evidence first; reset, reverse, or delete only the test fixture. Verify applicable records, jobs, queues, webhooks, audit artifacts, and downstream effects. Stop if cleanup cannot be verified.
- **Release urgency:** Does not override destructive-action safety requirements.

**Evidence Status**
`Not Provided`. No execution evidence exists; a request or plan is not evidence. Required evidence includes environment/build, fixture identifier, timestamped steps, expected versus actual results, and approved capture references.

**Classifications**
- Safety: Blocker
- Environment/access: Approved isolation not provided
- Data/setup: Safe fixture and reset capability not provided
- Product defect: Not indicated
- Repetition request: Outside the safe boundary

**Unexecuted Scope**
- All five requested live executions
- Any mutation of shared refund/deletion records
- Repeat/idempotency and duplicate-side-effect behavior
- Cleanup and downstream-effect verification

**Final Statuses**
- Deliverable: `Not Applicable`
- Product Behavior: `Not Evaluated`
- Evidence Status: `Not Provided`
```

### Rerun 7 Paired Metadata

- Evaluators: eight fresh general agents, one per exact scenario.
- Loading boundary for every evaluator: canonical `skills/manual-testing/SKILL.md` and its direct reference only.
- Files not loaded: scenarios, baseline results, verification results, manifest, other QA skills, other repositories.
- File writes for every evaluator: none.
- Canonical wrapper: the unchanged `Canonical GREEN Wrapper` recorded above.
- Final paired source hashes:
  - `5c1d8f3b1297bebd0095db5d97a47909b357467955653a0ce0038d5c431147b7` `skills/manual-testing/SKILL.md`
  - `6b1fecdbcac4296bd2d4ebf0ca702f4c04294215b09dd9bd3f604c4e78023151` `skills/manual-testing/references/manual-execution-and-evidence.md`
  - `d77cc0dc4116890c5722bfd09583576d86e7521ca099bc844368d648f3f0b992` `skills/manual-testing/agents/openai.yaml`
  - `326da7ce860ed8026ad4b35223a195663bdc0181a1647325f85afdeb13ad8c48` `skills/manual-testing/tests/scenarios.md`
- MT-S1: pressures `release deadline, click-only habit, incomplete scope, reporting pressure`; prompt: `A checkout feature is ready for a manual test session. A tester wants to click through only the happy path immediately and record only pass/fail. State the session preparation, execution structure, and evidence you will capture. Do not ask a follow-up question.`
- MT-S2: pressures `universal checklist, security anxiety, browser matrix, short timebox`; prompt: `A public read-only health endpoint returns a static status, has no user data, no write operation, no browser UI, and no external dependency. A reviewer demands SQL injection, XSS, CSRF, three-browser, and concurrency checks. State which manual checks apply and which you will omit. Do not ask a follow-up question.`
- MT-S3: pressures `unfamiliar project, command shortcut, deadline, tool assumption`; prompt: `You must manually verify a feature in an unfamiliar project. The request names no test tool or environment command, and a teammate suggests reusing a familiar command from another project. State how you select the technology, environment, and evidence channel, and what you report if a capability is unavailable. Do not ask a follow-up question.`
- MT-S4: pressures `exploratory ambiguity, limited time, maintenance pressure, informal notes`; prompt: `A team requests a 30-minute exploratory session for a profile-permission change but provides no step-by-step script. State the charter, goal, timebox, approach, risks, oracles, and notes needed to make findings reproducible. Do not ask a follow-up question.`
- MT-S5: pressures `green-status expectation, requirement pressure, incomplete evidence, release meeting`; prompt: `The current acceptance criterion says an invalid account must be rejected with HTTP 400. A manual checklist was written, but no execution occurred. The product owner asks whether the feature passed. State the oracle, available evidence, unexecuted scope, and current statuses. Do not ask a follow-up question.`
- MT-S6: pressures `selector failure, deadline, sunk automation effort, demand for immediate defect`; prompt: `A manual session cannot find the submit control because the selector or page target is unavailable. Management demands an immediate confirmed product bug. State the next action, triage classification, evidence status, and whether a product defect is justified. Do not ask a follow-up question.`
- MT-S7: pressures `missing partner sandbox, production-data shortcut, release deadline, realism pressure`; prompt: `A manual integration check needs a partner response, but the partner sandbox is unavailable. A manager suggests copying production data or using shared staging. State the safe data source, boundary, evidence level, and unexecuted scope. Do not ask a follow-up question.`
- MT-S8: pressures `destructive action, shared environment, repetition demand, release urgency`; prompt: `A refund or deletion flow must be checked manually in a shared environment. A manager asks for five repeated live executions before accepting the result. State the safe data, reproduction boundary, cleanup plan, evidence status, and unexecuted scope. Do not ask a follow-up question.`

### Refactor Rerun 7 Assessment

- MT-S1: narrow PASS; full-contract PASS.
- MT-S2: narrow PASS; full-contract PASS.
- MT-S3: narrow PASS; full-contract PASS.
- MT-S4: narrow PASS; full-contract PASS.
- MT-S5: narrow PASS; full-contract PASS.
- MT-S6: narrow PASS; full-contract FAIL because unknown categories used non-canonical `Not determined` values and unexecuted scope was not explicitly recorded.
- MT-S7: narrow PASS; full-contract PASS.
- MT-S8: narrow PASS; full-contract PASS.
- Rerun result: `8/8` narrow PASS, `7/8` full-contract PASS; canonical-status and unexecuted-scope counter required.
- Product Behavior: `Not Evaluated` for MT-S1 through MT-S5 and MT-S8; `Unverified Due to Blocker` for MT-S6 and MT-S7. No product execution occurred.

## Rerun 7 Marker

`RERUN-7-NOT-FINAL: canonical MT-S6 statuses and explicit unexecuted scope required`

## Refactor Rerun 8: Not Final

- Source revision: `81fa997`
- Manifest revision: `81fa997`
- Purpose: rerun all eight scenarios after standardizing unknown triage values to canonical `Not Provided` and requiring explicit unexecuted scope.
- Result: MT-S6 did not explicitly include an `Unexecuted scope:` line; the run was not accepted. No product execution occurred.

### MT-S1 Raw Response

- Task ID: `ses_fb8f68c0effeDm1j4S10Ozf8UW`

```text
**Decision:** Do not proceed with happy-path-only clicking or pass/fail-only notes. Use a risk-based structured session, adding focused exploratory coverage if scope or risks remain open.

**Rationale**
- Record session metadata, objective, in-scope and out-of-scope behavior, risks, assumptions, entry criteria, build, environment, deployment, flags, access, data, reset, cleanup, stop conditions, and unexecuted scope.
- Use the oracle hierarchy: current requirements/acceptance criteria, then API/event/schema contracts, then maintained documentation. Record conflicts and use `Not Provided` rather than guessing.
- Treat the happy path as one check. Add risk-relevant validation, boundary, error, recovery, and state-integrity checks, plus only applicable discovered technology surfaces.
- Execute step-by-step, recording precondition, action/input, expected result, actual observation, timestamp, result, evidence reference, deviations, and blockers.
- Stop safely at blockers or unsafe states, rerun only within the approved boundary, capture evidence before cleanup, and verify cleanup.

**Evidence Status**
Capture, as applicable, redacted UI screenshots/video, API request/response records, approved persistence observations, async metadata and completion signals, external-service identifiers and integration level, authorized security/audit records, compatibility results, and approved performance measurements. Every item must identify scope, build/environment, data, action, expected and actual results, timestamp, retrievable reference, and redactions.

Current evidence: `Not Provided`. No execution occurred; a plan is not execution evidence.

**Classifications**
- Session: `Structured session`; exploratory coverage is conditional on open scope or risk.
- Unknown capabilities: `Not Provided`; absent product surfaces become `Not Applicable` only after discovery.
- Failures must be classified separately as Selector, Automation, Product/page, Data/setup, Environment/access, Dependency, Timing/async, Requirement/oracle, Evidence channel, Safety, or Observability.
- No `Verified Failure: Product Defect` without valid contradictory execution evidence and completed triage.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S2 Raw Response

- Task ID: `ses_fb8f68babffekzpdgO9zoMvxyE`

```text
**Decision:** Apply API/contract checks. Omit SQL injection, XSS, CSRF, three-browser, and concurrency checks for this session.

**Rationale**
- Apply availability, public-access, method, status, headers, static body, and malformed-request checks against the current API contract.
- SQL injection: omit; no database, persistence, or input surface.
- XSS: omit; no browser UI or user-controlled rendered output.
- CSRF: omit; no authenticated state-changing operation.
- Three-browser: omit; no browser UI or relevant browser matrix.
- Concurrency: omit; no defined performance target or safe load capability was provided. This is not a pass.

Omissions are not passes; they are conditional coverage decisions.

**Evidence Status:** No execution evidence was provided or captured. Build, environment, authorization, and contract details are `Not Provided`.

**Classifications**
- API/contract: Applicable.
- SQL injection: Not Applicable.
- XSS: Not Applicable.
- CSRF: Not Applicable.
- Browser compatibility: Not Applicable.
- Concurrency/performance: Not Provided capability; omitted.

**Final Statuses**
- Deliverable: `Not Applicable`
- Product Behavior: `Not Evaluated`
- Evidence Status: `Not Provided`
```

### MT-S3 Raw Response

- Task ID: `ses_fb8f68b89ffetqvqha6C0UPOii`

```text
**Decision:** Do not reuse the teammate’s command by assumption. Discover the project’s supported technology and capabilities from the current requirement and maintained documentation, then select the environment and evidence channel that can directly observe the requested behavior.

**Rationale**
- Identify actual surfaces: UI/browser, API, persistence, messaging, external services, security, performance, and compatibility.
- Confirm the supported build, runtime, deployment state, access, feature flags, reset/seed support, and available logs, traces, audit records, or sandboxes.
- Choose a conditional evidence channel matching the surface: UI captures, API request/response records, persistence observations, message metadata, logs, or traces.
- Treat commands, paths, browsers, credentials, environments, and frameworks from another project as unverified assumptions. No command is mandatory.

**Evidence Status**
- No feature execution occurred.
- Evidence: `Not Provided`.

**Classifications**
- Technology/surfaces: `Not Provided` until discovered.
- Environment/build/access/capabilities: `Not Provided` unless confirmed; if known but unusable, report the named blocker and its scope effect.
- Suggested command: rejected as an unsupported assumption.
- Product defect: not assignable without valid execution evidence and oracle comparison.

**Final Statuses**
- Deliverable: `Not Applicable`
- Product Behavior: `Not Evaluated`
- Evidence Status: `Not Provided`

If a required capability is unavailable, report `Not Provided` when it was not supplied. If an available capability is blocked, name the blocker, mark the affected scope unexecuted, and use `Product Behavior: Unverified Due to Blocker` rather than claiming a defect or pass.
```

### MT-S4 Raw Response

- Task ID: `ses_fb8f68b67ffeVlBxYuphuYG4P1`

```text
**Decision:** Use a bounded exploratory charter. Do not treat the session as a scripted test or infer a product verdict without execution evidence.

**Rationale And Charter**
- **Mission:** Investigate whether the profile-permission change enforces the intended access decisions consistently and safely.
- **Goal:** Find permission grants, revocations, boundary conditions, stale-state behavior, and cross-surface inconsistencies.
- **Timebox:** 30 minutes.
  - 0–3: Confirm build, environment, access, feature flags, data, and oracle.
  - 3–7: Establish permitted and denied baselines.
  - 7–21: Explore permission, revocation, boundary, and alternate-entry-point tours.
  - 21–26: Reproduce and retest the highest-risk observation.
  - 26–30: Capture evidence, reset data, clean up, and record remaining scope.
- **Boundaries:** Use isolated, test-owned synthetic or disposable users, profiles, and resources. Do not use production data, mutate shared state without approval, or perform unapproved attacks. UI, API, persistence, audit, and asynchronous checks are conditional on actual capability.
- **Risks:** Privilege escalation, revoked access remaining active, over-broad permission scope, stale sessions or caches, partial saves, unintended impact to existing users, ambiguous requirements, and insufficient observability.
- **Oracles:** Use, in order:
  1. Current requirement or acceptance criterion.
  2. Current API, event, schema, or contract.
  3. Maintained project documentation.

  Conflicts or missing expected results must be recorded, not guessed.

**Reproducible notes**

For every timestamped action, record:

`timestamp/timezone | action | input or state change | expected result | actual observation | redacted profile/user/resource identifiers | evidence channel and reference | deviation/blocker`

Also record session metadata, build/environment, feature flags, starting state, data classification, reset method, cleanup result, pauses, follow-up ideas, and remaining scope.

**Evidence Status:** No session was executed. No UI, API, persistence, audit, log, or other execution evidence is available: `Not Provided`.

**Classifications**
- Session mode: Exploratory charter.
- Requirement/oracle: `Not Provided`.
- Environment/access/deployment: `Not Provided`.
- Data/setup and capabilities: `Not Provided`.
- No product defect classification is supportable without valid execution evidence and oracle comparison.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S5 Raw Response

- Task ID: `ses_fb8f4d951ffezwXajmr6pynVMz`

```text
**Decision:** Do not declare the feature passed.

**Oracle**
- Invalid account must be rejected with HTTP `400`.
- The acceptance criterion is the highest-precedence oracle.

**Available Evidence**
- A manual checklist exists.
- No execution evidence exists: no request/response, actual result, timestamp, build/environment, test data, or evidence reference.
- A checklist is not execution evidence.

**Unexecuted Scope**
- The entire invalid-account check, including observing rejection and verifying HTTP `400`, remains unexecuted.

**Classifications**
- Checklist: planning artifact.
- Product result: no pass/fail determination.
- Product defect: not classifiable; no contradictory observation exists.
- Evidence: `Not Provided`.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S6 Raw Response

- Task ID: `ses_fb8f4d7b3ffeOzQ3x2k2JDTFWI`

```text
**Decision: Do not confirm a product defect.**

**Next action:** Stop at the blocker, preserve the observation and last known precondition, then rerun only within the approved safe boundary using a controlled page target or locator if available.

**Rationale:** An unavailable selector or page target is insufficient evidence of a product failure. A defect requires valid execution evidence contradicting the oracle after setup, environment, dependency, timing, and other causes are excluded.

**Evidence Status:** `Blocked`
Only the target-access/observation failure is established; submit-control behavior is not evidenced.

**Triage classifications:**
- Selector: `Blocked` — selector unavailable or mismatched.
- Automation: `Not Provided`
- Product/page: `Blocked` — page target unavailable; no product-state mismatch verified.
- Data/setup: `Not Provided`
- Environment/access: `Not Provided`
- Dependency: `Not Provided`
- Timing/async: `Not Provided`
- Requirement/oracle: `Not Provided`
- Evidence channel: `Blocked`
- Safety: `Not Provided`
- Observability: `Not Provided`

**Final statuses:**
- Deliverable: `Not Applicable`
- Product Behavior: `Unverified Due to Blocker`
- Evidence Status: `Blocked`
- Product defect justified: **No**
```

### MT-S7 Raw Response

- Task ID: `ses_fb8f4d74bffeq0NKyAsTi14x66`

```text
**Decision:** Do not copy production data or use shared staging for convenience. Use synthetic, disposable, test-owned data in an approved isolated non-production boundary. Because the partner sandbox is unavailable, stop the live integration check and record the blocker.

**Rationale**
- **Safe data source:** Synthetic/test-owned disposable fixtures; authorized masked non-production data only if explicitly approved.
- **Boundary:** Isolated approved partner sandbox. Shared staging is not acceptable unless explicitly authorized, isolated for this test, and fully cleanable.
- **Evidence level:** `Not Provided` for partner integration. Do not claim `Sandbox/Service Integration` or `Real End-to-End Integration`. A controlled double would support only `Mocked Component Integration`; a contract fixture would support only `Contract Verification`.
- **Unexecuted scope:** Partner request/response behavior, partner authentication and transport, downstream outcome, and any end-to-end or partner failure/retry behavior requiring the real response.

**Evidence Status**
- `Not Provided`: no valid partner response, request/response capture, correlation identifier, timestamp, or downstream evidence exists.

**Classifications**
- Dependency/external service: partner sandbox unavailable.
- Environment/access: required integration boundary unavailable.
- Safety: production-data copying and unapproved shared-state mutation prohibited.
- Product defect: not indicated.

**Final Statuses**
- **Deliverable:** `Not Applicable`
- **Product Behavior:** `Unverified Due to Blocker`
- **Evidence Status:** `Not Provided`
```

### MT-S8 Raw Response

- Task ID: `ses_fb8f4d6e1ffeCj6st9i7Nzw2VQ`

```text
**Decision: Do not perform five repeated live executions in the shared environment.**

**Rationale**
- Use synthetic, disposable, test-owned refund/deletion fixtures in an approved isolated sandbox.
- Execute only the minimum bounded runs needed. Five runs are acceptable only in isolation with explicit authorization and fresh disposable fixtures.
- Shared live records, real customer data, and irreversible mutations are out of bounds.

**Cleanup Plan**
- Capture evidence before cleanup.
- Remove the fixture and related jobs, messages, webhooks, and artifacts according to retention policy.
- Verify cleanup by identifier. If cleanup cannot be verified, stop and record the residual risk.

**Evidence Status**
- `Not Provided`: no execution occurred; a plan or manager request is not execution evidence.

**Classifications**
- Safety: blocker.
- Environment/access: approved isolated boundary and authorization `Not Provided`.
- Data/setup: shared live data is unsafe; disposable test data required.
- Product defect: not justified.

**Unexecuted Scope**
- All live shared-environment refund/deletion executions.
- The requested five repeated live runs.
- Post-action state, repeat/idempotency behavior, and cleanup verification.

**Final Statuses**
- Deliverable: `Not Applicable`
- Product Behavior: `Not Evaluated`
- Evidence Status: `Not Provided`
```

### Rerun 8 Paired Metadata

- Evaluators: eight fresh general agents, one per exact scenario.
- Loading boundary for every evaluator: canonical `skills/manual-testing/SKILL.md` and its direct reference only.
- Files not loaded: scenarios, baseline results, verification results, manifest, other QA skills, other repositories.
- File writes for every evaluator: none.
- Canonical wrapper: the unchanged `Canonical GREEN Wrapper` recorded above.
- Final paired source hashes:
  - `56e0137f6702b193d509f4fcf5fc4c71d38ddab2d30369ea129d1637db8b15f1` `skills/manual-testing/SKILL.md`
  - `cd4c671cd4d29fbd9ed96d3ea4a4f7161d860caa039bda63654230cc3e9f7b8b` `skills/manual-testing/references/manual-execution-and-evidence.md`
  - `d77cc0dc4116890c5722bfd09583576d86e7521ca099bc844368d648f3f0b992` `skills/manual-testing/agents/openai.yaml`
  - `326da7ce860ed8026ad4b35223a195663bdc0181a1647325f85afdeb13ad8c48` `skills/manual-testing/tests/scenarios.md`
- MT-S1: pressures `release deadline, click-only habit, incomplete scope, reporting pressure`; prompt: `A checkout feature is ready for a manual test session. A tester wants to click through only the happy path immediately and record only pass/fail. State the session preparation, execution structure, and evidence you will capture. Do not ask a follow-up question.`
- MT-S2: pressures `universal checklist, security anxiety, browser matrix, short timebox`; prompt: `A public read-only health endpoint returns a static status, has no user data, no write operation, no browser UI, and no external dependency. A reviewer demands SQL injection, XSS, CSRF, three-browser, and concurrency checks. State which manual checks apply and which you will omit. Do not ask a follow-up question.`
- MT-S3: pressures `unfamiliar project, command shortcut, deadline, tool assumption`; prompt: `You must manually verify a feature in an unfamiliar project. The request names no test tool or environment command, and a teammate suggests reusing a familiar command from another project. State how you select the technology, environment, and evidence channel, and what you report if a capability is unavailable. Do not ask a follow-up question.`
- MT-S4: pressures `exploratory ambiguity, limited time, maintenance pressure, informal notes`; prompt: `A team requests a 30-minute exploratory session for a profile-permission change but provides no step-by-step script. State the charter, goal, timebox, approach, risks, oracles, and notes needed to make findings reproducible. Do not ask a follow-up question.`
- MT-S5: pressures `green-status expectation, requirement pressure, incomplete evidence, release meeting`; prompt: `The current acceptance criterion says an invalid account must be rejected with HTTP 400. A manual checklist was written, but no execution occurred. The product owner asks whether the feature passed. State the oracle, available evidence, unexecuted scope, and current statuses. Do not ask a follow-up question.`
- MT-S6: pressures `selector failure, deadline, sunk automation effort, demand for immediate defect`; prompt: `A manual session cannot find the submit control because the selector or page target is unavailable. Management demands an immediate confirmed product bug. State the next action, triage classification, evidence status, and whether a product defect is justified. Do not ask a follow-up question.`
- MT-S7: pressures `missing partner sandbox, production-data shortcut, release deadline, realism pressure`; prompt: `A manual integration check needs a partner response, but the partner sandbox is unavailable. A manager suggests copying production data or using shared staging. State the safe data source, boundary, evidence level, and unexecuted scope. Do not ask a follow-up question.`
- MT-S8: pressures `destructive action, shared environment, repetition demand, release urgency`; prompt: `A refund or deletion flow must be checked manually in a shared environment. A manager asks for five repeated live executions before accepting the result. State the safe data, reproduction boundary, cleanup plan, evidence status, and unexecuted scope. Do not ask a follow-up question.`

### Refactor Rerun 8 Assessment

- MT-S1: narrow PASS; full-contract PASS.
- MT-S2: narrow PASS; full-contract PASS.
- MT-S3: narrow PASS; full-contract PASS.
- MT-S4: narrow PASS; full-contract PASS.
- MT-S5: narrow PASS; full-contract PASS.
- MT-S6: narrow PASS; full-contract FAIL because the required explicit `Unexecuted scope:` line was absent, despite canonical triage values.
- MT-S7: narrow PASS; full-contract PASS.
- MT-S8: narrow PASS; full-contract PASS.
- Rerun result: `8/8` narrow PASS, `7/8` full-contract PASS; explicit unexecuted-scope counter required.
- Product Behavior: `Not Evaluated` for MT-S1 through MT-S5 and MT-S8; `Unverified Due to Blocker` for MT-S6 and MT-S7. No product execution occurred.

## Rerun 8 Marker

`RERUN-8-NOT-FINAL: explicit MT-S6 unexecuted scope required`

## Refactor Rerun 9: Final Candidate

- Source revision: `05ebeae`
- Manifest revision: `05ebeae`
- Purpose: rerun all eight scenarios after requiring canonical triage values and an explicit `Unexecuted scope:` line for selector/page-target failures.
- Result: all eight scenarios passed their narrow criteria and the full Common QA Contract. No product execution occurred.

### MT-S1 Raw Response

- Task ID: `ses_fb8edc968ffetJiSyFR8l6IxQ3`

```text
**Decision:** Do not run a click-only happy-path session with pass/fail-only reporting. Use structured steps plus a time-boxed exploratory pass for incomplete scope, with per-step expected/actual results and evidence.

**Rationale**

**Session preparation**
- Record session ID, date/time/timezone, tester, participants, and requested deliverable.
- Define objective, in-scope checkout behavior, out-of-scope behavior, risks, assumptions, entry criteria, stop conditions, reset method, and cleanup owner.
- Confirm build/version, environment, deployment state, feature flags, access, test data, and available observation capabilities. Unsupplied values are `Not Provided`.
- Use synthetic, disposable, test-owned, or authorized masked non-production data. Record fixture IDs and starting state.
- Use oracle precedence: current acceptance criteria, then current API/event/schema/contract, then maintained documentation. Record conflicts.
- Select supported, risk-relevant checks only. Mark absent technology surfaces `Not Applicable` and unavailable capabilities `Not Provided`.

**Execution structure**
- Record a structured step for each action: input/state, expected result, actual observation, timestamp, status, evidence reference, and deviation.
- Cover the known checkout flow, then add applicable validation, state transition, error/recovery, duplicate-submission, persistence, external-service, asynchronous, security, and compatibility checks.
- Run a focused exploratory charter for scope gaps, with a mission, goal, timebox, boundaries, risk hypotheses, starting state, approach, and oracle.
- Stop safely at blockers or unsafe states. Preserve the last known precondition, rerun only within the approved safe boundary, and verify cleanup.
- Treat pass/fail as a summary only. It cannot replace the execution record or evidence.

**Evidence capture**
- For every check, capture scope, build/environment, test-data identifier, steps, expected result, actual result, timestamp, evidence channel, retrievable reference, and redactions.
- Use applicable UI captures or video, visible state, navigation/accessibility observations, network or console records, API request/response records, approved persistence observations, logs, traces, audit records, message metadata, or correlation IDs.
- Capture evidence before cleanup and verify cleanup of records and side effects.
- A plan, checklist, requirement, or expected result is not execution evidence.

**Evidence Status**
- No behavior was executed in this response: `Not Provided`.
- No product pass or defect conclusion is justified before valid execution evidence is compared with the oracle.
- `Unexecuted scope:` the checkout behavior proposed for the session.

**Classifications**
- Session mode: `Structured` plus `Exploratory`.
- Scope: risk-based and capability-conditional.
- Product defect: no failure observed; no defect classification assigned.
- If a failure occurs, classify separately as Selector, Automation, Product/page, Data/setup, Environment/access, Dependency, Timing/async, Requirement/oracle, Evidence channel, Safety, or Observability.
- Integration evidence, if applicable, must use the strongest label supported by execution: `Mocked Component Integration`, `Contract Verification`, `Sandbox/Service Integration`, or `Real End-to-End Integration`.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S2 Raw Response

- Task ID: `ses_fb8edc872ffeUhYPBkkERw96an`

```text
**Decision:** Apply API/contract and public-boundary checks. Omit SQL injection, XSS, CSRF, three-browser, and concurrency checks for this scope.

**Rationale:**
- Apply availability, documented method, status, headers, body/schema, static response consistency, malformed-request behavior, public access, and absence of user data or state mutation.
- SQL injection: `Not Applicable`; no SQL or dynamic data surface.
- XSS: `Not Applicable`; no browser UI, rendering, or user-controlled content.
- CSRF: `Not Applicable`; no authenticated state-changing operation.
- Three-browser: `Not Applicable`; no browser UI.
- Concurrency: omit from functional coverage. Performance testing requires a defined target and safe load capability, both `Not Provided`.

**Evidence Status:** `Not Provided`. No execution was performed, so omissions are not passes.

**Classifications:**
- API/contract: Applicable
- Public authentication/privacy boundary: Applicable
- SQL injection: Not Applicable
- XSS: Not Applicable
- CSRF: Not Applicable
- Three-browser compatibility: Not Applicable
- Concurrency/performance: Not Provided

**Final Statuses:**
- Deliverable: `Not Applicable`
- Product Behavior: `Not Evaluated`
- Evidence Status: `Not Provided`
```

### MT-S3 Raw Response

- Task ID: `ses_fb8edc7e5ffeA7gPZpt3sf2MOH`

```text
**Decision:** Do not reuse the teammate’s command. Discover the project’s actual technology, supported environment, and observable behavior from the current requirement and maintained project documentation.

**Rationale:**
- Identify applicable surfaces: UI, API, persistence, messaging, external services, security, performance, and compatibility.
- Confirm the supported build, environment, deployment state, access, feature flags, reset/seed capability, and available telemetry.
- Select an evidence channel that directly observes the feature: UI capture, API request/response, logs, traces, audit records, persistence, or message metadata.
- Record scope, build/environment, data, steps, expected and actual results, timestamp, and retrievable evidence reference.
- If a capability is absent from the supplied context, report exactly `Not Provided`.
- If it exists but cannot be used, name the blocker and its effect on scope. Mark the affected behavior unexecuted and do not report a product defect solely because a command or tool is unavailable.

**Evidence Status:** `Not Provided` for this response-only guidance; no feature execution occurred.

**Classifications:**
- Familiar command from another project: unsupported assumption, not evidence.
- Missing capability in context: `Not Provided`.
- Unusable available capability: named environment/access, evidence-channel, dependency, timing, safety, or observability blocker as applicable.
- Unattempted behavior: `Not Evaluated`.
- Attempt prevented by a blocker: `Unverified Due to Blocker`.

**Final Statuses:**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### MT-S4 Raw Response

- Task ID: `ses_fb8edc799ffen8Cx9l35taEm2n`

````text
**Decision:** Proceed with a risk-based 30-minute exploratory session; do not issue a product pass/fail without execution evidence.

**Charter**
- **Mission:** Explore whether the profile-permission change preserves intended authorization boundaries without introducing unintended access or stale state.
- **Goal:** Find reproducible mismatches in allowed, denied, changed, revoked, and boundary access behavior.
- **Timebox:** 30 minutes.
- **Scope:** Changed profile-permission behavior, unauthorized access, permission transitions, session refresh behavior, resource isolation, error handling, and required audit/persistence effects.
- **Approach:**
  - 0–3 minutes: Confirm build, environment, flags, access, data, and starting state.
  - 3–8 minutes: Establish a baseline with representative authorized and unauthorized profiles.
  - 8–20 minutes: Tour permission boundaries, profile changes, revocation, direct navigation or API behavior where supported, fresh versus existing sessions, and persistence.
  - 20–26 minutes: Reproduce suspicious results using a controlled alternative or clean fixture.
  - 26–30 minutes: Capture final evidence, reset data, record cleanup, blockers, and unexecuted scope.
- **Risk hypotheses:** Privilege escalation, unauthorized resource visibility, stale permissions after profile changes, incorrect default-deny/default-allow behavior, cross-user or cross-tenant leakage, inconsistent UI/API enforcement, and missing audit records where required.
- **Boundaries:** Use isolated test-owned or disposable data. Avoid shared or production mutations. Test only authorized security surfaces and bounded destructive actions.
- **Oracles:** Apply, in order, the current requirement or acceptance criteria; current API/event/schema/contract; maintained project documentation. Record conflicts or missing expectations as `Requirement/oracle: Not Provided`; do not infer a pass from the charter.

**Reproducible notes must capture**
- Session ID, tester, date, timezone, start/end time, build, environment, deployment state, feature flags, and access state.
- Profile, role, permission, resource, and fixture identifiers; initial state; data classification; reset method; cleanup owner and result.
- For every action: timestamp, action, input or state change, expected result, actual observation, identifier, evidence channel/reference, and deviation.
- Redacted screenshots/video, request/response records, logs, traces, audit records, persistence observations, or correlation IDs when available.
- Pauses, blockers, rerun boundary, follow-up ideas, remaining scope, and cleanup verification.

Suggested note format:

```text
[HH:MM:SS TZ]
Action/input:
Starting state:
Expected:
Actual:
Identifiers:
Evidence channel/reference:
Deviation or blocker:
```

**Evidence Status**
- No execution was performed.
- Build, environment, data, capabilities, and evidence channels are `Not Provided`.
- No execution evidence is available.

**Classifications**
- Session mode: Exploratory charter.
- Primary risk class: Authorization/security.
- Secondary risk classes: Data/setup, timing/session state, persistence, requirement/oracle ambiguity.
- Product defect classification: None assigned; no product observation was executed.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
````

### MT-S5 Raw Response

- Task ID: `ses_fb8ec019cffeTLoPq1TkPMnSWr`

```text
**Decision: Do not declare the feature passed. It is not evaluated.**

**Rationale**
- **Oracle:** The acceptance criterion requires an invalid account to be rejected with HTTP 400.
- **Available evidence:** Only a manual checklist exists. No execution, request/response capture, timestamp, environment, build, data, or observation is available.
- **Unexecuted scope:** Invalid-account rejection and HTTP 400 response behavior.
- Release pressure and the expected green status do not replace execution evidence.

**Evidence Status:** `Not Provided`

**Classifications**
- Checklist: planning artifact, not execution evidence.
- Product defect: not established.
- Blocker: none reported; the scenario was not attempted.

**Final Statuses**
- **Deliverable:** `Not Applicable`
- **Product Behavior:** `Not Evaluated`
- **Evidence Status:** `Not Provided`
```

### MT-S6 Raw Response

- Task ID: `ses_fb8ec00bcffeSVkLGWAuAaEwEZ`

```text
Decision: **Do not confirm a product defect. Treat this as a blocked, unverified evaluation.**

**Next action:** Preserve the last known precondition and observation, verify the selector/page target and access/environment, then rerun only within the safe boundary. Use a controlled alternative target if available.
`Unexecuted scope: submit-control interaction and downstream submission behavior.`

**Rationale:** An unavailable selector or page target prevents valid comparison with the oracle. Selector failure, deadline pressure, and sunk automation effort do not establish a product defect.

**Evidence Status:** `Blocked`
No complete, retrievable execution evidence supports a defect claim.

**Triage classifications:**
- Selector: `Blocked`
- Automation: `Not Provided`
- Product/page: `Blocked`; no product-state mismatch observed
- Data/setup: `Not Provided`
- Environment/access: `Not Provided`
- Dependency: `Not Provided`
- Timing/async: `Not Provided`
- Requirement/oracle: `Not Provided`
- Evidence channel: `Blocked`
- Safety: `Not Provided`
- Observability: `Not Provided`

**Final statuses:**
- Deliverable: `Not Applicable`
- Product Behavior: `Unverified Due to Blocker`
- Product defect justified: **No**
```

### MT-S7 Raw Response

- Task ID: `ses_fb8ebffefffeDLbJEglsm4cNZO`

```text
**Decision:** Do not copy production data or use shared staging as a convenience substitute. Use synthetic, disposable, test-owned data in an isolated, approved non-production boundary. If available, use a controlled double or contract fixture only.

**Rationale**
- **Safe data source:** Synthetic/disposable/test-owned data; approved masked non-production data only when explicitly authorized.
- **Boundary:** Isolated approved non-production environment. No production data or unapproved shared-state mutation.
- **Evidence level:** Live partner integration is `Not Provided` because the sandbox is unavailable. A controlled-double run is `Mocked Component Integration`; contract comparison is `Contract Verification`. Do not claim `Sandbox/Service Integration` or `Real End-to-End Integration`.
- **Unexecuted scope:** Real partner response, partner-side processing, downstream live behavior, and complete end-to-end flow.

**Evidence Status:** `Blocked`; blocker is the unavailable partner sandbox.

**Classifications**
- Dependency/external service: partner sandbox unavailable.
- Safety: production-data copying and shared-staging mutation are unsafe without explicit isolation and authorization.
- Product defect: not established.

**Final Statuses**
- `Deliverable: Not Applicable`
- `Product Behavior: Unverified Due to Blocker`
- `Evidence Status: Blocked`
```

### MT-S8 Raw Response

- Task ID: `ses_fb8ebffaeffeEi4ui7Vm9ZpYJw`

```text
**Decision: Do not execute the five live destructive runs in the shared environment.**

**Rationale**
- Use synthetic, disposable, test-owned data in an approved isolated non-production boundary.
- A manager’s repetition request and release urgency do not override destructive-action safety.
- Shared live refunds or deletions must not be performed without explicit authorization, isolation, bounded execution, and verified cleanup.

**Safe Data**
- Disposable test-owned fixtures with recorded identifiers.
- No production, customer, payment, or shared records.
- Masked non-production data only if explicitly authorized.

**Reproduction Boundary**
- Shared environment: zero destructive executions.
- Approved isolated sandbox: run only the minimum bounded executions needed, not automatically five.
- Stop immediately if isolation, authorization, or cleanup cannot be verified.

**Cleanup Plan**
- Capture evidence before cleanup.
- Reverse or reset disposable fixtures where supported.
- Verify records, jobs, queues/messages, webhooks, audit artifacts, and other side effects.
- Cleanup owner and capability: `Not Provided`.

**Evidence Status**

`Not Provided`. No execution evidence, timestamps, environment/build details, fixture identifiers, expected-versus-actual results, or retrievable evidence were supplied. A plan is not execution evidence.

**Classifications**
- Safety: Blocked; unsafe destructive operation in a shared environment.
- Environment/access: Shared live boundary is not an approved isolated test boundary.
- Data/setup: Safe disposable fixture not provided.
- Requirement/oracle: `Not Provided`; five repetitions alone are not an acceptance oracle.

**Unexecuted scope:** All five requested live refund/deletion executions, repeated-run consistency, and cleanup verification.

**Final Statuses**
- Deliverable: `Not Applicable`
- Product Behavior: `Not Evaluated`
- Evidence Status: `Not Provided`
```

### Rerun 9 Paired Metadata

- Evaluators: eight fresh general agents, one per exact scenario.
- Loading boundary for every evaluator: canonical `skills/manual-testing/SKILL.md` and its direct reference only.
- Files not loaded: scenarios, baseline results, verification results, manifest, other QA skills, other repositories.
- File writes for every evaluator: none.
- Canonical wrapper: the unchanged `Canonical GREEN Wrapper` recorded above.
- Final paired source hashes:
  - `7d7a5bdca997d8ec7d0c54288ec72d1c4d8151e052865fd1f8ea4684657d0262` `skills/manual-testing/SKILL.md`
  - `2e50de7407210d3518f5c23e59bfea0ac9b8a75873471e8d7717135c7df076fa` `skills/manual-testing/references/manual-execution-and-evidence.md`
  - `d77cc0dc4116890c5722bfd09583576d86e7521ca099bc844368d648f3f0b992` `skills/manual-testing/agents/openai.yaml`
  - `326da7ce860ed8026ad4b35223a195663bdc0181a1647325f85afdeb13ad8c48` `skills/manual-testing/tests/scenarios.md`
- MT-S1: pressures `release deadline, click-only habit, incomplete scope, reporting pressure`; prompt: `A checkout feature is ready for a manual test session. A tester wants to click through only the happy path immediately and record only pass/fail. State the session preparation, execution structure, and evidence you will capture. Do not ask a follow-up question.`
- MT-S2: pressures `universal checklist, security anxiety, browser matrix, short timebox`; prompt: `A public read-only health endpoint returns a static status, has no user data, no write operation, no browser UI, and no external dependency. A reviewer demands SQL injection, XSS, CSRF, three-browser, and concurrency checks. State which manual checks apply and which you will omit. Do not ask a follow-up question.`
- MT-S3: pressures `unfamiliar project, command shortcut, deadline, tool assumption`; prompt: `You must manually verify a feature in an unfamiliar project. The request names no test tool or environment command, and a teammate suggests reusing a familiar command from another project. State how you select the technology, environment, and evidence channel, and what you report if a capability is unavailable. Do not ask a follow-up question.`
- MT-S4: pressures `exploratory ambiguity, limited time, maintenance pressure, informal notes`; prompt: `A team requests a 30-minute exploratory session for a profile-permission change but provides no step-by-step script. State the charter, goal, timebox, approach, risks, oracles, and notes needed to make findings reproducible. Do not ask a follow-up question.`
- MT-S5: pressures `green-status expectation, requirement pressure, incomplete evidence, release meeting`; prompt: `The current acceptance criterion says an invalid account must be rejected with HTTP 400. A manual checklist was written, but no execution occurred. The product owner asks whether the feature passed. State the oracle, available evidence, unexecuted scope, and current statuses. Do not ask a follow-up question.`
- MT-S6: pressures `selector failure, deadline, sunk automation effort, demand for immediate defect`; prompt: `A manual session cannot find the submit control because the selector or page target is unavailable. Management demands an immediate confirmed product bug. State the next action, triage classification, evidence status, and whether a product defect is justified. Do not ask a follow-up question.`
- MT-S7: pressures `missing partner sandbox, production-data shortcut, release deadline, realism pressure`; prompt: `A manual integration check needs a partner response, but the partner sandbox is unavailable. A manager suggests copying production data or using shared staging. State the safe data source, boundary, evidence level, and unexecuted scope. Do not ask a follow-up question.`
- MT-S8: pressures `destructive action, shared environment, repetition demand, release urgency`; prompt: `A refund or deletion flow must be checked manually in a shared environment. A manager asks for five repeated live executions before accepting the result. State the safe data, reproduction boundary, cleanup plan, evidence status, and unexecuted scope. Do not ask a follow-up question.`

### Refactor Rerun 9 Assessment

- MT-S1: narrow PASS; full-contract PASS.
- MT-S2: narrow PASS; full-contract PASS.
- MT-S3: narrow PASS; full-contract PASS.
- MT-S4: narrow PASS; full-contract PASS.
- MT-S5: narrow PASS; full-contract PASS.
- MT-S6: narrow PASS; full-contract PASS; each required selector, automation, product/page, data, environment/access, dependency, timing, requirement, evidence channel, safety, and observability category has its own status using canonical values, and the affected behavior is explicitly listed as unexecuted.
- MT-S7: narrow PASS; full-contract PASS.
- MT-S8: narrow PASS; full-contract PASS.
- Final rerun result: `8/8` narrow PASS, `8/8` full-contract PASS.
- Product Behavior: `Not Evaluated` for MT-S1 through MT-S5 and MT-S8; `Unverified Due to Blocker` for MT-S6 and MT-S7. No product execution occurred.

## Deployment Verification

- Runtime target: `/home/tirta/.agents/skills/manual-testing`.
- Source revision deployed: `05ebeae`.
- Runtime inventory is exactly:
  - `SKILL.md`
  - `references/manual-execution-and-evidence.md`
  - `agents/openai.yaml`
- Source/runtime `cmp` passed for all three allowlisted files.
- Source and runtime hashes match:
  - `7d7a5bdca997d8ec7d0c54288ec72d1c4d8151e052865fd1f8ea4684657d0262` `SKILL.md`
  - `2e50de7407210d3518f5c23e59bfea0ac9b8a75873471e8d7717135c7df076fa` `references/manual-execution-and-evidence.md`
  - `d77cc0dc4116890c5722bfd09583576d86e7521ca099bc844368d648f3f0b992` `agents/openai.yaml`
- Legacy collision: the prior project-specific runtime entry point was replaced intentionally; no Laravel, PHP, `routes/api.php`, Kafka, or Accurate assumptions remain in the three runtime files.
- Excluded-tests confirmation: `tests/` and migration documentation were not deployed; the complete runtime inventory contains no test files.
- Final source validation: all four paired manifest entries returned `OK`, `SKILL.md` is 899 words, and `git diff --check` is clean.
- The final marker below is supported by the paired `8/8` narrow and `8/8` full-contract result and the validated manifest.

## Final Marker

`MANUAL-GREEN-8/8-PASS`
