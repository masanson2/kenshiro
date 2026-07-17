---
name: kenshiro-validate-implementation
description: Wait for and process the explicit command that authorizes approved work to begin.
version: 1.0.0
---

# Validate Implementation

## Responsibility

Authorize implementation only after all human-facing planning documents are approved.

## Inputs

- `.kenshiro/features/<feature-id>/workflow.yaml`
- `.kenshiro/features/<feature-id>/impact.yaml`
- `.kenshiro/features/<feature-id>/tasks.yaml`
- `.kenshiro/features/<feature-id>/docs/tasks.md`
- `.kenshiro/stack.yaml`
- Exact command: `START IMPLEMENTATION`
- `../shared/gates.yaml`

## Outputs

- Updated `.kenshiro/features/<feature-id>/workflow.yaml`
- Appended `.kenshiro/activity.log`

## Rules

Require `current_phase: IMPLEMENTATION_APPROVAL`, approved `impact`, `branch`, and `tasks` gates, and a complete technical, discursive Italian `docs/tasks.md` with no unresolved placeholders, a Mermaid class/component diagram, query analysis, and As-is and To-be variations. Until the exact case-insensitive command `START IMPLEMENTATION` is received, make no source or workflow changes and wait for the user. On that command, set `gates.implementation.status: APPROVED` and `current_phase: IMPLEMENTATION`. Do not modify source code.

## State update

Persist the authorization and append `Implementation Started`.

## Completion criteria

Only `START IMPLEMENTATION` transitions the workflow to `IMPLEMENTATION`, with the implementation gate `APPROVED`.
