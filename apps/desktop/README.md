# Desktop

Desktop is the normal user entry point. It owns installation UX, first-run setup, tray or menu-bar presence, daemon lifecycle integration, notifications, and opening the embedded WebUI.

Desktop does not implement identity, leases, networking, catalogs, or metering itself. It talks to the same local Control API as the CLI and WebUI.

Expected behavior:

- ensure `hivemeshd` is installed and running;
- display identity and daemon health;
- open the local WebUI;
- surface connection, lease, expiry, revocation, and security notifications;
- let the user choose whether closing the window leaves the daemon running;
- provide an explicit stop-service action.

Window lifecycle must never implicitly destroy leases or corrupt in-flight accounting.
