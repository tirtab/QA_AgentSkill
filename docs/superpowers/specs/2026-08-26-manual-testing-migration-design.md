# Manual Testing Migration Design

**Date:** 2026-08-26
**Status:** Approved; written-spec review pending

## Goal

Migrate `manual-testing` from a Laravel/PHP- and project-specific checklist into a reusable manual-execution skill. The skill remains project-agnostic while staying technology-aware: it discovers the active project's supported surfaces, tools, environments, data capabilities, and oracles, then applies only the checks that are relevant and supported.

Manual execution evidence records what was observed during a test session. A requirement, acceptance criterion, API contract, schema, or maintained behavior contract supplies the expected-result oracle. `Product Behavior` is determined by comparing valid execution evidence with that oracle; the requirement alone never establishes a product status.

## Scope Boundary

This migration changes only `manual-testing`. It does not modify `qa-engineering`, `bug-reporting`, `writing-test-cases`, `test-data-management`, `ui-testing`, `integration-testing`, or any other specialist skill.

The source repository owns `SKILL.md`, one direct reference, runtime metadata, and repository-only RED/GREEN/refactor evidence. Runtime receives only `SKILL.md`, `references/`, and `agents/`; it never receives `tests/` or migration documentation.

## Core Interpretation

Project-agnostic does not mean technology-blind.

- Do not assume an organization, domain, application, repository layout, environment name, credential, database, browser, framework, or command.
- Use technology categories such as UI, API, database, messaging, browser/device, and external service only as conditional test surfaces.
- Inspect current project documentation and supported toolchains before selecting a command, client, browser, environment, or evidence channel.
- Preserve a repository's established framework and toolchain when the current task requires it; route framework-specific automation to its specialist skill.
- If a capability, environment, tool, route, credential, or oracle is absent, report `Not Provided` or a named blocker instead of inventing it.

## Manual Testing Responsibility

`manual-testing` is responsible for human-led, structured, and exploratory execution against an identified product scope. It can use a browser, API client, command-line tool, logs, database viewer, message inspector, or other supported technology, but it does not make any one tool universal.

The skill owns:

1. Session scope, risk, entry conditions, and execution order.
2. Selection of applicable happy-path, validation, edge, security, integration, exploratory, compatibility, and performance checks.
3. Safe execution and capture of actual observations.
4. Evidence reconciliation against the selected oracle.
5. Session reporting, blocker classification, residual risk, and routing to the appropriate specialist.

The skill does not own:

- test-case authoring, which belongs to `writing-test-cases`;
- test-data lifecycle, which belongs to `test-data-management`;
- UI automation implementation, which belongs to `ui-testing` or the repository-standard framework;
- cross-system verification, which belongs to `integration-testing`;
- regression suite selection, which belongs to `regression-testing`;
- formal defect report creation, which belongs to `bug-reporting`.

## Workflow

1. **Understand:** identify the requested session outcome, feature scope, acceptance criteria, risk, user impact, environment, and requested report.
2. **Inspect:** inspect only task-critical requirements, routes or screens, API/contracts, configuration, test conventions, data capabilities, dependencies, and available tools. Do not crawl the entire project by default.
3. **Plan:** define entry criteria, data ownership, safe boundaries, applicable checks, timebox, oracle, evidence channels, and unexecuted scope.
4. **Prepare:** verify the supported environment and test data. Reuse `test-data-management` for isolated, synthetic, deterministic data. Stop or narrow the session when a required capability is unavailable.
5. **Execute:** run structured checks and exploratory charters. Use conditional technology checks only when the behavior, risk, or requirement supports them. Preserve exact steps, actual results, timestamps, build/environment, and evidence references.
6. **Triage:** classify failures as Test/Automation Defect, Test Data Issue, Environment/Configuration Issue, External Dependency Issue, Requirement Ambiguity, or Product Defect. Fix in-scope test issues and rerun; do not weaken assertions or repeat unsafe actions for a count.
7. **Report:** state the session deliverable, execution scope, oracle, evidence, blockers, unexecuted coverage, residual risk, and Product Behavior status separately.

## Conditional Technology Checks

Select checks from the supported product surface rather than from a mandatory checklist.

- **UI or browser:** use the supported browser/device and repository conventions when a UI exists. Cross-browser, responsive, accessibility, and visual checks require an applicable risk or requirement.
- **API or contract:** use the supported client and current API/contract. Capture request, response, status, headers, body, correlation identifiers, and relevant logs when available.
- **Database or persistence:** inspect state only through an approved read-only or controlled boundary. Do not assume table names, ORM commands, or direct production access.
- **Messaging or asynchronous behavior:** verify publication, consumption, retries, ordering, and failure handling only when the project exposes that behavior and a supported observer or sandbox exists.
- **External services:** identify the actual evidence level as `Mocked Component Integration`, `Contract Verification`, `Sandbox/Service Integration`, or `Real End-to-End Integration`. A mock never proves live external behavior.
- **Security, performance, and compatibility:** include them when supported by the feature, oracle, risk, or request. Do not force SQL injection, XSS, CSRF, load, or multi-browser checks without applicable behavior or a safe test boundary.

## Session Artifact And Status Contract

When a session report or test summary is requested, include scope, environment/build, data and boundary, entry criteria, timebox, executed checks, expected result oracle, actual result, evidence, blocker/triage classification, unexecuted scope, and residual risk.

Report these axes independently:

- `Deliverable: Complete` only when the requested report or session artifact exists and feasible validation passed.
- `Deliverable: Incomplete` when requested content or feasible validation remains missing.
- `Deliverable: Not Applicable` for a decision, classification, or planning-only response without a requested artifact.
- `Product Behavior: Verified Pass` only when valid manual execution met a supported oracle.
- `Product Behavior: Verified Failure: Product Defect` only after valid execution contradicted the oracle and test, data, environment, dependency, and requirement causes were reasonably excluded.
- `Product Behavior: Unverified Due to Blocker` only when an attempted evaluation was prevented by a named blocker.
- `Product Behavior: Not Evaluated` when the product scope was not executed and no attempted evaluation was blocked.
- `Evidence Status` describes evidence scope and strength separately from lifecycle, deliverable, and product behavior.

Authored plans, checklists, screenshots without a comparable oracle, and test implementation do not establish a product pass. A historical failure or an untriaged manual observation does not establish a product defect.

## Safety And Defect Routing

Use synthetic or test-owned data and the narrowest approved environment. Prefer read-only checks, controlled doubles, sandbox boundaries, or fault injection for destructive, costly, security-sensitive, flaky, or externally visible behavior. Do not repeat live destructive actions to satisfy a requested count.

Use `bug-reporting` when a formal defect artifact is requested. Include an explicit expected-result oracle and actual result. A selector, data, environment, dependency, or tool failure is triaged before any Product Defect status is assigned.

## Verification Design

The migration uses isolated decision-based skill tests, not application tests. The stable scenario set will cover:

1. Structured session planning versus click-only execution.
2. Conditional coverage instead of universal security, browser, or performance checklists.
3. Technology and environment discovery without invented project commands.
4. Exploratory charter, timebox, oracle, and reproducible notes.
5. Execution evidence compared with a requirement oracle and correct status axes.
6. Selector, tool, data, environment, and dependency failure triage.
7. Mock, contract, sandbox, and real integration evidence boundaries.
8. Safe handling of destructive or sensitive manual checks.

Each scenario is graded against its narrow criterion and the applicable Common QA Contract. The evidence record preserves exact prompts, pressure labels, raw outputs, task handles, loaded files, grading, source hashes, final counts, and deployment equality.

## Acceptance Criteria

- The runtime is project-agnostic and technology-aware without hardcoded project commands or mandatory stack assumptions.
- Manual testing remains human-led and distinct from test-case authoring, test planning, data management, automation, integration, regression, and defect reporting.
- Technology checks are conditional on supported capabilities, risk, requirements, and safe evidence boundaries.
- Oracle precedence, execution evidence, triage, and Product Behavior status are explicit and separate.
- No product pass or defect claim is emitted without valid execution and an applicable oracle.
- RED/GREEN/refactor evidence passes for all finalized scenarios.
- Source/runtime files match exactly, and no test evidence is deployed.
- No other skill is modified.

## Out Of Scope

- Executing a real product test session during skill migration.
- Implementing UI, API, integration, Karate, or performance automation.
- Rewriting `bug-reporting`, `test-data-management`, or other specialist skills.
- Adding project-specific commands, credentials, routes, data models, or environment assumptions.
- Building a universal manual-testing tool or test management system.
