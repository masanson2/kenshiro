---
name: kenshiro
description: Workflow-Driven Development Engine that routes only to independently reusable Kenshiro phase skills.
version: 2.0.0
---

# Kenshiro Orchestrator

Kenshiro is a Workflow-Driven Development Engine that orchestrates a Spec-Driven Development workflow. Its architecture focuses on workflow control, persistent state, approval gates, traceability, and reproducibility. It does not analyze repositories, generate artifacts, validate gates, or write reviews itself. It does not modify application code. Those operations belong exclusively to the phase skills in this directory.

Deterministic behavior may be claimed only when validation MCPs independently verify all critical workflow outputs. Kenshiro's current architecture does not provide formal deterministic verification.

## Authority

- Repository state has priority over Kenshiro state files, which have priority over approved artifacts.
- `.kenshiro/workflow.yaml` is the project feature registry.
- `.kenshiro/features/<feature-id>/workflow.yaml` is the only workflow authority for that feature.
- `.kenshiro/project-index.yaml`, `.kenshiro/stack.yaml`, and `.kenshiro/docs/stack-review.md` are the only project-wide stack artifacts.
- Feature folders must never contain `stack.yaml`, `project-index.yaml`, or `stack-review.md`.
- Chat history and `.kenshiro/activity.log` are not state sources.
- `shared/state-machine.yaml` defines the only legal transitions.
- `shared/implementation-guard.yaml` forbids source modification unless all gates are approved.
- Git push is forbidden. Kenshiro may create local commits only.

## Runtime information

Before routing a command or executing a workflow step, print the `Runtime Information` heading and only the available lines in this exact format:
   ```text
   Project       : <target repository directory name>
   Feature       : <workflow.yaml active_feature>
   Phase         : <active feature workflow.yaml current_phase>
   Gate Status   : <current project or active feature gate for the current approval phase>
   Git Branch    : <current Git branch>
   ```
Omit `Feature` when `active_feature` is absent, `Phase` when neither a project nor active feature workflow is available, and `Git Branch` when Git has no current branch. `Gate Status` is available only for `STACK_APPROVAL`, `FEATURE_APPROVAL`, `IMPACT_APPROVAL`, `BRANCH_APPROVAL`, and `TASK_APPROVAL`; use that phase's corresponding gate status. Resolve the Git branch from the target repository, not from an inferred name.

## Routing

1. Read `.kenshiro/project-index.yaml`, `.kenshiro/stack.yaml`, and root `.kenshiro/workflow.yaml`.
2. Never create a feature directory, Git branch, task plan, impact analysis, or implementation merely because a request was received.
3. If either project stack artifact is absent, route root `project_phase: ANALYZE_PROJECT` to `analyze-project`.
4. If project stack artifacts exist, do not route to `analyze-project` except for the exact case-insensitive command `REFRESH STACK`. That command sets root `project_gates.stack.status: PENDING` and `project_phase: ANALYZE_PROJECT` before routing.
5. Route root project phases exactly as follows:
   - `ANALYZE_PROJECT` → `analyze-project`
   - `STACK_APPROVAL` → `validate-stack`
   - `REQUIREMENTS_ANALYSIS` → `analyze-spec`
   - `REQUIREMENT_DECOMPOSITION` → `requirement-decomposition`
   - `FEATURE_IDENTIFICATION` → `feature-identification`
   - `FEATURE_APPROVAL` → `validate-features`
6. Only after `APPROVE FEATURES` has created feature workspaces, resolve an active feature and route its workflow exactly by `current_phase`:
   - `IMPACT_ANALYSIS` → `impact-analysis`
   - `IMPACT_APPROVAL` → `validate-impact`
   - `PROPOSE_BRANCH` → `propose-branch`
   - `BRANCH_APPROVAL` → `validate-branch`
   - `GENERATE_TASKS` → `generate-tasks`
   - `TASK_APPROVAL` → `validate-tasks`
   - `IMPLEMENTATION` → `implementation`
   - `REVIEW` → `review`
   The root `active_feature` selects only the feature being routed; no feature may depend on the completion of another feature.
7. A skill owns its phase until it reaches the completion criteria declared in its `SKILL.md`.
8. If a project or feature gate status is `FAILED` or `REJECTED`, stop. Route only when a human command creates a legal transition in `shared/state-machine.yaml`.

## Command handling

Route approval commands to the matching validator only. Validators accept exact case-insensitive commands defined in `shared/gates.yaml` and persist the result. `APPROVE FEATURES` and `REJECT FEATURES` route only to `validate-features`; `APPROVE FEATURES` is the sole exception that performs the approved feature-creation transition and the only command path that can create feature directories. `APPROVE BRANCH` and `RENAME BRANCH <new-name>` route only to `validate-branch`. `REFRESH STACK` is the only command that authorizes stack analysis when `.kenshiro/project-index.yaml` already exists.
