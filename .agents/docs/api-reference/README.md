# API Reference

> **Status:** planned local API surface. Paths and schemas remain provisional until implementation and the protocol schemas land.

## API separation

`hivemeshd` exposes two logically separate local surfaces:

- **Control API:** peer, resource, invitation, connection, lease, and status management.
- **Invocation API:** compatible model and resource calls for local applications.

Neither surface should be reachable from a remote peer as an unrestricted administration API.

## Hive-native API

HiveMesh-specific operations use the `/v1/hive/` namespace. This avoids colliding with OpenAI, Anthropic, Gemini, Codex, or other compatibility routes while giving Desktop, CLI, WebUI, and third-party HiveMesh clients one stable local surface.

```text
GET    /v1/hive/status
GET    /v1/hive/identity
GET    /v1/hive/capabilities

GET    /v1/hive/peers
GET    /v1/hive/catalog?peer_id={peer_id}
POST   /v1/hive/invitations
POST   /v1/hive/connections

GET    /v1/hive/resources
POST   /v1/hive/resources
GET    /v1/hive/resources/{resource_id}
PATCH  /v1/hive/resources/{resource_id}

GET    /v1/hive/leases
POST   /v1/hive/leases
POST   /v1/hive/leases/{lease_id}/accept
POST   /v1/hive/leases/{lease_id}/counter
POST   /v1/hive/leases/{lease_id}/revoke

GET    /v1/hive/mounts
POST   /v1/hive/mounts
POST   /v1/hive/invocations
GET    /v1/hive/invocations/{invocation_id}
POST   /v1/hive/invocations/{invocation_id}/cancel
GET    /v1/hive/receipts
```

`/v1/hive/invocations` is the generic entry point for agents, tools, workflows, and services that do not fit a model-provider dialect. Its request still names a mounted resource and active execution lease.

Desktop, CLI, and WebUI use this same API. Local authorization must distinguish read-only inspection, invocation, peer management, resource administration, and lease administration.

The namespace is a **local daemon API**, not a remote peer administration API. Peer-to-peer equivalents travel through authenticated HiveLink channels. A daemon must not blindly forward `/v1/hive/*` HTTP requests to another peer.

## Model-compatible invocation

The baseline local surface includes:

```text
GET  /v1/models
POST /v1/responses
POST /v1/chat/completions
```

Adapters may expose additional provider-native or compatible surfaces when the selected resource declares them:

```text
POST /v1/messages                              # Anthropic-compatible
POST /v1beta/models/{model}:generateContent    # Gemini-compatible
POST /v1beta/models/{model}:streamGenerateContent
POST /v1/images/generations                    # image-capable profiles
POST /v1/images/edits
GET  /v1/ws                                    # real-time/WebSocket profiles
```

The local model name resolves to a specific provider peer, resource ID, and active execution lease. The caller receives a local credential; it never receives the provider's upstream key or a reusable remote bearer URL.

Compatibility is capability-driven. A model appearing in `/v1/models` does not prove that every route, modality, tool mode, streaming mode, or usage field is supported. `/v1/hive/capabilities` and the resource descriptor provide the negotiated profile for the mounted resource.

## CLIProxyAPI boundary

CLIProxyAPI may serve as a provider-local adapter for OpenAI, Responses/Codex, Gemini, Claude, Grok, image, and WebSocket-capable routes. HiveMesh exposes only the subset explicitly discovered, allowed, and leased for a resource.

CLIProxyAPI's `/v0/management` routes and bundled management panel are provider administration surfaces. They must remain local, require their own management key when enabled, and must never be projected through `/v1/hive`, a resource mount, or HiveLink.

## Errors

The local API will distinguish inactive lease, scope denial, quota exhaustion, rate limit, concurrency limit, unavailable resource, unsupported capability, interrupted invocation, and provider failure. Remote errors are sanitized before reaching local clients.

## Streaming and cancellation

Compatible APIs preserve their normal streaming format while the gateway maps HiveLink sequence and terminal events to that format. Client disconnect triggers best-effort cancellation rather than silently abandoning provider work.
