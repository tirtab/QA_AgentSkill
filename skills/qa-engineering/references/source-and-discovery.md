# Source and Discovery Contract

## Knowledge Reuse

Use information in this discovery-efficient order:

1. User-provided requirements in the active conversation.
2. Relevant knowledge already established in the current session.
3. Existing repository docs, specifications, runbooks, and indexes.
4. Optional Project Context when already known or encountered normally and useful.
5. Relevant code, executable configuration, and tests.
6. Generic skill examples.

Do not perform a separate repository-wide search merely to find Project Context.

Do not invent repository-specific commands, paths, or facts; inspect the current source or report the information as unknown.

## Behavioral Source of Truth

When deciding expected behavior, use this precedence:

1. Current requirement and acceptance criteria.
2. Current API, event, schema, or contract specification.
3. Current repository implementation and executable configuration.
4. Current maintained project documentation.
5. Optional Project Context.
6. Generic skill examples.

Requirements and contracts define intended behavior. Implementation and executable configuration establish current behavior and may evidence a defect; they are not automatically the oracle. A maintained document that explicitly contains an approved requirement or design belongs with the corresponding higher-authority category. When authority, applicability, and recency do not safely resolve a conflict, report Requirement Ambiguity and do not select an oracle.

## Targeted Discovery

Inspect only facts needed for the task, typically the relevant requirement, existing automation, framework/runtime version, auth/environment mechanism, test-data convention, and narrow execution command.

Full-repository discovery requires a concrete reason such as an explicitly requested project audit, unknown system boundaries that materially affect coverage, or repeated cross-module failures without an identifiable scope.

## Project Context On Demand

Project Context is a cache/index, not a dependency or authoritative database. Use it to reduce discovery, not duplicate discovery.

Create or refresh it only when explicitly requested or when repeated expensive discovery across sessions demonstrates a material documentation gap. Do not create it when current context or existing docs are sufficient, the repository is simple, the task is one-off, or facts change too quickly.

If Project Context exists, validate only task-critical claims likely to be stale. Never repeat full discovery merely because the file was loaded.
