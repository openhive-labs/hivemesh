# Maintenance Rules

- Preserve the persistent-daemon architecture.
- Keep CLI, Desktop, and WebUI as interfaces to the same daemon.
- Preserve normal model-compatible invocation as a baseline capability.
- Bind identity and leases to HiveMesh peer keys, never transport addresses.
- Keep protocol changes synchronized with `hivemesh-protocol`.
- Require end-to-end tests for lease and visibility behavior.
