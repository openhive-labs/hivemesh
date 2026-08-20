# CLI

The `hive` CLI is an advanced control and automation interface for the local daemon.

Planned command groups:

```text
hive start | stop | status | ui
hive identity
hive resource add | list | inspect | remove
hive invite create | inspect
hive connect
hive peers
hive lease request | accept | counter | list | revoke
hive invocation status
```

The CLI must not reimplement protocol behavior or read private keys directly. It calls the local Control API, supports machine-readable output, and makes destructive operations such as identity reset, resource removal, and broad revocation explicit.

Exact names and flags remain provisional until implementation.
