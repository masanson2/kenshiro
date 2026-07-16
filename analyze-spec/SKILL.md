---
name: kenshiro-analyze-spec
description: Normalize an approved feature request into a compact deterministic feature state artifact.
version: 1.0.0
---

# Analyze Specification

## Responsibility

Extract only feature requirements into `feature.yaml`.

## Inputs

- User-provided feature specification
- `.kenshiro/state/workflow.yaml` with approved stack gate
- `../shared/schemas/feature.schema.yaml`

## Outputs

- `.kenshiro/state/feature.yaml`
- Updated `.kenshiro/state/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Store stable feature ID, name, requirements, acceptance criteria, and constraints. Use English compact YAML. Do not analyze code, impacts, tasks, alternatives, or rationale.

## State update

Set `skills.analyze-specification.status: DONE` and `current_phase: IMPACT_ANALYSIS`. Append `Analyze Specification Completed`.

## Completion criteria

`feature.yaml` conforms to its schema and contains no inferred requirements.
