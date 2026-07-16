# Shared Contracts

This directory holds non-executable, versioned contracts consumed by every Kenshiro skill:

- `state-machine.yaml`: legal phases and transitions
- `gates.yaml`: accepted approval commands and gate behavior
- `implementation-guard.yaml`: source-modification preconditions
- `schemas/`: canonical YAML artifact schemas
- `templates/`: initial state and human-document templates
- `examples/`: reference artifacts

All paths are resolved relative to the Kenshiro skill root.
