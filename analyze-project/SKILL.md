---
name: kenshiro-analyze-project
description: Detect a target repository's required technology stack and initialize deterministic Kenshiro state.
version: 1.0.0
---

# Analyze Project

## Responsibility

Initialize project-level Kenshiro state and validate or refresh project knowledge.

## Inputs

- Target repository files
- `../shared/schemas/stack.schema.yaml`
- `../shared/templates/project/`

## Outputs

- `.kenshiro/project-index.yaml`
- `.kenshiro/workflow.yaml`
- `.kenshiro/docs/stack.md`
- `.kenshiro/features/<feature-id>/workflow.yaml`
- `.kenshiro/activity.log`

## Rules

When `.kenshiro/project-index.yaml` is absent, initialize `.kenshiro/`, `.kenshiro/features/`, `.kenshiro/docs/`, root `workflow.yaml`, and `activity.log`. Detect and persist repository metadata, technology stack, architecture style, and conventions in `project-index.yaml`.

When `project-index.yaml` exists, do not re-analyze the repository. Validate recorded evidence paths and refresh only missing or outdated fields. Detect `.git` directly. `build.compile.command` must be an exact repository-supported command evidenced by project configuration or CI; never derive, transform, or substitute a command. Undetermined fields use `status: UNKNOWN`. State is compact English YAML. The Markdown document is concise Italian.

## State update

Set the active feature `skills.analyze-project.status: DONE`. Set its `gates.stack.status: FAILED` if any required field is `UNKNOWN`; otherwise `PENDING`. Set its `current_phase: STACK_APPROVAL`. Append `Analyze Project Completed`.

## Completion criteria

All required fields, including `build.compile.command`, exist in `project-index.yaml`, every field has evidence, `project-index.yaml` conforms to its schema, and `docs/stack.md` exists.
