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
- `.kenshiro/project-index.yaml`
- `.kenshiro/workflow.yaml`
- `.kenshiro/features/<feature-id>/workflow.yaml` with approved stack gate
- `../shared/schemas/feature.schema.yaml`

## Outputs

- `.kenshiro/features/<feature-id>/feature.yaml`
- Updated `.kenshiro/features/<feature-id>/workflow.yaml`
- Updated `.kenshiro/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

The orchestrator creates and registers the feature workspace before this skill runs. Store its ID, name, explicit `branch_type` (`FEAT`, `FIX`, or `REFACTOR`), requirements, acceptance criteria, and constraints in that workspace's `feature.yaml`. If branch type is not explicit, store `UNKNOWN`; later branch proposal must fail. Use English compact YAML. Do not analyze code, impacts, tasks, alternatives, or rationale.

## State update

Set feature `skills.analyze-specification.status: DONE` and `current_phase: IMPACT_ANALYSIS`. Append `Analyze Specification Completed`.

## Completion criteria

`feature.yaml` conforms to its schema and contains no inferred requirements.
