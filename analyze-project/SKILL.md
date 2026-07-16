---
name: kenshiro-analyze-project
description: Detect a target repository's required technology stack and initialize deterministic Kenshiro state.
version: 1.0.0
---

# Analyze Project

## Responsibility

Initialize `.kenshiro/` when absent and detect only the mandatory stack fields.

## Inputs

- Target repository files
- `../shared/schemas/stack.schema.yaml`
- `../shared/templates/state/`

## Outputs

- `.kenshiro/state/workflow.yaml`
- `.kenshiro/state/stack.yaml`
- `.kenshiro/docs/stack.md`
- `.kenshiro/activity.log`

## Rules

Inspect manifests, source layout, configuration, tests, containers, and CI definitions. Detect `backend.language`, `backend.framework`, `backend.version`, `build.tool`, `build.compile.command`, `frontend.framework`, `database.type`, `architecture.style`, `api.style`, `testing.framework`, `security.authentication`, `logging.framework`, `containerization`, and `cicd.platform`.

`build.compile.command` must be an exact repository-supported command evidenced by project configuration or CI; never derive, transform, or substitute a command. Undetermined fields use `status: UNKNOWN`. State is compact English YAML. The Markdown document is concise Italian.

## State update

Set `skills.analyze-project.status: DONE`. Set `gates.stack.status: FAILED` if any required field is `UNKNOWN`; otherwise `PENDING`. Set `current_phase: STACK_APPROVAL`. Append `Analyze Project Completed`.

## Completion criteria

All required fields, including `build.compile.command`, exist in `stack.yaml`, every field has evidence, `stack.yaml` conforms to its schema, and `stack.md` exists.
