# Hybrid QA Skills Standardization Design

**Date:** 2026-08-10
**Status:** Approved

## Problem

The global QA and testing skills contain useful techniques, but several still mix reusable QA practice with project- or stack-specific assumptions such as ABL, Accurate, Kafka, Laravel, Playwright, and Anata templates. Their discovery, evidence, completion, and reporting rules are also inconsistent.

The skills need a shared engineering standard without forcing every specialist to perform the same work. Planning skills, execution skills, artifact skills, and defect-reporting skills have different completion boundaries.

## Goal

Standardize all QA and testing skills around this operating model:

> The prompt defines WHAT. The skill defines HOW.

For an actionable request, the agent should complete all applicable internal steps with minimal prompting while never claiming product behavior beyond available execution evidence.

## Design Principles

1. Global skills contain reusable ways of working, not project business knowledge.
2. Requirements, current specifications, repository implementation/configuration, and maintained documentation remain the source of truth.
3. Current session context and existing documentation should be reused before any additional repository discovery.
4. Discovery is targeted to the task, not a mandatory full-repository crawl.
5. Project Context is an optional, on-demand optimization for repeated expensive discovery, not a default step or dependency.
6. All QA skills share quality guardrails, but each specialist performs only the steps applicable to its responsibility.
7. Deliverable completion and verified product behavior are reported separately.
8. No pass or product-defect claim is allowed without sufficient runtime evidence.

## Hybrid Architecture

The selected approach combines a shared `qa-engineering` foundation with independently usable Specialist Skills.

The standalone Git repository is the authoring source of truth:

```text
qa-agent-skills/
|-- docs/
|   |-- specs/
|   `-- plans/
`-- skills/
    `-- <skill-name>/
        |-- SKILL.md
        |-- references/
        |-- agents/
        `-- tests/
```

Source skills and their test evidence are authored and verified under `skills/<skill-name>`. The runtime directory `~/.agents/skills/<skill-name>` is a deployment destination only. After a source skill passes all of its tests, mirror only `SKILL.md`, `references/`, and `agents/` to runtime; retain `tests/` solely in this repository.

```text
                         User Prompt
                              |
                 +------------+------------+
                 |                         |
                 v                         v
          Broad QA Request          Specific QA Request
                 |                         |
                 v                         v
          qa-engineering            Specialist Skill
                 |                         |
                 |                  load common contract
                 +------------+------------+
                              |
                              v
                 Assess Available Knowledge
                              |
          +-------------------+-------------------+
          |                   |                   |
          v                   v                   v
 Current Session       Existing Project Docs    Relevant Repo
 Context               and Specifications       Sources
          |                   |                   |
          +-------------------+-------------------+
                              |
                              v
                     Targeted Discovery
                       only if needed
                              |
                              v
                    Specialist Workflow
                              |
                              v
                       Verify and Report
```

### qa-engineering Responsibilities

The foundational skill owns:

- the shared QA operating contract;
- broad-request classification and Specialist Skill routing;
- source-of-truth and targeted-discovery rules;
- shared evidence and status semantics;
- shared quality guardrails and Definition of Done;
- coordination and consolidation for multi-skill work.

It does not own:

- project business rules, endpoints, selectors, or credentials;
- framework-specific implementation details;
- organization-specific templates;
- specialist workflows already owned by a domain skill.

### Routing and Recursion

- A broad QA request may enter through `qa-engineering`, which selects only the necessary specialists.
- A specific request may enter directly through a Specialist Skill, which applies the common contract without requesting routing again.
- When `qa-engineering` dispatches a specialist, the specialist returns its result for consolidation and does not recursively invoke the orchestrator.
- Specialist Skills retain a concise fallback of non-negotiable rules so they remain safe and useful when invoked directly.

## Knowledge and Discovery Policy

### Knowledge Reuse Order

Before reading more repository content, the agent checks:

1. user-provided information and requirements in the active conversation;
2. relevant knowledge already present in the current session context;
3. existing repository documentation, specifications, runbooks, and indexes;
4. optional Project Context when it is already known or encountered in normal high-signal documentation discovery;
5. relevant code, configuration, and tests;
6. generic skill examples.

This order controls discovery efficiency. It does not change behavioral source-of-truth precedence.

The agent does not perform a separate repository-wide search merely to find Project Context. When a known or readily discovered context file is useful, it may be read before targeted code inspection so that it can narrow that inspection.

### Behavioral Source-of-Truth Precedence

1. Current requirement and acceptance criteria.
2. Current API, event, schema, or contract specification.
3. Current repository implementation and executable configuration.
4. Current maintained project documentation.
5. Optional Project Context.
6. Generic skill examples.

When sources conflict, the agent follows the higher-precedence source for the test oracle, reports the conflict, and does not invent a resolution when the intended behavior cannot be derived safely.

### Targeted Discovery

The agent inspects only facts needed for the current task, such as:

- requirement and expected behavior;
- relevant existing automation and repository conventions;
- framework/runtime versions that affect implementation;
- authentication and environment mechanisms;
- test-data and cleanup conventions;
- the narrowest relevant execution command.

A full-repository crawl requires a concrete reason.

## Project Context On Demand

`project-context` remains an independent generic utility. It does not automatically execute a Specialist Skill, and Specialist Skills do not automatically create or refresh it.

Project Context should be considered only when:

- the same expensive discovery is repeated across sessions;
- repository knowledge is large, fragmented, and repeatedly needed;
- multiple agents or skills need the same stable project map;
- the compression benefit exceeds bootstrap and maintenance cost.

It should not be created when current session context or existing documentation is already sufficient, the repository is simple, the task is one-off, or the facts change too quickly.

When explicitly requested, `project-context` first checks for a material documentation gap. If existing documentation already provides a concise, navigable map, the correct result may be to avoid creating duplicate context files.

If Project Context exists, it is used to narrow discovery. Only task-critical facts that may be stale are revalidated. The agent must not repeat full discovery merely because context was loaded.

## Common QA Contract

Every QA skill follows a simple six-step contract, applying only the steps relevant to its responsibility:

```text
Understand -> Inspect -> Design -> Perform -> Verify -> Report
```

### Understand

- Determine the requested outcome and completion boundary.
- Do not invent missing requirements or expected behavior.

### Inspect

- Reuse current context and existing high-signal documentation.
- Perform targeted discovery only where needed.

### Design

- Define applicable scope, risks, oracle, data, and evidence.

### Perform

- Create the artifact, implement automation, or execute testing according to the Specialist Skill.

### Verify

- Validate the artifact or execution result.
- Fix test or artifact defects within scope and verify again.

### Report

- State what was completed, what was actually executed, the evidence, blockers, unexecuted scope, and residual risks.

### Non-Negotiable Rules

- Do not invent expected behavior.
- Do not persist or expose secrets.
- Do not claim pass without successful execution evidence.
- Do not classify a product defect from code inspection alone.
- Do not equate a mock test with real external integration.
- Do not hide unexecuted scope.
- Do not weaken a valid assertion merely to make a test pass.

## Status Model

Deliverable status and product behavior status are separate axes.

### Deliverable Status

- `Complete`
- `Incomplete`
- `Not Applicable`

### Product Behavior Status

- `Verified Pass`
- `Verified Failure: Product Defect`
- `Unverified Due to Blocker`
- `Not Evaluated`

`Verified Failure: Product Defect` means the product behavior was validly exercised against a supported oracle and the observed behavior did not meet that oracle.

Problems encountered during an attempted verification are triaged separately as:

- Test/Automation Defect
- Test Data Issue
- Environment/Configuration Issue
- External Dependency Issue
- Requirement Ambiguity

These triage categories do not become final product failures by themselves. The agent fixes in-scope test defects and reruns. If a problem prevents valid product evaluation and cannot be resolved, the final product status is `Unverified Due to Blocker`. Only a supported, validly executed mismatch may be reported as `Verified Failure: Product Defect`.

Examples:

```text
Test plan created and validated
Deliverable: Complete
Product Behavior: Not Evaluated
```

```text
Automation implemented but the test environment is unavailable
Deliverable: Complete
Product Behavior: Unverified Due to Blocker
```

In this example, `Deliverable: Complete` is valid only when all requested implementation artifacts are present and every feasible non-runtime validation has passed. It does not waive the execution requirement: environment-dependent product verification remains explicitly blocked. If required artifacts or feasible validation are also unfinished, the deliverable is `Incomplete`.

## Specialist Boundaries

| Specialist Skill | Primary Responsibility | Completion Boundary |
|---|---|---|
| `shift-left-testing` | Requirement review, risk, testability, and acceptance criteria | Does not claim product pass before implementation is tested |
| `writing-test-cases` | Risk-based, traceable test-case design | Validates the artifact; new cases remain `Not Evaluated` |
| `test-planning` | Scope, strategy, environment, entry/exit criteria, and execution order | Validates the plan; does not determine release results |
| `test-data-management` | Isolated, safe, reusable, and rerunnable test data | Verifies the data mechanism, not business behavior |
| `bug-reporting` | Reproduction, triage, evidence, and actionable defect reporting | Confirms product defects only with sufficient oracle and runtime evidence |
| `manual-testing` | Structured and exploratory manual execution | Reports product behavior using execution evidence |
| `ui-testing` | UI automation design, implementation, execution, debugging, and evidence | Follows the repository framework; does not default universally to Playwright |
| `integration-testing` | Boundary, failure-mode, and cross-system verification | Reports the actual integration level and never promotes mocks to real E2E evidence |
| `regression-testing` | Impact analysis, suite selection, execution, and release evidence | Does not claim full regression when only a limited scope ran |
| `karate-crud-web-testing` | Karate-only CRUD web automation | Implements, executes, debugs, and verifies with real CRUD and authorization evidence where applicable |

The existing `karate-framework` skill remains the specialist authority for Karate API, contract, integration, mock, UI, visual, E2E, and performance automation.

## Integration and Framework Rules

Integration evidence must identify its actual level:

1. Mocked Component Integration
2. Contract Verification
3. Sandbox/Service Integration
4. Real End-to-End Integration

Framework-sensitive skills must inspect the repository's installed framework and version, preserve the existing toolchain unless change is requested, and treat generic examples as illustrative. Karate is preferred in a Karate-standardized repository; Playwright or another framework remains valid when established by the repository or justified by a proven capability need.

## Parallel Execution

Specialists may run in parallel only when their work is independent, does not edit the same files, does not share mutable test data or constrained environments, and does not depend on another specialist's output.

Read-only discovery of independent domains may run in parallel and then be consolidated. Dependent flows such as data setup, API creation, UI verification, and regression validation run sequentially.

## Project-Specific Knowledge Migration

For each project-specific statement found in a global skill:

1. Check whether it is relevant and supported by the current repository.
2. Remove it from the global skill when it is not reusable.
3. Do not copy it when existing repository documentation already covers it.
4. Add still-valid, stable knowledge to a suitable existing repository document only when a material gap exists.
5. Consider Project Context only when no suitable document exists and repeated discovery justifies the additional cache.

Specific migration expectations:

- remove ABL, Accurate, Kafka, and organization-specific paths from global rules;
- remove hardcoded credentials;
- remove Playwright as a universal default;
- remove Anata-branded templates from global output contracts;
- use generic fallback templates;
- keep stack-specific examples only when clearly optional and materially useful.

## Skill Testing

Skill testing evaluates whether the SOP causes an agent to make correct QA decisions. It is not application testing.

Each skill is changed through one verified cycle:

1. Give an isolated agent a realistic scenario using the current skill behavior.
2. Record an actual unsafe, incomplete, project-leaking, or inconsistent decision.
3. Make the smallest skill change that addresses the observed gap.
4. Run the same scenario with the updated skill.
5. Confirm the decision now meets the expected QA rule.
6. Add a variation only when needed to close a demonstrated loophole.

The checks focus on decisions rather than exact wording:

- correct skill routing;
- no unsupported pass or product-defect claim;
- correct mock-versus-real integration classification;
- no invented requirements;
- no project-specific leakage;
- correct artifact-versus-product status;
- completion of all applicable one-prompt workflow steps.

For example, if an environment is unavailable after automation is implemented, the expected result is `Deliverable: Complete` and `Product Behavior: Unverified Due to Blocker`, never `Verified Pass`.

## Rollout

Each skill completes baseline observation, minimal update, post-change verification, and loophole review before the next skill begins.

1. `qa-engineering`
2. `bug-reporting`
3. `writing-test-cases`
4. `test-data-management`
5. `manual-testing`
6. `ui-testing`
7. `karate-crud-web-testing`
8. `integration-testing`
9. `regression-testing`
10. `test-planning`
11. `shift-left-testing`

Afterward, `karate-framework` and `project-context` receive compatibility checks rather than automatic full rewrites. If a compatibility finding requires either skill to change, that modification must complete the same baseline-observation, minimal-change, and post-change verification cycle before final acceptance.

## Shared Definition of Done

All applicable conditions must be satisfied:

- requested scope and exclusions are clear;
- relevant sources were inspected without unnecessary rediscovery;
- assumptions and conflicts are recorded;
- output follows repository conventions or identifies an explicit fallback;
- tool/version compatibility is checked when relevant;
- test data is safe, isolated, and rerunnable when relevant;
- the artifact or automation is validated;
- discovered test or artifact defects are fixed and verified again;
- product claims do not exceed execution evidence;
- blockers are separated from product defects;
- unexecuted scope and residual risk are reported;
- no secrets or unsafe production data are introduced.

## Final Verification

After all individual skills pass their scenarios:

- check triggers and overlap across skills;
- check for recursive routing;
- check status and source-precedence consistency;
- confirm Project Context remains on-demand;
- scan for remaining ABL, Accurate, Anata, hardcoded credential, and unjustified framework assumptions;
- run broad multi-skill and direct-specialist scenarios;
- confirm each specialist remains independently usable;
- confirm `qa-engineering` can coordinate broad QA work without taking over specialist details.

## Acceptance Criteria

The standardization is complete when:

- all targeted global skills are project-agnostic;
- the revised Common QA Contract is applied consistently and only where applicable;
- Specialist Skill boundaries remain explicit;
- planning and artifact completion are not reported as product verification;
- mocked integration is not reported as real integration;
- current context and existing documentation are reused before new discovery;
- Project Context is neither a dependency nor a default step;
- project-specific knowledge does not leak through global skills;
- every modified skill has baseline and post-change decision evidence;
- final reporting identifies verified skills, unverified areas, and remaining blockers.
