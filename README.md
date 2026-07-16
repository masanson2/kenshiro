# Kenshiro Skill Ecosystem

Kenshiro is an orchestrator plus independent phase skills. Each phase has one responsibility, explicit deterministic inputs and outputs, completion criteria, and machine-readable state updates.

| Area | Purpose | Format | Language |
|---|---|---|---|
| `.kenshiro/project-index.yaml` | Project stack, architecture, conventions, and repository metadata | YAML | English |
| `.kenshiro/workflow.yaml` | Feature registry | YAML | English |
| `.kenshiro/features/<feature-id>/` | Feature state and documents | YAML / Markdown | English / Italian |
| `.kenshiro/docs/` | Project-level human documents | Markdown | Italian |
| `.kenshiro/activity.log` | Append-only audit trail | Text | ISO 8601 timestamps |

The root `workflow.yaml` is the feature registry. Each feature's `workflow.yaml` is the sole workflow authority for that feature. `activity.log` is never used to reconstruct state.

`project-index.yaml.stack.build.compile.command` is the exact repository-supported compilation command. The implementation skill executes it after every source modification and records the result in the active task. It never derives or substitutes a command. Tasks use TDD and require a passing compile result before completion.

## Installation

1. Copy `kenshiro/` into an agent skill directory, for example `.agents/skills/kenshiro/`.
2. Ensure the agent loads `SKILL.md` for development-planning requests.
3. Run the skill in a target repository. It creates `.kenshiro/` from `shared/templates/`.
4. Approve each mandatory gate before requesting implementation.

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
├── workflow.yaml
├── docs/
│   └── stack.md
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
