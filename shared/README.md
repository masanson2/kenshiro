# Shared Contracts

This directory holds non-executable, versioned contracts consumed by every Kenshiro skill:

- `state-machine.yaml`: project and feature phases, including feature and branch approval
- `gates.yaml`: accepted approval commands and gate behavior
- `implementation-guard.yaml`: source-modification preconditions
- `schemas/`: canonical YAML artifact schemas
- `templates/project/`: project-index, the single stack description, feature registry, project documents, and feature-analysis document
- `templates/feature/`: feature state and feature documents

All paths are resolved relative to the Kenshiro skill root. The only stack artifacts generated for a target repository are `.kenshiro/project-index.yaml`, `.kenshiro/stack.yaml`, and `.kenshiro/docs/stack-review.md`; shared assets contain no feature-specific artifact.
