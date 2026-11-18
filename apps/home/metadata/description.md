# Home

The hub of the Home apps (iPhone, Apple Watch, Android, Wear OS, desktop) and the web app it serves
at its own address. It talks to the devices in the home network, Shellys first, keeps rooms, people
and their rights, runs scenes and rules, records history and energy, and rings the phones when
someone is at the door. The data stays on this server.

**Early version (0.1).** The hub reads its devices and shows them to the apps; it does not switch
anything yet, and there is no pairing: one access token, printed on this server with
`docker exec <container> /app/docker-entrypoint.sh token`, opens it.

## Setup

Expose the app on its domain with a certificate the phones trust; the apps and the web app connect
there (their WebSockets must pass the proxy). The domain is reachable at home and over the VPN; it
does not need to be reachable from the internet.

- **Shellys**: the hub searches the "Home network" for Shellys and lists them in the app, where
  each one is imported or ignored. It connects to an imported Shelly over the Shelly's own
  WebSocket and only reads it; nothing changes on the Shelly. "Shellys to import" imports
  addresses at once. With neither field, the hub shows a simulated flat.
  Whatever a Shelly can do on its own (timers, schedules, wall switches) keeps working while this
  server is down.
- **Live video and talk**: the hub plays the door station's video to the phones itself, over
  port `8557` (TCP and UDP). "Address of this server for the phones" is the address the phones
  reach it on, in the home network or over the VPN.
- **Pushes**: for iPhones put the `.p8` key from developer.apple.com as `apns-key.p8` into
  `<runtipi>/app-data/geymayer-apps/home/data/` and enter its key id and the team id in this app's
  settings. Android uses UnifiedPush through your ntfy server (no Google services): enter the
  token of an ntfy user that may write to `up*`; each Android device needs the ntfy app, signed in
  with its own ntfy user. Restart the app.

## Backups

The hub writes a consistent, encrypted snapshot of its databases into its data folder every night,
so runtipi's app backup and any copy of the app-data folder hold a usable backup.

Design and source: [geymayer-apps/home](https://github.com/geymayer-apps/home) (private).
