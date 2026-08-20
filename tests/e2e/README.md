# End-to-End Tests

Two-peer tests exercise public behavior through daemon APIs and real HiveLink sessions rather than calling packages directly.

Minimum vertical scenarios:

1. create identities and mutually authenticate;
2. consume a one-time invitation and reject replay;
3. register multiple resources and return a peer-filtered catalog;
4. negotiate and persist visibility and execution leases;
5. project a remote model into the consumer's local `/v1/models`;
6. complete streaming model invocation and verify its receipt;
7. enforce request, token, rate, concurrency, and expiry limits;
8. supersede visibility and remove hidden catalog entries;
9. revoke execution and reject subsequent requests;
10. interrupt transport, reconnect, and recover invocation status;
11. restart both daemons without losing identity, quota, or revocation state;
12. reject wrong-peer lease use, conflicting revisions, and invocation replay.

The suite should run over an in-process test transport and at least one real TLS adapter. Transport substitution must not change protocol outcomes.
