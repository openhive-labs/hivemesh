# Identity

Implements local peer identity, signing, verification, key custody, fingerprints, and handshake proofs defined by `hivemesh-protocol`.

Boundaries:

- private keys never leave this package through serializable protocol objects;
- transport addresses and Tailscale identity are not accepted as HiveMesh identity;
- call sites request purpose-bound signatures instead of raw key access;
- key loss, rotation, export, and reset require explicit user-visible workflows;
- tests cover unknown keys, wrong-purpose signatures, transcript replay, and corrupted local state.
