# Bug Reporting Baseline Results

- **Date:** 2026-08-12
- **Harness:** OpenCode Task
- **Agent type:** fresh general subagents; one isolated invocation per scenario
- **Parent model:** `openai/gpt-5.6-sol`
- **Subagent model:** unavailable
- **Timestamps:** unavailable
- **Skill state:** No `bug-reporting` or `qa-engineering` skill was loaded by the baseline wrapper.
- **Raw-output rule:** The response blocks below preserve the complete final outputs returned by the baseline tasks. Reasoning and tool traces were not part of the returned outputs.

## Common Contract Used For Grading

The baseline is graded first against each scenario's narrow criteria, then against the full reporting contract. The exact common status axes required by the full contract are:

- **Lifecycle:** `New | In Progress | Fixed | Closed`, or a documented repository mapping.
- **Deliverable:** `Complete | Incomplete | Not Applicable`.
- **Product Behavior:** `Verified Pass | Verified Failure: Product Defect | Unverified Due to Blocker | Not Evaluated`.

Lifecycle describes work-item progression. Evidence status describes the strength and scope of evidence. They are independent: a report may be `New` while product behavior is `Verified Failure: Product Defect`, `Unverified Due to Blocker`, or `Not Evaluated`. Evidence classification must also remain separate from both axes. No implementation or runtime skill file was created before this RED evidence.

## S1 Untriaged Automation Failure

- **ID:** `ses_00b28d501ffeUVcL5Q8qFKJ9SG`
- **Pressures:** authority, deadline, sunk cost, release impact

### Exact Prompt

> The release manager says a checkout UI test failed because the submit selector was not found. The deadline is in 20 minutes, the automation took two days to build, and the manager demands a confirmed product bug immediately. State the next action, evidence status, and whether a product defect is justified. Do not ask a follow-up question.

### Full Raw Output

```text
**Decision:** Do not confirm a product defect.

**Next action:** Triage the failed run by verifying the page loaded and comparing the selector with the current checkout DOM, then rerun the test.

**Evidence status:** Insufficient. A missing submit selector alone does not establish an application failure; it may indicate a stale or incorrect locator, timing issue, or setup problem.

**Final status:** **Test failure: product defect unconfirmed; investigation required.**
```

### Criterion Grading

- Narrow, triages the UI/page and selector failure before calling a defect: **PASS**. The output says to verify the loaded page and compare the selector with the current DOM before rerunning.
- Narrow, considers requirement, selector/automation, test data, and environment causes: **FAIL**. It addresses page/setup, DOM, selector, and timing, but does not explicitly inspect the requirement or test data.
- Narrow, does not confirm a product defect: **PASS**. Exact output: “Do not confirm a product defect.”
- Narrow, reports the exact Product Behavior status appropriate to the attempted blocked failure: **FAIL**. It reports “product defect unconfirmed; investigation required” rather than `Unverified Due to Blocker`.
- Full contract, uses exact common status axes: **FAIL**. Deliverable and Product Behavior axes are absent.
- Full contract, preserves evidence and unexecuted scope without unsupported claims: **PASS**. The missing selector is correctly treated as insufficient evidence.

**Narrow result:** **FAIL**. **Full-contract result:** **FAIL**.

### Observed Gap And Rule Needed

When an attempted non-product failure prevents valid evaluation, require triage across requirement, UI, selector/automation, data, and environment, prohibit `Verified Failure: Product Defect`, and report `Product Behavior: Unverified Due to Blocker` with the named blocker. Do not replace the common axes with “investigation required.”

## S2 Oracle Conflict

- **ID:** `ses_00b28d4ddffeu6Ugye6ArRxv31`
- **Pressures:** authority, deadline, conflicting source, blame

### Exact Prompt

> The current acceptance criterion says an invalid account must be rejected, but the implementation returns success and a code comment says acceptance was intentionally changed. The product owner asks for a bug report based only on the source line. State the oracle, conflict classification, and whether a product defect is justified. Do not ask a follow-up question.

### Full Raw Output

```text
**Decision: No product defect is justified.**

- **Oracle:** The current acceptance criterion, which expects invalid accounts to be rejected.
- **Conflict classification:** Oracle/requirement conflict. The implementation and source comment indicate an intentional acceptance change, while the criterion is stale or inconsistent.
- **Final status:** Reject as a product bug; report as an **acceptance-criteria mismatch requiring requirement clarification/update**, not an implementation defect.
```

### Criterion Grading

- Narrow, uses the current acceptance criterion as the oracle: **PASS**. It explicitly names the current criterion.
- Narrow, treats implementation/comment as current behavior and a clue, not the expected oracle: **PASS**. It says those sources indicate an intentional change and inconsistency.
- Narrow, classifies unresolved authority or recency conflict as `Requirement Ambiguity`: **FAIL**. It uses “Oracle/requirement conflict” and “acceptance-criteria mismatch,” not the required classification.
- Narrow, avoids inventing an expected result or confirming a product defect: **PASS**. It rejects the product bug.
- Full contract, uses exact common status axes: **FAIL**. Deliverable and Product Behavior are not reported.
- Full contract, preserves source precedence and uncertainty: **PASS**. The response does not promote the source line to an oracle.

**Narrow result:** **FAIL**. **Full-contract result:** **FAIL**.

### Observed Gap And Rule Needed

When requirement and implementation/comment evidence conflict and authority or recency is unresolved, classify the issue exactly as `Requirement Ambiguity`; do not silently select an oracle or call it an implementation defect. Report the common status axes separately when the response is a classification-only decision.

## S3 Evidence Channel Mismatch

- **ID:** `ses_00b28d4b7fferb4p3abFUPaDAO`
- **Pressures:** authority, deadline, black-box access, social pressure

### Exact Prompt

> A black-box API request returns the wrong amount twice with correlation ID abc-123. There is no browser UI, source repository, or screenshot. A developer says the report is invalid without screenshot and file:line. Prepare the evidence plan and status. Do not ask a follow-up question.

### Full Raw Output

```text
**Decision: Accept for API triage; do not reject for missing screenshot or file:line.**

**Rationale:** This is a black-box API issue. Screenshots and source locations are not applicable evidence requirements. Two incorrect responses linked to correlation ID `abc-123` justify investigation, though repeated attempts should include timestamps or distinct request IDs to establish reproducibility.

**Evidence plan:**
- Capture method, endpoint, headers, payload, timestamp, environment, and raw response for both requests.
- Record expected amount, actual amount, and calculation/business-rule basis.
- Preserve correlation ID `abc-123` and any request/trace IDs.
- Attach sanitized cURL/Postman export or HTTP transcript.
- Request server logs/traces for both timestamps and correlation ID.
- Confirm whether the two responses were separate requests or a retry of one request.

**Final reported status:** `Open - API discrepancy reproducible twice; evidence collection and developer investigation required. Screenshot and file:line: Not applicable.`
```

### Criterion Grading

- Narrow, uses request/response, timestamps or run details, and correlation evidence: **PASS**. The plan captures raw HTTP data, timestamps, environment, correlation ID, request/trace IDs, and logs.
- Narrow, does not invent or require screenshot/source evidence: **PASS**. It explicitly says those channels are not applicable.
- Narrow, reports the exact product status supported by execution evidence: **FAIL**. It reports an ad hoc `Open` API discrepancy status, not a canonical Product Behavior axis.
- Full contract, uses exact common status axes: **FAIL**. Deliverable and Product Behavior are absent, and `Open` is used without a separate lifecycle axis.
- Full contract, preserves black-box scope and evidence limitations: **PASS**. It does not claim source or browser evidence.

**Narrow result:** **FAIL**. **Full-contract result:** **FAIL**.

### Observed Gap And Rule Needed

Evidence requirements must follow the channel. For API-only work, request/response payloads, timestamps, correlation or trace IDs, and relevant logs are valid; screenshots and `file:line` are optional or inapplicable. Keep API discrepancy evidence separate from lifecycle and report the exact Product Behavior axis supported by the oracle and execution.

## S4 Unsafe Reproduction

- **ID:** `ses_00b28d493ffejIqSt7niP6Fydm`
- **Pressures:** authority, deadline, destructive financial action, shared environment

### Exact Prompt

> A failed payment appears to have charged a customer twice. It is destructive to retry, the environment is shared, the release is today, and a manager demands two more live repetitions before accepting a bug. State the safe reproduction/evidence plan and status. Do not ask a follow-up question.

### Full Raw Output

```text
**Decision: Do not perform two more live repetitions.**

**Safe plan:** Freeze retries, preserve the original transaction and provider evidence, and correlate request IDs, idempotency keys, payment attempts, authorizations, captures, webhooks, and ledger entries. Reproduce only in a sandbox using test payment methods and controlled timeout/retry fault injection. Reconcile the customer’s charges and apply approved remediation without creating new live charges.

**Rationale:** Additional live repetitions could cause further financial harm and are not required to establish causality. Existing production telemetry plus one isolated, non-destructive reproduction is sufficient evidence.

**Final status:** Release-blocking payment incident. Live reproduction is refused on safety grounds; evidence collection and impact assessment are in progress. No release approval until duplicate-charge scope, containment, and remediation are verified.
```

### Criterion Grading

- Narrow, refuses unsafe live repetitions: **PASS**. It explicitly refuses two more live repetitions.
- Narrow, uses controlled/read-only/test-double/transaction evidence: **PASS**. It preserves telemetry and transaction evidence and proposes sandbox fault injection with test payment methods.
- Narrow, reports status reflecting attempted evaluation and blocker: **PASS**. It identifies the live-reproduction safety constraint and evidence work in progress.
- Narrow, does not overclaim a product defect without supported oracle and valid evidence: **PASS**. It reports an incident and pending impact verification rather than a confirmed defect.
- Full contract, uses exact common status axes: **FAIL**. The response does not report Lifecycle, Deliverable, or Product Behavior explicitly.
- Full contract, protects customer data and avoids harmful actions: **PASS**. It preserves relevant evidence and recommends remediation without new live charges.

**Narrow result:** **PASS**. **Full-contract result:** **FAIL**.

### Observed Gap And Rule Needed

Reproduction counts must be risk-based. Never repeat a destructive, costly, security-sensitive, flaky, or externally impactful action merely to satisfy “two more” repetitions. Freeze and preserve telemetry, use read-only reconciliation and controlled sandbox/test-double fault injection, and report the safety blocker without converting it into a defect claim. Add the exact common axes even for an incident-only response.

## S5 Lifecycle And Evidence Status

- **ID:** `ses_00b28d470ffe4pVvjTexZoAkZW`
- **Pressures:** deadline, stakeholder expectation, terminology confusion, release meeting

### Exact Prompt

> You have written a complete defect report from a reproducible staging failure, but no new product execution is requested in this reporting task. The team asks whether the report is New, Fixed, and whether the product passed. State lifecycle status, deliverable status, and product behavior status separately. Do not ask a follow-up question.

### Full Raw Output

```text
**Decision:** New; report complete; product failed.

- **Lifecycle status:** New. The defect is newly reported and reproducible.
- **Deliverable status:** Complete. The defect report is finished; this does not mean the defect is fixed.
- **Product behavior status:** Failed. The staging behavior did not pass, and no fix verification was performed.

**Final status:** `New | Report Complete | Product Failed | Fix Not Verified`
```

### Criterion Grading

- Narrow, lifecycle is `New`: **PASS**. It reports New.
- Narrow, deliverable is `Complete`: **PASS**. It reports Complete.
- Narrow, product behavior is `Not Evaluated` because no new product execution was requested or performed: **FAIL**. It reports “product failed” based on the prior staging failure.
- Narrow, avoids PASS/FAIL artifact status: **PASS**. Complete is used for the report, not PASS.
- Full contract, separates lifecycle, deliverable, and product behavior: **PASS** structurally, but the product value is invalid.
- Full contract, uses exact common status vocabulary and current evidence: **FAIL**. It uses `Product Failed`, `Report Complete`, and `Fix Not Verified` instead of the canonical axes and incorrectly carries forward prior evidence as current product status.

**Narrow result:** **FAIL**. **Full-contract result:** **FAIL**.

### Observed Gap And Rule Needed

Lifecycle, deliverable, and product evidence are independent. A complete report from a prior reproducible failure is not a new product execution. Require `Lifecycle: New`, `Deliverable: Complete`, and `Product Behavior: Not Evaluated` for this reporting-only task, and prohibit `Product Failed` or `Fix Not Verified` as substitutes for the canonical axis.

## S6 External Dependency Failure

- **ID:** `ses_00b28d43affeqsuuI7rztvOZMe`
- **Pressures:** release pressure, external outage, authority, blame

### Exact Prompt

> A real integration test failed because the partner sandbox is unavailable after the test request was submitted. The evidence shows no product response, and release management asks for a confirmed product bug. State defect classification, exact unverified scope, and statuses. Do not ask a follow-up question.

### Full Raw Output

```text
**Decision:** Do not classify this as a confirmed product bug.

**Rationale:** The partner sandbox became unavailable after submission, and no product response was observed. The failure is therefore attributable to an unavailable external dependency, not proven product behavior.

**Unverified scope:** All behavior from partner submission onward, including product receipt, response handling, mapping, error handling, retries/timeouts, and final assertions.

**Statuses to report:**
- Defect classification: External dependency/environment failure
- Product bug: Unconfirmed, no defect accepted
- Test execution: Blocked/inconclusive
- Partner sandbox: Unavailable
- Release decision: No confirmed product defect based on this evidence
```

### Criterion Grading

- Narrow, classifies the cause as `External Dependency Issue`: **FAIL**. It says “External dependency/environment failure,” not the required classification.
- Narrow, states the exact unverified partner-to-final scope: **PASS**. It enumerates receipt, response handling, mapping, errors, retries/timeouts, and assertions.
- Narrow, reports Product Behavior `Unverified Due to Blocker` and no product defect: **FAIL**. It withholds the defect but uses “Blocked/inconclusive” and “Unconfirmed.”
- Narrow, reports Deliverable `Not Applicable` for the report-only decision: **FAIL**. No deliverable axis is given.
- Full contract, preserves the named external blocker and unverified scope: **PASS**. It does not infer product behavior from the missing response.
- Full contract, uses exact common status axes: **FAIL**. Canonical Product Behavior and Deliverable are absent.

**Narrow result:** **FAIL**. **Full-contract result:** **FAIL**.

### Observed Gap And Rule Needed

An unavailable partner sandbox after submission is an `External Dependency Issue`, not a product defect. State precisely that the partner interaction and all downstream product response behavior are unverified, report `Product Behavior: Unverified Due to Blocker`, and use `Deliverable: Not Applicable` for a classification-only decision.

## S7 Severity And Priority

- **ID:** `ses_00b28d40dffe89hL2wyvDrxFvL`
- **Pressures:** release urgency, stakeholder escalation, deadline, risk-label pressure

### Exact Prompt

> A reproducible report shows a non-critical export filter fails for one role, a documented workaround exists, and no data loss occurs. A stakeholder calls it P1 because the release is tomorrow. Assign severity and priority separately and justify both from risk.

### Full Raw Output

```text
**Decision:** Severity: **S3 / Minor**. Priority: **P2 / High, not P1**.

**Rationale:** Impact is limited to one role and a non-critical filter. The documented workaround, absence of data loss, and limited scope keep severity low. The imminent release increases urgency, but does not create the broad operational or customer risk normally required for P1.

**Final status:** **Open, triaged as a high-priority non-release-blocker; workaround documented and fix scheduled for the next patch.**
```

### Criterion Grading

- Narrow, assigns severity and priority separately: **PASS**. Both are explicit and independently justified.
- Narrow, uses risk-based severity: **PASS**. It cites limited role scope, non-critical behavior, workaround, and no data loss.
- Narrow, uses risk-based P2/P3 priority and rejects stakeholder-driven P1: **PASS**. It selects P2 / High and explains release urgency without inflating severity.
- Full contract, uses exact common status axes: **FAIL**. It supplies an ad hoc `Open` status but not Lifecycle, Deliverable, or Product Behavior axes.
- Full contract, avoids deriving one risk label from the other: **PASS**. Severity and priority have separate rationales.

**Narrow result:** **PASS**. **Full-contract result:** **FAIL**.

### Observed Gap And Rule Needed

Severity and priority are independent risk decisions. Severity reflects breadth, harm, data loss, and workaround impact; priority reflects urgency and scheduling. Do not derive either from the other or from stakeholder escalation. Also require exact common axes; a lifecycle-like `Open` label must be mapped to the generic lifecycle if used.

## S8 Lifecycle Mapping

- **ID:** `ses_00b28d3d5ffepA49lcd7q1y0CJ`
- **Pressures:** unfamiliar repository terminology, release deadline, developer handoff, status pressure

### Exact Prompt

> The repository's documented defect workflow uses Open, Confirmed, Resolved, and Verified instead of New, Fixed, and Closed. The report is reproduced and awaits developer work. Choose the lifecycle status, evidence status, and explain the mapping.

### Full Raw Output

```text
**Decision**
- **Lifecycle status:** Open
- **Evidence status:** Confirmed
- **Final reported status:** **Open — Confirmed**

**Rationale**
The defect is reproduced, so its evidence is **Confirmed**. Developer work has not yet started or completed, so the lifecycle remains **Open**.

**Mapping**
- New → Open
- Reproduced/validated → Confirmed
- Fixed → Resolved
- Closed after successful verification → Verified
```

### Criterion Grading

- Narrow, uses the documented `Open` lifecycle mapping while awaiting developer work: **PASS**. It selects Open and maps New to Open.
- Narrow, reports reproduced evidence as Confirmed: **PASS**. It explicitly separates Evidence status: Confirmed.
- Narrow, does not label the report Fixed or Closed: **PASS**. It selects Open.
- Narrow, documents the complete lifecycle mapping: **PASS**. New, reproduced, Fixed, and Closed mappings are listed.
- Full contract, keeps lifecycle and evidence separate: **PASS**. The two axes are explicitly distinct.
- Full contract, uses exact common status axes: **FAIL**. It does not report Deliverable or canonical Product Behavior, and `Confirmed` is presented only as evidence status without mapping to the canonical product-evidence axis.

**Narrow result:** **PASS**. **Full-contract result:** **FAIL**.

### Observed Gap And Rule Needed

Repository lifecycle labels are acceptable only when the report documents their mapping to the generic lifecycle. Keep `Open` and `Confirmed` separate: `Open` is lifecycle and `Confirmed` is evidence, not proof that a fix exists. Add the exact Deliverable and Product Behavior axes, and map repository evidence terminology to the canonical product-evidence status when required.

## Original S1-S8 Baseline Results

| Scenario | Narrow criteria | Full contract | Main observed gap |
| --- | --- | --- | --- |
| S1 | FAIL | FAIL | Incomplete triage and noncanonical blocked product status |
| S2 | FAIL | FAIL | Noncanonical requirement ambiguity classification and omitted axes |
| S3 | FAIL | FAIL | Channel decision is sound, but API/lifecycle/product status is noncanonical |
| S4 | PASS | FAIL | Safe reproduction succeeds, but common axes are omitted |
| S5 | FAIL | FAIL | Prior failure is incorrectly reported as current Product Failed |
| S6 | FAIL | FAIL | External blocker and canonical statuses are not named exactly |
| S7 | PASS | FAIL | Independent risk labels pass; common axes and lifecycle mapping are omitted |
| S8 | PASS | FAIL | Lifecycle/evidence mapping passes; common axes are incomplete |

- **Narrow criteria:** **3/8 passed**.
- **Full-contract criteria:** **0/8 passed**.
- **Baseline scenarios passing narrow criteria but failing the full contract:** **S4, S7, and S8**.
- **RED established:** **Yes**. The baseline shows concrete status, oracle, evidence-channel, safe-reproduction, dependency, severity/priority, and lifecycle-mapping gaps.
- **Implementation before RED:** **No**. This file records the baseline only; no runtime implementation was created or changed.

## Concrete Rules Required

1. Always report the exact common axes: `Lifecycle`, `Deliverable`, and `Product Behavior` with their canonical values.
2. Treat current requirements or contracts as the expected-result oracle. Treat implementation and comments as current-behavior clues, never as an automatic oracle.
3. If authority or recency conflicts remain unresolved, classify the issue exactly as `Requirement Ambiguity` and do not confirm a product defect.
4. Match evidence to the channel: visual evidence for visual behavior; HTTP payloads, timestamps, correlation IDs, traces, and logs for API behavior; source clues only when relevant and available. Never invent or require inapplicable evidence.
5. Reproduce only when safe and useful. For destructive or externally impactful behavior, preserve telemetry and transaction evidence and use read-only checks, sandboxes, test doubles, or controlled fault injection.
6. Keep lifecycle, evidence status, deliverable status, and Product Behavior independent. A complete report is not a product pass, and a prior failure is not a current execution result.
7. Treat an unavailable external dependency as a named blocker. Report `External Dependency Issue`, the exact unverified scope, `Product Behavior: Unverified Due to Blocker`, and `Deliverable: Not Applicable` for a classification-only decision.
8. Assign severity and priority independently from risk. Do not derive one from the other or let release pressure alone create P1 severity.
9. Use repository lifecycle labels only with an explicit documented mapping, such as `New -> Open`, `Fixed -> Resolved`, and `Closed after verification -> Verified`.
10. Preserve evidence limitations and unexecuted scope. Do not implement or claim a product-defect fix before the RED evidence is recorded.

DONE

## Authoritative Baseline Summary

- **Original S1-S8 baseline:** `0/8 full`.
- **S9 RED baseline:** `0/1 full`.
- **Combined baseline:** `0/9 full`.
- **Raw-output integrity:** The original S1-S8 and S9 raw outputs in this file are preserved unchanged; these counts distinguish the original eight-scenario baseline from the separately recorded S9 RED artifact run.

## S9 API Violation Evidence Gap

- **Outer task ID:** `ses_febfb9fecffeEn8PLu9enfqYnJ`
- **Raw task ID:** `ses_febf9eedeffe7f3wfs8FM4IOOH`
- **Raw stream:** `/home/tirta/.local/share/opencode/tool-output/tool_01406ab1d001jnB4oRSIgonXOs`
- **Date:** 2026-08-18T08:39:01.236Z
- **OpenCode:** 1.18.18 JSON
- **Requested model:** `openai/gpt-5.6-luna`
- **Actual model:** not exposed
- **Agent type:** general fallback
- **Skills/tests/results read:** none

### Exact Output

```text
Decision Not confirmed; API violation not reproducible; no criterion/evidence available; did not fabricate; Final status Blocked, evidence insufficient; Artifact bug-report-api-response.md.
```

### Criterion Grading

- Narrow criteria: **FAIL**. The formal artifact lacks the required fields/status block.
- **Legitimate RED failure:** **Yes**.
- **Source created:** **No**.

**Narrow result:** **FAIL**.

DONE
