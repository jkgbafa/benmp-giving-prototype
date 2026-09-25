# BENMP Global Giving Platform

Interactive frontend prototype for BENMP and the Healing Jesus Campaign.

- Live preview: https://jkgbafa.github.io/benmp-giving-prototype/
- Product brief: [`BENMP_Global_Giving_Platform_Brief_Updated.md`](BENMP_Global_Giving_Platform_Brief_Updated.md)
- Project handover: [`HANDOVER.md`](HANDOVER.md)
- Hostinger deployment: [`HOSTINGER_DEPLOYMENT.md`](HOSTINGER_DEPLOYMENT.md)

## Run locally

```bash
python3 -m http.server 4173 --directory dist
```

Open `http://localhost:4173/#home`.

There is no build step. The deployable static site is in `dist/`.

## Verify

With Playwright installed and the local server running:

```bash
node qa.mjs
node qa-states.mjs
```

## Deploy

GitHub Pages deploys `dist/` automatically after a push to `main`.

For Hostinger, upload the **contents** of `dist/` to `public_html`. See [`HOSTINGER_DEPLOYMENT.md`](HOSTINGER_DEPLOYMENT.md) for backup, upload, cache and rollback steps.

## Production boundary

This repository is an interactive static prototype. Browser state is stored in `localStorage`. Real authentication, payment processing, messaging, databases, webhooks and compliance controls must be implemented on a secure backend before accepting live donations.
