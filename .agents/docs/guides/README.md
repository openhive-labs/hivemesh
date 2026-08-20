# Guides

> The following workflows describe the planned Alpha behavior. Commands are illustrative until a release exists.

## Share a model

1. Run the model or upstream gateway on localhost.
2. Register it with HiveMesh; do not expose its administration interface.
3. Describe compatible capabilities such as chat, streaming, tools, and vision.
4. Define local exposure policy for the intended peer.
5. Create an invitation with a short expiry and initial visibility scope.

Illustrative flow:

```sh
hive resource add local-coder \
  --kind model \
  --protocol openai-compatible \
  --endpoint http://127.0.0.1:9000

hive invite create \
  --peer friend \
  --show local-coder \
  --ttl 24h
```

The endpoint is private local configuration and must not enter the catalog or invitation.

## Connect to a provider

1. Receive the capsule through IM, QR, or a file.
2. Import it once.
3. Compare the provider identity fingerprint using the trusted channel.
4. Let the daemons select a mutually supported HiveLink candidate.
5. Inspect the filtered catalog.

```sh
hive connect <capsule>
hive peers
hive resources list --peer <provider>
```

## Lease and use a model

```sh
hive lease request <provider>/local-coder \
  --duration 2h \
  --max-output-tokens 100000 \
  --max-concurrency 2
```

After both peers accept the final terms, mount the resource and point an existing client at the local endpoint. The normal model request and stream semantics remain intact.

## Change visibility

Providers issue a new visibility-lease revision to expand or narrow catalog scope. Hidden resources disappear from subsequent catalog queries and local projections. Historical lease content remains unchanged.

## Revoke access

Either participant may request revocation; the provider can always enforce a local kill switch. Revocation must survive daemon restarts. If peers are disconnected, the provider's local denial takes effect immediately and the signed event is delivered when connectivity returns.

## Handle interrupted streams

Do not blindly replay a request after a disconnect. Query the invocation status using its stable ID. Retry only when the resource profile and application semantics make it safe.
