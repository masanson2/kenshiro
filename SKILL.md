---
name: kenshiro
description: Workflow-Driven Development Engine that loads the applicable Kenshiro phase instructions.
version: 2.0.0
---

# Kenshiro Orchestrator

Kenshiro is a Workflow-Driven Development Engine that orchestrates a Spec-Driven Development workflow. Its architecture focuses on workflow control, persistent state, approval gates, traceability, and reproducibility.

Deterministic behavior may be claimed only when validation MCPs independently verify all critical workflow outputs. Kenshiro's current architecture does not provide formal deterministic verification.

## Installation model

Install only this root `kenshiro` skill. The directories below it are bundled phase contracts, not separately installed or invokable skills. After resolving a phase, load that phase's `<phase>/SKILL.md` from this installed skill directory and follow it as the instructions for the current step. Do not ask the user to install a phase contract, and do not invoke it through the agent skill registry.

## Authority

- Repository state has priority over Kenshiro state files, which have priority over approved artifacts.
- `.kenshiro/workflow.yaml` is the project feature registry.
- `.kenshiro/features/<feature-id>/workflow.yaml` is the only workflow authority for that feature.
- `.kenshiro/project-index.yaml`, `.kenshiro/stack.yaml`, and `.kenshiro/docs/stack-review.md` are the only project-wide stack artifacts.
- Feature folders must never contain `stack.yaml`, `project-index.yaml`, or `stack-review.md`.
- English YAML artifacts are the agent's execution state and technical guidance. Italian Markdown artifacts are human-facing documents and must be complete before their corresponding approval gate can advance.
- For projects with APIs, the OpenAPI or technology-equivalent contract file recorded in `stack.yaml.api.source_of_truth` is the API source of truth.
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
Omit `Feature` when `active_feature` is absent, `Phase` when neither a project nor active feature workflow is available, and `Git Branch` when Git has no current branch. `Gate Status` is available only for `STACK_APPROVAL`, `FEATURE_APPROVAL`, `IMPACT_APPROVAL`, `BRANCH_APPROVAL`, `TASK_APPROVAL`, and `IMPLEMENTATION_APPROVAL`; use that phase's corresponding gate status. Resolve the Git branch from the target repository, not from an inferred name.

## Routing

1. Read `.kenshiro/project-index.yaml`, `.kenshiro/stack.yaml`, and root `.kenshiro/workflow.yaml`.
2. Never create a feature directory, Git branch, task plan, impact analysis, or implementation merely because a request was received.
3. If either project stack artifact is absent, set root `project_phase: ANALYZE_PROJECT` and load `analyze-project/SKILL.md`.
4. If project stack artifacts exist, do not load `analyze-project/SKILL.md` except for the exact case-insensitive command `REFRESH STACK`. That command sets root `project_gates.stack.status: PENDING` and `project_phase: ANALYZE_PROJECT` before loading it.
5. Route root project phases exactly as follows:
   - `ANALYZE_PROJECT` -> load `analyze-project/SKILL.md`
   - `STACK_APPROVAL` -> load `validate-stack/SKILL.md`
   - `REQUIREMENTS_ANALYSIS` -> load `analyze-spec/SKILL.md`
   - `REQUIREMENT_DECOMPOSITION` -> load `requirement-decomposition/SKILL.md`
   - `FEATURE_IDENTIFICATION` -> load `feature-identification/SKILL.md`
   - `FEATURE_APPROVAL` -> load `validate-features/SKILL.md`
6. Only after `APPROVE FEATURES` has created feature workspaces, resolve an active feature and load the phase contract matching its `current_phase`:
   - `IMPACT_ANALYSIS` -> load `impact-analysis/SKILL.md`
   - `IMPACT_APPROVAL` -> load `validate-impact/SKILL.md`
   - `PROPOSE_BRANCH` -> load `propose-branch/SKILL.md`
   - `BRANCH_APPROVAL` -> load `validate-branch/SKILL.md`
   - `GENERATE_TASKS` -> load `generate-tasks/SKILL.md`
   - `TASK_APPROVAL` -> load `validate-tasks/SKILL.md`
   - `IMPLEMENTATION_APPROVAL` -> load `validate-implementation/SKILL.md`
   - `IMPLEMENTATION` -> load `implementation/SKILL.md`
   - `REVIEW` -> load `review/SKILL.md`
   The root `active_feature` selects only the feature being routed; no feature may depend on the completion of another feature.
7. The loaded phase contract owns its phase until it reaches the completion criteria declared in its `SKILL.md`.
8. If a project or feature gate status is `FAILED` or `REJECTED`, stop. Route only when a human command creates a legal transition in `shared/state-machine.yaml`.

## Command handling

Load the matching validator contract only for approval commands. Validators accept exact case-insensitive commands defined in `shared/gates.yaml` and persist the result. `APPROVE FEATURES` and `REJECT FEATURES` load only `validate-features/SKILL.md`; `APPROVE FEATURES` is the sole exception that performs the approved feature-creation transition and the only command path that can create feature directories. `APPROVE BRANCH` and `RENAME BRANCH <new-name>` load only `validate-branch/SKILL.md`. After `APPROVE TASKS`, wait in `IMPLEMENTATION_APPROVAL`; only `START IMPLEMENTATION` loads `validate-implementation/SKILL.md` and authorizes source changes. `REFRESH STACK` is the only command that authorizes stack analysis when `.kenshiro/project-index.yaml` already exists.
