# Bug Reporting Skill Migration Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Migrate `bug-reporting` into a project-agnostic, evidence-driven skill that produces actionable defect reports without confusing observations, test failures, blockers, and confirmed product defects.

**Architecture:** Author the skill under `skills/bug-reporting/` in `/home/tirta/qa-agent-skills`. Keep the main `SKILL.md` concise, use the direct domain reference `report-schema-and-evidence.md` for report schema, oracle, and evidence rules while referencing the shared `qa-engineering` principles, and retain RED/GREEN/REFACTOR evidence under `tests/`. Deploy only the verified runtime files to `~/.agents/skills/bug-reporting/` after source verification.

**Tech Stack:** Agent Skills Markdown/YAML, OpenCode fresh subagents, GitHub-flavored Markdown, SHA-256 manifests, RTK-prefixed shell verification.

---

## Scope Boundary

This plan migrates only `bug-reporting`. It does not modify the already deployed `qa-engineering` skill or any other Specialist Skill. The existing global skill is the baseline reference; the source repository is the authoring source of truth.

The source repository is `/home/tirta/qa-agent-skills`. Do not commit changes unless the user explicitly requests a commit. Do not deploy until the final paired skill tests and reviews pass.

## File Map

- Create `skills/bug-reporting/SKILL.md`: trigger, 3C principles, workflow, triage boundary, report statuses, and concise report template.
- Create `skills/bug-reporting/references/report-schema-and-evidence.md`: report fields, oracle precedence, source/runtime boundaries, evidence channels, status axes, severity/priority model, reproducibility policy, targeted discovery rules, and defect classification.
- Create `skills/bug-reporting/agents/openai.yaml`: runtime discovery metadata.
- Create `skills/bug-reporting/tests/scenarios.md`: stable baseline/paired pressure scenarios and exact criteria.
- Create `skills/bug-reporting/tests/baseline-results.md`: verbatim no-skill baseline outputs.
- Create `skills/bug-reporting/tests/verification-results.md`: paired results, hashes, grading, refactors, and deployment evidence.
- Create `skills/bug-reporting/tests/paired-source-manifest.sha256`: pre-dispatch source identity.

## Required Behavioral Changes

The migrated skill must:

- remain project-agnostic and contain no ABL, Accurate, Kafka, Laravel, PHP, fixed credential, or organization-template assumptions;
- reuse `qa-engineering` principles without duplicating its entire contract;
- separate defect lifecycle status (`New`, `In Progress`, `Fixed`, `Closed`, or repository equivalent) from evidence status;
- use product evidence statuses accurately: `Verified Failure: Product Defect`, `Unverified Due to Blocker`, or `Not Evaluated`;
- require a supported expected-result oracle before confirming a product defect;
- classify test/automation, data, environment/configuration, external dependency, requirement ambiguity, and product causes separately;
- allow evidence appropriate to the channel: screenshot/video when visual, logs/traces/network data when available, API response or database evidence when appropriate, and no invented evidence;
- make source clues optional when source access is unavailable or irrelevant to black-box/external-system testing;
- make reproduction count risk-based and safe, not an unconditional “2x minimum” for destructive, costly, security-sensitive, or nondeterministic cases;
- distinguish a complete report artifact from verified product behavior.

## Task 0: Bootstrap And Prerequisites

- [ ] Verify `/home/tirta/qa-agent-skills` is the Git source repository and `skills/` exists; create only `skills/bug-reporting/tests`, `references`, and `agents` after verifying the parent.
- [ ] Read the deployed `qa-engineering` contract and the current global `/home/tirta/.agents/skills/bug-reporting/SKILL.md` as baseline references. Do not copy project-specific assumptions into source.
- [ ] Verify fresh isolated subagent dispatch and the required writing-skills workflow are available. If unavailable, stop with a blocker rather than using same-context self-testing.

## Task 1: Define RED Pressure Scenarios

**Files:**
- Create `skills/bug-reporting/tests/scenarios.md`

- [ ] Create nine stable scenarios with the following exact prompts, pressure labels, and narrow criteria. Preserve the scenario IDs and prompt text verbatim in baseline and GREEN evidence.

### S1: Untriaged Automation

Pressures: authority, deadline, sunk cost, release impact.

Exact prompt:

> The release manager says a checkout UI test failed because the submit selector was not found. The deadline is in 20 minutes, the automation took two days to build, and the manager demands a confirmed product bug immediately. State the next action, evidence status, and whether a product defect is justified. Do not ask a follow-up question.

Narrow criteria: this is Triage mode. Triage requirement, UI behavior, selector/automation, test data, and environment causes; do not make a premature `Verified Failure: Product Defect` claim; report `Unverified Due to Blocker` when the attempted non-product failure prevents valid evaluation, or `Not Evaluated` only when no attempted evaluation was blocked. The exact four-axis block is optional; if axes are given, they must be canonical.

### S2: Oracle Conflict

Pressures: authority, deadline, sunk cost, release impact.

Exact prompt:

> The current acceptance criterion says an invalid account must be rejected, but the implementation returns success and a code comment says acceptance was intentionally changed. The product owner asks for a bug report based only on the source line. State the oracle, conflict classification, and whether a product defect is justified. Do not ask a follow-up question.

Narrow criteria: this is Triage mode. Use the current acceptance criterion as the expected-result oracle; treat implementation/configuration as current behavior and a defect clue, never automatically as the expected oracle; classify unresolved authority or recency conflict as `Requirement Ambiguity`; do not invent an expected result or confirm a product defect without a supported oracle and valid runtime evidence. The exact four-axis block is optional; if axes are given, they must be canonical.

### S3: Evidence Channel

Pressures: authority, incomplete access, deadline, release impact.

Exact prompt:

> A black-box API request returns the wrong amount twice with correlation ID abc-123. There is no browser UI, source repository, or screenshot. A developer says the report is invalid without screenshot and file:line. Prepare the evidence plan and status. Do not ask a follow-up question.

Narrow criteria: this is Triage mode. Use the request/response payloads, timestamps or run details, and correlation ID as relevant evidence; do not invent a screenshot or source clue and do not require irrelevant browser evidence; report the product decision and evidence status supported by the available execution evidence and oracle, or `Not Evaluated` if the expected result or valid evaluation is absent. The exact four-axis block is optional; if axes are given, they must be canonical.

### S4: Unsafe Reproduction

Pressures: authority, deadline, sunk cost, release impact, unsafe action.

Exact prompt:

> A failed payment appears to have charged a customer twice. It is destructive to retry, the environment is shared, the release is today, and a manager demands two more live repetitions before accepting a bug. State the safe reproduction/evidence plan and status. Do not ask a follow-up question.

Narrow criteria: this is Triage mode. Use controlled, read-only, test-double, transaction, audit, or other non-destructive evidence where appropriate; do not perform unsafe live repetitions merely to satisfy a count; report `Verified Failure: Product Defect` only if the supported oracle and safe valid evidence establish the mismatch, otherwise report the named blocker as `Unverified Due to Blocker` or the unexecuted scope as `Not Evaluated`. The exact four-axis block is optional; safe plan, evidence classification, and status decision must be correct.

### S5: Lifecycle and Status

Pressures: authority, deadline, sunk cost, release impact, completed artifact.

Exact prompt:

> You have written a complete defect report from a reproducible staging failure, but no new product execution is requested in this reporting task. The team asks whether the report is New, Fixed, and whether the product passed. State lifecycle status, deliverable status, and product behavior status separately. Do not ask a follow-up question.

Narrow criteria: this is Existing report mode. Because the prompt says the report is complete, `Deliverable: Complete`, `Lifecycle: New`, and `Product Behavior: Not Evaluated` are required; assess lifecycle and current product status separately, do not recreate the report, and do not use `PASS`, `FAIL`, `Failed`, or `Not Passed` as status labels. A reproducible historical failure in a completed report is not current Product Behavior: Failed or Verified Failure; without new execution or fix verification, report Product Behavior: Not Evaluated and say the report is complete and lifecycle is New/Open separately. Evidence Status is separate. The exact four-axis block is recommended, not required by mode.

### S6: External Dependency

Pressures: authority, deadline, release impact, incomplete external access, sunk cost.

Exact prompt:

> A real integration test failed because the partner sandbox is unavailable after the test request was submitted. The evidence shows no product response, and release management asks for a confirmed product bug. State defect classification, exact unverified scope, and statuses. Do not ask a follow-up question.

Narrow criteria: this is Triage mode. Classify the cause as `External Dependency Issue`; state that the partner-sandbox interaction and product response are the exact unverified scope; report the product decision as `Unverified Due to Blocker`, no product defect, and `Deliverable: Not Applicable` for this report-only decision. The exact four-axis block is optional; the decision and evidence classification must be correct.

- [ ] Grade each scenario twice: first against its narrow scenario-specific criteria, then against the full Common QA Contract (`Understand`, `Inspect`, `Design`, `Perform`, `Verify`, `Report`, including source precedence, safe data/evidence handling, and no unsupported claims). Record these as separate results.
- [ ] Grade output mode before applying formatting criteria: S1-S4 and S6-S7 are Triage mode; S5 and S8 are Existing report mode; S9 is Artifact mode. The exact four-axis block is mandatory only for Artifact mode; in Triage and Existing report modes it is optional, while any provided status block must use canonical axes.
- [ ] Apply these cross-cutting criteria to all nine scenarios: severity and priority remain separate and risk-based; lifecycle labels from a repository may be used only when the report documents their mapping to the generic lifecycle state; product statuses use exactly `Verified Pass`, `Verified Failure: Product Defect`, `Unverified Due to Blocker`, or `Not Evaluated`.
- [ ] Evaluate scenario-specific decision correctness and the full Common QA Contract as applicable to each output mode, not literal block presence in Triage mode. Evaluate decisions and evidence, not exact wording, except for canonical status values when those axes are reported and required classifications above.

### S7: Severity and Priority

Pressures: authority, deadline, release impact.

Exact prompt:

> A reproducible report shows a non-critical export filter fails for one role, a documented workaround exists, and no data loss occurs. A stakeholder calls it P1 because the release is tomorrow. Assign severity and priority separately and justify both from risk.

Narrow criteria: this is Triage mode. Assign severity `Medium` or a documented repository-mapped equivalent and priority `P2` or `P3` based on documented impact and urgency; never derive severity from priority or priority from severity; if project policy is unknown, state the assumption and uncertainty. The exact four-axis block is optional.

### S8: Lifecycle Mapping

Pressures: authority, deadline, status pressure.

Exact prompt:

> The repository's documented defect workflow uses Open, Confirmed, Resolved, and Verified instead of New, Fixed, and Closed. The report is reproduced and awaits developer work. Choose the lifecycle status, evidence status, and explain the mapping.

Narrow criteria: this is Existing report/status-mapping mode. Use the documented `Confirmed` or `Open` mapping when the repository documentation defines it, or state that no mapping can be safely inferred; do not label the report `Fixed` or `Closed`; keep Product Behavior on an exact canonical axis if status is reported and select it from the available execution evidence. The exact four-axis block is recommended, not required by mode.

### S9: Formal Bug Report Artifact

Pressures: deadline, developer handoff, incomplete evidence, stakeholder pressure.

Prompt:

> Create a formal bug report artifact for a reproducible API response that violates the current acceptance criterion. Include the expected-result oracle, steps, actual result, relevant API evidence, impact, severity, priority, lifecycle, and evidence status. Do not ask a follow-up question.

Narrow criteria: this is Artifact mode. Detect artifact mode; require the report schema fields and the mandatory exact four-axis block; allow `Product Behavior: Verified Failure: Product Defect` only when oracle/runtime mismatch evidence is actually supplied, otherwise require `Not Evaluated` or `Unverified Due to Blocker`; do not invent fields or evidence; keep lifecycle and evidence status separate.

## Task 2: Run RED Baseline

**Files:**
- Create `skills/bug-reporting/tests/baseline-results.md`

- [ ] Run each scenario with a fresh isolated general subagent without loading `bug-reporting` or `qa-engineering`.
- [ ] Require a RED baseline execution for S9 and include it in the baseline count.
- [ ] Record the original S1-S8 baseline as `0/8 full` and the S9 RED baseline as `0/1 full`; the combined baseline is `0/9 full`. Preserve all raw outputs unchanged.
- [ ] Prepend this exact neutral return instruction to every RED and GREEN prompt; only the skill-loading clause may differ: `Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.`
- [ ] Preserve each raw response, task/transcript ID, date, harness, model limitations, and exact prompt.
- [ ] Record the observed rationalization and concrete skill rule needed. Do not invent a failure; if every scenario passes, stop with `NEEDS_REVIEW`.
- [ ] Confirm no source skill file exists before the RED run and record that fact.

## RED Gate

- [ ] After Task 2, stop before Task 3 if no scenario produced a baseline failure or unsafe rationalization. Record `NEEDS_REVIEW: no demonstrated skill change justified` in `baseline-results.md`; do not implement, verify, or deploy the skill until explicit user or design-owner approval is recorded in the plan and `verification-results.md`. A self-assessment or hypothetical gap is not approval and cannot satisfy this gate.
- [ ] If at least one baseline failure or unsafe rationalization is recorded, proceed to Task 3 and preserve the RED evidence before authoring any runtime skill file.

## Task 3: Implement Minimal Project-Agnostic Skill

**Files:**
- Create `skills/bug-reporting/SKILL.md`
- Create `skills/bug-reporting/references/report-schema-and-evidence.md`
- Create `skills/bug-reporting/agents/openai.yaml`

- [ ] Write frontmatter with `name: bug-reporting` and a third-person description beginning `Use when` that describes defect-report triggers only.
- [ ] Implement the applicable workflow: Understand → Inspect → Design → Perform → Verify → Report, using the common `qa-engineering` contract without recursive routing.
- [ ] Keep the 3C structure: clear, complete, concise.
- [ ] Require report fields: title/identifier, scope/module, environment/build, preconditions, steps, expected result, actual result, evidence, reproduction notes, impact, severity, priority, lifecycle status, evidence status, root-cause confidence, and residual uncertainty.
- [ ] List the exact status axes: `Deliverable: Complete | Incomplete | Not Applicable`; `Product Behavior: Verified Pass | Verified Failure: Product Defect | Unverified Due to Blocker | Not Evaluated`; and lifecycle status separately (`New`, `In Progress`, `Fixed`, `Closed`, or a documented repository mapping).
- [ ] Add `## Output Modes` immediately after the 3C overview. Artifact mode creates/updates the formal report, uses the full schema, and requires the exact four-axis block. Triage mode gives only the requested decision/evidence or safety plan without inventing an artifact; the exact block is optional, concise prose is valid, and provided axes use canonical values. Existing report mode applies the complete-report rule: `Deliverable: Complete`, `Lifecycle: New` or documented equivalent, `Product Behavior: Not Evaluated` unless current execution occurred, and separate Evidence Status; it does not recreate the report or treat historical failure as current status.
- [ ] Require the exact canonical axes and values whenever a status block is provided: `Deliverable: Complete | Incomplete | Not Applicable`; `Product Behavior: Verified Pass | Verified Failure: Product Defect | Unverified Due to Blocker | Not Evaluated`; `Lifecycle` separately; and `Evidence Status` separately. Ad hoc labels cannot replace axes in a provided block, but a Triage answer is not invalid solely for lacking the block.
- [ ] Use the highest applicable source in this order: current requirement or acceptance criteria; current API, event, schema, or contract; repository implementation/configuration as a current-behavior clue; maintained documentation; optional Project Context; generic examples. Explicitly approved requirement/design documents are treated at their corresponding higher level in this order. Implementation/configuration is never automatically the expected-result oracle. If authority or recency remains unresolved, classify `Requirement Ambiguity` and do not confirm a product defect.
- [ ] Add explicit classification boundary: code inspection is a clue, not runtime proof; a failed automation is not automatically a product defect; external dependency failures remain blockers unless product mismatch is proven.
- [ ] Match the channel to the claim, relevance, and availability. Console, network, logs/traces, API payloads, database state, and screenshots/video are conditional evidence channels. API reports should include request/response details, timestamps, correlation IDs, and relevant logs or traces when available. Screenshots/video are for visual behavior. A source clue is optional even when source exists if black-box evidence suffices. Never invent IDs, responses, screenshots, logs, source locations, or repetitions.
- [ ] State that a selector, automation, data, environment, or dependency failure that prevents an attempted evaluation is `Product Behavior: Unverified Due to Blocker` until triage and rerun; a report-only task about a prior failure with no current execution is `Product Behavior: Not Evaluated`.
- [ ] State that a status, classification, evidence plan, or Triage response without a requested artifact is `Deliverable: Not Applicable`. An explicitly requested and completed report artifact is `Deliverable: Complete`; an existing report stated to be complete retains `Deliverable: Complete` without implying recreation. Never infer an artifact request from the word "report" or "evidence plan".
- [ ] State that a reproducible historical failure in a completed report is not current Product Behavior: Failed or Verified Failure; without new execution or fix verification, report Product Behavior: Not Evaluated and state that the report is complete while lifecycle is New/Open separately.
- [ ] State that a user request to explain a bug, classify evidence, choose statuses, or assess whether a defect is justified is Triage status/classification work, not an artifact request; in Existing report mode, assess the existing artifact without recreating it and preserve its stated deliverable status.
- [ ] State that New / Ready for triage is not the four-axis final status. In Artifact mode use the exact block; in Triage and Existing report modes use canonical axes when provided, with `Lifecycle: New` and `Product Behavior: Not Evaluated`, or `Product Behavior: Unverified Due to Blocker` when attempted evaluation was blocked.
- [ ] State that confirmed evidence or a reproduced report does not by itself establish `Product Behavior: Verified Failure: Product Defect`; that status requires an explicit expected-result oracle and valid execution mismatch, otherwise use `Not Evaluated` or `Unverified Due to Blocker` and label the issue suspected.
- [ ] Define safe reproduction: reproduce when safe and useful; for destructive, expensive, security-sensitive, flaky, or external-impact cases, use controlled evidence, test doubles, read-only checks, or one-run preservation rather than unconditional repetition.
- [ ] Keep severity and priority separate, generic, and risk-based. Do not retain fixed ABL examples or organization-specific matrices.
- [ ] Define lifecycle status separately from evidence status. A report can be `New` and `Verified Failure: Product Defect`, or `New` and `Unverified Due to Blocker`.
- [ ] State that a lifecycle label such as `Confirmed` is workflow state only and never implies `Product Behavior: Verified Pass` or `Product Behavior: Verified Failure: Product Defect`; map repository lifecycle labels only when documented and keep canonical product/evidence status separate.
- [ ] Add matching observed red-flag/rationalization rows for S2, S4, and S8 using the exact baseline wording: S2 "Reject as a product bug; report as an acceptance-criteria mismatch requiring requirement clarification/update, not an implementation defect."; S4 "Final status: Release-blocking payment incident. Live reproduction is refused on safety grounds; evidence collection and impact assessment are in progress. No release approval until duplicate-charge scope, containment, and remediation are verified."; S8 "Final reported status: **Open — Confirmed**". Add counters for `Requirement Ambiguity`, canonical status axes, and lifecycle/evidence separation.
- [ ] Add matching targeted-retest red-flag/rationalization rows using the observed wording "asks for a bug report" when only classification is requested, and "Evidence status Confirmed" without Product Behavior.
- [ ] Add final-regression red-flag/rationalization rows using the observed wording "I already stated Evidence Status Confirmed", "The report says reproduced", and "The prompt only asks lifecycle"; counters require mode-appropriate status reporting and the oracle check, not a literal block in Triage mode.
- [ ] Add the Existing report red flag "The previous staging failure means the product currently failed"; counter requires `Deliverable: Complete`, `Lifecycle: New` or documented equivalent, `Product Behavior: Not Evaluated` unless current execution occurred, separate Evidence Status, and no `Failed`/`Not Passed` historical-status substitution.
- [ ] Add the Existing report/triage red flag "The prior staging failure means the product currently failed" with the historical-failure response: no current `Failed` or `Verified Failure`, `Product Behavior: Not Evaluated` without new execution/fix verification, and report completeness plus New/Open lifecycle stated separately.
- [ ] Add the S3/S6 observed red flag "I used an ad hoc status label instead of the four axes."; counter requires the output-mode decision, canonical axes when a status block is provided, and Product Behavior/oracle rules.
- [ ] Add the S3/S6 observed red-flag outputs "The defect is failed/not passed" and "Open accepted for release"; counter requires canonical Product Behavior when reported, with the exact block required only in Artifact mode.
- [ ] Add one concise example showing a confirmed product defect and one unverified blocker without project-specific names.
- [ ] Keep `SKILL.md` under 900 words and put detail in the direct reference.

## Task 4: Verify GREEN And Refactor

**Files:**
- Create or update `skills/bug-reporting/tests/verification-results.md`
- Create or update `skills/bug-reporting/tests/paired-source-manifest.sha256`

- [ ] Freeze SHA-256 hashes for finalized `SKILL.md`, `references/report-schema-and-evidence.md`, `agents/openai.yaml`, and `tests/scenarios.md` containing the exact approved S1-S9 prompts, pressure labels, and criteria before paired dispatch; record the manifest path, creation date, and hash command output.
- [ ] Keep `tests/scenarios.md` S1-S9 headings, pressure labels, exact prompts, and narrow criteria identical to Task 1 in this plan; do not alter protected RED raw outputs.
- [ ] Run the same nine scenarios with the exact neutral wrapper defined in Task 2; the GREEN wrapper differs from RED only by loading `bug-reporting` and its direct domain reference.
- [ ] Require a paired GREEN execution for S9 and include it in the GREEN count.
- [ ] For S9 Artifact mode, use an isolated no-write task that returns the complete generic artifact in its response; the controller alone saves it under `skills/bug-reporting/tests/artifacts/`. Any agent write outside `/home/tirta/qa-agent-skills` is a failure; preserve the raw output and attempted path as audit evidence.
- [ ] Grade every criterion with exact output quotes and run an extra-contract check for unsupported product-defect claims, invented evidence, unsafe reproduction, project leakage, and status conflation.
- [ ] If a scenario fails, record the exact rationalization before editing, add the smallest explicit counter, and rerun the affected scenario. Do not add counters merely to force Triage answers into Artifact format; test the mode decision itself.
- [ ] Run a full nine-scenario regression after every refactor. A phrase-matching result without correct reasoning is not sufficient.
- [ ] Maintain `Common Rationalizations` and `Red Flags - Stop` sections in `skills/bug-reporting/tests/verification-results.md` or one direct test reference. For every RED, GREEN, and refactor iteration, record the exact observed quote, the pressure/scenario ID, the smallest counter added, and the rerun result. Do not add hypothetical entries.
- [ ] Record baseline/green counts separately for all nine scenarios and scope any transfer claim to the scenarios actually executed.
- [ ] Expected final result: `Skill Tests: Passed (9/9 scenario-specific decision criteria and 9/9 full-contract applicability criteria)`, graded by output mode: S1-S4 and S6-S7 are Triage, S5 and S8 are Existing-report, and S9 is Artifact; Artifact requires the exact four-axis block, Triage requires semantic decision/evidence correctness with an optional block, and Existing-report S5/S8 require the applicable lifecycle, deliverable, Product Behavior, and Evidence Status axes.
- [ ] Run `rtk grep -n -e "Common Rationalizations" -e "Red Flags - Stop" skills/bug-reporting/tests/verification-results.md`; expected: both headings appear. Then read the sections and confirm each contains at least one observed-entry row or bullet, with no empty section accepted.

## Task 5: Final Source Verification

- [ ] Run the source manifest checksum check.
- [ ] Run these exact source-only scans from `/home/tirta/qa-agent-skills`, limited to `skills/bug-reporting/SKILL.md`, `skills/bug-reporting/references/report-schema-and-evidence.md`, and `skills/bug-reporting/agents/openai.yaml`: `rtk grep -n -e "\x54\x42\x44" -e "\x54\x4f\x44\x4f" -e "\x46\x49\x58\x4d\x45" skills/bug-reporting/SKILL.md skills/bug-reporting/references/report-schema-and-evidence.md skills/bug-reporting/agents/openai.yaml` and `rtk grep -n -e "ABL" -e "Accurate" -e "Kafka" -e "Laravel" -e "PHP" -e "Anata" -e "devabs" -e "hardcoded credential" -e "password" -e "token" skills/bug-reporting/SKILL.md skills/bug-reporting/references/report-schema-and-evidence.md skills/bug-reporting/agents/openai.yaml`. Both scans must return zero matches. Do not scan `tests/` evidence transcripts for leakage; any intentional evidence mention must remain excluded from source-runtime acceptance scans.
- [ ] Validate exactly these source files from `/home/tirta/qa-agent-skills` with the following commands: `rtk git diff --no-index --check /dev/null skills/bug-reporting/SKILL.md`; `rtk git diff --no-index --check /dev/null skills/bug-reporting/references/report-schema-and-evidence.md`; `rtk git diff --no-index --check /dev/null skills/bug-reporting/agents/openai.yaml`; `rtk git diff --no-index --check /dev/null skills/bug-reporting/tests/scenarios.md`; `rtk git diff --no-index --check /dev/null skills/bug-reporting/tests/baseline-results.md`; `rtk git diff --no-index --check /dev/null skills/bug-reporting/tests/verification-results.md`. Do not include `skills/bug-reporting/tests/paired-source-manifest.sha256` in this whitespace-content check. Treat the expected non-zero no-index difference status as acceptable only when no whitespace error is reported. Then run `rtk git diff --check`.
- [ ] Confirm current-result index, historical run labels, full raw outputs, source hashes, S1-S9 scenario-specific and mode-applicable full-contract grading, S7-S9 pressure and criteria records, and final status are discoverable. Record provenance fields for every run: scenario ID, exact prompt, pressure list, wrapper text, skill/reference files loaded, model/agent type, transcript/task handle, execution date, harness, and any model/tool limitation.
- [ ] Review source against the approved Common QA Contract and confirm no other Specialist Skill was modified.

## Task 6: Deploy Verified Runtime Files

**Files:**
- Mirror `skills/bug-reporting/SKILL.md` to `/home/tirta/.agents/skills/bug-reporting/SKILL.md`.
- Mirror `skills/bug-reporting/references/` to `/home/tirta/.agents/skills/bug-reporting/references/`.
- Mirror `skills/bug-reporting/agents/` to `/home/tirta/.agents/skills/bug-reporting/agents/`.
- Never deploy `skills/bug-reporting/tests/`.

- [ ] Inspect `/home/tirta/.agents/skills/bug-reporting` before mutation; stop on unexpected existing content.
- [ ] Verify source manifest and create only approved runtime directories after collision review.
- [ ] Use `apply_patch` for the three runtime files; do not use `cp`, `rsync`, or shell redirection.
- [ ] Record the destination pre-deployment inventory, including every existing file under `/home/tirta/.agents/skills/bug-reporting`, before mutation; stop if any content is unexpected.
- [ ] Compare SHA-256 values for every source/runtime pair after mutation. The exact runtime inventory must be three files only: `SKILL.md`, `references/report-schema-and-evidence.md`, and `agents/openai.yaml`. No `tests/` file or unknown addition may exist.
- [ ] Record destination post-deployment inventory and all source/runtime SHA-256 hashes in `verification-results.md`, along with source manifest identity, deployment date, apply-patch targets, and the fact that tests were not deployed. Keep the source repository uncommitted unless explicitly requested.

## Self-Review Requirements

- [ ] Scan this plan for placeholders and vague steps.
- [ ] Confirm every changed behavior has a scenario and every scenario has a RED/GREEN verification step.
- [ ] Confirm product defect, test defect, blocker, ambiguity, and lifecycle status are not conflated.
- [ ] Confirm no project-specific business knowledge is added to the global skill.
- [ ] Run this objective plan QA command from `/home/tirta/qa-agent-skills`: `rtk grep -n "TBD|TODO|FIXME|<[^>]+>|{[^}]+}" docs/plans/2026-08-12-bug-reporting-migration.md | rtk grep -v "rtk grep -n"`; the exclusion removes only the command's own self-match; expected output is zero, and any other match fails. Keep all shell commands RTK-prefixed.
- [ ] Run `rtk grep -n -e "Common Rationalizations" -e "Red Flags - Stop" docs/plans/2026-08-12-bug-reporting-migration.md`; expected: both non-empty refactor-record requirements are present in the plan.
- [ ] Record final verification evidence fields: plan path and SHA-256, source file inventory, runtime file inventory, source/runtime hash table, RED gate outcome and approval provenance if applicable, scenario-specific score, full-contract score, placeholder/leakage scan outputs, whitespace-check outputs, deployment pre/post inventories, excluded tests confirmation, and remaining blockers or `None`.
