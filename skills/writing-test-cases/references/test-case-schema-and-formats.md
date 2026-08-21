# Test Case Schema And Formats

## Output Modes

Artifact mode creates requested test cases. Unless the user requests another format, draft and validate Markdown first, convert it to XLSX, and return both. A triage-, classification-, or planning-only response has `Deliverable: Not Applicable` when no artifact is requested. BDD/Gherkin is only on explicit request. Karate is not a universal output; Karate-specific executable design and execution belong to Karate specialists.

## Markdown Canonical Format

Markdown is the reviewable, version-controlled source of truth. Use the following detail fields in exactly this order:

`No`, `ID`, `Group`, `Title`, `Priority`, `Type`, `Preconditions`, `Test Steps`, `Expected Result`, `Actual Result`, `Test Data`, `Notes`, `Status`, `Evidence`, `Date`

Use stable, unique IDs that remain unchanged across format conversion. Keep one case per row or clearly delimited detail record. Expected results must be specific and measurable. New cases use `Status: Not Run`; `Actual Result`, `Evidence`, and `Date` stay blank until execution. Missing facts are `Not Provided` or recorded as assumptions, never invented.

## XLSX Workbook Contract

The workbook is derived from the validated Markdown source. It contains one `Summary` sheet and one detail sheet per group. The Summary fields are exactly:

`Group`, `Case Count`, `Dominant Priority`, `Pass`, `Fail`, `Not Run`, `Status`, `Progress`, `Notes`

Each detail sheet uses the exact Markdown field order. Preserve stable IDs, group membership, priorities, all detail fields, and case status. Case-level `Status` is only `Pass`, `Fail`, or `Not Run`; new and unexecuted cases default to `Not Run`, with blank `Actual Result`, `Evidence`, and `Date`.

Use short, unique, Excel-safe sheet names: avoid reserved characters, excessive length, and ambiguous duplicates. When the workbook tool supports it, wrap long text, freeze detail headers, and apply filters. Use formulas for `Case Count`, `Pass`, `Fail`, `Not Run`, `Status`, and `Progress`: count detail rows; count detail `Status` values exactly; use this ordered Summary `Status` precedence exactly: `if Case Count = 0, Status = Not Started; otherwise, if all cases are Not Run, Status = Not Started; otherwise, if at least one case is executed and at least one remains Not Run, Status = In Progress; otherwise, Status = Completed.` Set `Progress` to `0%` for zero cases and otherwise `Pass / Case Count`. Do not convert blank execution fields into results or case status into report-level axes.

## Risk-Based Coverage

Coverage is conditional, not a mandatory checklist. Consider happy path, validation, authorization, security, edge, data integrity, integration, reliability, performance, contract, and end-to-end categories only when supported by the feature, current oracle, user scope, or material risk. Omit categories with no applicable behavior or risk and state why.

Severity describes consequence; priority describes test or remediation urgency. Assess priority from impact, exposure, dependency, urgency, and available workaround. A deadline may inform priority only with impact and exposure evidence; it does not automatically make a case P1 or change severity. Treat a supported blocker as a blocker, not as a product defect or a pass.

## Oracle And Traceability

Use oracle precedence in this order: current requirement or acceptance criterion first; then the current API, event, schema, or other contract; then maintained domain documentation. Implementation and configuration are behavior clues and can identify a risk, but do not override the first oracle automatically.

When sources conflict or their authority or recency cannot be established, record `Requirement Ambiguity`, preserve the competing statements, and avoid an invented expected result or product classification. Link each case to the relevant requirement or source where available, and keep the stable case ID through Markdown and XLSX.

## Status And Execution Semantics

Case-level `Status` values are only `Pass`, `Fail`, and `Not Run`. New or unexecuted cases use `Not Run`. A blocked attempted case remains `Not Run`; document the blocker in `Notes` and state the affected evidence scope. `Pass` means the executed case met its expected result. `Fail` means observed execution contradicted the expected result. Neither case value alone establishes `Product Behavior: Verified Failure: Product Defect`.

Report-level axes are separate:

- Deliverable: `Complete`, `Incomplete`, or `Not Applicable`.
- Product Behavior: `Verified Pass`, `Verified Failure: Product Defect`, `Unverified Due to Blocker`, or `Not Evaluated`.
- Lifecycle: report the workflow stage reached independently of case status and product behavior.
- Evidence Status: report the evidence scope and sufficiency independently of case status and product behavior.

Lifecycle and Evidence Status are report-level axes, not additional detail fields or workbook columns. Do not use either to replace the canonical Deliverable or Product Behavior values.

`Verified Pass` requires valid execution against a supported oracle. Authored cases, static review, workbook structure, or conversion do not prove Product Behavior. Without execution, use Product Behavior: `Not Evaluated`. For a conversion or execution blocker, keep affected case status `Not Run`, state the blocker in `Notes`, and state the evidence scope. Use report-level `Deliverable: Incomplete` for a missing requested workbook. Use `Product Behavior: Unverified Due to Blocker` only when an attempted product evaluation was blocked by a named test, data, environment, dependency, or other non-product cause. A report-only unexecuted task remains `Product Behavior: Not Evaluated`. Triage failures before using `Verified Failure: Product Defect`; an observed failing case alone is insufficient.

## Data Safety And Reproduction

Use synthetic, isolated, reusable fixtures and least-privilege test identities. Never place secrets or unsafe production data in Markdown, XLSX, logs, or evidence. Do not invent payloads, records, permissions, or environment facts; label gaps `Not Provided` and state assumptions. For destructive, costly, security-sensitive, flaky, or externally impactful behavior, use approved fixtures, read-only checks, test doubles, sandbox boundaries, or controlled fault injection.

## Markdown/XLSX Reconciliation

Treat Markdown as canonical and XLSX as derived. Before returning both, verify equal case count; exact, unique, stable ID sets; matching group membership and group counts; matching priorities; matching detail-field names, order, and values; and matching case statuses. Summary `Pass`, `Fail`, and `Not Run` must count detail case statuses only. Summary `Status` uses this ordered precedence exactly: `if Case Count = 0, Status = Not Started; otherwise, if all cases are Not Run, Status = Not Started; otherwise, if at least one case is executed and at least one remains Not Run, Status = In Progress; otherwise, Status = Completed.` Summary `Progress` is `0%` for zero cases and otherwise `Pass / Case Count`; a `Fail` is not a Product Behavior defect and case status must not be converted into report-level axes. Check that every Summary row maps to one group, every detail row maps to one source case, and default execution fields remain blank. Verify formulas reference the correct detail sheets and that sheet names are safe. Report any conversion or parity failure as an incomplete artifact scope rather than silently repairing or claiming parity.

## Format Selection Boundaries

Use Markdown plus XLSX by default for requested test-case artifacts. Honor an explicit Markdown-only or XLSX-only request when feasible, while preserving the canonical source and reporting any unavailable conversion. Do not default to BDD/Gherkin; produce it only when explicitly requested. Do not make Karate universal: when executable Karate coverage is requested or required by repository convention, defer framework-specific design and execution to Karate specialists rather than changing this generic authoring contract.
