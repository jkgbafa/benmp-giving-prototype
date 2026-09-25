# Hostinger Deployment Guide

## Ready-to-upload package

Use `BENMP_Hostinger_Deploy_2026-09-25.zip`. Its root contains:

- `index.html`
- `app.js`
- `style.css`
- `assets/`

These are the exact files that belong in Hostinger's web root.

## Before replacing benmp.com

1. In Hostinger hPanel, open **Files → File Manager**.
2. Open the domain's `public_html` folder.
3. Download a backup of the current site or compress the existing contents into a dated backup archive.
4. If available, deploy to a staging subdomain first.
5. Record any existing `.htaccess`, verification file, analytics file or email-domain file that must remain.

## Upload

1. Open the target `public_html` directory.
2. Upload `BENMP_Hostinger_Deploy_2026-09-25.zip`.
3. Extract it directly in `public_html`.
4. Confirm `public_html/index.html` exists. Do not leave the site nested under `public_html/dist/`.
5. Retain any required Hostinger or domain-verification files from the previous site.
6. Load the domain in a private browser window and complete the smoke test in `HANDOVER.md`.

## Cache

The files use relative paths and can run at the domain root. If an older version appears:

1. Clear the Hostinger/LiteSpeed cache.
2. Purge any CDN cache.
3. Hard-refresh the browser.

## Rollback

If validation fails, restore the dated backup into `public_html` and purge the same caches.

## Production warning

This package is the interactive frontend prototype. It does not contain a production backend, real authentication, live payment processing or a database. Complete the production work described in `HANDOVER.md` before accepting real donations.
