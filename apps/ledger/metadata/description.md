# Ledger

Invoicing for a one-person business in Austria: the monthly invoice in a minute, the quarter's
invoices for the Steuerberater in one ZIP, and which invoice has been paid. Issued invoices get a
gap-free number and an immutable PDF; corrections are credit notes. Ledger is not a bookkeeping or
filing tool.

The server holds the business's address, bank account and clients: expose the app on a domain
reached at home and over the VPN only, never from the internet.

## First start

Open the app's domain in a browser. The setup code is in the app's log (runtipi's log view) at
every start until the first device has connected:

```
Setup code: K7M2QX9P (pair the first device with it; also in /data/setup-code.txt)
```

Enter it, then fill in the business (name, address, UID, IBAN). More devices (the desktop app on
the Mac or the PC, another browser) pair with a code from Einstellungen → Geräte.

## Backups

Nightly, from the host's cron (copy the archives off-site):

```sh
docker exec <container> /app/docker-entrypoint.sh backup /data/backups/ledger-$(date +%F).tar.gz
```

Safe while the server runs. Restore: stop the app, empty its data folder, untar the archive into
it, start. Issued invoices, payments and exports are never deleted (7 years, BAO); download the
year's archive (Export → Jahr) every January and keep it off-site too.

## Image

`ghcr.io/geymayer-apps/ledger-server`; the homelab's `docker login ghcr.io` token must be able to
read the `geymayer-apps` organisation's packages. The server runs as uid 10001; its entrypoint
hands the data folder to that user on start.
