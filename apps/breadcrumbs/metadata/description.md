# Breadcrumbs

Server for the Breadcrumbs app: a private location timeline (where was I, when, for how long) and a
KlimaTicket ride counter. The phone records stays and moves with little battery and syncs them here;
nothing goes to Google or any other vendor. This server can read the data (encrypted in transit only),
so treat it as the most sensitive store on the homelab.

There is no web interface yet. Expose the app on a domain with a certificate the phone trusts, reached
at home and over the VPN only; never forward it from the internet.

## First start

The pairing code is in the app's log (runtipi's log view) at every start while no phone is paired:

```
Breadcrumbs pairing code: K7M2 QX9P
```

In the app: More › Server & sync, the server's `https://` address and the code. With the "Public address" set,
the log also shows a QR code: "Scan the server's QR code" in the app fills in both. For another phone, or
after the code expired (24 h):

```sh
docker exec <container> /app/docker-entrypoint.sh pair-code
```

The server runs as uid 10001; its entrypoint hands the data folder to that user on start.

## Backups

Set the backup passphrase above first. Then a nightly job on the host (14 daily and 12 monthly copies)
is in the repository's `deploy/backup.sh` and `deploy/README.md`; one backup by hand:

```sh
docker exec <container> /app/docker-entrypoint.sh backup /data/backups/breadcrumbs-$(date +%F).tar.gz
```

Restore (server stopped): `restore <file> --force`; with the passphrase set, an unencrypted file needs
`--allow-plain`. Wipe everything: `wipe` (asks you to type WIPE).

## Image

`ghcr.io/geymayer-apps/breadcrumbs-server:dev` is the newest dev build until the first release; the
homelab's `docker login ghcr.io` token must be able to read the `geymayer-apps` organisation's packages.
