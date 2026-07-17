---
name: kenshiro-feature-identification
description: Produce the approval-ready isolated feature analysis from decomposed business capabilities.
version: 1.0.0
---

# Feature Identification

## Responsibility

Produce the feature proposal document without creating feature workspaces or branches.

## Inputs

- `.kenshiro/workflow.yaml` with `project_phase: FEATURE_IDENTIFICATION`
- Decomposed root `workflow.yaml.capabilities`
- `../shared/templates/project/features-analysis.md`

## Outputs

- `.kenshiro/features-analysis.md`
- Updated `.kenshiro/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Generate the Italian human-facing `features-analysis.md` from the shared template. For every capability include the feature name, description, reason for separation, and proposed branch. The root `workflow.yaml.capabilities` remains the agent's technical planning state. Every detected feature represents exactly one business capability and has exactly one proposed branch. Do not create feature directories, Git branches, tasks, impact analyses, or implementation.

## State update

Set root `project_skills.feature-identification.status: DONE`, root `project_gates.features.status: PENDING`, and `project_phase: FEATURE_APPROVAL`. Append `Features Identified`.

## Completion criteria

The document covers every capability, gives each feature one unique proposed branch, and no feature directory or Git branch exists.
