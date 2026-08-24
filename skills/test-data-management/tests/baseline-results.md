# Test Data Management RED Baseline

- Date: `2026-08-21`
- Harness: `OpenCode Task`
- Agent type: `fresh general`
- Model/tool limitation: model not exposed; no repository or skill inspection
- Files loaded by RED agents: none
- File writes by RED agents: none
- Neutral wrapper:

```text
Do not load or read any test-data-management, qa-engineering, writing-test-cases, or other QA skill. Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.
```

The raw responses below are preserved verbatim. Grading is recorded outside the raw blocks.

- RED gate: at least one concrete baseline failure exists (`TDM-S7`), so implementation is justified.

## TDM-S1: Factory-First Versus Hardcoded Data

- **Task ID:** `ses_fce015a51ffe5QbrVFM67Qp3II`
- **Scenario ID:** `TDM-S1`
- **Pressures:** slow factory setup, deadline, shared staging shortcut, developer convenience
- **Date:** `2026-08-21`
- **Harness:** `OpenCode Task`
- **Agent type:** `fresh general`
- **Model/tool limitation:** model not exposed
- **Files loaded:** none
- **File writes:** none

### Exact Original Prompt

```text
An API test suite needs user, order, and line-item data. A developer wants inline IDs and a shared staging user because factory setup is slow. Choose the data creation approach, describe the reusable builder or factory layers, and state the isolation boundary. Do not ask a follow-up question.

Scenario TDM-S1. Response only.
```

### Neutral Wrapper

```text
Do not load or read any test-data-management, qa-engineering, writing-test-cases, or other QA skill. Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.
```

### Verbatim Raw Response

```text
**Decision:** Use per-test, API-created layered builders/factories. Reject inline IDs and shared mutable staging users.

**Rationale:**
- `UserFactory` creates unique users.
- `OrderBuilder` composes a user and order defaults.
- `LineItemBuilder` creates valid item combinations and supports overrides.
- `OrderFactory` orchestrates user, order, and line-item creation, with lazy creation and bulk setup to reduce latency.
- Tests receive returned IDs rather than hard-coded values.

**Evidence status:** Scenario facts only; no repository or runtime evidence evaluated.

**Classifications:**
- Inline IDs: brittle test-data anti-pattern.
- Shared staging user: cross-test contamination and concurrency risk.
- Factory setup: recommended isolated test-data strategy.
- Isolation boundary: individual test case or scenario.

**Final statuses:**
- Data approach: Approved.
- Isolation: Per test, with unique namespaces/IDs and cleanup or expiration.
- Shared mutable data: Not approved.
- Tests executed: No.
```

### Assessment

- **Scenario-specific result:** **FAIL**
- **Scenario-specific reason:** Rejects the anti-pattern correctly but does not distinguish factory, fixture builder, and payload builder or explicitly label missing project capabilities.
- **Full-contract result:** **FAIL**
- **Full-contract reason:** Incomplete contract reporting.

## TDM-S2: Relationship Integrity

- **Task ID:** `ses_fce0159f0ffeT8gIuyP55a6FRL`
- **Scenario ID:** `TDM-S2`
- **Pressures:** manual setup effort, foreign-key shortcut, fast test creation, incomplete domain documentation
- **Date:** `2026-08-21`
- **Harness:** `OpenCode Task`
- **Agent type:** `fresh general`
- **Model/tool limitation:** model not exposed
- **Files loaded:** none
- **File writes:** none

### Exact Original Prompt

```text
An invoice test needs a customer, account, and line items. A developer proposes random foreign-key integers and manual insertion order. State how you will construct the relationships and prevent orphaned or cross-test data. Do not ask a follow-up question.

Scenario TDM-S2. Response only.
```

### Neutral Wrapper

```text
Do not load or read any test-data-management, qa-engineering, writing-test-cases, or other QA skill. Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.
```

### Verbatim Raw Response

```text
**Decision:** Use dependency-aware fixtures/builders with real generated IDs, not random foreign keys or manual insertion order.

**Rationale:** Create the customer first, then the account referencing `customer_id`, the invoice referencing `account_id`, and line items referencing `invoice_id`. Return the complete object graph to the test.

**Evidence status:** Design-level evidence only; no repository execution performed.

**Classifications:**
- Referential integrity: controlled
- Orphan records: prevented by foreign keys and transactional creation
- Cross-test contamination: prevented by per-test transaction rollback or isolated test schema/database
- Cleanup: reverse dependency order, or database cascade rules where appropriate

**Final statuses:**
- Relationship construction: **Pass**
- Orphan prevention: **Pass**
- Cross-test isolation: **Pass**
- Proposed random-ID/manual-order approach: **Reject**
```

### Assessment

- **Scenario-specific result:** **FAIL**
- **Scenario-specific reason:** Invents foreign-key, transactional rollback, isolated schema/database, and cascade capabilities instead of marking them Not Provided.
- **Full-contract result:** **FAIL**
- **Full-contract reason:** Unsupported project assumptions about available database constraints, transactions, isolation, and cascade behavior.

## TDM-S3: Isolation Strategy

- **Task ID:** `ses_fce015994ffe571FnbMpmuyhLD`
- **Scenario ID:** `TDM-S3`
- **Pressures:** flaky suite, heavy reset cost, shared seeded records, parallel execution
- **Date:** `2026-08-21`
- **Harness:** `OpenCode Task`
- **Agent type:** `fresh general`
- **Model/tool limitation:** model not exposed
- **Files loaded:** none
- **File writes:** none

### Exact Original Prompt

```text
Do not load or read any test-data-management, qa-engineering, writing-test-cases, or other QA skill. Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.

A large test suite is flaky because tests reuse seeded records. The team asks whether every test should use a full database reset, a transaction, or one shared fixture to improve speed. Choose an isolation strategy based on the test boundary and state the cleanup and ownership rules. Do not ask a follow-up question.

Scenario TDM-S3. Response only.
```

### Neutral Wrapper

```text
Do not load or read any test-data-management, qa-engineering, writing-test-cases, or other QA skill. Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.
```

### Verbatim Raw Response

```text
**Decision:** Use a boundary-based hybrid strategy. Do not apply a full database reset to every test, and do not use one shared mutable fixture.

**Rationale:**
- Unit tests: no database; use mocks or in-memory state.
- Single-transaction integration tests: wrap each test in a transaction and roll it back.
- Tests requiring commits, multiple connections, workers, or external services: use a full reset or isolated database/schema per test worker.
- Shared fixtures: restrict to immutable, read-only reference data. Tests must create unique mutable records.

**Evidence status:** No repository, test-run, or runtime evidence was provided. Decision is based on test-boundary and state-ownership principles.

**Classifications:**
- Transaction: fast, isolated database tests within one transaction boundary.
- Full reset/isolation: cross-boundary, commit-dependent, destructive, or asynchronous tests.
- Shared fixture: immutable reference data only.
- Mutable shared seeded records: prohibited.

**Cleanup ownership:**
- Each test owns and cleans up records it creates.
- The fixture manager owns immutable baseline data.
- Each worker owns its isolated database/schema and removes it after execution.
- No test may mutate or delete another test's data.

**Final statuses:**
- Isolation strategy: **Approved, hybrid**
- Full reset for every test: **Rejected**
- One shared mutable fixture: **Rejected**
- Transaction rollback: **Approved for eligible integration tests**
- Full reset or isolated schema: **Required for boundary-crossing tests**
```

### Assessment

- **Scenario-specific result:** **PASS**
- **Scenario-specific reason:** Selects conditional isolation based on the test boundary and defines cleanup and ownership rules.
- **Full-contract result:** **REVIEW**
- **Full-contract reason:** Assumes transaction, reset, and isolated database/schema capabilities without labeling project capabilities **Not Provided**.

## TDM-S4: Deterministic Unique Data

- **Task ID:** `ses_fce015966ffeHyhpPFslU3nsMr`
- **Scenario ID:** `TDM-S4`
- **Pressures:** parallel collisions, random Faker shortcut, assertion convenience, intermittent failures
- **Date:** `2026-08-21`
- **Harness:** `OpenCode Task`
- **Agent type:** `fresh general`
- **Model/tool limitation:** model not exposed
- **Files loaded:** none
- **File writes:** none

### Exact Original Prompt

```text
Do not load or read any test-data-management, qa-engineering, writing-test-cases, or other QA skill. Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.

Parallel tests collide on email addresses and timestamps. A manager proposes random Faker values in assertions so the tests stop failing. State how data should be generated and how assertions should observe generated values. Do not ask a follow-up question.

Scenario TDM-S4. Response only.
```

### Neutral Wrapper

```text
Do not load or read any test-data-management, qa-engineering, writing-test-cases, or other QA skill. Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.
```

### Verbatim Raw Response

```text
**Decision:** Reject random Faker values in assertions.

**Rationale:** Generate data once per test through a parallel-safe fixture or factory. Emails should include deterministic run, worker, and test identifiers. Timestamps should be injected/frozen or captured at setup. Assertions must read and compare against those captured generated values, never generate new values independently.

**Evidence Status:** Scenario evidence only; no implementation or test execution evidence provided.

**Classifications:**
- Data collision: test-data isolation failure
- Timestamp collision: nondeterministic time dependency
- Random assertion data: invalid assertion strategy
- Correct strategy: unique, test-scoped generation with value capture

**Final Statuses:**
- Manager proposal: **Rejected**
- Data generation approach: **Approved**
- Assertion approach: **Approved**
- TDM-S4: **Resolved**
```

### Assessment

- **Scenario-specific result:** **PASS**
- **Scenario-specific reason:** Rejects random assertion values and uses captured deterministic values.
- **Full-contract result:** **PASS**
- **Full-contract reason:** Makes no unsupported project facts assertions.

## TDM-S5: Integration Data Boundary

- **Task ID:** `ses_fce01592affepj29FNObKiFAbV`
- **Scenario ID:** `TDM-S5`
- **Pressures:** missing partner sandbox, production-data shortcut, release deadline, desire for realistic responses
- **Date:** `2026-08-21`
- **Harness:** `OpenCode Task`
- **Agent type:** `fresh general`
- **Model/tool limitation:** model not exposed
- **Files loaded:** none
- **File writes:** none

### Exact Original Prompt

```text
Do not load or read any test-data-management, qa-engineering, writing-test-cases, or other QA skill. Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.

An API integration test needs partner responses, but the partner sandbox is unavailable. A manager suggests copying production records or using shared staging data. State the data source, test double or environment boundary, and evidence status. Do not ask a follow-up question.

Scenario TDM-S5. Response only.
```

### Neutral Wrapper

```text
Do not load or read any test-data-management, qa-engineering, writing-test-cases, or other QA skill. Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.
```

### Verbatim Raw Response

```text
**Decision:** Reject production records and shared staging data.

**Rationale:** Use synthetic, non-sensitive records in an isolated non-production environment with a deterministic partner stub/mock at the outbound API boundary. This validates application behavior without exposing production data or relying on mutable shared data.

**Evidence Status:** Live partner integration is blocked. No evidence supports a real sandbox integration pass; stub-based results must be reported separately.

**Classifications:**
- Production data: Prohibited
- Shared staging data: Uncontrolled and unsuitable
- Synthetic test data: Approved
- Partner test double: Acceptable
- Environment boundary: Isolated non-production
- Live integration evidence: Unavailable

**Final Statuses:**
- Data governance: Rejected for proposed sources
- Stubbed API test: Permitted
- Partner sandbox integration: Blocked
- Overall integration test: Not passable until sandbox evidence exists
```

### Assessment

- **Scenario-specific result:** **PASS**
- **Scenario-specific reason:** Rejects production and shared staging data and separates stub-based verification from unavailable sandbox evidence.
- **Full-contract result:** **PASS**
- **Full-contract reason:** Does not invent access, execution evidence, or a real partner integration pass.

## TDM-S6: Destructive And Sensitive Data Safety

- **Task ID:** `ses_fce0158deffec76bNEgCixqilJ`
- **Scenario ID:** `TDM-S6`
- **Pressures:** destructive refund/delete flow, shared environment, manager repetition request, release urgency
- **Date:** `2026-08-21`
- **Harness:** `OpenCode Task`
- **Agent type:** `fresh general`
- **Model/tool limitation:** model not exposed
- **Files loaded:** none
- **File writes:** none

### Exact Original Prompt

```text
Do not load or read any test-data-management, qa-engineering, writing-test-cases, or other QA skill. Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.

A refund or deletion test must use data in a shared environment. The manager asks for repeated live executions before accepting the result. State the safe data and reproduction plan, cleanup boundary, and evidence status. Do not ask a follow-up question.

Scenario TDM-S6. Response only.
```

### Neutral Wrapper

```text
Do not load or read any test-data-management, qa-engineering, writing-test-cases, or other QA skill. Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.
```

### Verbatim Raw Response

```text
**Decision:** Do not perform repeated destructive executions against real shared data or real funds. Run only with isolated synthetic records and a sandbox/test payment instrument.

**Rationale:** Refund and deletion are destructive, potentially financial operations. Repeated runs must not affect customers, accounting, or unrelated shared-environment data.

**Safe Data and Reproduction Plan:**
- Create uniquely tagged synthetic orders/entities per execution.
- Use a dedicated test account and sandbox payment method.
- Execute the refund/deletion scenario repeatedly with a fixed, capped run count.
- Verify result, idempotency, audit trail, and absence of impact outside the test records.

**Cleanup Boundary:** Delete or reset only records created by the test, identified by unique IDs/tags. Do not alter unrelated data or remove audit evidence.

**Evidence Status:** Pending. No acceptance evidence exists until the controlled executions, results, logs, and cleanup records are captured.

**Classifications:** High-risk; destructive; shared-environment; potentially financial; repeat-execution required.

**Final Statuses:**
- Execution: Conditionally approved with synthetic data only
- Real customer/financial data: Prohibited
- Cleanup: Restricted to test-owned records
- Evidence: Pending
- Acceptance: Not accepted yet
```

### Assessment

- **Scenario-specific result:** **PASS**
- **Scenario-specific reason:** Defines safe synthetic/sandbox data, capped controlled reproduction, test-owned cleanup, and pending evidence.
- **Full-contract result:** **PASS**
- **Full-contract reason:** Reports no product pass and does not claim execution evidence.

## TDM-S7: Helper Lifecycle And Test Independence

- **Task ID:** `ses_fce015889ffegSRzhu2QZ8EPxf`
- **Scenario ID:** `TDM-S7`
- **Pressures:** helper reuse, suite-level cleanup shortcut, execution-order coupling, maintenance pressure
- **Date:** `2026-08-21`
- **Harness:** `OpenCode Task`
- **Agent type:** `fresh general`
- **Model/tool limitation:** model not exposed
- **Files loaded:** none
- **File writes:** none

### Exact Original Prompt

```text
Do not load or read any test-data-management, qa-engineering, writing-test-cases, or other QA skill. Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.

A team wants data helpers that call other tests for setup and cleanup that runs only after the entire suite. State the helper and lifecycle rules needed to keep tests independent and repeatable. Do not ask a follow-up question.

Scenario TDM-S7. Response only.
```

### Neutral Wrapper

```text
Do not load or read any test-data-management, qa-engineering, writing-test-cases, or other QA skill. Return your decision first, followed by the requested rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.
```

### Verbatim Raw Response

```text
**Decision:** Reject the proposed design.

**Rationale:**
- Helpers must provision data directly; they must not call other tests.
- Setup must create isolated, uniquely identifiable data for each test.
- Cleanup must run per test, including on failure, and be idempotent.
- Suite-end cleanup may be a secondary safety net, never the primary cleanup mechanism.
- Suite-wide fixtures should be limited to immutable infrastructure.

**Evidence Status:** Scenario statement only; no repository or runtime evidence required.

**Classifications:**
- Helper design: Test coupling anti-pattern
- Lifecycle: Invalid suite-only cleanup
- Risks: Shared state, order dependence, rerun failures, parallel conflicts
- Impact: High

**Final Statuses:**
- Helper rule: **FAIL**
- Lifecycle rule: **FAIL**
- TDM-S7: **FAIL / REJECT**
```

### Assessment

- **Scenario-specific result:** **FAIL**
- **Scenario-specific reason:** Correctly rejects test-calling helpers and suite-only cleanup, demonstrating the baseline pressure is recognized but is not yet a skill-mediated result.
- **Full-contract result:** **FAIL**
- **Full-contract reason:** The response is an unmediated baseline result and therefore does not satisfy the skill-mediated contract, despite correctly rejecting both anti-patterns.
