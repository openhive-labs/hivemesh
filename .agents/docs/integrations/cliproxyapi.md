# CLIProxyAPI Integration

CLIProxyAPI is a provider-local model gateway and translation layer. HiveMesh supplies peer identity, visibility, leases, remote transport, quota admission, and usage receipts around it.

This design is based on the current official [CLIProxyAPI overview](https://github.com/router-for-me/CLIProxyAPIDocs/blob/main/docs/en/introduction/what-is-cliproxyapi.md), [configuration reference](https://help.router-for.me/configuration/options), and [embedding SDK guide](https://github.com/router-for-me/CLIProxyAPI/blob/main/docs/sdk-usage.md).

## Capability mapping

| CLIProxyAPI capability | HiveMesh use |
|---|---|
| OpenAI-compatible model catalog and chat routes | Project visible and leased models through `/v1/models` and `/v1/chat/completions` |
| OpenAI Responses and Codex compatibility | Project eligible resources through `/v1/responses` |
| Claude-compatible interfaces | Advertise and mount `/v1/messages` when the resource supports it |
| Gemini-compatible interfaces | Advertise native Gemini operations rather than assuming OpenAI translation is always equivalent |
| Grok-compatible interfaces | Expose the declared compatible profile for eligible resources |
| Streaming and non-streaming responses | Populate per-resource stream capabilities and preserve cancellation semantics |
| WebSocket responses where supported | Advertise a real-time/WebSocket capability instead of enabling it globally |
| Function calling and tools | Expose only when both the selected model and lease permit tools |
| Text and image input | Populate input modalities in the resource descriptor |
| Image generation/edit routes | Expose only explicitly image-enabled models and operations |
| Model aliases and display names | Build stable HiveMesh mount names without revealing credential selection |
| Multi-account routing, priority, cooldown, retry, and session affinity | Keep provider-local and opaque to the consumer |
| Hot reload and credential refresh | Trigger safe catalog/capability refresh without restarting HiveMesh identity or leases |
| Reusable Go SDK, middleware, custom routes, and access providers | Support a future embedded adapter without forking CLIProxyAPI |

CLIProxyAPI capability is dynamic. The adapter must not hard-code a universal matrix for all models or providers.

## Integration modes

### Sidecar mode

The first implementation may run CLIProxyAPI as a separate localhost process:

```text
remote peer
  -> HiveLink
  -> provider hivemeshd: identity, lease, quota, receipt
  -> loopback CLIProxyAPI: model routing and translation
  -> provider credential/account
```

Requirements:

- bind CLIProxyAPI to `127.0.0.1` or an equivalent protected local endpoint;
- authenticate the local hop with a dedicated adapter credential;
- keep provider auth files, OAuth state, API keys, and management key outside HiveMesh protocol state;
- allowlist inference routes and methods;
- disable or keep `/v0/management` local-only;
- never expose `management.html`, logs, credential files, or configuration through a lease.

### Embedded SDK mode

The Go SDK can embed routing, authentication, hot reload, translation, and lifecycle management. This mode may later reduce process boundaries and allow HiveMesh-specific middleware or custom local routes.

Embedding does not merge security domains. HiveMesh must still terminate peer authentication and lease admission before CLIProxyAPI selects credentials or sends upstream traffic. CLIProxyAPI management operations remain provider-local.

## Capability discovery

The adapter builds a resource descriptor from four sources, in decreasing trust order:

1. provider-explicit allowlist and HiveMesh exposure policy;
2. CLIProxyAPI configuration and declared model metadata, including aliases and modalities;
3. current model catalog such as `/v1/models`;
4. optional safe capability probes performed by the provider.

Catalog presence alone is insufficient. The adapter records protocol profiles and operations separately, for example:

```yaml
protocols:
  openai:
    operations: [models.list, responses.create, chat.completions]
  anthropic:
    operations: [messages.create]
capabilities:
  streaming: true
  websocket: false
  tools: true
  input_modalities: [text, image]
  output_modalities: [text]
```

The consumer sees only the intersection of this snapshot, provider exposure policy, visibility lease, and execution lease.

## API projection

HiveMesh keeps provider compatibility routes intact for existing clients and adds its own namespace:

```text
/v1/models
/v1/responses
/v1/chat/completions
/v1/messages
/v1beta/models/{model}:generateContent
/v1/images/*
/v1/ws

/v1/hive/capabilities
/v1/hive/resources
/v1/hive/catalog
/v1/hive/leases
/v1/hive/mounts
/v1/hive/invocations
/v1/hive/receipts
```

Only supported and leased compatibility routes are mounted. `/v1/hive/*` represents HiveMesh-native state and generic resource operations; it is not implemented by CLIProxyAPI and must not be passed through to its upstream server.

## Metering and retry

CLIProxyAPI may retry across credentials, apply cooldown, route by priority or round robin, and report provider-specific usage. HiveMesh treats those as one provider-side execution for the same stable invocation ID.

- Credential retries must not consume additional lease request counts.
- A final receipt records the best available token usage and its source.
- Missing or provider-incomparable usage remains `unavailable` or `estimated`; HiveMesh must not invent exact counts.
- Session affinity and credential identity stay private unless a deliberately coarse disclosure is required for troubleshooting.

## Compliance boundary

CLIProxyAPI technical support for OAuth, multiple accounts, or an upstream API does not establish permission to share or lease that access. Providers must expose only resources they are authorized to make available.
