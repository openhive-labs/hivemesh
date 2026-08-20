# HiveLink

Implements HiveLink session authentication, version and capability negotiation, standard channels, heartbeat, connection status, reconnection, and safe resumption over replaceable transport adapters.

Internal adapter boundary:

```text
listen()
dial(candidate)
openStream(channel)
acceptStream()
connectionInfo()
close()
```

Initial work should support one generic authenticated TLS path that can run over LAN, Tailscale, or an existing tunnel. Future QUIC, WebRTC, WebSocket, and relay adapters plug in below the same session.

This package reports connectivity; it never decides resource visibility or invocation permission.
