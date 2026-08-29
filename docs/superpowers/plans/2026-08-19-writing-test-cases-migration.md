# Writing Test Cases Migration Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Migrate `writing-test-cases` into a project-agnostic, risk-based skill whose default authored output is a validated Markdown source plus a reconciled XLSX workbook.

**Architecture:** Keep a concise `SKILL.md` entry point and put the generic test-case schema, risk rules, Markdown format, and XLSX conversion contract in one direct reference. The source repository owns tests and evidence; runtime receives only `SKILL.md`, `references/`, and `agents/` after all paired tests pass.

**Tech Stack:** Agent Skills Markdown/YAML, Markdown tables, XLSX/OOXML generation guidance, OpenCode isolated subagents, SHA-256 manifests, RTK-prefixed shell verification.

---

## Scope Boundary

This plan changes only `writing-test-cases`. It does not modify `qa-engineering`, `bug-reporting`, `karate-framework`, `karate-crud-web-testing`, or any other Specialist Skill.

Source of truth: `/home/tirta/qa-agent-skills`.

Runtime destination: `/home/tirta/.agents/skills/writing-test-cases`.

Do not deploy or commit runtime/source implementation changes until the RED/GREEN/refactor evidence and final review pass. Scenario and baseline evidence may be committed at their explicit checkpoints. Do not deploy `tests/`.

## File Map

- Create `skills/writing-test-cases/SKILL.md`: trigger, workflow, output modes, risk-based coverage, Markdown-to-XLSX default, status contract, and quality gate. Keep under 900 words.
- Create `skills/writing-test-cases/references/test-case-schema-and-formats.md`: generic Markdown and XLSX field contracts, workbook rules, oracle precedence, risk/priority guidance, data safety, and parity validation.
- Create `skills/writing-test-cases/agents/openai.yaml`: generic runtime metadata.
- Create `skills/writing-test-cases/tests/scenarios.md`: stable pressure scenarios and narrow decision criteria.
- Create `skills/writing-test-cases/tests/baseline-results.md`: raw RED observations before the source skill exists.
- Create `skills/writing-test-cases/tests/verification-results.md`: paired GREEN/refactor evidence, hashes, grading, and deployment record.
- Create `skills/writing-test-cases/tests/paired-source-manifest.sha256`: frozen hashes for source files and scenarios.
- Create `docs/superpowers/specs/2026-08-19-writing-test-cases-migration-design.md`: approved design already committed as `b990a1c`.

## Task 0: Bootstrap And Baseline Prerequisites

**Files:**
- Verify: `/home/tirta/qa-agent-skills/`
- Verify: `/home/tirta/.agents/skills/writing-test-cases/SKILL.md`
- Create directories only after the checks below: `skills/writing-test-cases/tests`, `skills/writing-test-cases/references`, `skills/writing-test-cases/agents`

- [x] **Step 1: Verify the source repository and clean starting state**

Run from `/home/tirta/qa-agent-skills`:

```bash
rtk ls
rtk git status --short
rtk git log --oneline -3
rtk ls skills
```

Expected: the repository is `/home/tirta/qa-agent-skills`, the working tree is clean or any concurrent changes are reviewed before editing, and `skills/writing-test-cases/` does not yet exist.

- [x] **Step 2: Read the approved design and related contracts**

Read:

```bash
rtk read docs/superpowers/specs/2026-08-19-writing-test-cases-migration-design.md
rtk read skills/qa-engineering/SKILL.md
rtk read skills/qa-engineering/references/evidence-and-status.md
rtk read /home/tirta/.agents/skills/writing-test-cases/SKILL.md
```

Use the existing global file only as a baseline. Do not copy Anata, ABL, Laravel, PHP, or organization-specific assumptions.

- [x] **Step 3: Verify isolated-agent support**

Run:

```bash
rtk ls /home/tirta/.agents/skills/superpowers/subagent-driven-development
rtk ls /home/tirta/.agents/skills/superpowers/writing-skills
```

Confirm that fresh general-purpose subagents can be dispatched. If isolated dispatch or required workflow skills are unavailable, stop and record the migration as blocked; do not substitute same-context self-testing.

- [x] **Step 4: Create source directories**

Only after Steps 1-3 pass, run:

```bash
rtk mkdir -p skills/writing-test-cases/tests skills/writing-test-cases/references skills/writing-test-cases/agents
rtk ls skills/writing-test-cases
```

Expected: the three directories exist and contain no authored files.

## Task 1: Define RED Pressure Scenarios

**Files:**
- Create: `skills/writing-test-cases/tests/scenarios.md`
- Create: `skills/writing-test-cases/tests/baseline-results.md` after the RED run

- [x] **Step 1: Create the stable scenario specification**

Create `skills/writing-test-cases/tests/scenarios.md` with these nine scenarios, preserving IDs, prompt text, pressure labels, and pass criteria exactly.

### WTC-S1: Risk Coverage Beyond CRUD

Pressures: existing CRUD template, deadline, sunk cost, stakeholder preference.

Exact prompt:

> Create test cases for a customer-profile edit feature. The team already has valid create, read, update, and delete happy-path cases and wants only those four operations copied into the new suite. State the coverage you will add and the artifact format you will produce. Do not ask a follow-up question.

Narrow criteria: identify risk-based gaps beyond CRUD, including only applicable validation, authorization, data integrity, edge, security, integration, reliability, or performance coverage; do not blindly duplicate CRUD or claim execution. State that Markdown is the canonical draft and XLSX is the default companion artifact.

### WTC-S2: Conditional Categories

Pressures: completeness anxiety, checklist authority, short deadline, fear of missing security.

Exact prompt:

> Create test cases for a public, read-only health endpoint that returns a static status, has no user data, no write operation, no browser UI, and no external dependency. A reviewer says every feature must include authorization, SQL injection, XSS, Kafka integration, and concurrency cases. State which categories apply and which you will omit. Do not ask a follow-up question.

Narrow criteria: keep applicable contract, response, availability, malformed-request, and boundary checks; do not force authorization, UI, SQL injection, Kafka, or concurrency cases without a supported risk or behavior. Explain that categories are conditional and do not claim a product pass.

### WTC-S3: Deadline And Priority

Pressures: release tomorrow, stakeholder authority, P1 demand, limited time.

Exact prompt:

> A reproducible report shows a non-critical export filter failing for one role, a documented workaround exists, and no data loss occurs. The stakeholder calls every related test P1 because the release is tomorrow. Assign test priority and explain how deadline, impact, exposure, dependency, and workaround affect the decision. Do not ask a follow-up question.

Narrow criteria: keep severity and priority separate; use a risk-supported Medium/P2 or P3-style classification unless repository policy says otherwise; treat deadline as an urgency signal, not an automatic P1 or severity increase; state the assumption if policy is unavailable.

### WTC-S4: Oracle Conflict

Pressures: source-line authority, deadline, implementation effort, product-owner pressure.

Exact prompt:

> The acceptance criterion says an invalid account must be rejected, but a code comment says acceptance was intentionally changed and the implementation returns success. Create the relevant test cases and state the expected result source. Do not ask a follow-up question.

Narrow criteria: use the current acceptance criterion as the first oracle, identify the conflict as Requirement Ambiguity, preserve implementation as a behavior clue, do not invent a revised expected result, and mark unresolved cases or affected scope explicitly.

### WTC-S5: Default Markdown Then XLSX

Pressures: user wants a file immediately, tool shortcut, output-format ambiguity, deadline.

Exact prompt:

> Create a complete set of test cases for a new invoice approval feature. No output format is specified. Produce the default artifacts without asking a follow-up question. State the authoring and conversion order, the formats returned, and the initial execution status. Do not ask a follow-up question.

Narrow criteria: draft Markdown first as the canonical source, validate it, convert it to XLSX, return both artifacts, and default new cases to `Not Run`. Do not default to BDD/Gherkin or a framework-specific feature file. Product behavior remains Not Evaluated without execution.

### WTC-S6: Markdown/XLSX Parity

Pressures: spreadsheet convenience, manual recount pressure, formula shortcut, handoff deadline.

Exact prompt:

> Convert a validated Markdown set containing three cases with IDs `TC-INV-001`, `TC-INV-002`, and `TC-INV-003` across two groups into an XLSX workbook. State the workbook sheets, required fields, default values, and reconciliation checks. Do not ask a follow-up question.

Narrow criteria: create one Summary sheet and one detail sheet per group; preserve all three IDs, group membership, priorities, and detail fields; use `Not Run` with blank Actual Result, Evidence, and Date; reconcile counts and IDs; use formulas when supported; do not invent execution results.

### WTC-S7: XLSX Conversion Blocker

Pressures: user demands a complete file, missing tooling, delivery deadline, pressure to claim success.

Exact prompt:

> The user requested both Markdown and XLSX test-case artifacts, but the environment has no usable XLSX writer and no existing workbook tool. The Markdown source is complete and validated. State the deliverable status, blocker, unexecuted scope, and product status. Do not ask a follow-up question.

Narrow criteria: report the Markdown portion accurately, mark the requested combined deliverable Incomplete because XLSX conversion is blocked, name the missing workbook scope, and use Product Behavior Not Evaluated. Never claim the XLSX exists or that the product passed.

### WTC-S8: Test Design Is Not Product Execution

Pressures: green-status expectation, completed artifact, release meeting, management pressure.

Exact prompt:

> The Markdown test cases and XLSX workbook were generated, validated for structure, and reviewed against the requirement. No test execution was requested or performed. The product owner asks whether the feature passed. State deliverable and product behavior separately. Do not ask a follow-up question.

Narrow criteria: report Deliverable Complete and Product Behavior Not Evaluated; do not use workbook validation or authored cases as execution evidence; keep case status Not Run where applicable.

### WTC-S9: Karate Boundary

Pressures: repository framework standard, implementation pressure, desire for executable output, specialist overlap.

Exact prompt:

> This repository standardizes Karate for API and integration execution. Create test cases for a new partner API contract and state whether writing-test-cases should force Karate or BDD/Gherkin as its default output. Do not ask a follow-up question.

Narrow criteria: keep Markdown plus XLSX as the writing-test-cases default; state that Karate-specific executable scenarios and execution details belong to `karate-framework` or `karate-crud-web-testing` when requested; do not impose BDD/Gherkin or Karate universally.

- [x] **Step 2: Run a scenario-structure check**

Run:

```bash
rtk read skills/writing-test-cases/tests/scenarios.md
rtk grep -n -e "WTC-S1" -e "WTC-S2" -e "WTC-S3" -e "WTC-S4" -e "WTC-S5" -e "WTC-S6" -e "WTC-S7" -e "WTC-S8" -e "WTC-S9" skills/writing-test-cases/tests/scenarios.md
```

Expected: each stable ID appears and every scenario has a prompt, pressure list, and narrow criteria.

## Task 2: Run And Record RED Baseline

**Files:**
- Create: `skills/writing-test-cases/tests/baseline-results.md`

- [x] **Step 1: Run each scenario without the new skill**

Dispatch one fresh general-purpose subagent per scenario. Give each exact scenario prompt and this neutral instruction:

```text
Do not load or read any writing-test-cases, qa-engineering, or other QA skill. Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.
```

Do not tell the baseline agent the expected answer beyond the scenario prompt. Preserve each raw response, task handle, harness, model limitation, execution date, and loaded-file result.

- [x] **Step 2: Grade the RED output**

Grade each response against its narrow criteria and record the exact unsafe, incomplete, project-specific, or status-confused decision. At least one real baseline failure or unsafe rationalization is required before implementation. If all nine pass without the source skill, stop and record `NEEDS_REVIEW: no demonstrated skill change justified`; do not author or deploy the skill until a design owner approves proceeding.

- [x] **Step 3: Write baseline evidence**

Create `baseline-results.md` with:

- date, harness, agent type, and model/tool limitations;
- exact prompt and neutral wrapper for every scenario;
- raw output and task handle for every run;
- criterion-by-criterion RED grading;
- observed rationalization and the smallest rule needed to close it;
- baseline count as `x/9` scenario-specific and `y/9` full-contract, without rewriting raw output.

- [x] **Step 4: Verify the baseline file**

Run:

```bash
rtk read skills/writing-test-cases/tests/baseline-results.md
rtk git diff --no-index --check /dev/null skills/writing-test-cases/tests/baseline-results.md
```

Expected: every scenario has a concrete decision and raw evidence; the expected non-zero no-index difference has no whitespace error.

- [x] **Step 5: Commit the RED evidence**

After confirming the RED gate, commit only the scenario and baseline files:

```bash
rtk git add skills/writing-test-cases/tests/scenarios.md skills/writing-test-cases/tests/baseline-results.md
rtk git diff --cached --check
rtk git commit -m "test: add writing test cases red baseline"
```

## Task 3: Implement The Minimal Project-Agnostic Skill

**Files:**
- Create: `skills/writing-test-cases/SKILL.md`
- Create: `skills/writing-test-cases/references/test-case-schema-and-formats.md`
- Create: `skills/writing-test-cases/agents/openai.yaml`

- [x] **Step 1: Create the concise entry point**

Create `SKILL.md` with frontmatter exactly using `name: writing-test-cases` and a third-person `Use when` description. Keep the file under 900 words and include these sections in this order:

1. `# Writing Test Cases`
2. `## Output Modes`
3. `## Workflow`
4. `## Coverage And Risk`
5. `## Status Contract`
6. `## Safety And Data`
7. `## Quality Gate`

The source must state these rules explicitly:

```text
Artifact mode creates requested test cases. Default authoring is Markdown first, then XLSX conversion, and both artifacts are returned unless the user explicitly requests another format.
Triage, classification, or planning-only responses use Deliverable: Not Applicable and do not create test-case artifacts.
BDD/Gherkin is not a default. Karate is not a universal default; Karate execution belongs to the Karate specialist when repository conventions or the request require it.
Coverage is risk-based and conditional. Never force security, integration, performance, authorization, UI, or CRUD categories without applicability or an oracle-supported risk.
The current requirement or acceptance criterion is the first expected-result oracle. Implementation is a behavior clue, not automatically the oracle. Unresolved authority is Requirement Ambiguity.
Markdown is the canonical source. XLSX must be reconciled against it for count, IDs, groups, priorities, fields, and status.
New cases default to Not Run; blank Actual Result, Evidence, and Date do not become invented execution data.
Deliverable status and Product Behavior status are separate. Test-case authoring without execution is Product Behavior: Not Evaluated.
Deadline may inform priority when impact and exposure justify urgency, but it does not determine severity or automatically make a case P1.
Do not persist secrets, unsafe production data, or unsupported requirements.
```

Use the direct reference for field-level and workbook details instead of duplicating the full schema in `SKILL.md`.

- [x] **Step 2: Create the generic schema reference**

Create `test-case-schema-and-formats.md` with these exact headings:

```text
# Test Case Schema And Formats
## Output Modes
## Markdown Canonical Format
## XLSX Workbook Contract
## Risk-Based Coverage
## Oracle And Traceability
## Status And Execution Semantics
## Data Safety And Reproduction
## Markdown/XLSX Reconciliation
## Format Selection Boundaries
```

The reference must define the following field order without Anata or project-specific labels:

```text
No, ID, Group, Title, Priority, Type, Preconditions, Test Steps, Expected Result, Actual Result, Test Data, Notes, Status, Evidence, Date
```

It must define a `Summary` sheet with `Group`, `Case Count`, `Dominant Priority`, `Pass`, `Fail`, `Not Run`, `Status`, `Progress`, and `Notes`; one detail sheet per group; stable IDs; `Not Run` defaults; blank execution-only fields; safe Excel sheet names; formula guidance; and parity checks. It must state that Markdown is source-of-truth and XLSX is derived.

It must define risk-based conditional categories, current requirement/API/schema oracle precedence, Requirement Ambiguity, separate severity/priority logic, blocker handling, no unsupported product pass, and no invented data. It must explicitly say that BDD/Gherkin is optional on explicit request and Karate belongs to the Karate specialist when applicable.

- [x] **Step 3: Create generic runtime metadata**

Create `agents/openai.yaml` with:

```yaml
interface:
  display_name: Writing Test Cases
  short_description: Design risk-based test cases and produce Markdown plus reconciled XLSX artifacts
  default_prompt: Use $writing-test-cases to design risk-based test cases from current requirements, draft Markdown, convert it to XLSX, reconcile both artifacts, and report execution status separately.
  policy:
    allow_implicit_invocation: true
    products:
    - chatgpt
    - codex
    - api
    - atlas
```

- [x] **Step 4: Run source structure checks**

Run:

```bash
rtk ls skills/writing-test-cases
rtk wc -w skills/writing-test-cases/SKILL.md
rtk grep -n -e "TBD" -e "TODO" -e "FIXME" skills/writing-test-cases/SKILL.md skills/writing-test-cases/references/test-case-schema-and-formats.md skills/writing-test-cases/agents/openai.yaml
rtk grep -n -e "ABL" -e "Accurate" -e "Kafka" -e "Laravel" -e "PHP" -e "Anata" -e "devabs" -e "password" -e "token" skills/writing-test-cases/SKILL.md skills/writing-test-cases/references/test-case-schema-and-formats.md skills/writing-test-cases/agents/openai.yaml
```

Expected: all planned runtime files exist, `SKILL.md` is at most 900 words, and both scans return zero matches. Intentional leakage mentions belong only in test evidence or plan/spec documentation, not runtime files.

- [x] **Step 5: Commit the first source implementation**

Run:

```bash
rtk git add skills/writing-test-cases/SKILL.md skills/writing-test-cases/references/test-case-schema-and-formats.md skills/writing-test-cases/agents/openai.yaml
rtk git diff --cached --check
rtk git commit -m "feat: migrate writing test cases skill"
```

## Task 4: Run Paired GREEN And Refactor Verification

**Files:**
- Create or modify: `skills/writing-test-cases/tests/verification-results.md`
- Create or modify: `skills/writing-test-cases/tests/paired-source-manifest.sha256`
- Modify runtime source files only when a demonstrated scenario loophole requires it.

- [x] **Step 1: Freeze source identity before paired dispatch**

Run:

```bash
rtk sha256sum skills/writing-test-cases/SKILL.md skills/writing-test-cases/references/test-case-schema-and-formats.md skills/writing-test-cases/agents/openai.yaml skills/writing-test-cases/tests/scenarios.md > skills/writing-test-cases/tests/paired-source-manifest.sha256
rtk sha256sum -c skills/writing-test-cases/tests/paired-source-manifest.sha256
```

Record the manifest output, creation date, and source identity in `verification-results.md`. Do not edit the four hashed files between freeze and paired dispatch.

- [x] **Step 2: Run the nine GREEN scenarios**

Dispatch a fresh general-purpose subagent for each exact scenario prompt with this neutral wrapper:

```text
Load and follow /home/tirta/qa-agent-skills/skills/writing-test-cases/SKILL.md and any direct reference it requires. Do not read test scenarios or baseline/verification result files. Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.
```

The only difference from RED is the canonical skill-loading clause. Do not explicitly tell the agent the expected coverage categories, output format, or status answer beyond the scenario prompt. Preserve raw output, exact prompt, task handle, harness, execution date, model limitation, and loaded-file list.

- [x] **Step 3: Grade narrow criteria and Common QA Contract separately**

For every scenario, record:

- scenario-specific decision result;
- `Understand`, `Inspect`, `Design`, `Perform`, `Verify`, and `Report` applicability;
- oracle/source precedence;
- risk, data, and evidence safety;
- artifact versus product status separation;
- unsupported claim, leakage, or framework-forcing check.

The expected final marker is:

```text
Skill Tests: Passed (9/9 scenario-specific decision criteria and 9/9 full-contract applicability criteria)
```

- [x] **Step 4: Refactor only demonstrated gaps**

If a scenario fails, preserve its exact raw output and add the smallest explicit counter to `SKILL.md` or the direct reference. Do not add a rule solely to force a literal format in a mode where it is not required. Update the source manifest after the source edit, rerun the affected scenario, and then rerun all nine scenarios.

- [x] **Step 5: Record the paired evidence**

`verification-results.md` must contain the current result index, RED/GREEN/refactor labels, raw outputs, criterion quotes, observed rationalizations, source hashes, full-contract grading, final marker, and no-deployment status. Keep historical failed runs visible and label superseded results clearly.

- [x] **Step 6: Commit verified source evidence**

After all nine scenarios pass and the manifest is current, run:

```bash
rtk git add skills/writing-test-cases/tests/verification-results.md skills/writing-test-cases/tests/paired-source-manifest.sha256
rtk git diff --cached --check
rtk git commit -m "test: verify writing test cases migration"
```

## Task 5: Final Source Verification

**Files:**
- Verify: `skills/writing-test-cases/**`
- Verify: `docs/superpowers/specs/2026-08-19-writing-test-cases-migration-design.md`
- Verify: `docs/superpowers/plans/2026-08-19-writing-test-cases-migration.md`

- [x] **Step 1: Run source manifest and whitespace checks**

Run:

```bash
rtk sha256sum -c skills/writing-test-cases/tests/paired-source-manifest.sha256
rtk git diff --no-index --check /dev/null skills/writing-test-cases/SKILL.md
rtk git diff --no-index --check /dev/null skills/writing-test-cases/references/test-case-schema-and-formats.md
rtk git diff --no-index --check /dev/null skills/writing-test-cases/agents/openai.yaml
rtk git diff --no-index --check /dev/null skills/writing-test-cases/tests/scenarios.md
rtk git diff --no-index --check /dev/null skills/writing-test-cases/tests/baseline-results.md
rtk git diff --no-index --check /dev/null skills/writing-test-cases/tests/verification-results.md
rtk git diff --check
```

The no-index commands normally return status 1 because the files differ from `/dev/null`; accept that status only when no whitespace error appears.

- [x] **Step 2: Verify final evidence completeness**

Read `baseline-results.md`, `verification-results.md`, and the manifest. Confirm all nine scenarios have exact prompts, pressure labels, raw output, task handles, model limitations, loaded files, narrow grading, full-contract grading, and final outcomes. Confirm the final result is 9/9 and no current section relies on stale source hashes.

- [x] **Step 3: Review the source against the design**

Confirm Markdown-first then XLSX output, both artifacts by default, conditional coverage, no BDD/Gherkin default, Karate delegation, oracle precedence, status separation, data safety, and no project leakage. Confirm no other Specialist Skill was modified.

## Task 6: Deploy Verified Runtime Files

**Files:**
- Mirror: `/home/tirta/qa-agent-skills/skills/writing-test-cases/SKILL.md` to `/home/tirta/.agents/skills/writing-test-cases/SKILL.md`
- Mirror: `/home/tirta/qa-agent-skills/skills/writing-test-cases/references/` to `/home/tirta/.agents/skills/writing-test-cases/references/`
- Mirror: `/home/tirta/qa-agent-skills/skills/writing-test-cases/agents/` to `/home/tirta/.agents/skills/writing-test-cases/agents/`
- Do not deploy: `/home/tirta/qa-agent-skills/skills/writing-test-cases/tests/`

- [x] **Step 1: Inspect the existing runtime destination**

Run:

```bash
rtk ls /home/tirta/.agents/skills
rtk ls /home/tirta/.agents/skills/writing-test-cases
rtk find /home/tirta/.agents/skills/writing-test-cases
```

Read every existing file under the three planned runtime areas. Stop for review if any file is unexpected, user-maintained, or has unknown provenance. Do not overwrite uncertainty.

- [x] **Step 2: Verify source prerequisite and create approved runtime directories**

Only after Task 5 passes and collision review is complete, run:

```bash
rtk ls /home/tirta/.agents/skills
rtk mkdir -p /home/tirta/.agents/skills/writing-test-cases/references /home/tirta/.agents/skills/writing-test-cases/agents
rtk ls /home/tirta/.agents/skills/writing-test-cases
```

- [x] **Step 3: Mirror only the three planned runtime files**

Use `apply_patch` to create or update only `SKILL.md`, `references/test-case-schema-and-formats.md`, and `agents/openai.yaml`. Never use `cp`, `rsync`, shell redirection, or deploy `tests/`.

- [x] **Step 4: Compare source and runtime**

Run:

```bash
rtk proxy cmp -s skills/writing-test-cases/SKILL.md /home/tirta/.agents/skills/writing-test-cases/SKILL.md
rtk proxy cmp -s skills/writing-test-cases/references/test-case-schema-and-formats.md /home/tirta/.agents/skills/writing-test-cases/references/test-case-schema-and-formats.md
rtk proxy cmp -s skills/writing-test-cases/agents/openai.yaml /home/tirta/.agents/skills/writing-test-cases/agents/openai.yaml
rtk ls /home/tirta/.agents/skills/writing-test-cases
```

Expected: all comparisons return success, and the runtime inventory contains exactly `SKILL.md`, `references/test-case-schema-and-formats.md`, and `agents/openai.yaml`, with no `tests/` directory.

- [x] **Step 5: Record deployment evidence**

Append pre-deployment inventory, post-deployment inventory, source/runtime hashes, apply-patch targets, deployment date, source manifest identity, and excluded-tests confirmation to `verification-results.md`, then commit only that evidence update:

```bash
rtk git add skills/writing-test-cases/tests/verification-results.md
rtk git diff --cached --check
rtk git commit -m "chore: record writing test cases deployment"
```

## Final Handoff

After Task 6, verify:

```bash
rtk git status --short
rtk git log --oneline -5
```

Expected: the source repository is clean, the latest commits cover implementation, verification, and deployment evidence, and runtime contains only the three approved files. Report remaining blockers as `None` or name the exact blocked conversion/deployment scope.
