# Homelab

The server of the Homelab app: one place to see and steer Flo's build machines, their GitHub Actions runners, CI
across the app repositories, the projects and releases dev-infra lists, and the apps of this Runtipi. When something
breaks (the Mac awake but its runner down, a disk filling up, jobs waiting for a runner that never comes, an app that
stopped) it sends an alert through ntfy, with the steps to fix it and which of them need you at the machine.

The server makes every outside call itself: GitHub with its own token, this Runtipi with one session, ntfy and
Wake-on-LAN. The phones only talk to it, on the home network or over the VPN. Keep it off the internet.

## Setup

1. Create the GitHub token (permissions in the token field's hint) and pick a pairing secret.
2. Optional: install ntfy from Runtipi's store, choose a topic and subscribe to it in the ntfy app on the phone.
3. Install Homelab and fill in the fields. It runs on the host network and listens on port 8230.
4. In the Homelab app on the phone: the server's address (`<homelab's address>:8230`) and the pairing secret.
5. Log in to Runtipi from the app's Apps tab (with the TOTP code); Runtipi ends that session after 7 days.

The machine reports come from dev-infra's heartbeat (`contracts/machine-status/`): run `setup-mac.sh admin` and
`setup-pc.sh install` again after pulling dev-infra.
