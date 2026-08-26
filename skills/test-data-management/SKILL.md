---
name: test-data-management
description: Use when a QA engineer needs to create, manage, or organize isolated, reusable, deterministic, and safe test data for automated or manual tests.
---

# Test Data Management

## Output Modes

- **Guidance mode:** Recommend a data strategy, builders, ownership, lifecycle, and evidence boundary.
- **Artifact mode:** Create requested factories, fixtures, payloads, or data plans only when the project supplies the required capabilities. Mark missing capabilities as `Not Provided`.
- When detailed strategy guidance is needed, load `references/data-strategy-and-isolation.md` before responding.
- Canonical status: use `Deliverable: Complete` when requested data work and feasible validation are complete, `Deliverable: Incomplete` when requested content or validation is missing, and `Deliverable: Not Applicable` when no data deliverable is requested. Data authorship is not product execution. Without valid execution against a supported oracle, report `Product Behavior: Not Evaluated`; report `Evidence Status` separately.

## Workflow

1. Identify the test boundary, data needed, relationships, mutability, external effects, and acceptance oracle.
2. Inspect the project's available factories, builders, stores, reset or transaction support, environments, and doubles. Do not invent unavailable capabilities; record them as `Not Provided`.
3. Apply a factory-first design: create reusable valid defaults, controlled overrides, and explicit states rather than inline records or shared mutable data.
4. Select isolation and lifecycle controls conditionally, then define ownership, cleanup, parallelism assumptions, and evidence limits.
5. Build relationships from created entities or relationship-aware builders, capture generated values, and verify data is synthetic, non-secret, and deterministic.
6. Report data deliverables separately from Product Behavior and identify unexecuted or blocked boundaries.

## Factory And Builder Roles

Factory-first means reusable factories are the default source of test entities. A **factory** returns a valid default entity or value set with controlled overrides and optional states. A **fixture builder** assembles the persisted or in-memory domain state a scenario needs, including related entities and lifecycle state. A **payload builder** creates serializable request or event input; it does not imply persistence. `build` may return in-memory data, while `create` may persist it when the project supports that distinction. Neither term requires a particular language, database, or data library. Helpers set up data directly and never call other tests.

Reject hardcoded identifiers, arbitrary foreign keys, and shared mutable staging or production records. Use focused, reusable builders and make mutable records test-owned.

## Isolation And Lifecycle

Choose the narrowest strategy that matches the boundary:

- Use a **reset or isolated schema** only for durable state in a controlled store, such as committed state or cross-process visibility. It does not isolate queues, files, email, partner calls, or other external effects; use boundary-specific doubles or sandboxes for those effects.
- Use a **transaction with rollback** when the system and runner keep all relevant work in one rollback-capable boundary.
- Use a **focused fixture** for stable immutable reference data, while each test creates its own mutable records.
- Use a **sandbox, container, or in-memory store** when it faithfully represents the boundary and is available.

Define per-test or per-worker ownership for every record, when cleanup runs, what happens on failure or interruption, and whether parallel workers have separate namespaces or stores. Cleanup must be idempotent and run after normal completion, failure, or interruption within an explicitly bounded scope; suite-only cleanup remains insufficient. Isolation cannot undo effects that cross an external boundary, so document what is isolated, what is not, and the resulting evidence limit.

## Determinism And Relationships

Use deterministic scoped sequences or controlled seeds with a run/scenario namespace plus a worker/test stable counter for values that must be unique. For timestamp assertions, use a frozen or injected clock, or capture the generated timestamp. Do not use uncontrolled randomness or current time to hide collisions. Construct parents before children, or use relationship-aware builders, and preserve valid cardinality, ownership, and lifecycle rules. Unknown schema or capability details are `Not Provided`, not guesses.

## Safety And Integration

Use synthetic, non-secret data and disposable records. Never copy credentials, sensitive records, or shared mutable staging or production data into tests. Use controlled doubles for destructive, costly, or externally impactful operations unless an approved sandbox is explicitly in scope. State the integration boundary as a local double, contract fixture, sandbox, container, in-memory store, or real controlled dependency. Name the source of truth and evidence available at that boundary; missing access is `Not Provided` or a blocker, not an implied pass.

## Quality Gate

- Factory, fixture builder, payload builder, and `build`/`create` roles are distinct.
- No hardcoded IDs, random foreign keys, test-calling helpers, shared mutable data, or suite-only cleanup.
- Relationships are valid, values are deterministic and scoped, and generated values are captured.
- Isolation, ownership, cleanup, failure handling, parallelism, and cross-boundary limits are explicit.
- Data is synthetic and non-secret; destructive effects use controlled doubles or approved boundaries.
- Deliverable status and Product Behavior status are separate; authoring data does not establish product behavior.
