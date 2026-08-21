# Report Schema And Evidence

Use this template in Artifact mode. Enter observed information for each field or `Not Applicable`; do not leave unsupported claims.

## Output Modes

- **Artifact mode:** Create or update the formal report with the complete schema below and the exact four-axis status block.
- **Triage mode:** For analysis, classification, evidence planning, safety planning, or defect-justification decisions, do not invent a formal report artifact. Give the decision and evidence plan. The exact block is optional; canonical axes are recommended when status is discussed, and concise prose is valid when it remains decision-correct.
- **Existing report mode:** When the prompt states that a complete bug report already exists and asks only whether it is New/Fixed or whether the product passed, report `Deliverable: Complete`, `Lifecycle: New` (or documented equivalent), `Product Behavior: Not Evaluated` unless current execution occurred, and `Evidence Status` separately. Never use Failed, Not Passed, or a historical failure as current product status. Do not recreate the report; if a status block is provided, use all four canonical axes.

A reproducible historical failure in a completed report is not current Product Behavior: Failed or Verified Failure. If no new execution or fix verification occurred, report Product Behavior: Not Evaluated. Say the report is complete and the lifecycle is New/Open separately.

## Complete Report Template

```markdown
# Title
- ID:
- Scope/module:
- Environment/build:
- Preconditions:
- Steps:
  1.
  2.
- Expected result oracle/source:
- Actual result:
- Evidence by channel:
  - API:
  - Visual: Not Applicable unless visual behavior is at issue
  - Source clue: Not Applicable when source is inaccessible or the system is black-box
- Reproduction notes:
- Impact:
- Severity:
- Priority:
- Lifecycle status:
- Evidence status:
- Root-cause confidence:
- Residual uncertainty:
- Deliverable: Complete/Incomplete/Not Applicable
- Product Behavior: Verified Pass/Verified Failure: Product Defect/Unverified Due to Blocker/Not Evaluated
```

A status, classification, evidence plan, or triage response without a requested artifact is `Deliverable: Not Applicable`. An explicitly requested and completed report artifact is `Deliverable: Complete`; an existing report stated to be complete retains `Deliverable: Complete` without implying that it was recreated. Never infer an artifact request from the word "report" or "evidence plan".

A user request to explain a bug, classify evidence, choose statuses, or assess whether a defect is justified is status/classification work, not an artifact request. Use `Deliverable: Not Applicable`. Use `Deliverable: Complete` only when the user requests creation or update of a bug report artifact and its required fields are filled and validated.

Artifact mode requires exactly these four labeled axes: `Deliverable`, `Product Behavior`, `Lifecycle`, and `Evidence Status`. In Triage and Existing report modes, the axes are recommended when status is discussed, but the exact block is optional. If a status block is provided, never let evidence or lifecycle status replace Product Behavior. Product Behavior must use `Verified Pass`, `Verified Failure: Product Defect`, `Unverified Due to Blocker`, or `Not Evaluated`; lifecycle values such as `Open`, `Confirmed`, `Resolved`, and `Verified`, and Evidence Status `Confirmed`, do not replace it.

New / Ready for triage is not the four-axis final status. In Artifact mode use the exact block; in Triage and Existing report modes, use the canonical axes when provided, with `Lifecycle: New` and `Product Behavior: Not Evaluated`, or `Product Behavior: Unverified Due to Blocker` when attempted evaluation was blocked.

Never report Product Behavior as Failed, Not Passed, Unverified, or any ad hoc label when reporting that axis. Map it to exactly `Verified Pass`, `Verified Failure: Product Defect`, `Unverified Due to Blocker`, or `Not Evaluated`. In Artifact mode, or whenever a status block is provided, lifecycle or severity/priority prose cannot replace the axes.

## Artifact Mode Status Block

Artifact mode responses must end with this exact four-line status block. Triage and Existing report responses may use concise prose instead; if they provide a status block, do not replace labels or omit lines.

```text
Deliverable: <Complete | Incomplete | Not Applicable>
Product Behavior: <Verified Pass | Verified Failure: Product Defect | Unverified Due to Blocker | Not Evaluated>
Lifecycle: <documented or generic lifecycle>
Evidence Status: <scope and strength>
```

This block is mandatory only for Artifact mode. In Triage and Existing report modes, release, test, defect, and lifecycle decisions may be stated in concise prose, with canonical axes recommended when status is discussed.

### Triage Classification

| Classification | Use when | Product Behavior rule |
| --- | --- | --- |
| Product Defect | Valid oracle and execution show product mismatch | `Verified Failure: Product Defect` |
| Test/Automation Defect | Test, assertion, locator, harness, or setup is wrong | Do not infer a product defect |
| Test Data Issue | Data is missing, invalid, stale, or unsuitable | Usually `Unverified Due to Blocker` |
| Environment/Configuration Issue | Runtime, deployment, feature setting, or configuration blocks or changes evaluation | Usually `Unverified Due to Blocker` |
| External Dependency Issue | A required outside service or system is unavailable or invalid | `Unverified Due to Blocker` unless an independent product mismatch is proven |
| Requirement Ambiguity | Authority or recency conflict leaves expected behavior unresolved | Do not confirm a product defect |

## Oracle Order

Use the highest applicable source in this order: current requirement or acceptance criteria; current API, event, schema, or contract; repository implementation/configuration as a current-behavior clue; maintained documentation; optional Project Context; generic examples. Explicitly approved requirement/design documents are treated at their corresponding higher level in this order. Implementation/configuration is never automatically the expected-result oracle. If authority or recency remains unresolved, classify `Requirement Ambiguity` and do not confirm a product defect.

## Evidence And Reproduction

Match the channel to the claim, relevance, and availability. Console, network, logs/traces, API payloads, database state, and screenshots/video are conditional evidence channels. API reports should include request/response details, timestamps, correlation IDs, and relevant logs or traces when available. Screenshots/video are for visual behavior. A source clue is optional even when source exists if black-box evidence suffices. Never invent IDs, responses, screenshots, logs, source locations, or repetitions.

A selector, automation, data, environment, or dependency failure that prevents an attempted evaluation is `Product Behavior: Unverified Due to Blocker` until triage and rerun; do not relabel it `Not Evaluated`. A report-only task about a prior failure with no current execution is `Product Behavior: Not Evaluated`.

Confirmed evidence or a reproduced report does not by itself establish `Product Behavior: Verified Failure: Product Defect`. That status requires an explicit expected-result oracle and valid execution mismatch; otherwise use `Not Evaluated` or `Unverified Due to Blocker` and label the issue suspected.

Do not repeat destructive, costly, security-sensitive, flaky, or externally impactful actions just to satisfy a requested count. Preserve original state, transaction records, and telemetry. Prefer read-only reconciliation, a sandbox, test doubles, controlled fault injection, or an approved isolated check. Record what was not safely attempted and why.

## Severity And Priority Matrix

Use repository policy when present and cite it. Otherwise these defaults are only a starting point:

| Risk signal | Default severity | Default priority |
| --- | --- | --- |
| Broad outage, safety/security harm, severe data loss, no workaround | Critical | P1 |
| Major workflow blocked, substantial scope or harm, weak workaround | High | P1-P2 |
| Limited scope or non-critical failure, manageable impact, workaround | Medium | P2-P3 |
| Cosmetic, low-impact, narrow issue with low operational risk | Low | P3-P4 |

Assess severity from harm, breadth, data loss, criticality, and workaround quality. Assess priority from urgency, dependency, exposure, and scheduling. Do not derive one label from the other; release date alone does not justify P1.

## Lifecycle Mapping

Prefer labels documented by the repository. Cite the documented mapping next to the lifecycle value. If none exists, use generic `New`, `In Progress`, `Fixed`, or `Closed`. For a documented mapping such as `Open`, `Confirmed`, `Resolved`, `Verified`, state the mapping explicitly, for example: `New -> Open`, `Confirmed evidence with work item still open -> Open`, `Fixed -> Resolved`, and `Closed after successful verification -> Verified`. Lifecycle `Confirmed` is workflow state only; it never implies `Product Behavior: Verified Pass` or `Verified Failure: Product Defect`. Product verification still needs a supported oracle and valid runtime evidence. Do not infer `Fixed` or `Closed` before fix verification. Evidence status remains independent.

### Observed Red Flags And Counters

| Scenario | Observed wording | Counter |
| --- | --- | --- |
| S2 | "Reject as a product bug; report as an acceptance-criteria mismatch requiring requirement clarification/update, not an implementation defect." | Use `Requirement Ambiguity` when authority or recency is unresolved and give the correct decision; if axes are provided, use canonical values. |
| S4 | "Final status: Release-blocking payment incident. Live reproduction is refused on safety grounds; evidence collection and impact assessment are in progress. No release approval until duplicate-charge scope, containment, and remediation are verified." | Incident wording does not replace status axes; Artifact mode requires the block, while Triage mode may use concise safe-plan prose with a correct decision. |
| S8 | "Final reported status: **Open — Confirmed**" | `Confirmed` is workflow state only and never implies either product defect status. |
| Targeted retest | "asks for a bug report" | A classification request is not an artifact request; use `Deliverable: Not Applicable` unless artifact creation/update is explicitly requested. |
| Targeted retest | "Evidence status Confirmed" | Evidence status does not replace Product Behavior when status axes are reported; a Triage response may omit the block but must still make the product decision correctly. |
| Final regression | "I already stated Evidence Status Confirmed" | Require canonical Product Behavior when status is reported and the exact block in Artifact mode; evidence status alone is insufficient. |
| Final regression | "The report says reproduced" | Require an explicit expected-result oracle and valid execution mismatch before confirming a product defect; apply the exact-block requirement only in Artifact mode. |
| Final regression | "The prompt only asks lifecycle" | In Existing report mode, assess lifecycle and current product status separately; canonical Product Behavior is required if status is reported, but the exact block is optional. |
| Existing report regression | "The previous staging failure means the product currently failed" | A historical failure is not current execution. Use `Deliverable: Complete`, `Lifecycle: New` or a documented equivalent, `Product Behavior: Not Evaluated` unless current execution occurred, and separate Evidence Status. Never use `Failed` or `Not Passed` as Product Behavior. |
| Existing report/triage regression | "The prior staging failure means the product currently failed" | A reproducible historical failure in a completed report is not current Product Behavior: Failed or Verified Failure. If no new execution or fix verification occurred, report Product Behavior: Not Evaluated. Say the report is complete and the lifecycle is New/Open separately. |
| S3/S6 retest | "I used an ad hoc status label instead of the four axes." | In Artifact mode, end with the exact four-line block; in Triage mode, concise prose is valid, but any provided status axes must be canonical. |
| S3/S6 retest | "The defect is failed/not passed" or "Open accepted for release" | Use canonical Product Behavior when that axis is reported; the exact block is required only in Artifact mode, and lifecycle/severity prose cannot replace it when a block is provided. |
| S3/S6 retest | "The plan or release recommendation is the status" | Test the output mode and decision: a plan or recommendation cannot establish a product defect without the oracle and valid execution evidence. |
| S3/S6 retest | "New / Ready for triage is not the four-axis final status." | In Artifact mode use the exact block; in Triage or Existing report mode use canonical axes when provided, with `Lifecycle: New` plus `Product Behavior: Not Evaluated` or `Unverified Due to Blocker` when applicable. |

## Examples

### Confirmed Product Defect

`Requirement: invalid input must be rejected. Execution: controlled request returns success with a valid correlation ID; repeated safe checks show the same mismatch. Classification: Product Defect. Lifecycle: New. Evidence status: Confirmed execution evidence. Deliverable: Complete. Product Behavior: Verified Failure: Product Defect.`

### Blocker

`Required outside service is unavailable after submission; no product response was observed. Classification: External Dependency Issue. Unverified scope: receipt, response handling, mapping, retries, timeout handling, and final assertion. Deliverable: Not Applicable for a classification-only decision. Product Behavior: Unverified Due to Blocker. Lifecycle and evidence status are reported separately.`
