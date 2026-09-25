# BENMP Global Giving Platform — Project Handover

**Prepared:** 25 September 2026  
**Project owner:** `jkgbafa`  
**Handover contact:** `kojogyebi@gmail.com`  
**Repository:** https://github.com/jkgbafa/benmp-giving-prototype  
**Current preview:** https://jkgbafa.github.io/benmp-giving-prototype/

## 1. What this project is

This repository contains a polished, interactive frontend prototype for the BENMP Global Giving Platform. It demonstrates:

- A cinematic BENMP homepage and campaign hero.
- About and Contact pages.
- One-time and monthly donation journeys.
- Individual and organization giving.
- Country and currency selection across 243 supported territories.
- Country-specific payment methods and mobile-money networks.
- Email-first donor recognition and a BENMP ID journey.
- A searchable church and denomination selector.
- Card, wallet, mobile money, bank debit and bank-transfer screens.
- Donation confirmations, PDF receipts and giving statements.
- Giving history, recurring-plan management and inbox experiences.
- Responsive light and dark themes.

The current code is a static browser application. It can be uploaded directly to Hostinger without a build process.

## 2. Technology

- HTML5
- CSS3
- Vanilla JavaScript
- Browser `localStorage` for prototype state
- jsPDF loaded from a CDN for PDF receipts and statements
- GitHub Pages for the current public preview
- GitHub Actions for automatic preview deployment
- Playwright scripts for end-to-end browser verification

There is no framework, package manager, compilation step, application server or database in this version.

## 3. Important project boundaries

The current site is an interactive prototype. Payment screens, donor records, sign-in links, outgoing messages and recurring collections are frontend demonstrations. Data is stored only in the visitor's browser under the `benmp-playground-v1` localStorage key.

Before production launch, the next team must connect:

- A backend and production database.
- Secure authentication or passwordless magic links.
- Paystack, Stripe, PayPal and approved banking integrations.
- Webhooks and server-side payment verification.
- Email, SMS and WhatsApp providers.
- Production receipt numbering and financial reporting.
- Privacy, data-retention and consent controls.
- Role-based access for finance and organization accounts.
- Final compliance review of countries, currencies and payment methods.

Never place payment-provider secret keys in `dist/app.js` or any browser-delivered file.

## 4. Key files

- `dist/index.html` — page shell, navigation, logo and external script loading.
- `dist/style.css` — complete responsive design, light/dark themes and component styling.
- `dist/app.js` — routes, page rendering, donation flows, account state, receipts and statements.
- `dist/assets/` — logo, favicon, hero animation and official campaign images.
- `BENMP_Global_Giving_Platform_Brief_Updated.md` — product decisions and accumulated requirements.
- `HOSTINGER_DEPLOYMENT.md` — exact Hostinger upload and rollback instructions.
- `README.md` — repository overview and local run instructions.
- `qa.mjs` — full main-flow browser test.
- `qa-states.mjs` — additional UI-state checks.
- `.github/workflows/deploy-pages.yml` — GitHub Pages preview deployment.

## 5. Local use

From the repository root:

```bash
python3 -m http.server 4173 --directory dist
```

Open `http://localhost:4173/#home`.

Because navigation uses URL hashes such as `#home`, `#about` and `#give`, a Hostinger rewrite rule is not required.

## 6. Verification

With Playwright available and the local server running:

```bash
node qa.mjs
node qa-states.mjs
```

The main suite checks navigation, responsive layouts, the hero, About content, countries and currencies, denomination search, donor matching, monthly partnership, payment routes, bank details, receipts, statements, history, messages and persistence.

Manual smoke test after deployment:

1. Open `/#home` and confirm the animated hero and all three hero actions.
2. Open `/#about` and confirm the official BENMP portrait and all six campaign photographs.
3. Open `/#give`, select Ghana, enter an amount and complete the new-donor path.
4. Confirm Mobile Money, card and bank transfer appear in that order.
5. Complete a donation and download the PDF receipt.
6. Sign in with any value and confirm the prototype welcomes **Joshua GBAFA**.
7. Switch between light and dark modes.
8. Repeat the main pages on a mobile viewport.

## 7. Current design and product decisions

- The site uses orange as the only primary accent.
- The homepage headline is **You Can Help Bring Salvation To The Multitudes**, with **Salvation** in orange.
- The hero actions are **Donate**, **Become a BENMP partner**, and **What is BENMP?**
- BENMP is defined as **Beautiful, Exciting, Nice, Mood-Changing Partner**.
- Donation amounts are capped at 999,999 in the selected currency.
- Restricted countries omitted from the country selector are Cuba, Iran, North Korea, Syria, Russia and Belarus.
- Bank transfer is always the final payment choice where present.
- Guests see Home, About, Contact us, Donate and Sign in. My giving and Messages appear only after sign-in.
- The About page uses official BENMP and Healing Jesus Campaign photography stored locally in `dist/assets/`.
- User-facing copy consistently uses **donation**, not **gift**.

## 8. Deployment ownership

### GitHub preview

Pushes to `main` deploy `dist/` automatically through GitHub Actions.

```bash
git add .
git commit -m "Describe the change"
git push origin main
```

### Hostinger production

Upload the **contents** of `dist/` to the domain's `public_html` directory. Do not upload the outer `dist` folder as a nested directory. Full instructions and rollback steps are in `HOSTINGER_DEPLOYMENT.md`.

## 9. Recommended production architecture

A practical production implementation can keep the current frontend design while moving records and payment logic to server APIs:

- Frontend: current static UI, or migrate components into React/Next.js if the team prefers.
- API: Node.js/TypeScript, Laravel/PHP or another supported backend.
- Database: PostgreSQL or MySQL.
- Authentication: passwordless email links with expiring, single-use tokens.
- Payments: provider abstraction that maps country and method to Paystack, Stripe, PayPal or bank instructions.
- Jobs: queue for receipts, reminders, email and WhatsApp notifications.
- Storage: private object storage for generated statements and receipts.
- Monitoring: payment-webhook logs, failed-job alerts and an audit trail.

The frontend must receive only public provider identifiers. Secret keys and webhook verification belong on the server.

## 10. Immediate next steps

1. Upload the supplied Hostinger deployment ZIP to a staging subdomain and complete the smoke test.
2. Back up the current `benmp.com` web root before replacing files.
3. Confirm whether the BENMP prototype will replace the full domain or live under a subdirectory.
4. Choose the production backend, database and authentication approach.
5. Open verified production accounts for the selected payment and communications providers.
6. Replace prototype state and permissive sign-in with server-backed implementations.
7. Complete payment, privacy and finance review before accepting real donations.

## 11. Contact and access

The intended handover recipient is `kojogyebi@gmail.com`. If GitHub access is used, invite the recipient to:

https://github.com/jkgbafa/benmp-giving-prototype

The Hostinger deployment and project handover ZIP files can also be transferred independently of GitHub.
