# Test Data Management Paired Verification

- Date: `2026-08-26`
- Source revision: `aa8fe8f` plus the paired source manifest in this change
- Harness: `OpenCode Task`
- Agent type: `fresh general`
- Evaluators: seven paired GREEN agents, one per exact scenario
- Source files loaded by GREEN agents: `skills/test-data-management/SKILL.md` and its direct reference `skills/test-data-management/references/data-strategy-and-isolation.md`
- Files not loaded by GREEN agents: `tests/scenarios.md`, `baseline-results.md`, `verification-results.md`, `paired-source-manifest.sha256`, other QA skills, and other repositories
- File writes by GREEN agents: none
- Refactor state: source contract hardening was committed in `38736c0`; scenario criteria deduplication was committed in `aa8fe8f`; no scenario-specific counter was required after the final source freeze.

## Canonical GREEN Wrapper

```text
Load and follow only the canonical source skill `/home/tirta/.config/superpowers/worktrees/qa-agent-skills/pr-test-data-management/skills/test-data-management/SKILL.md` and the direct reference it requires. Do not read `tests/scenarios.md`, `baseline-results.md`, `verification-results.md`, the manifest, any other QA skill, or any other repository. Do not create or modify files. Your response will be recorded verbatim. Return the decision first, followed by rationale, evidence status, classifications, and final statuses. Do not ask a follow-up question.
```

Every evaluator also received its scenario's exact prompt, pressures, narrow criterion, and the common requirement to report separate canonical `Deliverable`, `Product Behavior`, and `Evidence Status` values. The source manifest was frozen before evaluation.

## Paired Source Manifest

```text
c66ada26ad025dc4034a94773740b7ed919bffcd9239910e8e13f85aa18cd7f8  skills/test-data-management/SKILL.md
3e81f2ca9b4b9fb725f71267fc13dee56a744db70922a7df341295b5b39b5e0a  skills/test-data-management/references/data-strategy-and-isolation.md
fad9355ee5f02f6edba183bcea0bd557325e007208683774538359143cb1a451  skills/test-data-management/agents/openai.yaml
659709d9152252b793e3a11445a339f957c67e62d26374ed649b7c930f4de346  skills/test-data-management/tests/scenarios.md
```

## TDM-S1: Factory-First Versus Hardcoded Data

- Task ID: `ses_fc3daaa6bffeGFP1b9K8wMY86F`
- Pressures: slow factory setup, deadline, shared staging shortcut, developer convenience
- Exact prompt: An API test suite needs user, order, and line-item data. A developer wants inline IDs and a shared staging user because factory setup is slow. Choose the data creation approach, describe the reusable builder or factory layers, and state the isolation boundary. Do not ask a follow-up question.
- Loaded files: canonical `SKILL.md` and direct reference only
- Files written: none

### Verbatim Raw Response

```text
Decision: GREEN

**Rationale**
- Reject inline IDs and shared staging users. Use synthetic, disposable, test-owned data.
- **Factories:** `userFactory`, `orderFactory`, and `lineItemFactory` provide valid defaults, controlled overrides, and explicit states. `build` is in-memory; `create` persists only when supported.
- **Fixture builder:** creates the user first, then the order using the generated user handle, then the line item using the generated order handle. Returns all entities, IDs, and generated values needed for assertions and cleanup.
- **Payload builder:** creates serializable API request/event bodies from captured handles; it does not persist data.
- Generate deterministic unique values scoped by run, worker, test, and stable counter. Cleanup is test-owned, idempotent, and runs after success, failure, or interruption.
- Isolation boundary: controlled disposable test datastore or isolated schema/sandbox. Queues, files, email, and partner effects require separate doubles or approved sandboxes.

**Evidence Status**
No product execution or project capability evidence was supplied.

**Classifications**
- Data strategy: Factory-first
- Data ownership: Test-owned synthetic data
- Relationships: Parent-before-child using captured generated handles
- Missing schema, persistence, reset/rollback, sandbox, cleanup, worker namespace, and external-double capabilities: `Not Provided`
- Hardcoded/shared staging data: Rejected

**Final Statuses**
- `Deliverable: Complete`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### Assessment

- Scenario-specific result: **PASS**
- Scenario-specific reason: Rejects hardcoded/shared data, distinguishes all three builder roles, requires test-owned synthetic data, scoped uniqueness, captured values, and `Not Provided` for unavailable capabilities.
- Full-contract result: **PASS**
- Full-contract reason: Reports separate status axes, uses no project-specific assumptions, defines isolation and external-effect limits, and makes no product execution claim.
- Final outcome: **PASS**

## TDM-S2: Relationship Integrity

- Task ID: `ses_fc3d8c213ffeJdGQbJhzmY9IED`
- Pressures: manual setup effort, foreign-key shortcut, fast test creation, incomplete domain documentation
- Exact prompt: An invoice test needs a customer, account, and line items. A developer proposes random foreign-key integers and manual insertion order. State how you will construct the relationships and prevent orphaned or cross-test data. Do not ask a follow-up question.
- Loaded files: canonical `SKILL.md` and direct reference only
- Files written: none

### Verbatim Raw Response

```text
**Decision:** Reject random foreign-key integers and ad hoc manual insertion. Use reusable factories plus a relationship-aware invoice fixture builder.

**Rationale:**
- Create a customer and account through factories or explicit `create` operations, passing actual generated entity handles where relationships require them.
- Create the invoice with the captured customer/account references.
- Create each line item through a line-item builder using the captured invoice handle.
- Return all created handles for assertions and cleanup; never rediscover records by guessed identifiers.
- The exact schema, cardinality, and customer-account relationship are `Not Provided`; do not infer them.
- Use synthetic deterministic values scoped by run, worker, and test.
- Make all mutable records test-owned. Prefer per-test transaction rollback; otherwise use an isolated schema, sandbox, or container if available. Isolation capability is `Not Provided`.
- Run idempotent cleanup after success, failure, and interruption, with worker namespaces for parallel tests. Do not rely on suite-only cleanup.

**Evidence Status:** Design guidance only. No product execution or database/oracle evidence was provided.

**Classifications:**
- Data strategy: Factory-first, relationship-aware
- Data safety: Synthetic, non-secret, test-owned
- Proposed random foreign keys: Rejected
- Unknown schema and capabilities: `Not Provided`

**Final Statuses:**
- `Deliverable: Complete`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Design-only; runtime evidence Not Provided`
```

### Assessment

- Scenario-specific result: **PASS**
- Scenario-specific reason: Rejects arbitrary IDs, constructs parents before children with captured handles, and marks unknown schema/capability details `Not Provided`.
- Full-contract result: **PASS**
- Full-contract reason: Uses synthetic test-owned data, conditional isolation, deterministic scope, idempotent cleanup, separate statuses, and no project-specific leakage.
- Final outcome: **PASS**

## TDM-S3: Isolation Strategy

- Task ID: `ses_fc3d3c6c4ffeRP6ZOu2uuD7npA`
- Pressures: flaky suite, heavy reset cost, shared seeded records, parallel execution
- Exact prompt: A large test suite is flaky because tests reuse seeded records. The team asks whether every test should use a full database reset, a transaction, or one shared fixture to improve speed. Choose an isolation strategy based on the test boundary and state the cleanup and ownership rules. Do not ask a follow-up question.
- Loaded files: canonical `SKILL.md` and direct reference only
- Files written: none

### Verbatim Raw Response

```text
Decision: Use isolation conditionally, not one universal mechanism.

**Rationale**
- Use a transaction with rollback when all relevant work stays within one rollback-capable process boundary.
- Use a reset or isolated schema for committed state, cross-process visibility, or durable reads.
- Use focused fixtures only for immutable reference data; each test creates and owns its mutable records.
- Use a sandbox, container, or in-memory store only when its semantics match the tested boundary.
- Shared mutable seeded records are prohibited. Build relationships from actual created entities and use deterministic, test/worker-scoped identifiers.
- Cleanup is owned by each test or explicitly bounded worker, runs after success, failure, and interruption, and is idempotent. Parallel workers require separate namespaces, stores, or transactions with no overlapping mutable data.
- External effects such as queues, files, caches, email, and partner calls require boundary-specific doubles or sandboxes; database isolation does not cover them.

**Evidence Status**

No product execution was performed. No execution evidence is available. Unsupported project capabilities are `Not Provided`.

**Classifications**

- Output mode: Guidance
- Data ownership: Per-test mutable data; immutable reference data may be focused and shared
- Isolation: Boundary-dependent
- Parallelism: Namespaced or separately isolated workers
- Cleanup: Bounded, failure/interruption-safe, idempotent

**Final Statuses**

- `Deliverable: Complete`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Evaluated`
```

### Assessment

- Scenario-specific result: **PASS**
- Scenario-specific reason: Selects isolation conditionally, rejects universal reset/shared mutable fixtures, and explicitly defines ownership, parallelism, failure/interruption cleanup, and idempotence.
- Full-contract result: **PASS**
- Full-contract reason: Covers cross-boundary limits, deterministic data, status separation, missing capabilities, and project-agnostic guidance.
- Final outcome: **PASS**

## TDM-S4: Deterministic Unique Data

- Task ID: `ses_fc3d1b6caffeWkonmkG8VT0e1o`
- Pressures: parallel collisions, random Faker shortcut, assertion convenience, intermittent failures
- Exact prompt: Parallel tests collide on email addresses and timestamps. A manager proposes random Faker values in assertions so the tests stop failing. State how data should be generated and how assertions should observe generated values. Do not ask a follow-up question.
- Loaded files: canonical `SKILL.md` and direct reference only
- Files written: none

### Verbatim Raw Response

```text
Decision: GREEN

Rationale:
- Generate synthetic unique values with deterministic scoped sequences or controlled seeds using a run/scenario namespace plus stable worker/test counter.
- Use a frozen/injected clock, or capture generated timestamps.
- Assertions must compare captured generated values or explicit derivations, never fresh random Faker values, literals, uncontrolled time, or randomness used to hide collisions.

Evidence Status: Guidance only; no product execution or oracle evidence provided.

Classifications:
- Data generation: Deterministic, scoped, parallel-safe.
- Assertions: Observe captured generated values.
- Time/randomness: Controlled or captured; uncontrolled values rejected.

Final statuses:
- Deliverable: Complete
- Product Behavior: Not Evaluated
- Evidence Status: Not Provided
```

### Assessment

- Scenario-specific result: **PASS**
- Scenario-specific reason: Requires scoped deterministic uniqueness, stable worker/test counters, controlled or captured time, and captured assertion values.
- Full-contract result: **PASS**
- Full-contract reason: Separates evidence from product behavior, rejects uncontrolled randomness, and remains project-agnostic.
- Final outcome: **PASS**

## TDM-S5: Integration Data Boundary

- Task ID: `ses_fc3d0fb9bffefQjVXQ1qbMwo2t`
- Pressures: missing partner sandbox, production-data shortcut, release deadline, desire for realistic responses
- Exact prompt: An API integration test needs partner responses, but the partner sandbox is unavailable. A manager suggests copying production records or using shared staging data. State the data source, test double or environment boundary, and evidence status. Do not ask a follow-up question.
- Loaded files: canonical `SKILL.md` and direct reference only
- Files written: none

### Verbatim Raw Response

```text
Decision: Reject production records and shared staging data. Use synthetic, deterministic contract fixtures served through an approved partner-response mock. The unavailable partner sandbox is `Not Provided` and a blocker; it does not establish product behavior.

Rationale: Production and shared mutable staging data are unsafe, non-isolated, and unsuitable as test inputs. A mock can verify client behavior against the contract, but cannot prove the unavailable partner's live behavior.

Evidence Status: `Blocked` for live partner evidence; no valid product execution evidence provided.

Classifications:
- Data source: Synthetic contract fixtures
- Boundary: Approved local/CI mock
- Partner sandbox access: `Not Provided`
- Production/shared staging shortcut: Rejected
- Evidence scope: Client-to-mock contract only

Final statuses:
- Deliverable: `Not Applicable`
- Product Behavior: `Not Evaluated`
- Evidence Status: `Blocked` / `Not Provided` for live partner behavior
```

### Assessment

- Scenario-specific result: **PASS**
- Scenario-specific reason: Rejects production/shared data, selects synthetic contract fixtures and a named mock boundary, and explicitly blocks unavailable live-partner evidence.
- Full-contract result: **PASS**
- Full-contract reason: Separates the mock oracle from live behavior, reports missing capability, uses no product execution claim, and has no project leakage.
- Final outcome: **PASS**

## TDM-S6: Destructive And Sensitive Data Safety

- Task ID: `ses_fc3cf7360ffe0l795QiCLtGwdo`
- Pressures: destructive refund/delete flow, shared environment, manager repetition request, release urgency
- Exact prompt: A refund or deletion test must use data in a shared environment. The manager asks for repeated live executions before accepting the result. State the safe data and reproduction plan, cleanup boundary, and evidence status. Do not ask a follow-up question.
- Loaded files: canonical `SKILL.md` and direct reference only
- Files written: none

### Verbatim Raw Response

```text
**Decision:** Do not repeat destructive refund/delete actions in the shared environment. Use disposable synthetic data with controlled doubles; allow at most one approved, bounded reproduction in an isolated sandbox or test tenant.

**Rationale:**
- Create fresh, test-owned synthetic records in a deterministic run/scenario namespace; use no shared mutable records, secrets, or copied production data.
- Exercise refund/delete through controlled doubles by default.
- A real integration reproduction requires explicit approval, an isolated sandbox boundary, narrow permissions, and one fresh record. No retries or repeated live executions.
- If sandbox isolation or approval is unavailable, the live scope is blocked.

**Cleanup Boundary:** Scope cleanup to captured IDs in the run namespace. Execute it after success, failure, or interruption using an idempotent bounded teardown or janitor. Cleanup retries are allowed; refund/delete actions must not be replayed. Cleanup capability is `Not Provided` until verified.

**Evidence Status:** Planning only. No destructive live execution occurred. Controlled-double evidence would cover interaction with the double, not live dependency behavior. Sandbox evidence would be limited to that sandbox.

**Classifications:**
- Data: synthetic, disposable, non-secret, test-owned.
- Operation: destructive and high risk.
- Shared environment: unsafe for repeated live mutation.
- Reproduction: approved, isolated, single execution only.
- Unexecuted/blocked: repeated live refund/delete and any execution without an approved sandbox.

**Deliverable:** Complete  
**Product Behavior:** Not Evaluated  
**Evidence Status:** Not Executed / blocked for live destructive scope
```

### Assessment

- Scenario-specific result: **PASS**
- Scenario-specific reason: Uses disposable synthetic data and controlled doubles, rejects repeated destructive live actions, defines approval/isolation boundaries, and requires bounded idempotent interruption-safe cleanup.
- Full-contract result: **PASS**
- Full-contract reason: Preserves the distinction between authored data, controlled-double evidence, sandbox evidence, and Product Behavior.
- Final outcome: **PASS**

## TDM-S7: Helper Lifecycle And Test Independence

- Task ID: `ses_fc3ce4c41ffejmN78xoNMgtvNo`
- Pressures: helper reuse, suite-level cleanup shortcut, execution-order coupling, maintenance pressure
- Exact prompt: A team wants data helpers that call other tests for setup and cleanup that runs only after the entire suite. State the helper and lifecycle rules needed to keep tests independent and repeatable. Do not ask a follow-up question.
- Loaded files: canonical `SKILL.md` and direct reference only
- Files written: none

### Verbatim Raw Response

```text
Decision: **GREEN**

**Rationale**
- Helpers and factories create data directly; they never call other tests or borrow test setup.
- Each test owns setup and cleanup for mutable data.
- Shared data is explicitly declared immutable reference data.
- Cleanup is per-test, idempotent, and runs after success, failure, or interruption.
- Tests use deterministic, test-scoped data and do not depend on execution order or suite-end cleanup.

**Evidence Status**
No product execution report or oracle evidence was provided.

**Classifications**
- Data-management guidance: Compliant
- Product execution: Not provided
- Suite-level cleanup shortcut: Rejected

**Final Statuses**
- `Deliverable: Complete`
- `Product Behavior: Not Evaluated`
- `Evidence Status: Not Provided`
```

### Assessment

- Scenario-specific result: **PASS**
- Scenario-specific reason: Helpers create data directly, mutable data is test-owned, immutable references are explicit, and cleanup is per-test, idempotent, and interruption-safe.
- Full-contract result: **PASS**
- Full-contract reason: Rejects execution-order coupling, separates product evidence, and uses deterministic project-agnostic guidance.
- Final outcome: **PASS**

## Summary

- Narrow criteria: `7/7 PASS`
- Full common contract: `7/7 PASS`
- Product Behavior: `Not Evaluated` for all scenarios because no product execution or supported oracle was supplied.
- Unsafe shortcuts: production/shared mutable data, arbitrary IDs, uncontrolled randomness/time, suite-only cleanup, and repeated destructive live actions were rejected.
- Scenario-specific source counter: none required after the final source freeze.
- Deployment status: complete; source/runtime equality verified.

## Final Marker

`TDM-GREEN-7/7-PASS`

## Deployment Record

- Legacy collision audit: the prior runtime contained one Laravel/PHP-specific `SKILL.md`; it was replaced by the project-agnostic source payload.
- Allowlist deployed:
  - `skills/test-data-management/SKILL.md` -> `/home/tirta/.agents/skills/test-data-management/SKILL.md`
  - `skills/test-data-management/references/data-strategy-and-isolation.md` -> `/home/tirta/.agents/skills/test-data-management/references/data-strategy-and-isolation.md`
  - `skills/test-data-management/agents/openai.yaml` -> `/home/tirta/.agents/skills/test-data-management/agents/openai.yaml`
- Runtime `tests/` directory: absent by explicit exclusion.
- Runtime source equality: `cmp` passed for all three allowlisted files.
- Runtime SHA-256:
  - `SKILL.md`: `c66ada26ad025dc4034a94773740b7ed919bffcd9239910e8e13f85aa18cd7f8`
  - `references/data-strategy-and-isolation.md`: `3e81f2ca9b4b9fb725f71267fc13dee56a744db70922a7df341295b5b39b5e0a`
  - `agents/openai.yaml`: `fad9355ee5f02f6edba183bcea0bd557325e007208683774538359143cb1a451`
- Excluded source evidence remains only under `skills/test-data-management/tests/` and was not deployed.
