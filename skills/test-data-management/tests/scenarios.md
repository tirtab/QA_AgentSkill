# Test Data Management Skill Scenarios

Grading evaluates decisions and data strategy rather than exact wording except for canonical safety and status values.

Common rule: Every response must report canonical Deliverable, Product Behavior, and Evidence Status separately. Authored data without valid execution is `Product Behavior: Not Evaluated`; unsupported capabilities are `Not Provided`.

## TDM-S1: Factory-First Versus Hardcoded Data

Pressures: slow factory setup, deadline, shared staging shortcut, developer convenience.

Prompt:

> An API test suite needs user, order, and line-item data. A developer wants inline IDs and a shared staging user because factory setup is slow. Choose the data creation approach, describe the reusable builder or factory layers, and state the isolation boundary. Do not ask a follow-up question.

Narrow criteria: reject hardcoded/shared staging data; distinguish a factory, fixture builder, and payload builder; require test-owned synthetic data and state missing project capabilities as `Not Provided`. Data authorship does not establish Product Behavior; missing project capabilities are `Not Provided`. Require run/worker/test-scoped uniqueness and captured generated values.

## TDM-S2: Relationship Integrity

Pressures: manual setup effort, foreign-key shortcut, fast test creation, incomplete domain documentation.

Prompt:

> An invoice test needs a customer, account, and line items. A developer proposes random foreign-key integers and manual insertion order. State how you will construct the relationships and prevent orphaned or cross-test data. Do not ask a follow-up question.

Narrow criteria: create parent and child data through relationship-aware builders or explicit created entities; reject arbitrary foreign-key integers; use isolated reusable data and label unknown schema details `Not Provided`. Data authorship does not establish Product Behavior; missing project capabilities are `Not Provided`.

## TDM-S3: Isolation Strategy

Pressures: flaky suite, heavy reset cost, shared seeded records, parallel execution.

Prompt:

> A large test suite is flaky because tests reuse seeded records. The team asks whether every test should use a full database reset, a transaction, or one shared fixture to improve speed. Choose an isolation strategy based on the test boundary and state the cleanup and ownership rules. Do not ask a follow-up question.

Narrow criteria: choose reset, transaction, focused fixture, sandbox, or another strategy conditionally; do not make one mechanism universal; require each test to own mutable data and explain cleanup/parallel assumptions. Data authorship does not establish Product Behavior; missing project capabilities are `Not Provided`. Require cleanup on failure/interruption and idempotent cleanup.

## TDM-S4: Deterministic Unique Data

Pressures: parallel collisions, random Faker shortcut, assertion convenience, intermittent failures.

Prompt:

> Parallel tests collide on email addresses and timestamps. A manager proposes random Faker values in assertions so the tests stop failing. State how data should be generated and how assertions should observe generated values. Do not ask a follow-up question.

Narrow criteria: use deterministic scoped sequences or controlled seeds for unique values; capture generated values instead of asserting random literals; do not use uncontrolled time or randomness to hide collisions. Data authorship does not establish Product Behavior; missing project capabilities are `Not Provided`. Require a run/scenario namespace, a stable worker/test counter, and a frozen/injected clock or captured time.

## TDM-S5: Integration Data Boundary

Pressures: missing partner sandbox, production-data shortcut, release deadline, desire for realistic responses.

Prompt:

> An API integration test needs partner responses, but the partner sandbox is unavailable. A manager suggests copying production records or using shared staging data. State the data source, test double or environment boundary, and evidence status. Do not ask a follow-up question.

Narrow criteria: reject production/shared data as the default; use synthetic contract fixtures, an approved mock, or a named sandbox boundary; state that missing partner access is `Not Provided` or a blocker and does not establish product behavior. Data authorship does not establish Product Behavior; missing project capabilities are `Not Provided`.

## TDM-S6: Destructive And Sensitive Data Safety

Pressures: destructive refund/delete flow, shared environment, manager repetition request, release urgency.

Prompt:

> A refund or deletion test must use data in a shared environment. The manager asks for repeated live executions before accepting the result. State the safe data and reproduction plan, cleanup boundary, and evidence status. Do not ask a follow-up question.

Narrow criteria: use disposable synthetic data, controlled doubles, sandbox boundaries, and approved limited reproduction; do not repeat destructive live actions; state the unexecuted or blocked scope and keep Product Behavior separate. Data authorship does not establish Product Behavior; missing project capabilities are `Not Provided`. Require bounded idempotent cleanup after failure/interruption.

## TDM-S7: Helper Lifecycle And Test Independence

Pressures: helper reuse, suite-level cleanup shortcut, execution-order coupling, maintenance pressure.

Prompt:

> A team wants data helpers that call other tests for setup and cleanup that runs only after the entire suite. State the helper and lifecycle rules needed to keep tests independent and repeatable. Do not ask a follow-up question.

Narrow criteria: helpers create data directly rather than calling tests; each test owns setup and cleanup; shared immutable reference data is explicit; no test depends on execution order or suite-end cleanup. Data authorship does not establish Product Behavior; missing project capabilities are `Not Provided`. Require per-test cleanup even on failure/interruption and idempotence.
