---
name: writing-test-cases
description: Use when a QA engineer needs to design risk-based test cases and reviewable test-case artifacts from current requirements.
---

# Writing Test Cases

Design project-agnostic, reviewable test cases from current requirements, acceptance criteria, contracts, and repository context. Artifact mode creates the requested cases without treating authorship as product execution.

## Output Modes

- Artifact mode creates requested cases. Default: draft Markdown, convert to XLSX, return both.
- When creating or converting a test-case artifact, load `references/test-case-schema-and-formats.md` for the canonical 15-field Markdown order, Summary/detail workbook contract, defaults, and reconciliation rules.
- For a requested test-case artifact, the default pair is Markdown plus XLSX even when the response describes rather than materializes the files.
- Red flag: a repository framework convention is used to justify forcing Karate or replacing the canonical artifact format. Required response: preserve the user's explicit Markdown-only or XLSX-only request; otherwise use the default artifact mode, and route executable Karate scenarios, assertions, mocks, and runtime execution to the Karate specialist.
- Triage, classification, or planning-only responses are `Deliverable: Not Applicable` when no case artifact is requested.
- BDD/Gherkin is not default; produce it only on explicit request. Karate is not universal; framework-specific execution belongs to Karate specialists.
- Markdown is canonical. Reconcile the derived XLSX with it for count, IDs, groups, priorities, fields, and status. If conversion is unavailable, report the XLSX blocker and do not claim it exists.

## Workflow

1. **Understand:** Identify requested outcome, scope, acceptance criteria, risks, and artifact boundary.
2. **Inspect:** Perform targeted discovery of current requirements, maintained documentation, conventions, configuration, and execution boundaries; do not crawl without need.
3. **Design:** Build a risk-based matrix with stable IDs, applicable categories, priorities, safe data, preconditions, steps, and measurable expected results.
4. **Perform:** Draft Markdown, then convert validated source to XLSX when required.
5. **Verify:** Check coverage, oracle traceability, assumptions, duplicate IDs, field completeness, execution defaults, and Markdown/XLSX parity.
6. **Report:** Return artifacts and state Deliverable and Product Behavior separately, including blockers, unexecuted scope, assumptions, and residual risk.

Use no invented facts. Mark missing information as `Not Provided` or an assumption, and use safe, isolated, reusable test data.

## Coverage And Risk

Coverage is risk-based and conditional. Add happy path, validation, authorization, security, edge, integration, reliability, performance, contract, or end-to-end cases only when the feature, oracle, or risk makes them applicable. Do not force a checklist category.

Existing CRUD cases or a request for `only CRUD` do not establish other coverage as inapplicable; for an edit feature, explicitly assess validation, authorization, data integrity, edge, and security risks, then include applicable cases or state the scope conflict and omitted-risk decision.

Red flag: a request to copy only CRUD operations for an edit feature. Required response: do not treat the CRUD happy paths as complete risk coverage; assess validation, authorization, data-integrity, edge, and security risks, then include applicable cases or record the explicit scope conflict. Use the existing artifact-format rule for Markdown plus XLSX.

Red flag: a static, public, read-only endpoint is used to justify a concurrency or capacity case and no stated concurrency, shared-state, rate-limit, or performance requirement, supported behavior, oracle evidence, or material risk exists. Required response: omit concurrency and capacity coverage; retain contract, response, availability, malformed-request, and boundary checks that are supported by the stated behavior, and do not invent a concurrency risk.

The current requirement or acceptance criterion is the first oracle. A current API, event, schema, or contract may provide the next authority. Implementation is a behavior clue, not an automatic oracle. When authority or recency is unresolved, record `Requirement Ambiguity` and do not invent expected behavior.

Keep severity separate from test priority. A deadline may inform priority only with evidence of impact and exposure, plus relevant dependency, urgency, and workaround considerations; it does not automatically make a case P1.

## Status Contract

Deliverable and Product Behavior are separate axes. Use `Complete` only when requested artifacts and feasible validation are complete; use `Incomplete` for a missing artifact, conversion, or validation; use `Not Applicable` for triage, classification, or planning-only work. New cases are `Not Run` with blank `Actual Result`, `Evidence`, and `Date`.

Without execution evidence, Product Behavior is `Not Evaluated`. Use `Unverified Due to Blocker` only for a named blocker that prevented an attempted evaluation. Use `Verified Pass` only after valid execution against a supported oracle; never infer a Product Behavior pass from authored cases or workbook validation. A confirmed failure after triage may be `Verified Failure: Product Defect`.

A reproducible report used only for priority or classification does not establish current Product Behavior: Verified Failure: Product Defect. Keep Product Behavior: Not Evaluated unless current valid execution against a supported oracle is supplied; priority and evidence status remain separate.

## Safety And Data

Use isolated fixtures, synthetic values, approved test doubles, and controlled reproduction for destructive, costly, security-sensitive, flaky, or externally impactful behavior. Never include secrets or unsafe production data in artifacts. Do not expose credentials or copy sensitive records.

## Quality Gate

Before reporting completion, confirm that scope and exclusions are explicit, applicable risks are covered without invented behavior, every case has the canonical fields and a stable ID, expected results are specific and measurable, new execution fields are blank, and Markdown remains the source of truth. Reconcile XLSX count, IDs, groups, priorities, fields, and status. Report blockers and unexecuted product scope explicitly, and make no claim beyond the evidence.
