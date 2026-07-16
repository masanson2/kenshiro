# Shared Contracts

This directory holds non-executable, versioned contracts consumed by every Kenshiro skill:

- `state-machine.yaml`: legal phases and transitions, including branch approval
- `gates.yaml`: accepted approval commands and gate behavior
- `implementation-guard.yaml`: source-modification preconditions
- `schemas/`: canonical YAML artifact schemas
- `templates/project/`: project-index, feature registry, and project documents
- `templates/feature/`: feature state and feature documents

All paths are resolved relative to the Kenshiro skill root. Shared assets contain no feature-specific artifact.
