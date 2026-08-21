# QA Agent Skills

This repository is the source of truth for reusable QA skill specifications, implementation plans, source files, and skill-test evidence.

## Layout

- `docs/specs/`: approved designs and operating standards.
- `docs/plans/`: task-by-task implementation and verification plans.
- `skills/<skill-name>/`: authoring source for `SKILL.md`, `references/`, `agents/`, and repository-only `tests/` evidence.

## Skill Lifecycle

Skills follow a TDD-style cycle: record baseline decisions, make the smallest source change that closes an observed gap, rerun the same scenarios, and retain verification evidence under the source skill's `tests/` directory.

Runtime deployment occurs only after source verification passes. Mirror only `SKILL.md`, `references/`, and `agents/` to `~/.agents/skills/<skill-name>`; do not deploy `tests/`.
