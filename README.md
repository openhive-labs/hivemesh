# HiveMesh

Peer-to-peer leasing for models, agents, tools, workflows, and machine intelligence.

HiveMesh is designed for friends and teams that want to share selected capabilities between independently operated computers without transferring upstream credentials or centralizing normal invocation traffic.

> **Status:** architecture and protocol planning. There is no installable release yet.

## Intended experience

- Run one persistent `hivemeshd` peer on each machine.
- Register a local model server, wrapped upstream gateway, agent, tool, or service.
- Send a one-time invitation through IM, QR, or a file.
- Reveal a peer-specific catalog through a visibility lease.
- Agree an execution lease with time, request, token, rate, and concurrency bounds.
- Use a leased model through a normal local OpenAI- or Anthropic-compatible API.
- Expire, exhaust, supersede, or revoke access without revealing provider credentials.

Tailscale is an optional connectivity adapter. HiveMesh identity and authorization are transport-independent.

## Applications

- `apps/daemon`: persistent runtime, local APIs, and policy enforcement.
- `apps/desktop`: normal user entry point and daemon lifecycle integration.
- `apps/web`: local UI served by the daemon and embedded by Desktop.
- `apps/cli`: advanced control and automation interface.

## Core packages

- `identity`: peer identity and key custody.
- `catalog`: resource registry, exposure policy, and filtered discovery.
- `lease`: visibility and execution lease state.
- `link`: HiveLink sessions and transport adapters.
- `gateway`: model-compatible and resource-specific invocation routing.
- `metering`: quota accounting and signed usage receipts.

The normative protocol lives in [`openhive-labs/hivemesh-protocol`](https://github.com/openhive-labs/hivemesh-protocol). Product documentation lives in `.agents/docs`; maintainer constraints live in `.agents/rules`.
