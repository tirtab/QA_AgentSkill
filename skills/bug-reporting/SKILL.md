---
name: bug-reporting
description: Use when a QA engineer needs to document suspected product behavior, distinguish defects from test or environment causes, or communicate evidence and risk for triage.
---

# Bug Reporting

Produce clear, complete, concise (3C) reports. Separate deliverable, evaluated behavior, evidence, and lifecycle.

## Output Modes

- **Artifact mode:** The user asks to create or update a formal bug report. Produce the full report schema in `references/report-schema-and-evidence.md` and end with the mandatory four-axis status block: `Deliverable`, `Product Behavior`, `Lifecycle`, and `Evidence Status`.
- **Triage mode:** For analysis, classification, evidence/safety plans, or defect justification, do not invent a formal report. Give the decision and evidence plan. Canonical axes are recommended when status is discussed, but the exact block is optional and concise prose is valid. If axes are given, use canonical values.
- **Existing report mode:** When the prompt states that a complete bug report already exists and asks only whether it is New/Fixed or whether the product passed, report `Deliverable: Complete`, `Lifecycle: New` (or documented equivalent), `Product Behavior: Not Evaluated` unless current execution occurred, and `Evidence Status` separately. Never use Failed, Not Passed, or a historical failure as current product status. Do not recreate the report; if axes are given, use canonical values.

## Status Axes

Artifact mode requires this exact four-line status block at the end of the response:

```text
Deliverable: <Complete | Incomplete | Not Applicable>
Product Behavior: <Verified Pass | Verified Failure: Product Defect | Unverified Due to Blocker | Not Evaluated>
Lifecycle: <documented or generic lifecycle>
Evidence Status: <scope and strength>
```

In Triage and Existing report modes, the exact block is optional. If provided, use all four labels and canonical values; ad hoc labels cannot replace axes. Concise Triage prose is valid.

## Status Contract

When reporting axes, use:

- **Deliverable:** `Complete`, `Incomplete`, or `Not Applicable`. Never use PASS/FAIL for a deliverable artifact.
- **Product Behavior:** `Verified Pass`, `Verified Failure: Product Defect`, `Unverified Due to Blocker`, or `Not Evaluated`.
- **Lifecycle:** the repository's documented status, or a generic mapped status such as `New`, `In Progress`, `Fixed`, or `Closed`.
- **Evidence Status:** a separate statement of evidence strength and scope. It is not lifecycle status.

In Artifact mode, or whenever a status block is provided, lifecycle or severity/priority prose cannot replace these axes.

A complete report is not a product pass; a prior failure, source inspection, or untriaged test failure cannot establish a product defect.

A selector, automation, data, environment, or dependency failure that prevents an attempted evaluation is `Product Behavior: Unverified Due to Blocker` until triage and rerun; do not relabel it `Not Evaluated`. A report-only task about a prior failure with no current execution is `Product Behavior: Not Evaluated`.

A status, classification, evidence plan, or Triage response without a requested artifact is `Deliverable: Not Applicable`. Use `Deliverable: Complete` only for a requested report artifact whose required fields are filled and validated; an existing report stated complete retains that status without implying recreation.

A reproducible historical failure in a completed report is not current Product Behavior: Failed or Verified Failure. If no new execution or fix verification occurred, report Product Behavior: Not Evaluated. Say the report is complete and the lifecycle is New/Open separately.

New / Ready for triage is not the four-axis final status. In Artifact mode use the exact block; in Triage and Existing report modes, use the canonical axes when provided, with `Lifecycle: New` and `Product Behavior: Not Evaluated`, or `Product Behavior: Unverified Due to Blocker` when blocked.

## Workflow

Apply only relevant steps, in order:

1. **Understand:** identify the request, user impact, acceptance criteria or contract, scope, and requested output.
2. **Inspect:** check requirements and relevant behavior, automation, data, environment, and dependencies. Use reference precedence; implementation/configuration is a clue, never the oracle.
3. **Design:** choose safe evidence and the expected-result oracle. Unresolved authority or recency is `Requirement Ambiguity`, not a product defect.
4. **Perform:** run applicable checks and match evidence channels to claims. Source clues are optional when black-box evidence suffices; never invent evidence.
5. **Verify:** compare actual behavior with the oracle, preserve exact unverified scope, and classify the cause. An unavailable external dependency is `External Dependency Issue` and requires `Product Behavior: Unverified Due to Blocker` unless a valid product mismatch is independently proven.
6. **Report:** Artifact mode fills the reference; Triage and Existing report modes give only the requested decision/status and never invent or recreate an artifact.

## Safety And Risk

Do not repeat destructive, costly, security-sensitive, flaky, or externally impactful actions merely to meet a count. Prefer a sandbox, test doubles, read-only checks, or approved fault injection; name blockers, not defects.

Severity and priority are separate risk decisions. Use generic labels only as defaults; cite repository policy. Release date alone must not make a narrow, workaround-backed issue P1.

Use documented lifecycle mappings when available; otherwise use generic lifecycle. Never infer `Fixed` or `Closed` before verification; `Confirmed` is workflow state, not product verification.

Confirmed evidence or a reproduced report does not by itself establish `Product Behavior: Verified Failure: Product Defect`. That status requires an explicit expected-result oracle and valid execution mismatch; otherwise use `Not Evaluated` or `Unverified Due to Blocker` and label the issue suspected.

## Quality Gate

Before publishing, confirm the named oracle, safe reproducibility, distinct expected/actual results, appropriate evidence, explicit unexecuted scope, supported classification, independent severity/priority rationales, and no unsupported claim or invented artifact.
