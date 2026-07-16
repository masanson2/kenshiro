---
name: kenshiro
description: Deterministic Spec-Driven Development workflow orchestrator that routes only to independently reusable Kenshiro phase skills.
version: 2.0.0
---

# Kenshiro Orchestrator

Kenshiro orchestrates a deterministic Spec-Driven Development workflow. It does not analyze repositories, generate artifacts, validate gates, or write reviews itself. It does not modify application code. Those operations belong exclusively to the phase skills in this directory.

## Authority

- Repository state has priority over Kenshiro state files, which have priority over approved artifacts.
- `.kenshiro/workflow.yaml` is the project feature registry.
- `.kenshiro/features/<feature-id>/workflow.yaml` is the only workflow authority for that feature.
- `.kenshiro/project-index.yaml` is project-level stack, architecture, conventions, and repository metadata.
- Chat history and `.kenshiro/activity.log` are not state sources.
- `shared/state-machine.yaml` defines the only legal transitions.
- `shared/implementation-guard.yaml` forbids source modification unless all gates are approved.
- Git push is forbidden. Kenshiro may create local commits only.

## Routing

1. Read `.kenshiro/project-index.yaml` and `.kenshiro/workflow.yaml`.
2. If the project index does not exist, route to `analyze-project`.
3. Reuse project index data. Route `analyze-project` only to validate or refresh outdated evidence.
4. Resolve `active_feature` from the project registry. If absent, generate an unused ID as `YYYYMMDD-NNN-short-name`, where `NNN` is the next zero-padded sequence for the current ISO date in root `workflow.yaml`; create `.kenshiro/features/<feature-id>/`, initialize it from `shared/templates/feature/`, and register it as `IN_PROGRESS`.
5. Read `.kenshiro/features/<feature-id>/workflow.yaml` and route exactly by `current_phase`:
   - `ANALYZE_PROJECT` → `analyze-project`
   - `STACK_APPROVAL` → `validate-stack`
   - `ANALYZE_SPECIFICATION` → `analyze-spec`
   - `IMPACT_ANALYSIS` → `impact-analysis`
   - `IMPACT_APPROVAL` → `validate-impact`
   - `PROPOSE_BRANCH` → `propose-branch`
   - `BRANCH_APPROVAL` → `validate-branch`
   - `GENERATE_TASKS` → `generate-tasks`
   - `TASK_APPROVAL` → `validate-tasks`
   - `IMPLEMENTATION` → `implementation`
   - `REVIEW` → `review`
6. A skill owns its phase until it reaches the completion criteria declared in its `SKILL.md`.
7. If a feature gate status is `FAILED` or `REJECTED`, stop. Route only when a human command creates a legal transition in `shared/state-machine.yaml`.

## Command handling

Route approval commands to the matching validator only. Validators accept exact case-insensitive commands defined in `shared/gates.yaml`, persist the result, and perform no next-phase work in that action. `APPROVE BRANCH` and `RENAME BRANCH <new-name>` route only to `validate-branch`.
