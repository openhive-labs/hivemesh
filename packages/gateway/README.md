# Gateway

Maps local compatible APIs and resource-specific calls to HiveLink invocations, and maps provider-side invocations to explicitly configured local adapters.

Responsibilities:

- resolve local mount names to peer, resource, and active execution lease;
- preserve model-compatible streaming, tools, vision, and error semantics where advertised;
- authorize on the provider before forwarding any body to a resource;
- inject provider-local upstream credentials only inside the local adapter;
- support cancellation, backpressure, idempotency, and status recovery;
- sanitize local paths, credentials, and adapter internals from remote errors.

The gateway is not a general reverse proxy. Every route, method, protocol, and capability must be explicitly registered and leased.
