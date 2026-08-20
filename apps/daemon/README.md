# Daemon

`hivemeshd` is the persistent source of runtime truth for one peer.

Responsibilities:

- peer identity and private-key access through the identity package;
- durable resource, exposure, lease, revocation, quota, and receipt state;
- local Control API and Invocation API;
- HiveLink listener, dialing, session lifecycle, and channel routing;
- provider-side authorization before resource execution;
- consumer-side remote resource mounts;
- restart recovery, health reporting, and structured audit events.

The daemon must continue operating independently of any Desktop or WebUI window. It must not expose provider-local management routes to remote peers or store upstream credentials in protocol-visible state.

Initial implementation should run with minimal host privileges and bind local administrative APIs to a protected local endpoint.
