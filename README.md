# Kenshiro Skill Ecosystem

Kenshiro is a Workflow-Driven Development Engine composed of an orchestrator and independent phase skills. Each phase has one responsibility, explicit inputs and outputs, completion criteria, and machine-readable state updates.

Its architecture focuses on workflow control, persistent state, approval gates, traceability, and reproducibility. It does not provide formal deterministic verification; deterministic behavior may be claimed only when validation MCPs independently verify all critical workflow outputs.

| Area | Purpose | Format | Language |
|---|---|---|---|
| `.kenshiro/project-index.yaml` | Project metadata, architecture, and conventions | YAML | English |
| `.kenshiro/stack.yaml` | The single repository-wide technology stack description | YAML | English |
| `.kenshiro/docs/stack-review.md` | Review of the repository-wide stack | Markdown | Italian |
| `.kenshiro/workflow.yaml` | Feature registry | YAML | English |
| `.kenshiro/features/<feature-id>/` | Feature state and documents | YAML / Markdown | English / Italian |
| `.kenshiro/docs/` | Project-level human documents | Markdown | Italian |
| `.kenshiro/activity.log` | Append-only audit trail | Text | ISO 8601 timestamps |

The root `workflow.yaml` is the feature registry. Each feature's `workflow.yaml` is the sole workflow authority for that feature. `activity.log` is never used to reconstruct state.

`stack.yaml.build.compile.command` and `stack.yaml.build.test.command` are exact repository-supported quiet console commands. The implementation skill executes quiet compilation after every source modification and quiet tests for every TDD phase. It never derives, substitutes, or suppresses either command externally.

Kenshiro creates local task commits after validation. `git.yaml.push` is permanently `FORBIDDEN`; Kenshiro never pushes or publishes changes remotely.

## Installation

1. Copy `kenshiro/` into an agent skill directory, for example `.agents/skills/kenshiro/`.
2. Ensure the agent loads `SKILL.md` for development-planning requests.
3. Run the skill in a target repository. It creates `.kenshiro/` from `shared/templates/`.
4. Approve the stack, then approve detected features before any feature directory can be created.
5. Approve each feature's mandatory gates before requesting its implementation.

## Skills

```text
kenshiro/
├── analyze-project/
├── validate-stack/
├── analyze-spec/
├── impact-analysis/
├── propose-branch/
├── validate-branch/
├── validate-impact/
├── generate-tasks/
├── validate-tasks/
├── implementation/
├── review/
└── shared/
```

`shared/` is not executable: it contains versioned state contracts, templates, gate definitions, and the state machine used by all skills.

## Target repository artifacts

```text
.kenshiro/
├── project-index.yaml
├── stack.yaml
├── workflow.yaml
├── docs/
│   └── stack-review.md
├── features/
│   └── <feature-id>/
│       ├── feature.yaml
│       ├── impact.yaml
│       ├── tasks.yaml
│       ├── workflow.yaml
│       ├── git.yaml
│       └── docs/
│           ├── impact.md
│           ├── tasks.md
│           └── review.md
└── activity.log
```

`stack.yaml`, `project-index.yaml`, and `docs/stack-review.md` are global artifacts. They exist only at `.kenshiro/`; feature folders must never contain them.

## Feature isolation flow

```text
Analyze Project
↓
Stack Approval
↓
Requirements Analysis
↓
Requirement Decomposition
↓
Feature Identification
↓
Feature Approval
↓
Feature Creation
↓
Branch Proposal
↓
Impact Analysis
```

Feature identification writes `.kenshiro/features-analysis.md`, listing each detected feature's name, description, separation reason, and proposed branch. Only `APPROVE FEATURES` creates one feature folder per approved business capability. `REJECT FEATURES` returns to identification without creating folders, branches, tasks, or implementation. Each feature subsequently receives its own impact analysis, task plan, and dedicated branch.
