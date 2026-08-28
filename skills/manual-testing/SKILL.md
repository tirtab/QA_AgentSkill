---
name: manual-testing
description: Use when a tester must evaluate product behavior through human-led execution in a project with technology-aware, capability-conditional coverage and evidence.
---

# Manual Testing

Manual testing is human-led verification of product behavior. It is project-agnostic, technology-aware, and capability-conditional: discover context, choose supported checks, and never substitute assumptions for execution.

## Output Modes

- **Structured session:** plan scope, entry criteria, risks, data, oracle, steps, expected and actual results, evidence, and unexecuted scope.
- **Exploratory session:** use a focused charter with a goal, timebox, approach, risks, boundaries, oracle, and reproducible notes. Exploratory work is structured investigation, not unrecorded clicking.
- **Session report:** always separate `Deliverable`, `Product Behavior`, and `Evidence Status`.
  - `Deliverable`: `Complete`, `Incomplete`, or `Not Applicable`.
  - `Product Behavior`: `Verified Pass`, `Verified Failure: Product Defect`, `Unverified Due to Blocker`, or `Not Evaluated`.
  - `Evidence Status`: report independently whether evidence is available, partial, blocked, or `Not Provided`.
- A response-only guidance, classification, planning, or status request is not an artifact: use `Deliverable: Not Applicable`, even when the response is complete.
- **Detailed guidance:** load `references/manual-execution-and-evidence.md` for detailed session fields, evidence, oracle, triage, safety, and reporting rules.

## Responsibility And Boundaries

Own planning, human observation, evidence capture, oracle comparison, failure triage, and status reporting. Do not invent commands, paths, credentials, browsers, environments, or product facts. No tool, framework, or command is mandatory.

Route work directly when needed to `writing-test-cases` for case design, `test-data-management` for fixtures and lifecycle, `ui-testing` for browser or UI automation, `integration-testing` for service or asynchronous integration, `regression-testing` for suite selection, `test-planning` for broader plans, and `bug-reporting` for defect reports. Do not route back through manual-testing or create recursive routing.

## Workflow

1. Plan: objective, scope boundaries, risks, entry criteria, build and environment, data, oracle, capabilities, evidence channels, stop conditions, cleanup, and requested deliverable.
2. Discover relevant project sources and supported capabilities. Select structured or exploratory execution; prioritize coverage by risk and technology.
3. Execute with a human observer. Record each structured step or timestamped exploratory action with expected and actual results. Stop safely at a blocker or unsafe state.
4. Capture evidence as behavior occurs. Preserve identifiers and timestamps while redacting sensitive values.
5. Compare valid execution evidence with the oracle and assign the exact Product Behavior status. Product Behavior is the comparison of valid execution evidence with the oracle, never a conclusion from a requirement, plan, checklist, or charter alone.
6. For every selector/page-target failure, enumerate selector/automation, product/page, data, environment/access, dependency, timing, requirement, and observability causes with status; do not collapse into one blocker. Report the blocker and unexecuted scope, then rerun within the safe boundary before any Product Defect status.

## Conditional Technology Checks

After discovery, include only supported and risk-relevant checks:

- **UI/browser:** rendering, navigation, input, state transitions, accessibility, and supported browser or platform compatibility when a UI exists.
- **API/contract:** method, status, headers, body, errors, authentication boundaries, schema, and contract behavior when an API exists.
- **Database/persistence:** durable state, consistency, isolation, migrations, and audit state when persistence exists and approved observation is available.
- **Messaging/async:** publication, consumption, ordering, retries, idempotency, eventual state, and failure routing when message flows exist.
- **External-service:** sandbox, controlled double, contract, or authorized live interaction, clearly labeled by evidence level.
- **Security:** authorization, authentication, injection, CSRF, privacy, and secret handling only for an authorized attack surface.
- **Performance:** latency, throughput, concurrency, or resource behavior only with a defined target and safe capability.
- **Compatibility:** only the supported browser, platform, runtime, or version matrix relevant to the product.

If a surface is absent, mark its category `Not Applicable`. If a needed capability is unavailable, report `Not Provided` or a named blocker. Never turn omitted conditional coverage into a universal pass.

## Evidence And Oracle

Use this oracle precedence: current requirement or acceptance criterion; then current API, event, schema, or contract; then maintained project documentation. Record conflicts or ambiguity rather than choosing a convenient interpretation.

Execution evidence must identify scope, environment and build, test data, steps or charter actions, expected and actual results, timestamp, and evidence channel. Channels can include approved UI captures, request and response records, logs, traces, audit records, persistence observations, message metadata, or service correlation identifiers. A plan, requirement, checklist, or expected result is not execution evidence.

## Status And Safety

Use `Verified Pass` only when valid evidence meets the oracle. Use `Verified Failure: Product Defect` only when valid evidence contradicts the oracle and triage excludes a blocker, data/setup fault, dependency fault, and oracle ambiguity. Use `Unverified Due to Blocker` when an attempted evaluation is prevented. Use `Not Evaluated` when the scope was not attempted. Keep Product Behavior separate from `Deliverable` and Evidence Status.

Use synthetic, disposable, test-owned, or authorized masked non-production data. Do not use unsafe production data, expose credentials, mutate shared state without approval, or repeat destructive actions without authorization and isolation. Bound destructive runs, prefer reversible operations, capture evidence before cleanup, and verify cleanup of records, jobs, messages, and artifacts. Do not make unsupported product, release, or defect claims.

## Quality Gate

Before reporting, confirm scope, entry criteria, build and environment, data, oracle precedence, applicable technology checks, structured or exploratory records, expected versus actual results, timestamps, evidence channels, triage, safety, cleanup, blockers, and unexecuted scope. Report all three status axes with exact values. Mark the deliverable `Complete` only when requested reporting is complete; otherwise use `Incomplete` or `Not Applicable`. No Product Behavior pass is valid without execution evidence compared with the oracle.
