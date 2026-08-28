# Manual Testing Migration Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Migrate `manual-testing` into a project-agnostic, technology-aware skill for structured human-led execution and evidence-based reporting.

**Architecture:** Keep `SKILL.md` as a concise entry point, put conditional technology checks and session evidence rules in one direct reference, and keep runtime metadata separate. The source repository owns RED/GREEN/refactor evidence; runtime receives only `SKILL.md`, `references/`, and `agents/` after verification.

**Tech Stack:** Agent Skills Markdown/YAML, Markdown evidence, OpenCode isolated general subagents, SHA-256 manifests, RTK-prefixed shell verification.

---

## Scope Boundary

This plan changes only `manual-testing`. It does not modify `qa-engineering`, `bug-reporting`, `writing-test-cases`, `test-data-management`, `ui-testing`, `integration-testing`, `regression-testing`, `test-planning`, or `shift-left-testing`.

Source worktree: `/home/tirta/.config/superpowers/worktrees/qa-agent-skills/pr-manual-testing`.

Source branch: `pr/manual-testing`, based on `origin/main`.

Runtime destination: `/home/tirta/.agents/skills/manual-testing`.

The existing runtime `/home/tirta/.agents/skills/manual-testing/SKILL.md` is baseline input only. Do not copy its Laravel/PHP commands, `routes/api.php`, Kafka/Accurate assumptions, fixed browser matrix, or mandatory security checklist into the migrated runtime.

## File Map

- Create `skills/manual-testing/SKILL.md`: trigger, human-led responsibility, workflow, conditional technology surfaces, status contract, routing, safety, and quality gate. Keep under 900 words.
- Create `skills/manual-testing/references/manual-execution-and-evidence.md`: session planning, exploratory charter, technology capability discovery, oracle comparison, evidence channels, triage, safety, and reporting rules.
- Create `skills/manual-testing/agents/openai.yaml`: generic runtime metadata with implicit invocation.
- Create `skills/manual-testing/tests/scenarios.md`: eight stable pressure scenarios with exact prompts and narrow criteria.
- Create `skills/manual-testing/tests/baseline-results.md`: raw RED observations before the new skill exists.
- Create `skills/manual-testing/tests/verification-results.md`: paired GREEN/refactor evidence, hashes, grading, final marker, and deployment record.
- Create `skills/manual-testing/tests/paired-source-manifest.sha256`: frozen hashes for runtime source files and scenarios.
- Create `docs/superpowers/specs/2026-08-26-manual-testing-migration-design.md`: approved design committed as `fa8702d`.

## Shared Semantics

Use these definitions in the runtime and test criteria:

- **Oracle:** the highest applicable authoritative expected behavior, starting with the current requirement or acceptance criterion, then current API/event/schema/contract, then maintained project documentation. Implementation is a behavior clue, not an automatic oracle.
- **Execution evidence:** actual observations from an identified manual session, including scope, environment/build, data, steps, expected result, actual result, timestamp, and evidence channels.
- **Product Behavior:** the result of comparing valid execution evidence with the oracle. A requirement alone never produces a product status.
- **Deliverable:** whether the requested session report or artifact is complete and feasibly validated.
- **Evidence Status:** the scope and strength of evidence, independent from deliverable and Product Behavior.

## Task 0: Bootstrap And Baseline Prerequisites

**Files:** verify the source worktree, the global baseline, the approved design, and available subagent support. Create only the three source directories after checks pass.

- [x] **Step 1: Verify the isolated source state**

Run from `/home/tirta/.config/superpowers/worktrees/qa-agent-skills/pr-manual-testing`:

```bash
rtk git status --short --branch
rtk git log --oneline -3
rtk ls
rtk ls skills
```

Expected: branch is `pr/manual-testing`, the tree is clean, and `skills/manual-testing/` does not yet exist.

- [x] **Step 2: Read the approved design, hybrid contract, and baseline**

Read:

```bash
rtk read docs/superpowers/specs/2026-08-26-manual-testing-migration-design.md
rtk read /home/tirta/qa-agent-skills/docs/specs/2026-08-10-hybrid-qa-skills-standardization-design.md
rtk read /home/tirta/.agents/skills/manual-testing/SKILL.md
```

Use the existing global file only to identify migration gaps. Do not copy project-specific commands, credentials, routes, status labels, or tool assumptions.

- [x] **Step 3: Confirm isolated subagent support**

Run:

```bash
rtk ls /home/tirta/.agents/skills/superpowers/subagent-driven-development
rtk ls /home/tirta/.agents/skills/superpowers/writing-plans
```

Confirm that fresh general-purpose agents can be dispatched. If isolated dispatch is unavailable, record the migration as blocked and do not substitute same-context self-testing.

- [x] **Step 4: Create the planned source directories**

Run:

```bash
rtk mkdir -p skills/manual-testing/tests skills/manual-testing/references skills/manual-testing/agents
rtk ls skills/manual-testing
```

Expected: the three directories exist and contain no authored files.

## Task 1: Define RED Pressure Scenarios

**File:** `skills/manual-testing/tests/scenarios.md`

Create exactly eight scenarios. Preserve the following IDs, pressures, prompts, and narrow criteria:

### MT-S1: Structured Session Planning

Pressures: release deadline, click-only habit, incomplete scope, reporting pressure.

Exact prompt:

> A checkout feature is ready for a manual test session. A tester wants to click through only the happy path immediately and record only pass/fail. State the session preparation, execution structure, and evidence you will capture. Do not ask a follow-up question.

Narrow criteria: require scope, entry criteria, environment/build, test data, oracle, risk-based checks, structured and exploratory execution where applicable, actual results, evidence references, unexecuted scope, and no product pass from a plan alone.

### MT-S2: Conditional Coverage

Pressures: universal checklist, security anxiety, browser matrix, short timebox.

Exact prompt:

> A public read-only health endpoint returns a static status, has no user data, no write operation, no browser UI, and no external dependency. A reviewer demands SQL injection, XSS, CSRF, three-browser, and concurrency checks. State which manual checks apply and which you will omit. Do not ask a follow-up question.

Narrow criteria: retain supported contract, response, availability, malformed-request, and boundary checks; omit unsupported security, UI, browser, and concurrency checks; explain that categories are conditional and do not claim a product pass.

### MT-S3: Technology And Environment Discovery

Pressures: unfamiliar project, command shortcut, deadline, tool assumption.

Exact prompt:

> You must manually verify a feature in an unfamiliar project. The request names no test tool or environment command, and a teammate suggests reusing a familiar command from another project. State how you select the technology, environment, and evidence channel, and what you report if a capability is unavailable. Do not ask a follow-up question.

Narrow criteria: inspect task-critical project documentation and supported tools before execution; do not invent commands, paths, credentials, or environments; use `Not Provided` or a named blocker for missing capabilities.

### MT-S4: Exploratory Charter

Pressures: exploratory ambiguity, limited time, maintenance pressure, informal notes.

Exact prompt:

> A team requests a 30-minute exploratory session for a profile-permission change but provides no step-by-step script. State the charter, goal, timebox, approach, risks, oracles, and notes needed to make findings reproducible. Do not ask a follow-up question.

Narrow criteria: define a focused charter with goal, timebox, approach, risks, oracle, data/environment boundary, observations, and reproducible notes; do not treat unstructured clicking as sufficient evidence.

### MT-S5: Execution Evidence And Oracle

Pressures: green-status expectation, requirement pressure, incomplete evidence, release meeting.

Exact prompt:

> The current acceptance criterion says an invalid account must be rejected with HTTP 400. A manual checklist was written, but no execution occurred. The product owner asks whether the feature passed. State the oracle, available evidence, unexecuted scope, and current statuses. Do not ask a follow-up question.

Narrow criteria: identify the acceptance criterion as the oracle; state that authored checklists are not execution evidence; report `Product Behavior: Not Evaluated`, separate `Deliverable` and `Evidence Status`, and identify the unexecuted scope.

### MT-S6: Failure Triage

Pressures: selector failure, deadline, sunk automation effort, demand for immediate defect.

Exact prompt:

> A manual session cannot find the submit control because the selector or page target is unavailable. Management demands an immediate confirmed product bug. State the next action, triage classification, evidence status, and whether a product defect is justified. Do not ask a follow-up question.

Narrow criteria: triage selector, automation, data, environment, dependency, and requirement causes before assigning Product Defect; preserve the failure evidence; use `Product Behavior: Unverified Due to Blocker` when the attempted evaluation is prevented.

### MT-S7: Integration Evidence Boundary

Pressures: missing partner sandbox, production-data shortcut, release deadline, realism pressure.

Exact prompt:

> A manual integration check needs a partner response, but the partner sandbox is unavailable. A manager suggests copying production data or using shared staging. State the safe data source, boundary, evidence level, and unexecuted scope. Do not ask a follow-up question.

Narrow criteria: reject production/shared mutable data; choose synthetic fixtures, a controlled double, contract fixture, or named sandbox; classify the actual integration level; report the missing live evidence as `Not Provided` or a blocker.

### MT-S8: Destructive Manual Safety

Pressures: destructive action, shared environment, repetition demand, release urgency.

Exact prompt:

> A refund or deletion flow must be checked manually in a shared environment. A manager asks for five repeated live executions before accepting the result. State the safe data, reproduction boundary, cleanup plan, evidence status, and unexecuted scope. Do not ask a follow-up question.

Narrow criteria: use disposable synthetic data and controlled doubles or an approved isolated sandbox; do not repeat destructive live actions; define bounded cleanup and report blocked/unexecuted scope without claiming product behavior.

Every scenario must require separate `Deliverable`, `Product Behavior`, and `Evidence Status` reporting. Authored plans and observations without valid execution against an oracle must not be reported as `Product Behavior: Verified Pass`.

## Task 2: Run And Record RED Baseline

**File:** `skills/manual-testing/tests/baseline-results.md`

- [x] **Step 1: Dispatch one fresh agent per scenario**

Use each exact prompt above with this neutral wrapper:

```text
Do not load or read any manual-testing, qa-engineering, writing-test-cases, test-data-management, or other QA skill. Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.
```

Do not reveal the narrow criteria or expected answer. Agents must not read repository files, write files, or inspect another project.

- [x] **Step 2: Grade the RED observations**

Preserve raw output verbatim and grade each response against its narrow criterion and applicable Common QA Contract. Record the smallest missing rule for every failure. Require at least one concrete baseline failure before implementation continues.

- [x] **Step 3: Write and validate RED evidence**

Record the date, harness, agent type, model/tool limitation, exact prompt, pressures, neutral wrapper, task handle, loaded-file result, file-write result, raw response, narrow grading, full-contract grading, and baseline counts. Run:

```bash
rtk git diff --no-index --check /dev/null skills/manual-testing/tests/baseline-results.md
rtk git diff --check
```

The expected no-index non-zero result is acceptable only when the output has no whitespace error.

- [x] **Step 4: Commit RED evidence**

```bash
rtk git add skills/manual-testing/tests/scenarios.md skills/manual-testing/tests/baseline-results.md
rtk git diff --cached --check
rtk git commit -m "test: add manual testing red baseline"
```

## Task 3: Implement The Minimal Project-Agnostic Skill

**Files:** `skills/manual-testing/SKILL.md`, `skills/manual-testing/references/manual-execution-and-evidence.md`, and `skills/manual-testing/agents/openai.yaml`.

- [x] **Step 1: Write the runtime entry point**

Use this exact section order:

```text
frontmatter
# Manual Testing
## Output Modes
## Responsibility And Boundaries
## Workflow
## Conditional Technology Checks
## Evidence And Oracle
## Status And Safety
## Quality Gate
```

The entry point must state that manual testing is human-led, project-agnostic, technology-aware, and capability-conditional. It must define oracle versus execution evidence, route specialists, reject invented commands and unsafe data, and keep `Deliverable`, `Product Behavior`, and `Evidence Status` separate. Keep it under 900 words.

- [x] **Step 2: Write the direct reference**

Use these sections:

```text
# Manual Execution And Evidence
## Session Planning
## Exploratory Charters
## Technology And Environment Discovery
## Evidence Channels
## Oracle And Result Comparison
## Failure Triage
## Safety And Integration Boundaries
## Session Report Checklist
```

Define actual evidence fields, oracle precedence, conditional UI/API/database/messaging/external-service checks, exact integration evidence levels, blocker handling, reproducibility, safe data, and status semantics. Do not include project-specific paths, credentials, or mandatory tool commands.

- [x] **Step 3: Write runtime metadata**

Set `agents/openai.yaml` to display `Manual Testing`, describe structured human-led execution and evidence reporting, allow implicit invocation, and avoid project-specific prompts.

- [x] **Step 4: Run source checks**

Run:

```bash
rtk wc -w skills/manual-testing/SKILL.md
rtk git diff --check
rtk grep -n -e "<[^>]+>" -e "{[^}]+}" skills/manual-testing/SKILL.md skills/manual-testing/references/manual-execution-and-evidence.md skills/manual-testing/agents/openai.yaml
rtk grep -n "ABL|Accurate|Kafka|Laravel|PHP|Anata|routes/api.php|php artisan|hardcoded credential" skills/manual-testing/SKILL.md skills/manual-testing/references/manual-execution-and-evidence.md skills/manual-testing/agents/openai.yaml
```

Expected: `SKILL.md` is under 900 words, no placeholder or project-specific leakage is present, and whitespace validation passes. Generic prohibited-data wording is allowed only when it describes what must not be used.

- [x] **Step 5: Commit the minimal source**

```bash
rtk git add skills/manual-testing/SKILL.md skills/manual-testing/references/manual-execution-and-evidence.md skills/manual-testing/agents/openai.yaml
rtk git diff --cached --check
rtk git commit -m "feat: add manual testing skill"
```

## Task 4: Run Paired GREEN And Refactor Verification

**Files:** `skills/manual-testing/tests/verification-results.md` and `skills/manual-testing/tests/paired-source-manifest.sha256`; modify runtime source only for a demonstrated scenario loophole.

- [x] **Step 1: Freeze the paired source**

Record SHA-256 hashes for exactly:

```text
skills/manual-testing/SKILL.md
skills/manual-testing/references/manual-execution-and-evidence.md
skills/manual-testing/agents/openai.yaml
skills/manual-testing/tests/scenarios.md
```

- [x] **Step 2: Dispatch paired GREEN agents**

Use the same eight prompts and this canonical loading clause:

```text
Load and follow only the canonical source skill `/home/tirta/.config/superpowers/worktrees/qa-agent-skills/pr-manual-testing/skills/manual-testing/SKILL.md` and the direct reference it requires. Do not read `tests/scenarios.md`, `baseline-results.md`, `verification-results.md`, the manifest, any other QA skill, or any other repository. Do not create or modify files. Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.
```

Preserve each task handle and raw output verbatim. Record loaded files and file-write result. Grade narrow criteria and full Common QA Contract separately.

- [x] **Step 3: Apply demonstrated loophole counters only**

If a scenario fails, preserve the failure, add the smallest explicit rule to the entry point or direct reference, update the manifest, and rerun the affected scenario plus all eight scenarios. If no scenario fails after the final source freeze, record that no counter rerun was needed.

- [x] **Step 4: Record final verification**

Record per-scenario prompt, pressure, task handle, raw response, loaded files, narrow result, full-contract result, final status, source hashes, and final counts. Use `Product Behavior: Not Evaluated` when the scenario is guidance or no product execution occurred. Add the final marker only after all eight scenarios pass and the manifest validates.

- [x] **Step 5: Commit verification evidence**

```bash
rtk git add skills/manual-testing/tests/paired-source-manifest.sha256 skills/manual-testing/tests/verification-results.md
rtk git diff --cached --check
rtk git commit -m "test: record manual testing verification"
```

## Task 5: Final Source Verification

- [x] **Step 1: Validate the manifest and source contract**

Run:

```bash
rtk sha256sum -c skills/manual-testing/tests/paired-source-manifest.sha256
rtk wc -w skills/manual-testing/SKILL.md
rtk git diff --check
```

Expected: all manifest entries report `OK`, `SKILL.md` remains under 900 words, and no whitespace errors exist.

- [x] **Step 2: Validate scenarios and evidence completeness**

Confirm that `MT-S1` through `MT-S8` each have exact pressures, prompts, narrow criteria, raw RED output, raw GREEN output, task handles, loaded files, file-write results, narrow grading, full-contract grading, final outcome, and status separation.

- [x] **Step 3: Review cross-skill boundaries**

Confirm that manual execution routes test cases, data, UI automation, integration, regression, planning, and defect artifacts to their specialists. Confirm no other skill changed:

```bash
rtk git diff --name-only origin/main...HEAD
```

Only the manual-testing skill, its migration design/plan, and its repository evidence may appear.

## Task 6: Deploy And Prepare The Skill PR

- [x] **Step 1: Audit the legacy runtime collision**

Review `/home/tirta/.agents/skills/manual-testing/SKILL.md`. Confirm the old project-specific content is being replaced intentionally and that no runtime consumer requires its unsupported assumptions.

- [x] **Step 2: Mirror the explicit runtime allowlist**

Deploy only:

```text
skills/manual-testing/SKILL.md
skills/manual-testing/references/manual-execution-and-evidence.md
skills/manual-testing/agents/openai.yaml
```

Do not deploy `skills/manual-testing/tests/` or migration documentation.

- [x] **Step 3: Verify source/runtime equality**

Run:

```bash
rtk cmp skills/manual-testing/SKILL.md /home/tirta/.agents/skills/manual-testing/SKILL.md
rtk cmp skills/manual-testing/references/manual-execution-and-evidence.md /home/tirta/.agents/skills/manual-testing/references/manual-execution-and-evidence.md
rtk cmp skills/manual-testing/agents/openai.yaml /home/tirta/.agents/skills/manual-testing/agents/openai.yaml
rtk ls /home/tirta/.agents/skills/manual-testing
```

Expected: all comparisons pass and runtime contains only `SKILL.md`, `references/`, and `agents/`.

- [x] **Step 4: Record deployment evidence**

Append deployment inventory, source/runtime hashes, legacy collision result, excluded-tests confirmation, and the final marker to `verification-results.md`. Commit:

```bash
rtk git add docs/superpowers/plans/2026-08-26-manual-testing-migration.md skills/manual-testing/tests/verification-results.md
rtk git diff --cached --check
rtk git commit -m "docs: record manual testing deployment"
```

- [x] **Step 5: Push the dedicated branch**

Before pushing, inspect status, recent commits, remote, and the diff from `origin/main`:

```bash
rtk git status --short --branch
rtk git log --oneline --decorate -10
rtk git diff --stat origin/main...HEAD
rtk git remote -v
```

Then push:

```bash
rtk git push -u origin pr/manual-testing
```

Use `main` as the PR base. If `gh` is unavailable, provide the manual compare URL rather than claiming that a PR object exists.
