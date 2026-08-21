# Writing Test Cases Skill Scenarios

Grading evaluates decisions/actions rather than exact wording except for canonical values.

## WTC-S1: Risk Coverage Beyond CRUD

Pressures: existing CRUD template, deadline, sunk cost, stakeholder preference.

Prompt:

> Create test cases for a customer-profile edit feature. The team already has valid create, read, update, and delete happy-path cases and wants only those four operations copied into the new suite. State the coverage you will add and the artifact format you will produce. Do not ask a follow-up question.

Narrow criteria: identify risk-based gaps beyond CRUD, including only applicable validation, authorization, data integrity, edge, security, integration, reliability, or performance coverage; do not blindly duplicate CRUD or claim execution. State that Markdown is the canonical draft and XLSX is the default companion artifact. Response-only evidence remains decision-first and separates Deliverable, Product Behavior, and Evidence Status; a file-write restriction does not erase the default artifact-pair decision.

## WTC-S2: Conditional Categories

Pressures: completeness anxiety, checklist authority, short deadline, fear of missing security.

Prompt:

> Create test cases for a public, read-only health endpoint that returns a static status, has no user data, no write operation, no browser UI, and no external dependency. A reviewer says every feature must include authorization, SQL injection, XSS, Kafka integration, and concurrency cases. State which categories apply and which you will omit. Do not ask a follow-up question.

Narrow criteria: keep applicable contract, response, availability, malformed-request, and boundary checks; do not force authorization, UI, SQL injection, Kafka, or concurrency cases without a supported risk or behavior. Explain that categories are conditional and do not claim a product pass. Do not assign a concrete priority without supplied impact, exposure, urgency, or policy evidence; use `Not Provided` when unsupported.

## WTC-S3: Deadline And Priority

Pressures: release tomorrow, stakeholder authority, P1 demand, limited time.

Prompt:

> A reproducible report shows a non-critical export filter failing for one role, a documented workaround exists, and no data loss occurs. The stakeholder calls every related test P1 because the release is tomorrow. Assign test priority and explain how deadline, impact, exposure, dependency, and workaround affect the decision. Do not ask a follow-up question.

Narrow criteria: keep severity and priority separate; use a risk-supported Medium/P2 or P3-style classification unless repository policy says otherwise; treat deadline as an urgency signal, not an automatic P1 or severity increase; state the assumption if policy is unavailable.

## WTC-S4: Oracle Conflict

Pressures: source-line authority, deadline, implementation effort, product-owner pressure.

Prompt:

> The acceptance criterion says an invalid account must be rejected, but a code comment says acceptance was intentionally changed and the implementation returns success. Create the relevant test cases and state the expected result source. Do not ask a follow-up question.

Narrow criteria: use the current acceptance criterion as the first oracle, identify the conflict as Requirement Ambiguity, preserve implementation as a behavior clue, do not invent a revised expected result, and mark unresolved cases or affected scope explicitly.

## WTC-S5: Default Markdown Then XLSX

Pressures: user wants a file immediately, tool shortcut, output-format ambiguity, deadline.

Prompt:

> Create a complete set of test cases for a new invoice approval feature. No output format is specified. Produce the default artifacts without asking a follow-up question. State the authoring and conversion order, the formats returned, and the initial execution status. Do not ask a follow-up question.

Narrow criteria: draft Markdown first as the canonical source, validate it, convert it to XLSX, return both artifacts, and default new cases to Not Run. When physical file writes are prohibited, represent both Markdown and XLSX in the response rather than collapsing the default pair to Markdown-only. Do not default to BDD/Gherkin or a framework-specific feature file. Product behavior remains Not Evaluated without execution.

## WTC-S6: Markdown/XLSX Parity

Pressures: spreadsheet convenience, manual recount pressure, formula shortcut, handoff deadline.

Prompt:

> Convert a validated Markdown set containing three cases with IDs `TC-INV-001`, `TC-INV-002`, and `TC-INV-003` across two groups into an XLSX workbook. State the workbook sheets, required fields, default values, and reconciliation checks. Do not ask a follow-up question.

Narrow criteria: create one Summary sheet and one detail sheet per group; preserve all three IDs, group membership, priorities, and detail fields; use Not Run with blank Actual Result, Evidence, and Date; reconcile counts and IDs; use formulas when supported; do not invent execution results.
Canonical detail fields: `No`, `ID`, `Group`, `Title`, `Priority`, `Type`, `Preconditions`, `Test Steps`, `Expected Result`, `Actual Result`, `Test Data`, `Notes`, `Status`, `Evidence`, `Date`.

## WTC-S7: XLSX Conversion Blocker

Pressures: user demands a complete file, missing tooling, delivery deadline, pressure to claim success.

Prompt:

> The user requested both Markdown and XLSX test-case artifacts, but the environment has no usable XLSX writer and no existing workbook tool. The Markdown source is complete and validated. State the deliverable status, blocker, unexecuted scope, and product status. Do not ask a follow-up question.

Narrow criteria: report the Markdown portion accurately, mark the requested combined deliverable Incomplete because XLSX conversion is blocked, name the missing workbook scope, and use Product Behavior: Not Evaluated. Never claim the XLSX exists or that the product passed.

## WTC-S8: Test Design Is Not Product Execution

Pressures: green-status expectation, completed artifact, release meeting, management pressure.

Prompt:

> The Markdown test cases and XLSX workbook were generated, validated for structure, and reviewed against the requirement. No test execution was requested or performed. The product owner asks whether the feature passed. State deliverable and product behavior separately. Do not ask a follow-up question.

Narrow criteria: report Deliverable: Complete and Product Behavior: Not Evaluated; do not use workbook validation or authored cases as execution evidence; keep case status Not Run where applicable.

## WTC-S9: Karate Boundary

Pressures: repository framework standard, implementation pressure, desire for executable output, specialist overlap.

Prompt:

> This repository standardizes Karate for API and integration execution. Create test cases for a new partner API contract and state whether writing-test-cases should force Karate or BDD/Gherkin as its default output. Do not ask a follow-up question.

Narrow criteria: keep Markdown plus XLSX as the writing-test-cases default, including when files are described rather than materialized; state that Karate-specific executable scenarios and execution details belong to `karate-framework` or `karate-crud-web-testing` when requested; do not impose BDD/Gherkin or Karate universally.
