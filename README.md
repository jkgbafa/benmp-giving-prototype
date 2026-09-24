# BENMP interactive prototype

Browser-local demonstration. No real payments, sign-in emails or external messages are sent. Use sample information.

Features: hero with Give and Become a BENMP partner; 243 available countries/territories; Cuba, Iran, North Korea, Syria, Russia and Belarus are omitted and illustrative local payment routes; custom amounts/currencies; individual and organization gifts; monthly plans; saved/email-only/first-time details; permissive Joshua GBAFA demo sign-in; received/pending/failed payment states; downloadable acknowledgments and CSV; edit/pause/resume/cancel monthly plans; simulated installments and reminders; country totals separated by currency; message previews, replies and notification preferences; light, dark and extra-simple modes.

Run locally: `python3 -m http.server 4173 --directory dist`. No build step. Sites identity is stored in .openai/hosting.json.

Browser checks passed: all-country selection, Kenya/Nigeria routing, custom amounts, all three identity paths, organization monthly gifts, pause/resume/edit/run/cancel plans, reminder mode, payment failures/retries and pending confirmation, receipts/CSV downloads, messages/replies, persistence after reload, reset, mobile layout and appearance modes. No runtime errors were observed. Optional native WebMCP validation was unavailable in the installed Chrome; helpers are feature-detected and ordinary interaction does not depend on them.

Production boundary: all records stay in this browser’s localStorage. Permissive sign-in is a demo feature, not authentication. Country methods are illustrative, not certified live availability. Production needs provider integrations, verified accounts, server records, organization permissions, mandate handling, and country/currency eligibility enforcement. No production services are connected.

See BENMP_Global_Giving_Platform_Brief_Updated.md for design decisions and implementation requirements.
