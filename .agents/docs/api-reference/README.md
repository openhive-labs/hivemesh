# API Reference

> **Status:** planned local API surface. Paths and schemas remain provisional until implementation and the protocol schemas land.

## API separation

`hivemeshd` exposes two logically separate local surfaces:

- **Control API:** peer, resource, invitation, connection, lease, and status management.
- **Invocation API:** compatible model and resource calls for local applications.

Neither surface should be reachable from a remote peer as an unrestricted administration API.

## Planned control resources

```text
GET    /control/v1/status
GET    /control/v1/identity
GET    /control/v1/peers
GET    /control/v1/resources
POST   /control/v1/resources
POST   /control/v1/invitations
POST   /control/v1/connections
GET    /control/v1/leases
POST   /control/v1/leases/{id}/revoke
GET    /control/v1/invocations/{id}
```

Desktop, CLI, and WebUI use this same API. Local authorization must distinguish read-only inspection from resource, lease, and daemon administration.

## Model-compatible invocation

The baseline local surface includes:

```text
GET  /v1/models
POST /v1/responses
POST /v1/chat/completions
```

An Anthropic-compatible integration may also expose:

```text
POST /v1/messages
```

The local model name resolves to a specific provider peer, resource ID, and active execution lease. The caller receives a local credential; it never receives the provider's upstream key or a reusable remote bearer URL.

## Errors

The local API will distinguish inactive lease, scope denial, quota exhaustion, rate limit, concurrency limit, unavailable resource, unsupported capability, interrupted invocation, and provider failure. Remote errors are sanitized before reaching local clients.

## Streaming and cancellation

Compatible APIs preserve their normal streaming format while the gateway maps HiveLink sequence and terminal events to that format. Client disconnect triggers best-effort cancellation rather than silently abandoning provider work.
