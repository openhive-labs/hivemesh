# Lease

Implements visibility and execution lease negotiation, canonical verification, persistence, current-revision selection, expiry, quota admission, supersession, and revocation.

Requirements:

- historical lease versions are immutable;
- every grant is bound to provider and consumer peer identities;
- visibility does not imply execution;
- provider-local policy may narrow any grant;
- quota admission is atomic across concurrent invocations;
- expiry, exhaustion, and revocation survive restart;
- a copied lease cannot be used by a different authenticated peer;
- protocol-visible behavior matches `hivemesh-protocol` and its conformance cases.

Pricing, payments, reputation, and marketplace settlement are outside this package.
