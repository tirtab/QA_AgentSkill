# Test Data Management Migration Design

**Date:** 2026-08-21
**Status:** Approved

## Goal

Migrate `test-data-management` into a project-agnostic skill that creates isolated, reusable, deterministic, safe test data without forcing an ORM, language, database, or vendor.

## Scope

The migration changes only `test-data-management`:

- `skills/test-data-management/SKILL.md` is a concise runtime entry point.
- `skills/test-data-management/references/data-strategy-and-isolation.md` contains detailed data, fixture, payload, relationship, isolation, lifecycle, and safety rules.
- `skills/test-data-management/agents/openai.yaml` contains generic runtime metadata.
- `skills/test-data-management/tests/` retains scenarios, RED baseline, paired verification, refactor evidence, and source manifest.

The existing global skill is a baseline only. Its Laravel/PHP syntax, ORM assumptions, organization-specific models, Kafka/Accurate examples, and production-like data assumptions are not copied into the reusable skill.

## Core Model

Factory-first means creating test data through reusable builders or factories instead of hardcoded records, shared staging records, global seeders, or random foreign-key integers.

- A factory produces a valid default entity with controlled overrides and optional states.
- A fixture builder prepares persisted or in-memory domain data, including valid relationships and lifecycle state.
- A payload builder produces serializable request input and does not imply persistence.
- `build` may represent in-memory data; `create` may persist data when the project supports that distinction. The terms remain generic and are not tied to a framework.

Factories may compose relationships, states, deterministic identifiers, and safe synthetic values. Tests own the data they need and do not depend on execution order or another test's records.

## Data Strategy

The skill chooses a data lifecycle based on the test boundary and available tooling:

- Transaction rollback is suitable when the system and test runner support it.
- Database reset or isolated schema is suitable when tests require committed state or cross-process visibility.
- Focused seeders or fixtures are suitable for stable reference data, but tests still create mutable records they own.
- Containers, sandboxes, in-memory stores, and API test doubles are valid when they match the integration boundary.

No isolation strategy is universal. The agent records the chosen boundary, cleanup behavior, concurrency assumptions, and evidence limits.

## Safety And Determinism

- Use synthetic, non-secret, isolated data; never copy production credentials or unsafe records.
- Avoid timestamps, random values, or shared identifiers in assertions unless the test controls or derives them.
- Generate unique values through a deterministic seed or scoped sequence when uniqueness is required.
- Build foreign-key relationships through factories or explicit created entities; never use arbitrary IDs that can orphan or collide.
- Keep destructive or costly data in disposable fixtures and use approved test doubles for external effects.
- Integration data must state the boundary: local mock, sandbox, contract fixture, or real controlled dependency.

## Acceptance Criteria

The migration is accepted only when:

- the source is project-agnostic and contains no organization or stack-specific default;
- factory, fixture, and payload-builder roles are distinct and reusable;
- relationship data is valid and test-owned;
- isolation, cleanup, determinism, safety, and integration boundaries are explicit;
- missing project capabilities are `Not Provided`, not invented;
- RED/GREEN/refactor evidence covers every finalized scenario;
- source/runtime files match exactly and `tests/` is not deployed;
- the final skill-test result is recorded separately from product execution.

## Out Of Scope

- Implementing application factories, database schemas, seeders, or test code in a product repository.
- Rewriting `writing-test-cases`, `qa-engineering`, or `bug-reporting`.
- Requiring Laravel, PHP, an ORM, a particular database, Kafka, Accurate, or a vendor-specific fixture API.
- Using production or shared staging data as the default test-data source.
