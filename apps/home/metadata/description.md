# Home

The hub of the Home apps (iPhone, Apple Watch, Android, Wear OS, desktop) and the web app it serves
at its own address. It talks to the devices in the home network, Shellys first, keeps rooms, people
and their rights, runs scenes and rules, records history and energy, and rings the phones when
someone is at the door. The data stays on this server.

**Pairing.** The first owner's phone pairs with a code printed on this server:
`docker exec <container> /app/docker-entrypoint.sh pair -name <your name>`. Every other phone,
watch, computer and browser gets its code from an owner's app (People). Each person sees and
switches only what their rights allow.

## Setup

Expose the app on its domain with a certificate the phones trust; the apps and the web app connect
there (their WebSockets must pass the proxy). The domain is reachable at home and over the VPN; it
does not need to be reachable from the internet.

- **Shellys**: the hub searches the "Home network" for Shellys and lists them in the app, where
  each one is imported or ignored. It follows an imported Shelly over the Shelly's own WebSocket
  and switches it from the apps, scenes and rules; a Shelly's settings change only when an owner
  approves the change, with a backup of the Shelly before. "Shellys to import" imports addresses
  at once. With neither field, the hub shows a simulated flat. Shelly BLU sensors (motion,
  door and window, temperature, buttons) paired with a Shelly Pro come in with that Pro. Shelly
  energy meters (Pro 3EM, Pro EM) are meters; "Grid meter" names the one at the grid connection.
  Shelly colour lights (Plus RGBW PM, Pro RGBWW PM) take a colour or a white from the app.
- **Battery Shellys** (H&T): they sleep, so the hub hears them over MQTT. Enter your broker in the
  Shelly plugin's settings and turn on MQTT with RPC notifications on each device.
- **Philips Hue**: enter the bridge's address in the app (Setup › Plugins › Philips Hue) and press
  the bridge's link button; its lights come in as dimmers, with their white and colour.
- **KNX**: the KNX IP interface's address and a list of devices with their group addresses, one per
  line (Setup › Plugins › KNX).
- **Other devices**: the door station and cameras, the inverters, the weather station, the TV, a
  Jellyfin server and a Roborock robot vacuum in the home network, and the vendors' clouds (air
  conditioning, appliances, heating, a MOVA robot vacuum, the day-ahead electricity price from
  aWATTar) are set up in the app (Setup), where each one asks for its address or a sign-in.
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
