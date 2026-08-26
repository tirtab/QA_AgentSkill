# Data Strategy And Isolation

## Factory-First Model

Factory-first is a design rule, not a framework feature. Prefer a reusable factory that produces a valid default entity or value set, accepts narrow controlled overrides, and exposes named states for meaningful variations. A test should request the smallest data shape that expresses its scenario rather than depend on records left by another test, a global seeder, or an environment.

Factories may compose related factories, deterministic sequences, safe synthetic values, and lifecycle states. Keep defaults valid and unsurprising. Keep scenario-specific exceptions in explicit states or builders instead of spreading inline records through tests. Do not add a factory merely to conceal unknown requirements: mark missing schema, capability, or behavior as `Not Provided`.

## Builder Roles

- **Factory:** Produces one valid default entity or value set, with controlled overrides and optional states. It is reusable and does not prescribe how the test runs.
- **Fixture builder:** Assembles a scenario's domain state, persisted or in memory. It creates or receives related entities, applies lifecycle transitions, and returns the handles and generated values the test needs.
- **Payload builder:** Produces serializable request, command, message, or event input. It models input shape and safe values; it does not imply that anything was persisted.
- **`build`:** Conventionally prepares an in-memory value or object. Use it when persistence is not part of the boundary.
- **`create`:** Conventionally persists data and returns the created handle when the project supports that distinction. Do not assume persistence semantics where the project has not provided them.

Builders create data directly. A helper must not call another test, borrow another test's setup, or make execution order a prerequisite. Separate immutable reference data from mutable scenario data, and make the latter test-owned.

## Relationship Data

Construct relationships through factories or explicit created entities. Create required parents before children when the boundary requires it, pass the actual generated parent references, and let relationship-aware builders enforce cardinality, ownership, and valid lifecycle state. A child must not point to a random integer or an identifier copied from a different test.

Capture every generated reference needed for setup, action, cleanup, or assertion. Prefer a returned scenario object or named handles over rediscovering records by a guessed attribute. If relationship rules or schema details are unavailable, state `Not Provided`; do not infer columns, defaults, or ordering.

## Isolation And Lifecycle

Select isolation from the test boundary and the available project capabilities:

- **Reset or isolated schema:** Use when tests need committed state, cross-process visibility, durable reads, or broad cleanup. It costs more but provides a clear starting point.
- **Transaction and rollback:** Use when all relevant writes remain inside one rollback-capable process boundary. Do not claim isolation for work committed by another process or service.
- **Focused fixture:** Use for stable, reusable reference data or a narrow scenario. Keep shared data immutable; create mutable entities per test.
- **Sandbox or container:** Use when a disposable environment is needed for realistic state, process boundaries, or destructive behavior.
- **In-memory store:** Use only when its semantics match the behavior under test; it cannot prove behavior that depends on a real persistence or network boundary.

For the selected strategy, document the owner, setup scope, cleanup trigger, failure cleanup, retry behavior, worker namespace, and parallel execution model. Cleanup must be idempotent and must run after normal completion or continue after failure or interruption within the bounded scope, such as each test, worker, transaction, or disposable environment. Suite-end cleanup alone leaves failures and interrupted runs coupled to later work.

No strategy isolates every effect. Record whether queues, files, caches, emails, partner calls, clocks, and other external effects are reset, doubled, sandboxed, or left outside the boundary. State the residual collision, leakage, and evidence risks.

## Determinism

Use a controlled seed or deterministic sequence scoped to the test, worker, run, or disposable environment. Combine the scope with a stable counter or scenario key when uniqueness is required. The strategy must be reproducible and must not depend on uncontrolled current time, process order, or random values.

Return and retain generated emails, identifiers, timestamps, tokens, and relationship handles. Assertions should compare against captured values or explicit derivations, not random literals. If time is relevant, control or observe the clock at the supported boundary rather than accepting incidental time.

## Integration Boundaries

Name the exact data and execution boundary: local controlled double, contract fixture, sandbox, containerized dependency, in-memory substitute, or real controlled dependency. Choose the boundary based on the behavior and oracle required. A double can establish the client's interaction with the double, not the unavailable dependency's live behavior. A sandbox can provide stronger evidence but still is not production evidence.

When an environment, credential, schema, fixture, or partner capability is absent, report `Not Provided` and identify the blocked evidence. Do not fill the gap with production data, shared mutable staging data, or invented access. Keep data authorship, environment setup, execution, and Product Behavior status separate.

## Safety

Use synthetic values with no secrets, personal records, credentials, or copied production content. Keep destructive, expensive, irreversible, or externally visible data in a disposable boundary. Use approved controlled doubles for payments, notifications, deletion, refunds, and other side effects unless a controlled sandbox is explicitly required and authorized.

Scope permissions narrowly, avoid repeating destructive actions in shared environments, and define cleanup before execution. Protect generated artifacts and logs from exposing sensitive values. If safe isolation or approval is unavailable, stop at planning, report the blocker, and do not represent authored data as executed behavior.

## Review Checklist

- Is factory-first the default, with valid defaults, controlled overrides, and explicit states?
- Are factory, fixture builder, payload builder, `build`, and `create` responsibilities distinct?
- Are all mutable records owned by the test or an explicitly bounded worker/environment?
- Are relationships built from actual entities rather than hardcoded or random identifiers?
- Is the isolation choice conditional on boundary, semantics, visibility, and available capabilities?
- Are setup, cleanup, failure handling, retries, namespaces, and parallelism explicit?
- Are values deterministic, scoped, reproducible, and captured for assertions?
- Are data sources synthetic and non-secret, with controlled doubles for risky effects?
- Is the integration boundary named, with cross-boundary limitations and missing capabilities marked `Not Provided`?
- Are cleanup actions interruption-safe and idempotent, and are canonical `Deliverable`, `Product Behavior`, and `Evidence Status` reported separately?
- Are deliverable and Product Behavior statuses separate, with no execution claim from data authorship?
