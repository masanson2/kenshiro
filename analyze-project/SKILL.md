---
name: kenshiro-analyze-project
description: Detect a target repository's required technology stack and initialize persistent Kenshiro state.
version: 1.0.0
---

# Analyze Project

## Responsibility

Initialize project-level Kenshiro state or explicitly refresh its stack.

## Inputs

- Target repository files
- Optional exact command: `REFRESH STACK`
- `../shared/schemas/stack.schema.yaml`
- `../shared/templates/project/`

## Outputs

- `.kenshiro/project-index.yaml`
- `.kenshiro/stack.yaml`
- `.kenshiro/workflow.yaml`
- `.kenshiro/docs/stack-review.md`
- `.kenshiro/activity.log`

## Rules

When `.kenshiro/project-index.yaml` is absent, initialize `.kenshiro/`, `.kenshiro/features/`, `.kenshiro/docs/`, root `workflow.yaml`, and `activity.log` only when each artifact is absent. Persist repository metadata, architecture style, and conventions in `project-index.yaml`; persist the single repository-wide technology stack only in `.kenshiro/stack.yaml`. These YAML artifacts are technical agent guidance. Write the corresponding concise Italian human-facing review only to `.kenshiro/docs/stack-review.md`.

When `project-index.yaml` exists, run only after the exact case-insensitive command `REFRESH STACK`. Do not validate evidence paths, inspect files, or refresh fields automatically. During an explicit refresh, update project metadata in `project-index.yaml`, the one stack description in `stack.yaml`, and `docs/stack-review.md`. Detect `.git` directly. `stack.yaml.build.compile.command` and `stack.yaml.build.test.command` must be exact repository-supported quiet console commands, each with `quiet: true` and evidence. For a project exposing APIs, discover and record the repository file that is their source of truth in `stack.yaml.api.source_of_truth`: use `OPENAPI` for HTTP APIs, or the applicable `ASYNCAPI`, `GRAPHQL_SCHEMA`, or `PROTOBUF` file for that technology. Do not create or infer an API contract; if no valid contract file is present, set its status to `UNKNOWN`. For a project without APIs, set the source-of-truth status to `NOT_APPLICABLE` and format to `NONE`. Use the build tool's quiet mode only; never use a verbose command, suppress output externally, derive, transform, or substitute a command. Undetermined fields use `status: UNKNOWN`. State is compact English YAML. The Markdown document is concise Italian. Never create `stack.yaml`, `project-index.yaml`, or `stack-review.md` in a feature directory.

## State update

Set root `project_skills.analyze-project.status: DONE`. Set root `project_gates.stack.status: FAILED` if any required stack field is `UNKNOWN`; otherwise `PENDING`. Set root `project_phase: STACK_APPROVAL`. Append `Analyze Project Completed`.

## Completion criteria

All required fields, including quiet `build.compile.command` and quiet `build.test.command`, exist in `stack.yaml`, every field has evidence, an API project has a detected source-of-truth contract file, both YAML artifacts conform to their schemas, and `docs/stack-review.md` exists.
