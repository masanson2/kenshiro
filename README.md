# Kenshiro Skill Ecosystem

Kenshiro is a Workflow-Driven Development Engine composed of one installed orchestrator and bundled phase contracts. Each phase has one responsibility, explicit inputs and outputs, completion criteria, and machine-readable state updates.

Its architecture focuses on workflow control, persistent state, approval gates, traceability, and reproducibility. It does not provide formal deterministic verification; deterministic behavior may be claimed only when validation MCPs independently verify all critical workflow outputs.

| Area | Audience and purpose | Format | Language |
|---|---|---|---|
| `.kenshiro/project-index.yaml` | Agent technical guidance: project metadata, architecture, and conventions | YAML | English |
| `.kenshiro/stack.yaml` | Agent technical guidance: repository-wide technology stack | YAML | English |
| `.kenshiro/docs/stack-review.md` | Human approval document: repository-wide stack review | Markdown | Italian |
| `.kenshiro/workflow.yaml` | Agent execution state: feature registry | YAML | English |
| `.kenshiro/features/<feature-id>/*.yaml` | Agent technical guidance and execution state | YAML | English |
| `.kenshiro/features/<feature-id>/docs/` | Human approval and review documents | Markdown | Italian |
| `.kenshiro/activity.log` | Append-only audit trail | Text | ISO 8601 timestamps |

The root `workflow.yaml` is the feature registry. Each feature's `workflow.yaml` is the sole workflow authority for that feature. YAML artifacts guide the agent; their corresponding Italian Markdown documents are technical, discursive analyses for the user and must be complete before approval. For projects with APIs, the OpenAPI or technology-equivalent contract file recorded in `stack.yaml.api.source_of_truth` is the API source of truth. Impact and task documents include a Mermaid class/component diagram, query analysis, API contract analysis, and As-is/To-be variations. After task-document approval, Kenshiro waits for the explicit `START IMPLEMENTATION` command before it changes source code. `activity.log` is never used to reconstruct state.

`stack.yaml.build.compile.command` and `stack.yaml.build.test.command` are exact repository-supported quiet console commands. The implementation skill executes quiet compilation after every source modification and quiet tests for every TDD phase. It never derives, substitutes, or suppresses either command externally.

Kenshiro creates local task commits after validation. `git.yaml.push` is permanently `FORBIDDEN`; Kenshiro never pushes or publishes changes remotely.

## Installation

1. Copy the complete `kenshiro/` directory into an agent skill directory, for example `.agents/skills/kenshiro/`.
2. Install or register only the root `kenshiro` skill (`kenshiro/SKILL.md`) for development-planning requests. Do not install any nested `SKILL.md` files separately.
3. Run the skill in a target repository. It creates `.kenshiro/` from `shared/templates/`.
4. Approve the stack, then approve detected features before any feature directory can be created.
5. Approve each feature's human-facing planning documents, then issue `START IMPLEMENTATION` to begin source changes.

## Bundled phase contracts

```text
kenshiro/
|-- analyze-project/
|-- validate-stack/
|-- analyze-spec/
|-- impact-analysis/
|-- propose-branch/
|-- validate-branch/
|-- validate-impact/
|-- generate-tasks/
|-- validate-tasks/
|-- validate-implementation/
|-- implementation/
|-- review/
`-- shared/
```

The root orchestrator resolves the active phase and loads its bundled `<phase>/SKILL.md` directly. Nested contracts are not agent skills and must not be registered independently. `shared/` is not executable: it contains versioned state contracts, templates, gate definitions, and the state machine used by all phase contracts.

## Target repository artifacts

```text
.kenshiro/
|-- project-index.yaml
|-- stack.yaml
|-- workflow.yaml
|-- docs/
|   `-- stack-review.md
|-- features/
|   `-- <feature-id>/
|       |-- feature.yaml
|       |-- impact.yaml
|       |-- tasks.yaml
|       |-- workflow.yaml
|       |-- git.yaml
|       `-- docs/
|           |-- impact.md
|           |-- branch.md
|           |-- tasks.md
|           `-- review.md
`-- activity.log
```

`stack.yaml`, `project-index.yaml`, and `docs/stack-review.md` are global artifacts. They exist only at `.kenshiro/`; feature folders must never contain them.

## Feature isolation flow

```text
Analyze Project
v
Stack Approval
v
Requirements Analysis
v
Requirement Decomposition
v
Feature Identification
v
Feature Approval
v
Feature Creation
v
Branch Proposal
v
Branch Approval
v
Impact Analysis
v
Impact Approval
v
Task Generation
v
Task Approval
v
Start Implementation
v
Implementation
v
Review
```

Feature identification writes `.kenshiro/features-analysis.md`, listing each detected feature's name, description, separation reason, and proposed branch. Only `APPROVE FEATURES` creates one feature folder per approved business capability. `REJECT FEATURES` returns to identification without creating folders, branches, tasks, or implementation. Each feature subsequently receives its own impact analysis, task plan, and dedicated branch.
