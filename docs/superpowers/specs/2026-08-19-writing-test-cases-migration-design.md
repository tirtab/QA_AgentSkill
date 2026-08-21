# Writing Test Cases Migration Design

**Date:** 2026-08-19
**Status:** Proposed for review

## Goal

Migrate `writing-test-cases` into a project-agnostic, risk-based skill that creates reviewable Markdown test cases and, by default, a matching XLSX workbook. The skill must improve test design without claiming that product behavior passed merely because test cases were authored.

## Scope

The migration changes only `writing-test-cases`. It keeps the skill independently usable and follows the approved hybrid QA architecture:

- `skills/writing-test-cases/SKILL.md` is a concise entry point.
- `skills/writing-test-cases/references/test-case-schema-and-formats.md` contains detailed schema, coverage, risk, Markdown, and XLSX rules.
- `skills/writing-test-cases/agents/openai.yaml` contains runtime metadata.
- `skills/writing-test-cases/tests/` retains baseline, paired verification, regression, and manifest evidence.

The skill contains no ABL, Accurate, Kafka, Laravel, PHP, Anata, hardcoded credentials, organization-specific workbook layout, or universal framework assumption. Karate execution remains owned by `karate-framework` and `karate-crud-web-testing`.

## Authoring And Output

The default workflow is:

1. Understand the requested feature, acceptance criteria, contract, risks, scope, and requested artifact.
2. Inspect only relevant requirements, maintained documentation, existing test conventions, configuration, and execution boundaries.
3. Design a risk-based test matrix with stable IDs, applicable categories, priorities, data, and expected outcomes.
4. Draft the complete Markdown test-case artifact as the canonical source of truth.
5. Validate coverage, traceability, field completeness, duplicate IDs, assumptions, and unexecuted scope.
6. Convert the validated Markdown to an XLSX workbook.
7. Reconcile Markdown and XLSX for case count, IDs, groups, priorities, status, and detail fields.
8. Return both Markdown and XLSX unless the user explicitly requests a different final format.

Markdown is the reviewable/version-controlled source. XLSX is the operational artifact. If XLSX tooling is unavailable, the agent reports the conversion blocker rather than claiming the workbook is complete. A valid OOXML writer or an existing repository-supported tool may be used; no framework or vendor is assumed.

BDD/Gherkin is not a default output and is not a required migration surface. It may be produced only when explicitly requested. Karate-specific executable design is delegated to the Karate specialist when the repository or request requires it.

## Generic Test-Case Schema

### Markdown

The default Markdown table uses the same generic detail fields as the workbook:

`No`, `ID`, `Group`, `Title`, `Priority`, `Type`, `Preconditions`, `Test Steps`, `Expected Result`, `Actual Result`, `Test Data`, `Notes`, `Status`, `Evidence`, `Date`.

New cases default to `Status: Not Run`; `Actual Result`, `Evidence`, and `Date` remain blank until execution. Expected results must be specific and measurable. Unknown facts are labeled as assumptions or `Not Provided`, never invented.

### XLSX

The workbook contains:

- `Summary`: `Group`, `Case Count`, `Dominant Priority`, `Pass`, `Fail`, `Not Run`, `Status`, `Progress`, and `Notes`.
- One detail sheet per group or module using the exact Markdown detail-field order.

Summary counts use formulas when the supported tooling allows it. New cases use `Not Run`, blank actual results, blank evidence, and blank dates. Sheet names are short and Excel-safe. Long text is wrapped and detail headers are frozen when supported.

## Coverage And Risk

Coverage is risk-based and conditional. Happy path, validation, authorization, security, edge, integration, reliability, performance, contract, and end-to-end cases are added when applicable to the feature, required by the oracle, or justified by risk. No category is mandatory merely because it appears in a generic checklist.

Priority is separate from severity and is justified from impact, exposure, dependency, urgency, and scheduling. A deadline may inform priority when supported by evidence, but it does not determine severity or automatically make a case P1. A narrow, workaround-backed issue remains appropriately prioritized even when release is near.

The highest applicable expected-result oracle is the current requirement or acceptance criterion, followed by the current API, event, schema, or contract; implementation and configuration are behavior clues, not automatic oracles. When authority or recency is unresolved, record `Requirement Ambiguity` and do not invent expected behavior.

## Status Contract

Test-case authoring is an artifact task when the user requests cases or a workbook:

- `Deliverable: Complete` only when the requested Markdown and XLSX artifacts exist and feasible validation passes.
- `Deliverable: Incomplete` when a required artifact, conversion, or feasible validation is missing.
- `Deliverable: Not Applicable` for status, classification, or planning-only responses without a requested artifact.
- `Product Behavior: Not Evaluated` for newly authored cases without execution evidence.
- `Product Behavior: Unverified Due to Blocker` only when an attempted product evaluation was prevented by a named blocker.
- `Product Behavior: Verified Pass` requires valid execution against a supported oracle and is never inferred from test-case design or workbook creation.

The report must state assumptions, blockers, unexecuted scope, and residual risk separately from artifact completion. Test-case status such as `Not Run` is not product status.

## Safety And Data

Use isolated, reusable, non-secret test data. Do not copy credentials or unsafe production data into Markdown or XLSX. For destructive, costly, security-sensitive, flaky, or externally impactful behavior, design controlled evidence, test doubles, read-only checks, or approved fault injection rather than unconditional repetition.

## Verification Design

The migration uses fresh isolated agents and decision-based grading:

- Baseline RED runs occur before the new source skill exists and preserve raw outputs.
- GREEN runs use the same prompts plus a neutral instruction to load the canonical skill and direct reference.
- Refactor runs repeat the full scenario set after each demonstrated loophole fix.
- Paired source hashes freeze `SKILL.md`, the direct reference, `agents/openai.yaml`, and `tests/scenarios.md` before dispatch.
- Deployment mirrors only `SKILL.md`, `references/`, and `agents/`; `tests/` remains repository-only.

The scenario set must cover at least:

1. A CRUD-only request that requires risk-based, not operation-only, coverage.
2. A feature where security or integration coverage is not applicable and must not be forced.
3. Deadline pressure that must not distort severity or priority.
4. A request with missing or conflicting oracle information.
5. Default Markdown-first output followed by XLSX generation.
6. Markdown/XLSX parity, required fields, defaults, formulas, and stable IDs.
7. Missing XLSX tooling or a conversion failure that must remain an explicit blocker.
8. Test-case completion without execution, requiring `Product Behavior: Not Evaluated`.
9. A Karate repository context where framework-specific execution is delegated rather than made a universal default.

Each scenario is graded first against its narrow criterion and then against the applicable Common QA Contract. The final record must include exact prompts, pressure labels, raw output, task handles, model limitations, loaded files, criterion-level grading, extra-contract checks, source hashes, final counts, and deployment evidence.

## Acceptance Criteria

The migration is accepted only when:

- the source is project-agnostic and concise;
- Markdown is the canonical authored artifact and XLSX is generated and reconciled by default;
- no BDD/Gherkin or Karate framework is imposed without an explicit request or repository convention;
- coverage is risk-based and categories are conditional;
- oracle precedence and ambiguity handling are explicit;
- severity, priority, test-case status, deliverable status, and product behavior remain separate;
- no unsupported product pass or invented requirement/evidence is emitted;
- RED/GREEN/refactor evidence passes for all finalized scenarios;
- source/runtime files match exactly;
- no test evidence files are deployed;
- the source repository is committed separately from runtime deployment.

## Out Of Scope

- Implementing product tests or executing the generated cases.
- Rewriting `karate-framework` or `karate-crud-web-testing`.
- Building a universal XLSX library outside the skill's generation guidance.
- Preserving Anata or other project-specific workbook compatibility.
