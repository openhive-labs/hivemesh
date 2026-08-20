# Getting Started

> **Project status:** HiveMesh is not installable yet. This page defines the planned first-use workflow and will become the installation guide when Alpha artifacts exist.

## What each machine runs

One persistent `hivemeshd` owns identity, resources, leases, connectivity, invocation, and local state. Desktop, CLI, WebUI, and model clients are interfaces to that same service.

```text
Desktop / CLI / WebUI / model client
                 |
             hivemeshd
                 |
       local models and agents
```

Closing the WebUI does not end leases or connections. Closing the Desktop window may leave the daemon running according to user preference. Only an explicit stop action terminates the service.

## Planned first run

```sh
hive start
hive status
hive ui
```

- `hive start` installs or starts the local background service.
- `hive status` reports identity, daemon health, connectivity, resources, and active leases.
- `hive ui` ensures the daemon is running and opens the local WebUI.

Exact command syntax is provisional until the CLI exists.

## First two-peer flow

### Provider

1. Register a local resource, such as an OpenAI-compatible model server.
2. Select which resource fields the intended peer may see.
3. Create a short-lived one-time invitation.
4. Send the invitation through a trusted out-of-band channel.

### Consumer

1. Import the invitation and verify the provider identity fingerprint through the same trusted channel.
2. Establish a HiveLink session using a mutually supported connection candidate.
3. View the provider's filtered catalog.
4. Request and accept an execution lease for a selected resource.
5. Mount the remote resource behind a local compatible endpoint.

### Use the model

The intended consumer configuration changes only three familiar values:

```text
Base URL: local HiveMesh endpoint
API key: local lease/mount credential
Model: mounted resource name
```

The local daemon adds peer identity and lease proof before forwarding the request. The provider's upstream key remains on the provider machine.

## Trust warning

The provider executes the request and may be able to observe prompts and outputs. HiveMesh protects credentials and authorization boundaries; it does not make a workload confidential from its provider.
