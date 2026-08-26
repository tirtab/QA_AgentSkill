# Test Data Management Migration Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans. Steps use checkbox (`- [ ]`) syntax.

**Goal:** Migrate `test-data-management` from a Laravel/PHP-specific baseline into a project-agnostic factory, fixture, payload, isolation, and data-safety skill.

**Architecture:** Keep a concise `SKILL.md` entry point, place detailed generic rules in one direct reference, and retain all RED/GREEN/refactor evidence under the source skill. Runtime receives only `SKILL.md`, `references/`, and `agents/`.

**Tech Stack:** Agent Skills Markdown/YAML, OpenCode isolated general subagents, Markdown evidence, SHA-256 manifests, RTK-prefixed verification commands.

---

## Scope Boundary

This plan changes only `test-data-management`. It does not modify `qa-engineering`, `bug-reporting`, `writing-test-cases`, or any specialist skill. The existing `/home/tirta/.agents/skills/test-data-management/SKILL.md` is baseline input only.

## File Map

- Create `skills/test-data-management/SKILL.md`: trigger, factory-first model, builder roles, lifecycle, isolation, safety, and quality gate.
- Create `skills/test-data-management/references/data-strategy-and-isolation.md`: generic strategies, relationship rules, determinism, integration boundaries, and safe-data contract.
- Create `skills/test-data-management/agents/openai.yaml`: generic runtime metadata.
- Create `skills/test-data-management/tests/scenarios.md`: stable pressure scenarios TDM-S1 through TDM-S7.
- Create `skills/test-data-management/tests/baseline-results.md`: raw RED observations before the new skill exists.
- Create `skills/test-data-management/tests/verification-results.md`: paired GREEN/refactor evidence, hashes, grading, and deployment record.
- Create `skills/test-data-management/tests/paired-source-manifest.sha256`: frozen hashes for runtime source files and scenarios.

## Task 0: Bootstrap And Baseline Prerequisites

**Files:** Verify repository and global baseline; create only the planned source directories.

- [x] Verify `/home/tirta/qa-agent-skills` is clean or concurrent changes are understood.
- [x] Read this design, the hybrid rollout design, and the existing global baseline skill.
- [x] Confirm fresh general subagents are available.
- [x] Create `skills/test-data-management/tests`, `references`, and `agents` only after the checks pass.

## Task 1: Define RED Pressure Scenarios

**File:** `skills/test-data-management/tests/scenarios.md`

Create seven stable scenarios with exact prompts, pressures, and narrow criteria:

1. **TDM-S1 Factory-first:** a team proposes inline IDs and a shared staging user because factories are slow. Reject shared/hardcoded data and distinguish reusable factory, fixture, and payload builder responsibilities.
2. **TDM-S2 Relationships:** an invoice needs customer, account, and line-item data while a developer proposes random foreign keys. Require relationship builders or explicit created parents and reject orphan-prone IDs.
3. **TDM-S3 Isolation:** a large suite is flaky because tests reuse seeded records. Choose reset, transaction, focused fixture, or another boundary conditionally and require test-owned mutable data.
4. **TDM-S4 Determinism:** parallel tests collide on emails and timestamps while a manager proposes random Faker values in assertions. Require deterministic scoped uniqueness and observable-value capture.
5. **TDM-S5 Integration boundary:** a partner API test is requested with production or shared staging records. Require synthetic data and identify mock, sandbox, contract-fixture, or real-dependency evidence boundaries without inventing access.
6. **TDM-S6 Safety:** a destructive payment/refund or deletion test is requested in a shared environment. Require disposable synthetic data, controlled doubles, explicit approval, and no unsafe repetition.
7. **TDM-S7 Helper lifecycle:** a team wants helpers that call other tests and cleanup only at the end of the suite. Require independent setup, explicit cleanup, and no test-order coupling.

Each scenario must state that product execution is not implied by data design and that missing project capabilities are `Not Provided`.

## Task 2: Run And Record RED Baseline

**File:** `skills/test-data-management/tests/baseline-results.md`

Dispatch one fresh general subagent per exact scenario with this neutral wrapper:

```text
Do not load or read any test-data-management, qa-engineering, writing-test-cases, or other QA skill. Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.
```

Preserve exact prompts, raw output, task handles, date, harness, model limitation, and file-write result. Grade each response against its narrow criterion and the applicable Common QA Contract. Require at least one demonstrated baseline failure before implementation proceeds.

## Task 3: Implement The Minimal Project-Agnostic Skill

**Files:** `SKILL.md`, direct reference, and `agents/openai.yaml`.

- [x] Create the exact runtime section order: frontmatter, `# Test Data Management`, `## Output Modes`, `## Workflow`, `## Factory And Builder Roles`, `## Isolation And Lifecycle`, `## Determinism And Relationships`, `## Safety And Integration`, and `## Quality Gate`.
- [x] Remove Laravel/PHP/ORM/vendor defaults from runtime output. Keep examples generic and label project capabilities `Not Provided`.
- [x] Put detailed strategy and schema guidance in the direct reference rather than bloating `SKILL.md`.
- [x] Create metadata with display name `Test Data Management`, a generic description, an implicit invocation policy, and no project-specific prompt.
- [x] Run word, placeholder, leakage, ASCII, and whitespace checks before committing source.

## Task 4: Run Paired GREEN And Refactor Verification

**Files:** `verification-results.md`, `paired-source-manifest.sha256`, and runtime source only when a demonstrated loophole requires it.

- [x] Freeze hashes for `SKILL.md`, the direct reference, `agents/openai.yaml`, and `tests/scenarios.md`.
- [x] Run the seven exact scenarios with the same prompts and only the canonical skill-loading clause added.
- [x] Grade narrow criteria and Common QA Contract separately, including builder roles, oracle, isolation, determinism, safety, status axes, and project-leakage checks.
- [x] Evaluate scenario failures; none occurred after the source freeze, so no counter or rerun was required.
- [x] Record the final marker only after all seven scenarios pass and the manifest is current.

## Task 5: Final Source Verification

- [x] Verify manifest, whitespace, word limit, exact required headings, no project leakage, and no runtime `tests/` files.
- [x] Confirm all seven scenarios have exact prompts, pressure labels, raw output, task handles, loaded files, narrow grading, full-contract grading, and final outcomes.
- [x] Review source against the design and confirm no other skill changed.

## Task 6: Deploy And Prepare The Skill PR

- [x] Review `/home/tirta/.agents/skills/test-data-management/SKILL.md` as a legacy collision before deployment.
- [x] Mirror only `SKILL.md`, `references/`, and `agents/` with an explicit allowlist.
- [x] Verify source/runtime exact equality and no runtime `tests/` directory.
- [x] Record deployment inventory, hashes, excluded-tests confirmation, and final test marker.
- [ ] Create a dedicated `pr/test-data-management` branch from `origin/main`, commit only this skill and its docs/evidence, push it, and use `main` as the PR base.
