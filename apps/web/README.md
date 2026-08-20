# WebUI

The WebUI is a local static application served by `hivemeshd` and embedded or opened by Desktop. It is not a separate network service and is not the system core.

Primary views:

- daemon health and peer identity;
- registered local resources and exposure policy;
- known peers and HiveLink status;
- invitations and filtered remote catalogs;
- visibility and execution lease negotiation;
- active usage, receipts, expiry, and revocation;
- safe connection diagnostics.

The WebUI consumes the local Control API. Closing it does not stop the daemon, disconnect peers, or revoke leases.
