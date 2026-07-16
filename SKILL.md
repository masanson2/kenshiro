---
name: kenshiro
description: Deterministic Spec-Driven Development workflow orchestrator that routes only to independently reusable Kenshiro phase skills.
version: 2.0.0
---

# Kenshiro Orchestrator

Kenshiro orchestrates a deterministic Spec-Driven Development workflow. It does not analyze repositories, generate artifacts, validate gates, or write reviews itself. It does not modify application code. Those operations belong exclusively to the phase skills in this directory.

## Authority

- `.kenshiro/state/workflow.yaml` is the only workflow authority.
- Chat history and `.kenshiro/activity.log` are not state sources.
- `shared/state-machine.yaml` defines the only legal transitions.
- `shared/implementation-guard.yaml` forbids source modification unless all gates are approved.

## Routing

1. Read `.kenshiro/state/workflow.yaml`.
2. If it does not exist, route to `analyze-project`.
3. If it exists, route exactly by `current_phase`:
   - `ANALYZE_PROJECT` → `analyze-project`
   - `STACK_APPROVAL` → `validate-stack`
   - `ANALYZE_SPECIFICATION` → `analyze-spec`
   - `IMPACT_ANALYSIS` → `impact-analysis`
   - `IMPACT_APPROVAL` → `validate-impact`
   - `GENERATE_TASKS` → `generate-tasks`
   - `TASK_APPROVAL` → `validate-tasks`
   - `IMPLEMENTATION` → `implementation`
   - `REVIEW` → `review`
4. A skill owns its phase until it reaches the completion criteria declared in its `SKILL.md`.

## Command handling

Route approval commands to the matching validator only. Validators accept exact case-insensitive commands defined in `shared/gates.yaml`, persist the result, and perform no next-phase work in that action.
