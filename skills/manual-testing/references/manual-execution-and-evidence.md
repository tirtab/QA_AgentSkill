# Manual Execution And Evidence

Manual execution records what a person actually did and observed. Planning artifacts define intent; they do not prove Product Behavior.

## Session Planning

Record concrete fields before execution:

- Session identifier, date, start and end time, timezone, tester, participants, and requested deliverable.
- Objective, feature or behavior under review, in-scope checks, out-of-scope checks, risks, assumptions, and entry criteria.
- Build or version, environment identity, deployment state, feature flags, access state, and relevant capability availability.
- Data source, data classification, fixture or record identifiers, starting state, reset method, and cleanup owner.
- Mode: structured steps, exploratory charter, or both; selected checks and reason for risk-based inclusion or omission.
- Oracle references in precedence order, evidence channels, stop conditions, safe rerun boundary, and unexecuted scope.

Unknown fields are not guesses. Record `Not Provided` when information or capability was not supplied, or name the blocker when it prevents execution.

## Exploratory Charters

A charter should state its mission in one sentence, the question or goal, timebox, time allocation, scope boundary, risk hypotheses, starting state, allowed data, environment boundary, available capabilities, approach or tours, and oracle. During the session, make notes timestamped and reproducible: action, input or state change, expected result, actual observation, identifier, evidence channel, and deviation. Record pauses, blockers, follow-up ideas, reset steps, cleanup, and remaining scope. A charter permits adaptive investigation but not unstructured clicking or an implied pass.

## Technology And Environment Discovery

Discover only what is needed for the requested behavior:

1. Read the current requirement and maintained project documentation relevant to the feature, then identify conflicts or missing acceptance detail.
2. Identify actual surfaces: UI/browser, API/contract, database or persistence, messaging or asynchronous work, external services, security boundaries, performance targets, and supported compatibility matrix.
3. Confirm the supported build, runtime, deployment state, access, feature flags, reset or seed capability, logs, traces, audit records, and service doubles or sandboxes that are actually available.
4. Choose an evidence channel that can observe the behavior. Do not import a command, path, credential, browser, environment, or framework assumption from another project.

If a capability is absent from the supplied context, say `Not Provided`. If the capability exists but cannot be used, name the blocker and its effect on scope. No technology command is mandatory.

## Evidence Channels

Use channels conditionally and label their limits:

- UI/browser: redacted screenshots or video, visible state, navigation result, accessibility observation, and relevant console or network record.
- API/contract: request and response capture, status, headers, body, error, correlation identifier, and comparison with the current schema or contract.
- Database/persistence: approved read-only observation, before and after state, record identifier, transaction or audit evidence, and isolation result. Never expose sensitive values.
- Messaging/async: producer and consumer observations, message metadata, correlation identifiers, timestamps, retry or failure-route records, and eventual state. Use the project's documented wait or completion signal rather than a fixed delay.
- External service: request and response, service identifier, sandbox or double designation, correlation record, and downstream outcome.
- Security: authorized test observation, access decision, redacted response, audit record, and scope of authorization. Do not perform unapproved attack activity.
- Performance: approved measurement, target, workload description, timing samples, environment, and limitations. Do not infer performance from a single casual click.
- Compatibility: the tested supported platform, runtime, browser, or version, observed result, and untested matrix entries.

Every channel still needs scope, build and environment, data, steps, expected and actual results, timestamp, and a retrievable evidence reference. `Not Provided` or a named blocker is preferable to invented telemetry.

## Oracle And Result Comparison

Apply oracle precedence in this order: current requirement or acceptance criterion; current API, event, schema, or contract; maintained project documentation. A lower-precedence source cannot silently override a higher-precedence source. If sources conflict, record the conflict and avoid an unsupported conclusion.

Examples:

- Acceptance criterion requires an invalid account to receive HTTP 400. A captured execution shows HTTP 400 with the relevant build and data, so `Product Behavior: Verified Pass`.
- The same valid execution shows HTTP 500 or accepts the invalid account. After triage excludes setup, dependency, and oracle ambiguity, report `Product Behavior: Verified Failure: Product Defect`.
- The submit control cannot be reached because the page target, access, or environment is unavailable. The attempted evaluation is `Product Behavior: Unverified Due to Blocker`, with the named blocker.
- A checklist or plan exists but no step ran. Report `Product Behavior: Not Evaluated`; the document is not execution evidence.

A prompt asking only to state, choose, or explain a session is not a requested report artifact: use `Deliverable: Not Applicable`, even when the answer is complete. `Complete` applies only to a requested report whose feasible validation is complete.

Report the axes separately. For example, a complete report with no run can be `Deliverable: Complete`, `Product Behavior: Not Evaluated`, and `Evidence Status: Not Provided`. A partial blocked report can be `Deliverable: Incomplete`, `Product Behavior: Unverified Due to Blocker`, and `Evidence Status: Blocked`. `Complete` describes the requested report, not product success. `Not Applicable` is for a deliverable genuinely outside the requested scope.

## Failure Triage

Preserve the original observation and classify before assigning a product defect:

- **Product behavior:** reproducible mismatch in the expected product state with a valid oracle.
- **Requirement or oracle:** ambiguity, conflict, stale source, or missing expected result.
- **Environment, access, or deployment:** unavailable build, permission, feature flag, target, or service.
- **Data or setup:** invalid fixture, wrong state, contamination, or missing precondition.
- **Tool, selector, or evidence channel:** unavailable target, capture failure, or observation limitation.
- **Dependency or external service:** partner outage, unsuitable double, contract mismatch, or downstream failure.
- **Timing or async:** race, timeout, eventual state, retry, or missing completion signal.
- **Safety or observability:** operation cannot be authorized safely, or logs and audit evidence are insufficient.

Capture the last known precondition, rerun only within the approved safe boundary, and compare with a controlled alternative when available. A missing selector, command, service, or data fixture alone does not justify `Verified Failure: Product Defect`.

## Safety And Integration Boundaries

Use synthetic, disposable, or test-owned data in an isolated approved boundary. Use approved masked non-production data only when its handling is authorized. Never copy unsafe production data, reveal credentials, or mutate shared records for convenience. For destructive behavior, obtain approval, use reversible or disposable fixtures, execute only the bounded number of runs needed, record identifiers, and clean records, queues, jobs, webhooks, audit artifacts, and other side effects. Verify cleanup; if cleanup cannot be performed, name the blocker and residual risk.

Label integration evidence exactly as follows:

- `Mocked Component Integration`: a controlled double verifies local integration logic only.
- `Contract Verification`: a request, response, event, or schema is checked against the current contract or fixture; this does not prove a live service.
- `Sandbox/Service Integration`: an approved sandbox or service is actually exercised with safe non-production data.
- `Real End-to-End Integration`: the complete flow is executed through the real authorized systems.

Use the strongest label supported by actual evidence, never by intent. Missing sandbox access is `Not Provided` or a named blocker, not live integration. Manual-testing owns observation and comparison; `writing-test-cases`, `test-data-management`, `ui-testing`, `integration-testing`, `regression-testing`, `test-planning`, and `bug-reporting` retain their specialist responsibilities.

## Session Report Checklist

- Session metadata, objective, requested deliverable, in-scope and out-of-scope behavior.
- Risks, entry criteria, build, environment, access, feature flags, and capability discovery.
- Data source, classification, identifiers, starting state, safe boundary, reset, cleanup owner, and cleanup result.
- Structured steps or exploratory charter, timebox, approach, deviations, and unexecuted scope.
- Oracle sources in precedence order, conflicts, assumptions, and applicable or omitted technology categories.
- For each evaluated check: expected result, actual result, timestamp, evidence channel, reference, and redactions.
- Separate `Deliverable`, `Product Behavior`, and `Evidence Status`, using the exact product and deliverable values.
- Failure triage category, preserved evidence, rerun boundary, named blocker, residual risk, and defect disposition.
- Exact integration evidence level when integration was involved.
- Specialist handoff, if needed, without claiming work another skill did not perform.
- No invented commands, paths, credentials, environments, evidence, or unsupported product claims.
