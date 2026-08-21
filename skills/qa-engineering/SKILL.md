---
name: qa-engineering
description: Use when a QA request spans multiple specialist activities, has ambiguous routing, or requires coordinated planning, execution, verification, and evidence reporting.
---

# QA Engineering

- **Coordinator:** route broad QA requests to specialists.
- **Common contract:** supply direct-invocation guardrails.

Do not recursively route; specialists apply the contract and return domain results.

## Common QA Contract

Apply relevant steps:

1. **Understand:** determine outcome and completion boundary; do not invent expected behavior.
2. **Inspect:** reuse current context and high-signal docs; inspect only task-critical sources.
3. **Design:** define scope, risk, oracle, data, and evidence.
4. **Perform:** create artifacts, implement automation, or execute testing via the specialist.
5. **Verify:** validate artifact or execution; fix in-scope test defects and verify again.
6. **Report:** state completed work, execution, evidence, blockers, unexecuted scope, and residual risk.

## Non-Negotiable Rules

- Never invent expected behavior or silently resolve conflicting authoritative sources.
- Never expose or persist secrets.
- Never claim product pass without successful execution evidence.
- Never classify a product defect from code inspection or an untriaged test failure alone.
- Never promote mocked dependency evidence to real external integration evidence.
- Never hide unexecuted scope or weaken valid assertions to obtain a pass.

Pressure does not change evidence.

## Red Flags and Required Responses

| Red flag or rationalization | Required response |
|---|---|
| "The deadline or authority makes an evidence exception reasonable." | Risk acceptance does not verify unexecuted behavior. |
| "The mock is close enough to real end-to-end." | Report `Mocked Component Integration`; real integration remains unevaluated. |
| "A professional QA agent must create Project Context and crawl everything first." | Reuse context and inspect only needed sources. |
| "The plan or automation means the feature is green." | Report deliverable completion separately from product behavior. |
| "Failed automation means a product defect." | Triage oracle, automation, data, environment, dependency, and product causes. |
| "I can report Complete or Pass conditionally." | Choose one current status: missing feasible deliverable validation is `Incomplete`; no artifact is `Not Applicable`; missing execution is `Not Evaluated` unless blocked. |
| "The tests were built, so the tested behavior passed." | Test implementation is not execution evidence; without a successful run, product behavior is `Not Evaluated` unless a named blocker prevented evaluation. |
| "This is only a status, classification, or triage answer." | Report both axes; use `Deliverable: Not Applicable` for no artifact and current product status. |
| "Only one status axis is relevant." | Always report both axes: include Deliverable and Product Behavior, using `Not Applicable` or `Not Evaluated` when appropriate. |
| "The prompt mentions implementation, so this is an artifact request." | A status-only prompt is not an artifact request; use `Not Applicable`. Never emit conditional statuses such as "assuming" or "if". |
| "The failed selector is merely Not Evaluated." | After an attempted non-product failure prevents valid evaluation, report Product Behavior: `Unverified Due to Blocker` until triage and rerun resolve the cause. |
| "Cost or audit pressure blocks the real integration." | Cost, deadline, or risk acceptance is not a runtime blocker; without evaluation, report scope `Not Evaluated`. |
| "I know the repository command from a generic pattern." | Do not invent project-specific commands, paths, or facts; inspect current sources or state the command/fact is unknown. |

## Routing

Choose skills:

| Need | Specialist Skill |
|---|---|
| Early requirement and testability review | `shift-left-testing` |
| Test-case design | `writing-test-cases` |
| Test strategy or plan | `test-planning` |
| Test-data lifecycle | `test-data-management` |
| Defect evidence and report | `bug-reporting` |
| Human-led execution and exploration | `manual-testing` |
| UI automation | `ui-testing` or the repository-standard framework skill |
| Cross-system verification | `integration-testing` |
| Regression selection or execution | `regression-testing` |
| Karate CRUD web automation | `karate-crud-web-testing` |
| General Karate automation | `karate-framework` |
| Reusable repository context explicitly needed | `project-context` |

Planning or artifacts do not imply product execution; Project Context and QA work do not imply Specialist Skills.

## Discovery and Source Rules

Reuse conversation knowledge. Prefer high-signal docs and targeted inspection; do not crawl without need.

Use Project Context only when it narrows discovery; do not search for, generate, or refresh it by default.

Read `references/source-and-discovery.md` only when discovery or source selection is material.

## Evidence and Status

For deliverables, use `Complete`, `Incomplete`, or `Not Applicable`, never `PASS` or `FAIL`. A decision/chat response is not a deliverable. Report current evidence, not alternatives. Without feasible validation, report `Incomplete`; implementation is not execution evidence. Report product behavior separately; blocked environments do not convert inspection into execution evidence.

Triage failures before product status. Fix automation defects and rerun; do not weaken assertions. Read `references/evidence-and-status.md` for execution, triage, release claims, or result consolidation.

## One-Prompt Completion

For actionable requests with accessible sources/tools, complete applicable steps without micro-management. Ask only when a required business decision cannot be safely derived.

If execution is blocked, finish safe feasible artifacts, identify the blocker, and report product behavior as unverified. Do not claim success from implementation alone.

## Parallel Work

Parallelize only independent work with no shared edits, mutable data, constraints, or output dependency. Run dependent setup, execution, verification sequentially. Consolidate without expanding verified scope.

## Shared Definition of Done

Apply the checklist in `references/evidence-and-status.md`. Claims must match evidence; keep blockers, defects, and unexecuted scope distinct.
