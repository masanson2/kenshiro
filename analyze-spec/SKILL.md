---
name: kenshiro-requirements-analysis
description: Normalize an approved stack's user request into project-scoped requirements before feature creation.
version: 1.0.0
---

# Requirements Analysis

## Responsibility

Extract the requested business requirements without creating feature state.

## Inputs

- User-provided change request
- `.kenshiro/project-index.yaml`
- `.kenshiro/stack.yaml`
- `.kenshiro/workflow.yaml`
- Root workflow with approved stack gate

## Outputs

- Updated `.kenshiro/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Require root `project_phase: REQUIREMENTS_ANALYSIS` and an approved stack gate. Normalize only explicit requirements, acceptance criteria, and constraints into root `workflow.yaml.requirements`. Do not create or register a feature directory, create a Git branch, generate tasks, analyze impacts, or implement changes. Use compact English YAML. Do not infer requirements.

## State update

Set root `project_skills.requirements-analysis.status: DONE` and `project_phase: REQUIREMENT_DECOMPOSITION`. Append `Requirements Analysis Completed`.

## Completion criteria

The normalized project requirements contain no inferred requirements and no feature directory, branch, task plan, or implementation was created.
