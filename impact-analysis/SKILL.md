---
name: kenshiro-impact-analysis
description: Find repository-wide feature impacts and dependency chains without modifying source code.
version: 1.0.0
---

# Impact Analysis

## Responsibility

Perform usage and dependency analysis only.

## Inputs

- Target repository
- `.kenshiro/stack.yaml`
- `.kenshiro/features/<feature-id>/feature.yaml`
- `.kenshiro/features/<feature-id>/workflow.yaml`
- `../shared/schemas/impact.schema.yaml`

## Outputs

- `.kenshiro/features/<feature-id>/impact.yaml`
- `.kenshiro/features/<feature-id>/docs/impact.md`
- Updated `.kenshiro/features/<feature-id>/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Inspect usages, callers, implementations, services, repositories, controllers, APIs, schedulers, batch jobs, database artifacts, tests, dependency chains, and potential regressions for this feature only. Record technical findings in `impact.yaml` for agent guidance, including the impacted class/component diagram, every component's As-is and To-be state, and every new or modified query with its location and statement. When an API is impacted, use the API source-of-truth file recorded in `stack.yaml.api.source_of_truth`, and record its format, path, and every affected operation in `impact.yaml.api_contract`; do not use controller code as the API authority. Render the same findings in `docs/impact.md` as a technical, discursive Italian analysis written by a technical analyst. For new components or queries, record `Non applicabile (nuovo)` as their As-is state; when no query is involved, state this explicitly. Do not combine impacts with another feature, alter source code, or propose implementation alternatives.

## State update

Set feature `skills.impact-analysis.status: DONE`, `gates.impact.status: PENDING`, and `current_phase: IMPACT_APPROVAL`. Append `Impact Analysis Completed`.

## Completion criteria

`impact.yaml` conforms to its schema, the Italian impact document contains the class/component diagram, As-is and To-be variations, and query analysis, and no application files changed.
