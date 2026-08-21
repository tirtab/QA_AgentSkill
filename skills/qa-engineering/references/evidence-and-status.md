# Evidence and Status Contract

## Two Status Axes

When reporting overall QA task status, report both axes, including for artifact-only and execution-only work.

**Deliverable:**

- `Complete`: all requested artifacts exist and every feasible validation passed.
- `Incomplete`: required artifacts or feasible validation remain unfinished.
- `Not Applicable`: no work product was requested.

Use `Complete`, `Incomplete`, or `Not Applicable` for deliverables, never `PASS` or `FAIL`.

A decision or chat response alone is not a deliverable. Report the status supported by current evidence, never conditional alternatives.
If the prompt asks only for a status, classification, or triage response, descriptions of existing implementation do not create an artifact request: use `Deliverable: Not Applicable`. Report one current status, never conditional alternatives such as "assuming" or "if".
For a status-only, classification-only, or triage-only request, the deliverable is `Not Applicable`; still report Product Behavior.
Implementation or static appearance alone does not prove deliverable completion. Cost, deadline, or risk acceptance is not an execution blocker; without attempted evaluation, use `Not Evaluated`.

If a requested deliverable's feasible validation is not evidenced, report `Incomplete`. Test implementation is not execution evidence.

**Product Behavior:**

- `Verified Pass`: valid execution met the supported oracle in an identified scope/environment.
- `Verified Failure: Product Defect`: valid execution contradicted the supported oracle after test, data, and environment causes were reasonably excluded.
- `Unverified Due to Blocker`: a named blocker prevented valid product evaluation.
- `Not Evaluated`: an identified product scope or evidence level was not executed, and no attempted evaluation was prevented by a blocker; this includes out-of-scope or explicitly unexecuted levels.

Use `Unverified Due to Blocker` only when a named blocker prevented evaluation; otherwise use `Not Evaluated` for unexecuted scope.

Without successful execution evidence, do not report `Verified Pass`; use `Not Evaluated` unless a named blocker prevented intended evaluation. Built or implemented tests do not count as successful execution.

## Failure Triage

Classify an attempted verification problem before assigning final product status:

- Test/Automation Defect
- Test Data Issue
- Environment/Configuration Issue
- External Dependency Issue
- Requirement Ambiguity
- Product Defect

Fix in-scope test defects and rerun. If a non-product issue remains and blocks a valid oracle, use `Unverified Due to Blocker`. An observed failing test is not automatically a product defect.

A selector, automation, data, environment, or dependency failure that prevents an attempted evaluation is `Unverified Due to Blocker` until triage and rerun; do not relabel it `Not Evaluated`.

## Evidence Minimum

State the scope, relevant environment/build, steps or command, observed result, and evidence location or summary. Report unexecuted scope explicitly.

Code inspection can support diagnosis but does not prove runtime pass or failure. A mock proves only the boundary represented by that mock. Name integration evidence exactly as `Mocked Component Integration`, `Contract Verification`, `Sandbox/Service Integration`, or `Real End-to-End Integration`.

## Examples

```text
Test plan created, reopened, and reviewed
Deliverable: Complete
Product Behavior: Not Evaluated
```

```text
Automation implemented and statically validated; environment unavailable
Deliverable: Complete
Product Behavior: Unverified Due to Blocker
```

The second deliverable is complete only when no requested artifact or feasible non-runtime validation remains. Environment-dependent execution is still blocked and must not be represented as pass evidence.

## Shared Definition of Done

- Scope and exclusions are explicit.
- Relevant sources were inspected without unnecessary rediscovery.
- Assumptions and conflicts are recorded.
- Output follows repository conventions or states a fallback.
- Tool/version compatibility is checked when material.
- Test data is safe and isolated when applicable.
- Artifacts or automation received every feasible validation.
- Test defects found during execution were fixed and rerun.
- Product claims do not exceed evidence.
- Blockers, product defects, unexecuted scope, and residual risk are distinct.
- No secrets or unsafe production data were introduced.
