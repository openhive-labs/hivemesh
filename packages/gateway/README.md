# Gateway

Maps local compatible APIs and resource-specific calls to HiveLink invocations, and maps provider-side invocations to explicitly configured local adapters.

Responsibilities:

- resolve local mount names to peer, resource, and active execution lease;
- preserve model-compatible streaming, tools, vision, and error semantics where advertised;
- authorize on the provider before forwarding any body to a resource;
- inject provider-local upstream credentials only inside the local adapter;
- support cancellation, backpressure, idempotency, and status recovery;
- sanitize local paths, credentials, and adapter internals from remote errors.

The local HTTP surface is layered:

- provider-compatible routes such as `/v1/models`, `/v1/responses`, `/v1/chat/completions`, `/v1/messages`, declared Gemini routes, image routes, and WebSocket routes;
- HiveMesh-native `/v1/hive/*` routes for peers, resources, catalogs, leases, mounts, generic invocation, receipts, and capabilities.

Compatibility routes are mounted per resource capability. The gateway must not infer that every model supports every dialect merely because it appears in a shared model catalog.

For CLIProxyAPI, the gateway may use a protected loopback sidecar first and an embedded SDK adapter later. In either mode, `/v0/management`, the management panel, credentials, configuration, and logs are provider-local administration surfaces and cannot become resource routes.

The gateway is not a general reverse proxy. Every route, method, protocol, and capability must be explicitly registered and leased.
