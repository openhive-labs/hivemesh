# Concepts

## Peer

A peer is one HiveMesh installation with its own identity keys, local policy, resources, leases, and durable state. Every peer can provide and consume resources. Those roles are defined by individual leases rather than separate server and client products.

## Resource

A resource is a capability owned by a peer:

- model;
- agent;
- tool;
- workflow;
- service.

The public descriptor explains how the resource may be used. Its implementation, upstream credentials, local address, private prompt, and internal configuration stay with the provider.

## Exposure policy

Exposure policy is the provider's local maximum. It answers which resources and fields may ever be shown to a particular peer or group. A lease cannot override a stricter local policy.

## Visibility lease

A visibility lease controls discovery. It determines which resources and metadata appear in the consumer's catalog and, for model resources, which entries may be projected into `/v1/models`.

Visibility does not authorize invocation.

## Execution lease

An execution lease authorizes a consumer peer to invoke a selected resource within limits such as:

- start and expiry time;
- request count;
- input and output tokens;
- requests per minute;
- concurrency;
- optional protocol capabilities.

The provider enforces these limits locally. Lease updates create new immutable revisions rather than rewriting history.

## HiveLink

HiveLink is the authenticated, transport-neutral session between peers. LAN, Tailscale, direct QUIC, WebRTC, WebSocket, or relay may provide connectivity underneath it. Changing the connection path does not change peer identity or lease authority.

## Offer capsule

An offer capsule is a short-lived invitation exchanged through IM, QR, or a file. It introduces the provider identity, connection candidates, initial visibility terms, and a one-time nonce. The nonce binds first acceptance; it is not a reusable API key.

## Local mount

A consumer uses a remote resource through a local daemon endpoint. A mounted model can look like an ordinary model to an existing client even though execution remains on the provider machine.

## Usage receipt

A usage receipt records one invocation's identity, lease, resource, status, and measurable consumption. Receipts support audit and reconciliation. They do not prove output quality or create a payment system by themselves.

## System boundary

```text
out-of-band invitation
          |
          v
consumer hivemeshd <--- HiveLink ---> provider hivemeshd
       |                                      |
local model client                   provider resource
```

No central service is required for discovery, agreement, or invocation in the trusted-network mode.
