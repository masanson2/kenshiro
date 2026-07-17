---
name: kenshiro-generate-tasks
description: Convert approved Kenshiro impact findings into minimal verifiable implementation tasks.
version: 1.0.0
---

# Generate Tasks

## Responsibility

Convert approved impact into tasks only.

## Inputs

- `.kenshiro/stack.yaml`
- `.kenshiro/features/<feature-id>/feature.yaml`
- `.kenshiro/features/<feature-id>/impact.yaml`
- `.kenshiro/features/<feature-id>/git.yaml`
- `.kenshiro/features/<feature-id>/workflow.yaml` with approved impact and branch gates
- `../shared/schemas/tasks.schema.yaml`
- `../shared/templates/feature/docs/tasks.md`

## Outputs

- `.kenshiro/features/<feature-id>/tasks.yaml`
- `.kenshiro/features/<feature-id>/docs/tasks.md`
- Updated `.kenshiro/features/<feature-id>/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Each task has an ID, compact English description, `PENDING` status, scope, validation, `tdd: {red: PENDING, green: PENDING, command: <stack.build.test.command>}`, and `compilation: {status: PENDING, command: <stack.build.compile.command>}`. Copy both quiet commands byte-for-byte from approved project state. `tasks.yaml` is the agent's technical execution plan. Tasks must be independent, minimal, verifiable, within this feature's approved impact, and designed with tests before production changes. When `impact.yaml.api_contract.format` is not `NONE`, include the API source-of-truth file in each relevant task scope and require its affected operations to be updated and validated with the API behavior. Render `docs/tasks.md` from its template as a technical, discursive Italian plan written by a technical analyst: replace every placeholder, include every task exactly once in execution order, and for each task state its identifier, technical objective, affected classes/components, As-is and To-be variation, affected or added queries, API contract impact, and planned verification. Include the Mermaid class/component diagram, query analysis, and API contract analysis from `impact.yaml`. For new components or queries, state `Non applicabile (nuovo)` as the As-is state; when no query is involved, state this explicitly. Do not expose TDD state or raw build commands in this document. Do not modify source code or create tasks for another feature.

## State update

Set feature `skills.generate-tasks.status: DONE`, `gates.tasks.status: PENDING`, and `current_phase: TASK_APPROVAL`. Append `Generate Tasks Completed`.

## Completion criteria

`tasks.yaml` conforms to its schema, every task scope is present in `impact.yaml`, API tasks include the source-of-truth contract file, and `docs/tasks.md` exists in Italian with no unresolved placeholders, the impacted class/component diagram, query analysis, API contract analysis, As-is and To-be variations, and one complete entry for every task.
