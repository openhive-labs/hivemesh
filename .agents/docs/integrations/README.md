# Integrations

HiveMesh integrations adapt local resources or connectivity without changing peer identity and lease semantics.

## Resource adapters

### OpenAI-compatible servers

The first integration target is a provider-local OpenAI-compatible endpoint such as a local model server or an authorized gateway. The adapter:

- connects only to local configured endpoints;
- maps provider models to HiveMesh resource IDs;
- advertises supported streaming, tool, vision, and token-usage features;
- injects local upstream credentials after lease authorization;
- sanitizes provider errors before returning them to a peer.

The management interface of CLIProxyAPI or any other gateway must never be exposed as a leasable route.

### Anthropic-compatible models

An Anthropic-facing profile may project eligible resources through `/v1/messages`. Capability differences must be explicit; the adapter must not pretend unsupported semantics are equivalent.

### Agents and generic HTTP services

An existing agent may be made leasable by placing the HiveMesh gateway in front of its localhost HTTP or MCP interface. The initial product does not require a packaging SDK or knowledge of the agent's internal logic.

The provider must explicitly allow routes and methods. Arbitrary reverse proxying to a host is not an acceptable integration.

### MCP tools

MCP exposure requires capability negotiation and tool-level authorization. Provider tool credentials remain local. Each call is still associated with a peer, execution lease, invocation ID, and receipt.

## Connectivity adapters

### LAN or existing tunnel

A generic authenticated TLS listener can run over LAN, a user-managed VPN, SSH tunnel, or other existing connectivity. This is sufficient for the first vertical prototype.

### Tailscale

Tailscale may supply device reachability or shared-machine connectivity. Tailnet identity, IP address, ACL, and device share are not HiveMesh identity or resource authorization. HiveLink mutual authentication and leases still apply.

### Future adapters

Direct QUIC, WebRTC, WebSocket, and encrypted relay implementations must satisfy the same HiveLink adapter contract. Adding one must not require changes to catalog, lease, gateway, or user-facing resource semantics.

## Provider responsibility

Technical compatibility does not grant legal or contractual permission to share an upstream account or service. Integration documentation must distinguish supported connectivity from allowed use under an upstream provider's terms.
