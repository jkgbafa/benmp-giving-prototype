# BENMP Global Giving Platform
## Product, Payments, Data & Dashboard Brief

**Product:** A responsive BENMP giving web app. The first screen is the giving form.

**Document purpose:** Consolidate the full BENMP global giving discussion into one implementation-ready brief covering partner identity, payment routing, payment providers, fees, worldwide coverage, data capture, large gifts, real-time dashboards, website flow, and the core presentation story.

**Working date:** 23 September 2026

> **Important:** Payment fees, country availability, limits, and charity pricing can change and can also vary by entity, country, currency, risk profile, and negotiated volume. Public rates below are a planning baseline; BENMP should obtain written commercial quotes before launch.

---

# 1. Executive Summary

BENMP should not be built as “a Stripe website,” “a Paystack website,” or “a PayPal website.”

It should be built as a **BENMP-owned global giving platform** with:

1. A **single worldwide partner identity system**
2. A unique **BENMP ID** for every partner
3. A **payment router** that selects the best payment rail by country, currency, method, and gift size
4. Multiple processors underneath one BENMP experience
5. Automatic confirmation through APIs/webhooks/bank reconciliation
6. A **single giving history per partner**
7. A **single worldwide dashboard**
8. Real-time or near-real-time partner and giving counters
9. Country-level breakdowns
10. A high-value bank-transfer/wire lane for very large gifts
11. Crypto support without forcing BENMP to self-custody crypto

The user experience should feel like **one BENMP system**, even though the payment may travel through Paystack, Stripe, PayPal, BitPay, bank transfer, ACH, Zelle, Cash App, M-PESA, MoMo, Pix, Alipay, WeChat Pay, or another local rail.

---

# 2. Core Product Promise

## Open BENMP. Start giving.

A partner should be able to:

- Join BENMP from anywhere in the world
- Receive a unique BENMP ID
- Give in a familiar local way
- See their giving history
- Receive confirmations/receipts
- Maintain one partner identity even when using different payment methods
- Give from different countries without creating duplicate records

BENMP leadership should be able to see:

- Worldwide partner count
- Active partners
- New signups
- Partner drop-off/inactive counts
- Number of givers today / this month
- Giving totals by country
- Giving totals by payment method
- Giving totals by currency
- Giving totals by entity/bank destination
- Gift status: pending / received / failed / refunded / disputed
- Individual partner history
- Country-by-country participation

---

# 3. Recommended Core Payment Stack

## 3.1 Paystack — African local rails

**Role:** Primary African local-payment layer where supported.

Why it belongs:
- Familiar in several African markets
- Supports country-specific local rails
- Ghana: Mobile Money + cards
- Kenya: M-PESA and other supported methods
- Nigeria: cards, transfers, USSD and wallet/bank options
- Can attach metadata such as BENMP ID / Gift ID
- Webhooks can confirm transaction success automatically

### Ghana public pricing
- Local transactions: **1.95%**
- International transactions: **1.95%**
- International cards are generally settled in GHS by default
- Volume discounts available
- No integration or maintenance fee on public pricing

Source: https://paystack.com/gh/pricing

### Nigeria public pricing
- Local: **1.5% + NGN 100**
- NGN 100 waived under NGN 2,500
- Local fee capped at NGN 2,000
- International: **3.9% + NGN 100**

Source: https://paystack.com/pricing

---

## 3.2 Stripe — Global cards + local digital methods

**Role:** Main global card/digital-payment infrastructure, especially for UK/global checkout.

Why it belongs:
- Cardholder support in 195+ countries
- 135+ currencies
- 100+ payment methods
- One integration
- Supports major local/digital methods including selected Alipay, WeChat Pay, Pix, iDEAL/Wero, UPI, bank methods and others
- Strong API/webhook ecosystem
- Excellent metadata support for BENMP ID and Gift ID
- Supports Cash App Pay in eligible US setups
- Supports bank-based rails that can be cheaper than cards
- Offers custom/volume pricing

### UK public card pricing
- Standard UK card: **1.5% + 20p**
- EEA card: **2.5% + 20p**
- Other international card: **3.15% + 20p**
- Currency conversion when required: **+2%**

Source: https://stripe.com/gb/pricing

### Selected Stripe UK local-method examples
- Alipay: **2.9% + 20p**
- WeChat Pay: **2.9% + 20p**
- Pix: **2%**
- Bacs Direct Debit: **1%**, minimum 20p, **£4 cap**
- ACH Direct Debit: **0.8%**, **£5 cap** on the published UK pricing page
- USD bank transfer: **0.5%**, **£5 cap** on the published UK pricing page
- Pay by Bank: **0.5% + 20p**, **£5 cap**

Additional international / FX fees may apply depending on the transaction.

Source: https://stripe.com/gb/pricing/local-payment-methods

---

## 3.3 PayPal — Familiar global wallet

**Role:** Optional but important familiar payment choice.

Why it belongs:
- Very high consumer recognition
- Some donors will prefer PayPal even when another rail is cheaper
- Can return payer identity/contact information depending on checkout configuration
- Webhooks/API can feed payment confirmation into BENMP
- Charity pricing may be available subject to approval

### UK charity public pricing
Subject to PayPal application and pre-approval:
- Domestic charity transaction: **1.4% + fixed fee**
- GBP fixed charity fee: **£0.20**
- International surcharge:
  - EEA sender: **+1.29%**
  - Other markets: **+1.99%**

Source: https://www.paypal.com/uk/business/paypal-business-fees

### Strategic note
PayPal should be kept mainly for **familiarity and donor preference**, not because it is always the lowest-cost rail.

---

## 3.4 BitPay — Bitcoin and crypto

**Role:** Dedicated crypto acceptance layer.

Why it belongs:
- Enables actual cryptocurrency payment flows
- Can handle Bitcoin and other supported assets
- Can reduce BENMP’s need to self-custody private keys
- Supports invoicing/payment confirmation workflows
- Can feed a BENMP Gift ID back into the platform
- Can settle according to the merchant’s available configuration

### Current public BitPay pricing
- Under $500,000 monthly crypto volume: **2% + $0.25**
- $500,000–$999,999: **1.5% + $0.25**
- $1,000,000+ monthly volume: **1% + $0.25**

Source: https://www.bitpay.com/pricing

---

# 4. Bank Transfer / Wire Is a Core Rail, Not an Afterthought

For large gifts, BENMP should not force a donor through a card processor.

Example:

```text
Partner P72882
Gift intent: $5,000,000
Gift ID: BEN-59283

BENMP displays:
- Bank name
- Account name
- Routing / sort code
- SWIFT
- Account number / IBAN
- Required reference: BEN-59283

Donor wires funds.

Bank feed / reconciliation:
BEN-59283
Expected: $5,000,000
Received: $5,000,000
Status: RECEIVED
```

The platform records the gift in exactly the same partner history as a $20 card gift.

### Why this matters
- Percentage card fees are inappropriate for very large gifts
- Large transfers often require bank/wire rails anyway
- BENMP still gets identity, reporting and reconciliation
- Leadership still sees everything in one dashboard

---

# 5. Gift-Size Routing Logic

The platform should route based on **cost + donor convenience + country + gift size**.

Example policy:

| Gift type | Preferred route |
|---|---|
| Small everyday gift | Local wallet / card / familiar method |
| Ghana | MoMo / Paystack |
| Kenya | M-PESA / Paystack |
| Nigeria | Paystack local methods |
| UK recurring | Bacs / bank debit first |
| US recurring | ACH first |
| Brazil | Pix / card |
| China | Supported Alipay / WeChat Pay / UnionPay / card routes |
| PayPal-preferring donor | PayPal |
| Crypto donor | BitPay |
| Very large gift | Bank transfer / wire |
| $1M+ | High-value wire workflow |

The checkout should **recommend** the lower-cost method without removing familiar alternatives.

Example:

```text
Give £1,000

Recommended
○ Bank account

Other ways to give
○ Debit / credit card
○ PayPal
○ Apple Pay
```

---

# 6. Partner Identity Model

The visible BENMP ID is the partner’s email address. Store a separate immutable internal partner key for payment relationships and email changes.

Example:

```text
BENMP ID: joshua@example.com
Name: Joshua Mensah
Country: Ghana
Phone: +233...
Email: ...
Preferred currency: GHS
Joined: 2026-09-23
Status: Active
```

The internal partner key ties payment routes together; display the verified email as BENMP ID. Merely entering an email does not authenticate a donor.

---

# 7. Intended Gift vs. Received Gift

BENMP should separate **gift intent** from **money actually received**.

Example:

```text
Internal partner key: P0253
Gift ID: BEN-847293
Expected: GHS 1,000
Received: GHS 0
Status: PENDING
```

After payment:

```text
Internal partner key: P0253
Gift ID: BEN-847293
Expected: GHS 1,000
Received: GHS 1,000
Status: RECEIVED
```

If a donor says they will give GHS 1,000 but sends GHS 900:

```text
Expected: GHS 1,000
Received: GHS 900
Difference: GHS 100
Status: PARTIALLY RECEIVED / REVIEW
```

This prevents pledged money from being counted as received cash.

---

# 8. Payment-First Web App Flow

**Revised 23 September 2026.** BENMP is a responsive web app for phone and laptop. Open directly to the giving form; remove the introductory welcome/signup screen and the previous “Every way to give” tagline. The three mobile screens are:

## Screen 1 — Start Giving

Show Individual / Organization, country, currency, One time / Monthly, gift amount and Continue. Provide a discreet passwordless sign-in link for returning partners. The laptop opens to the same giving form, with eligible local methods summarized alongside it. No account-creation wall precedes choosing the gift.

## Screen 2 — Your Details

Provide three paths: **Already signed in** uses saved details and displays **BENMP ID: joshua@example.com**; **Use my BENMP ID** accepts the email and continues without sign-in; **First-time giver** collects name, email and phone/WhatsApp, without a separate ID field. Reuse the selected country. Email-only giving must not reveal an existing profile, access history or automatically merge records based on unverified input. Offer optional passwordless verification to claim gifts and manage the account.

For organizations, collect the organization’s details and representative for first-time giving. Use its designated giving email as its visible BENMP ID, with a separate internal organization key. Signing in as an authorized representative is required to access saved organization details or manage commitments. Guest giving can record a stated organization without granting account authority. First-time payment can proceed without creating an authenticated account; verification is required before account access.

## Screen 3 — Pay

Show eligible methods for the selected country and frequency. Ghana example: Mobile Money with MTN MoMo, Telecel Cash or AT Money; card; bank transfer. After selecting a network, show its phone input and confirmation instructions. Recurring gifts show only eligible recurring methods; ordinary Mobile Money is not presented as automatic monthly debit.

Create a Gift ID and link it to the BENMP ID. Confirm Received only after the processor or reconciled bank receipt confirms payment; asynchronous payments remain Pending meanwhile. Confirmation is a resulting state, not a fourth introductory screen in the revised three-screen graphic.

---

# 9. Contact Data & Automation

BENMP should **collect the partner record before payment** instead of hoping to reconstruct identity afterwards.

Recommended fields:
- BENMP ID
- Full name
- Email
- Phone / WhatsApp
- Country
- Preferred language
- Preferred currency
- Communication consent
- Join date
- Partner status

Payment processors then return:
- Payment ID
- Gift ID / metadata
- Amount
- Currency
- Status
- Method
- Processor
- Timestamp
- Available payer/contact fields

BENMP links the transaction back to the existing BENMP ID.

### Important principle
Payment contact data is not automatically the same as permission for unlimited marketing. Communication consent should be stored separately.

---

# 10. Real-Time Worldwide Dashboard

The dashboard must be readable from across a room.

## Primary display

```text
BENMP WORLDWIDE

       728,492

    TOTAL PARTNERS

+1,284 THIS MONTH
+93 TODAY
```

The **worldwide total should be the largest element on screen**.

---

## Country breakdown

```text
PARTNERS BY COUNTRY

Ghana                 182,410
United States          96,820
United Kingdom         74,105
Nigeria                63,220
Kenya                  42,310
South Africa           30,110
Brazil                 19,804
China                  14,920
Other countries       204,793
```

Counts above are illustrative mockup data only.

---

## Giving dashboard

```text
GIVING TODAY

Givers                         21,714
Successful gifts               21,481
Pending                            188
Failed                              45

BY METHOD

MoMo                              ...
M-PESA                            ...
Bank / ACH                        ...
Card                              ...
PayPal                            ...
Crypto                            ...
Wire                              ...
```

---

## Live-feed option

```text
LIVE

P0253 • Ghana • MoMo • Received
P9921 • Kenya • M-PESA • Received
P1722 • UK • Bank Debit • Received
P8871 • USA • ACH • Received
P6112 • Brazil • Pix • Received
```

Sensitive information should be masked on public-facing displays.

---

# 11. Dashboard Filters

Leadership/admin views should support:

- Worldwide
- Continent
- Country
- Currency
- Payment method
- Processor
- Date range
- New partners
- Active partners
- Inactive partners
- Givers / non-givers
- Recurring partners
- One-time partners
- Pending gifts
- Received gifts
- Failed payments
- Refunds
- Disputes
- High-value gifts

---

# 12. Broader Real-Time Counter System

The same dashboard engine can be used beyond BENMP partners.

Examples:
- Worldwide BENMP partners
- Missions
- Pastors / ministry workers
- Churches
- Books published
- Campaigns
- Buildings / projects
- Other ministry departments

The concept is one **central metrics platform** with different data sources feeding live counters.

---

# 13. Global Coverage Strategy

BENMP should describe the platform as **near-global**, not literally guaranteed for every individual in every country.

The combined stack provides:
- Broad global card reach through Stripe
- PayPal coverage in many markets
- African local methods via Paystack
- Crypto via BitPay
- Bank transfer/wire for high-value or special cases

Some jurisdictions and payment scenarios remain restricted by:
- Sanctions
- Local laws
- Card-network restrictions
- Processor eligibility rules
- Issuing-bank restrictions
- Wallet availability
- Merchant-entity location
- Currency controls

The platform should maintain a **country/payment-method rules table** so routing can be updated without redesigning the website.

---

# 14. China

Do not describe China as “every person everywhere can always pay.”

A more accurate BENMP statement:

> BENMP can provide broad China coverage through supported Chinese and international payment methods, including Stripe-supported Alipay, WeChat Pay and international card routes where eligible; availability still depends on the payer, wallet, transaction and merchant setup.

China should be treated as a dedicated routing configuration rather than assuming one universal card method.

---

# 15. Brazil

Brazil should prioritize:
- Pix where supported
- Local/international cards
- PayPal as an alternative

Stripe’s UK pricing page currently lists Pix at **2% per successful charge**, with additional international/FX costs potentially applying.

Source: https://stripe.com/gb/pricing/local-payment-methods

---

# 16. America

Recommended US options:
- ACH / bank
- Card
- Cash App Pay
- PayPal
- Zelle
- Text-to-give
- Crypto
- Wire for large gifts

### Zelle
Treat primarily as a bank-to-bank payment rail, with BENMP creating the Gift ID first and then reconciling the bank transaction.

### Cash App
Where supported through the processor setup, it can feed back into the same BENMP payment record.

### Text-to-give
Text-to-give should be an **entry point**, not a separate donor database.

Example:

```text
Text GIVE
↓
BENMP identifies / creates partner
↓
Enter amount
↓
Choose payment method
↓
Payment
↓
Same BENMP ID
```

---

# 17. Large-Gift Limits

The platform itself should **not** impose a meaningful upper accounting limit.

A multimillion-dollar gift can be represented in BENMP.

The payment rail may have its own transaction limits, so large gifts should switch automatically to bank/wire.

Core principle:

> A $20 card gift and a $5 million wire should end up in the same BENMP partner record.

---

# 18. Why BENMP Should Accept Processing Fees

A processing fee is not just payment for a button.

It can include:
- Tokenization
- PCI security
- Fraud screening
- 3D Secure
- Authorization
- Card-network access
- Acquiring
- Settlement
- Currency handling
- Refund handling
- Chargeback handling
- Recurring-payment tokens
- Payment retries
- Webhooks
- Compliance infrastructure
- 24/7 payment systems

The target should therefore not be “zero fees everywhere.”

The target should be:

> **Use the cheapest appropriate rail for each transaction while preserving donor convenience.**

---

# 19. Least-Cost Routing

The BENMP router can compare eligible methods and recommend the lowest practical cost.

Example:

```text
UK £1,000 gift

Card:
1.5% + 20p
Estimated processor cost: £15.20

Bacs Direct Debit:
1%
£4 published cap
Estimated processor cost: £4

BENMP recommendation:
BANK ACCOUNT
```

This is one of the strongest financial reasons to own the BENMP orchestration layer instead of hard-wiring the entire platform to one payment company.

---

# 20. Example Worldwide Stress Test

At the same moment:

```text
Accra
→ MTN MoMo
→ Paystack

Nairobi
→ M-PESA
→ Paystack

Lagos
→ Bank transfer / USSD / card
→ Paystack

London
→ Bacs / card
→ Stripe or selected global processor

New York
→ ACH / Cash App / card / PayPal / Zelle

São Paulo
→ Pix / card

China
→ Eligible Alipay / WeChat Pay / card route

Crypto donor
→ BitPay

Major donor
→ $5M bank wire
```

All return:

```text
BENMP ID
Gift ID
Country
Amount
Currency
Method
Processor
Status
Timestamp
```

And all update:

```text
ONE BENMP PARTNER RECORD
+
ONE WORLDWIDE DASHBOARD
```

---

# 21. Architecture

```text
                         BENMP.COM
                            │
                    PARTNER IDENTITY
                            │
                       GIFT INTENT
                            │
                     PAYMENT ROUTER
                            │
      ┌─────────────┬───────┼────────┬───────────┐
      │             │       │        │           │
   PAYSTACK       STRIPE  PAYPAL   BITPAY    BANK/WIRE
      │             │       │        │           │
 Africa local     Global  Familiar  Crypto     High-value
   methods        cards    wallet               gifts
      │             │       │        │           │
      └─────────────┴───────┴────────┴───────────┘
                            │
                    PAYMENT EVENTS
                            │
                 RECONCILIATION ENGINE
                            │
                     BENMP DATABASE
                            │
       ┌────────────────────┼─────────────────────┐
       │                    │                     │
 PARTNER HISTORY      LIVE DASHBOARD       ADMIN REPORTING
```

---

# 22. Core Data Objects

## Partner

```yaml
partner_id:
name:
email:
phone:
country:
preferred_currency:
preferred_language:
joined_at:
status:
communication_consent:
```

## Gift

```yaml
gift_id:
partner_id:
expected_amount:
expected_currency:
received_amount:
received_currency:
status:
payment_method:
processor:
processor_transaction_id:
country:
created_at:
received_at:
```

## Payment Event

```yaml
event_id:
gift_id:
processor:
event_type:
amount:
currency:
status:
raw_reference:
received_at:
```

## Country Routing Rule

```yaml
country:
preferred_methods:
fallback_methods:
preferred_processor:
supported_currencies:
bank_destination:
high_value_threshold:
restrictions:
```

---

# 23. Admin Controls

Admins need to be able to:
- Merge duplicate partners
- Correct transaction matches
- Reassign an unmatched payment
- Mark manual bank/wire confirmations
- Handle partial payments
- Record refunds
- Record disputes
- Export reports
- Manage country-routing rules
- View processor health
- View unmatched transactions
- View pending gift intents
- Suspend or reactivate a partner
- Manage privacy / communication consent

---

# 24. Security & Compliance Principles

BENMP should:
- Never store raw CVV
- Avoid storing full card numbers
- Use processor-hosted/tokenized card components
- Apply role-based admin access
- Use MFA for admins
- Keep audit logs
- Encrypt sensitive partner data
- Separate marketing consent from payment identity
- Apply data-retention rules
- Maintain backup/recovery procedures
- Use sanctioned/restricted-country controls
- Reconcile bank and processor totals regularly

---

# 25. Presentation Story

Do **not** present:

> “We chose Stripe, PayPal, Paystack and BitPay.”

Present:

> **“We built one worldwide BENMP giving infrastructure.”**

The providers are components underneath BENMP.

The pitch:

> **Open BENMP. Start giving.**

> BENMP gives every partner one identity and one giving history while intelligently routing each gift through the most appropriate local or global payment method. From MoMo and M-PESA to cards, PayPal, Bitcoin and multimillion-dollar bank wires, every successful gift returns to one worldwide BENMP dashboard.

---

# 26. Recommended 16:9 Overview Slide

## Header

**BENMP GLOBAL GIVING PLATFORM**

**ONE PARTNER. ONE PLATFORM. EVERY WAY TO GIVE.**

## Center

Four primary brand/payment blocks:

**Paystack**
- Africa local rails
- Ghana: 1.95%
- MoMo / M-PESA / bank / cards depending on market

**Stripe**
- Global cards + local methods
- UK card: 1.5% + 20p
- International: 3.15% + 20p
- FX where required: +2%

**PayPal**
- Familiar global wallet
- UK approved charity: 1.4% + £0.20 domestic
- International surcharge applies

**BitPay**
- Bitcoin & crypto
- 2% + $0.25 below $500k monthly
- 1.5% + $0.25 at $500k–$999,999
- 1% + $0.25 at $1M+ monthly

## Bottom

**ONE BENMP PARTNER RECORD + ONE WORLDWIDE DASHBOARD**

**Bank Transfer / Wire for Major Gifts**

---

# 27. Dashboard Wireframes to Produce

The visual mockup set should include:

1. **Executive Worldwide Counter**
   - Huge partner total
   - New today
   - New this month
   - Active partners
   - Number of countries

2. **Country Breakdown**
   - Country
   - Partner count
   - Givers
   - Giving amount
   - Percentage of worldwide total

3. **Giving Operations Dashboard**
   - Received
   - Pending
   - Failed
   - Refunded
   - Disputed
   - Unmatched bank transfers

4. **Partner Profile**
   - Partner identity
   - Contact info
   - Country
   - Partner status
   - Lifetime giving
   - Recurring commitment
   - Payment history

5. **Live Giving Wall**
   - Masked BENMP IDs
   - Country
   - Method
   - Received status
   - Live counter animation

6. **Website Flow**
   - Join / Sign in
   - Enter amount
   - Country-aware payment-method selection
   - Processor handoff
   - Success screen
   - Giving history

---

# 28. Launch Standard

BENMP should launch as a **multi-rail system from the beginning**, not as a single-country pilot.

At launch, the architecture should already support:
- Multiple processors
- Multiple bank accounts/entities
- Country routing
- Multiple currencies
- BENMP ID metadata
- Webhooks
- Bank reconciliation
- Crypto routing
- High-value wire flow
- Real-time dashboard
- Country breakdown
- Processor-independent transaction history

New providers can then be added later without changing the BENMP identity or reporting model.

---

# 29. Commercial Actions Before Final Contracting

Before final processor selection:

1. Obtain written nonprofit/charity pricing from Stripe
2. Obtain high-volume pricing from Paystack
3. Confirm PayPal charity approval/rates for the relevant BENMP entity
4. Confirm BitPay nonprofit/merchant eligibility and settlement structure
5. Compare Checkout.com charity terms
6. Compare Adyen interchange-plus proposal
7. Obtain direct-bank/ACH/Bacs pricing
8. Confirm each BENMP legal entity and payout bank account
9. Confirm country/currency support in writing
10. Negotiate based on projected annual giving volume

The platform should be processor-independent so BENMP can switch or add providers without rebuilding the partner system.

---

# 30. Final Recommendation

**Build BENMP as the identity, orchestration, reconciliation and reporting layer.**

Keep **Paystack, Stripe, PayPal and BitPay** as major donor-facing payment options because together they combine local African behavior, global card reach, familiar wallet usage and crypto acceptance.

But do **not** make those four the only rails. Use **bank debit, ACH, direct transfer and wire** wherever they reduce cost or better suit the size of the gift.

The strongest version of the concept is:

> **A worldwide partner can join once, receive one BENMP ID, give through the method that makes sense in their country, and have every gift automatically appear in one BENMP record and one live worldwide dashboard.**

---

# Source Links

- Paystack Ghana pricing: https://paystack.com/gh/pricing
- Paystack Nigeria pricing: https://paystack.com/pricing
- Stripe UK pricing: https://stripe.com/gb/pricing
- Stripe UK local payment methods: https://stripe.com/gb/pricing/local-payment-methods
- PayPal UK business/charity fees: https://www.paypal.com/uk/business/paypal-business-fees
- BitPay pricing: https://www.bitpay.com/pricing


---

# 31. Monthly Recurring Giving — Individuals and Organizations

**Added 23 September 2026 at the user's request.** This section describes the proposed product and implementation plan; recurring collection is not yet configured. Where it differs from earlier method examples, use the eligibility qualifications below.

## 31.1 Two partner types, one giving platform

At registration and checkout, offer **“Give as an individual”** and **“Give as an organization.”** Both can choose **One time** or **Monthly**. Each recurring commitment has a Recurring Plan ID; every monthly payment has its own Gift ID linked to the appropriate individual or organization BENMP ID.

An organization is a separate partner record. The person operating its account is a named representative linked to that organization. Do not assign the organization's gifts to the representative's personal giving history or count the same gift twice.

## 31.2 Individual setup

1. Sign in or join BENMP; confirm name, email, country and preferred currency.
2. Select **Monthly**, enter the gift amount, and choose the first collection date. Default to the sign-up day; offer days 1–28 where the provider supports a selectable schedule.
3. Show only methods eligible for automatic recurring donations under the actual BENMP merchant configuration.
4. Display a review screen: **“Give [currency] [amount] each month, starting [date], until you cancel.”** State whether the first payment is taken now, identify the receiving BENMP entity, and show any donor-facing charge before authorization.
5. Collect payment details through the provider's secure form and obtain the required recurring authorization or bank mandate. Store its reference and consent timestamp, not raw card data or CVV. Keep ministry-update consent separate.
6. Complete any required bank verification or card authentication. Activate collection only after the required authorization is valid.
7. Send confirmation showing amount, frequency, first/next collection date and a **Manage monthly giving** link. A scheduled plan is not evidence that money has been received.

## 31.3 Organization setup

Collect the organization’s legal name, country, organization type, billing address, finance email and any registration/tax identifier needed for the chosen entity or receipt. Link a named representative and their role. Offer church, ministry, company, foundation and other organization types.

The representative selects the amount, currency, first date and payment method, then confirms authority to commit the organization's funds. Corporate bank mandates or provider verification may require additional signatories; if so, keep the plan **Awaiting approval** until those requirements are completed.

Support organization roles: **Owner/Admin**, **Finance Manager**, **Approver** and **Read-only**. For organizations that need it, allow one person to draft the commitment and a second authorized person to approve it. BENMP's internal approval does not replace bank-required signatures or payment authorization.

Issue payment acknowledgments in the organization's legal name and route finance notices to its designated contacts. Allow reference/department fields and exports of the organization's giving history. Tax-receipt wording depends on the receiving entity and applicable eligibility; do not promise tax deductibility globally.

Example: **Grace Outreach Church → Organization ID O0108 → USD 2,000 monthly → authorized finance representative → eligible bank debit → one new Gift ID each month.**

## 31.4 Recurring method plan

| Donor context | Proposed automatic recurring route | Required qualification |
|---|---|---|
| United States | ACH debit or card | Eligible BENMP entity, USD setup, bank verification and recurring authorization |
| United Kingdom | Bacs debit or card | UK Bacs merchant eligibility and valid mandate |
| Eligible European bank accounts | SEPA debit or card | EUR and appropriate bank/account/merchant eligibility |
| Canada / Australia | Card; local bank debit where eligible | Confirm merchant country, currency and method support |
| Paystack markets | Supported recurring card authorization; supported Nigeria direct debit where enabled | Verify channel, currency and BENMP merchant eligibility |
| PayPal-preferring donor | PayPal subscription where approved | Separate subscription approval and eligible account configuration |
| Mobile Money / M-PESA without reusable automatic authorization | Monthly reminder with a fresh payment approval | Label **Monthly reminder**, not automatic recurring debit |
| Organization paying by standing order | Donor creates a monthly bank standing order | BENMP reconciles each actual transfer using a stable plan reference |
| Crypto or manually initiated wire | Reminder or scheduled gift intent | Do not claim automatic debit without a specifically supported authorized mechanism |

Provider support must be checked for donations as well as recurring payments. A method being available for a one-time gift does not establish support for monthly collection.

## 31.5 Collection, receipts and failed payments

Create one scheduled installment per month. Keep the plan status separate from the payment status: a plan may be active while its current payment remains pending. Count received funds only after the payment is confirmed. Track subsequent returns, refunds and disputes separately.

Use signed provider webhooks and duplicate-event protection. Give each installment a stable key so retries or repeated webhooks cannot create duplicate charges, gifts or receipts. Reconcile provider records with BENMP regularly, including when webhook delivery is delayed.

Send scheme-required advance notices for bank debits and a confirmation after each successful collection. For a failure, send a clear update-payment link and follow the method's permitted retry policy. A proposed card policy is up to three attempts over seven days where allowed; bank-debit retries require their own rules. Stop retries after cancellation or revoked authorization. Never automatically collect missed months as a lump sum without fresh donor agreement.

## 31.6 Managing a monthly gift

Provide **Change amount**, **Update payment method**, **Pause**, **Resume** and **Cancel**. Clearly display when a change takes effect and whether any payment is already processing. Default amount changes to the next unprocessed installment; do not prorate gifts or charge an extra amount immediately unless explicitly chosen and authorized.

Pause and cancellation must stop future collection at the provider, not just hide a BENMP status. Show that an already-submitted bank payment may still settle. Resume with a clearly stated next date; do not back-charge paused months. Confirm all changes to the donor or authorized organization contacts and retain an audit trail. Limit organization changes to permitted roles and required approvals.

## 31.7 Data and dashboard additions

Store partner type; partner/organization ID; representative and role; Recurring Plan ID; provider subscription/mandate reference; currency and amount; start and next collection dates; consent record; plan status; cancellation/pause dates; and per-installment Gift IDs, payment states and attempts.

Report individual and organization monthly plans separately, with active plans, expected monthly commitments by currency, confirmed receipts, pending installments, failed collections, paused plans and cancellations. Expected commitments are not received income. Organization and individual partner counts should have explicit separate definitions.

## 31.8 Implementation sequence and acceptance checks

First configure eligible receiving entities and recurring methods. Then implement the individual flow, organization roles/approvals, mandates, provider scheduling, event processing, reconciliation and management screens. Enable monthly reminder/standing-order paths separately where automatic debit is unavailable.

Before launch, test a successful first and later installment, pending bank payment, failed payment and recovery, duplicate webhook, expired card, amount change, organization approval, pause/resume, revoked mandate, cancellation before collection, return/refund and receipt ownership. Confirm that no scenario causes duplicate collection or treats a pledge as received funds.

### Supporting documentation

- [Stripe subscription lifecycle](https://docs.stripe.com/billing/subscriptions/overview)
- [Stripe method support: country, currency and subscriptions](https://docs.stripe.com/payments/payment-methods/payment-method-support)
- [Paystack subscriptions](https://paystack.com/docs/payments/subscriptions/)
- [Paystack payment channels, including Mobile Money behavior](https://paystack.com/docs/payments/payment-channels/)
- [PayPal Subscriptions](https://developer.paypal.com/docs/subscriptions/)

### Related routing corrections from the graphic review

The earlier brief's China and Brazil examples need qualification: [Alipay's Stripe terms](https://stripe.com/legal/alipay) prohibit donations; do not offer it for BENMP giving. Current [Stripe method support](https://docs.stripe.com/payments/payment-methods/payment-method-support) lists Pix for eligible US businesses and invite-only Brazilian businesses, so a UK price listing alone does not prove UK account eligibility. [BitPay's country page](https://support.bitpay.com/hc/en-us/articles/360000123366-What-countries-or-jurisdictions-does-BitPay-support) states that new UK, EEA and Australian merchant onboarding is paused. Treat those earlier examples as conditional planning ideas, not confirmed launch capabilities.

---

# 32. Returning Partners — Passwordless Sign-In and Giving Consistency

**Recommended experience: collect details once, then use email sign-in links and an optional trusted-device session.** Partners should not need to remember a BENMP password or complete the registration form for every gift.

## First visit

Collect the partner's full name, email, phone/WhatsApp, country and preferred currency, plus separate optional communication consent. Verify ownership of the email before treating it as an account login. Create a stable internal partner key and save their details; use their email as the visible BENMP ID. Do not ask for the same information again unless it is missing or the partner chooses to update it.

## Returning visit on a trusted device

With a valid authenticated session, show **“Welcome back, Joshua”**, then **Give again**, **Manage monthly giving**, and **Giving history**. Pre-fill the saved country and currency, and optionally suggest the last gift amount. Let the partner change the amount or payment method, review and authorize the new gift. Never charge merely because they opened a link or pressed “Give again.”

Offer **“Keep me signed in on this device”** on personal devices. A proposed ordinary-partner session policy is 30 days, with server-side revocation and earlier expiry after suspicious activity. Show **“Not you? Sign out”** and an account option to sign out other devices. Do not expose partner history from an email typed into a form or from a device-recognition cookie alone.

## Returning visit on another device

1. Choose **Sign in** and enter the email address.
2. Select **Send me a sign-in link**. Use a neutral response that does not reveal whether an account exists.
3. Open the single-use email link and confirm sign-in. A proposed validity period is 15 minutes. A code from the same email can provide an alternative when switching devices or when the link is inconvenient.
4. Restore the existing partner profile and continue to the intended giving page without re-entering contact details.

Provide **Resend link**, **Use another email**, and a controlled account-recovery path. A changed or inaccessible email must not create an automatic duplicate account or allow an unverified profile merge. Email-only sign-in depends on access to the mailbox; for partners without usable email, assess verified phone-code sign-in separately, including delivery coverage and recovery.

## Organization representatives

Each representative signs in with their own verified identity, then chooses **Give personally** or **Give for [organization]** if authorized. Keep one organization BENMP ID across representatives and staff changes. Invitations, role removal and organization access require an auditable owner/admin process. Require stronger authentication or fresh verification for organization administration, changing account access and sensitive financial actions. A shared finance email may receive receipts; it should not be the only identity controlling the organization.

## Keeping track of consistent giving

Link every gift—card, bank, PayPal, local method or reconciled transfer—to the same individual or organization BENMP ID. Store both the provider's customer/payment references and BENMP's Gift IDs. Payment-provider emails may differ from the partner login email; match through established IDs, not an unverified email coincidence.

Define reporting explicitly:

- **Repeat giver:** confirmed gifts in at least two distinct reporting months.
- **Consistent monthly giver:** a confirmed gift in each of the last three completed reporting months; make the window configurable.
- **Monthly plan participant:** an authorized recurring plan, reported separately from successfully paid installments.
- **Needs follow-up:** a missed or failed expected installment, pending settlement or a paused plan—each shown distinctly.

These are proposed BENMP reporting definitions, not provider classifications. Count settled/confirmed gifts and adjust for returns or reversals. Show scheduled commitments separately. Respect country/time-zone reporting boundaries and avoid double-counting organization gifts as a representative's personal gifts.

## Implementation requirements

Use a maintained authentication service or library. Rate-limit sign-in requests and code attempts; store only hashed one-use tokens; use secure, HttpOnly session cookies and allowlisted return URLs. Do not place login tokens in analytics or logs. Handle email scanners without consuming the sign-in token on a passive link preview. Re-authenticate for contact changes, role changes and other sensitive actions. Keep account authentication separate from payment mandates and card/bank authorization.

Acceptance checks: first signup, remembered-device return, new-device sign-in, used/expired link, email code fallback, scanner preview, sign-out/revocation, account recovery, authorized organization switching, and a recurring gift arriving while the partner is signed out. All should retain the correct BENMP ID and giving history without duplicate records.


# 33. Revised Country Graphic and Visual Direction

The revised overview is one graphic with country flags and payment-provider marks. Prioritize Ghana, Nigeria, Kenya, South Africa and Côte d’Ivoire with named local methods; give US, UK, Canada, Australia, Brazil and Singapore compact examples; show secondary markets in a lower list. PayPal, BitPay and bank wire remain conditional alternatives. Merchant entity, currency, donation eligibility and enablement must be confirmed before methods appear in a live checkout. The two revised graphics supersede the earlier welcome-screen and country-overview designs. See the accompanying SOURCES.md for supported methods and qualifications.


# 34. Latest Web App Design Decisions — 23 September 2026

These decisions supersede earlier conflicting mockups and identifier examples. Earlier P-prefixed references represent internal keys, not the visible BENMP ID. Never display donor email addresses on public giving feeds; use anonymous or consented display names.

## Wording and three design branches

Use **Beautiful. Exciting. Nice. Move.** as the short line, **Partner** as the first-screen heading and **Give** on the final payment button, with amount and currency clear (example: **Give GHS 1,000**). This follows the latest dictated wording as interpreted; no introductory landing screen. Preserve individual/organization and one-time/monthly choices.

Deliver three separate laptop-and-mobile boards: standard, extra simple with larger controls and one question at a time, and dark mode. Each shows the three-step journey: amount, details, payment. Use the supplied Healing Jesus bird icon beside BENMP.

## Identification without repeated entry

1. Signed in: saved details are already available; show the email BENMP ID and allow editing.
2. Returning without signing in: enter BENMP ID (email) only and continue with the gift. Do not return saved personal data or disclose account existence.
3. First-time: enter name, email and phone. Email becomes the visible BENMP ID; do not ask for another ID.

Store a guest email as a claimed, unverified attribution. Confirm payment independently via provider verification. Reconcile gifts into verified giving history only with verified ownership or another established, trusted association; typing someone’s email is not proof of identity. Never update an existing profile or recurring mandate from guest email input alone. An optional single-use sign-in link or email code opens the account; no password is required. Keep individual and organization internal keys separate, including when contact emails overlap. Support verified email changes without losing history.

## Payment choices

Use clearly selectable radio-style accordion rows: Mobile Money, debit/credit card, bank transfer, and any other eligible methods for the selected country. Expand only the selected method. In the Ghana example, show MTN MoMo, Telecel Cash and AT Money as network choices and a phone-number field. Production availability is determined by the enabled merchant account, currency and method. Monthly giving only offers methods that support the relevant recurring authorization; otherwise offer explicit monthly reminders rather than claiming automatic collection.

## Provider branding and privacy

The published Paystack Ghana terms reviewed do not establish a blanket requirement to display its logo on BENMP-owned form pages. The marks clause requires written consent to use Paystack marks. Stripe’s official asset guidance permits checkout-logo use. These findings support omitting decorative provider logos from these custom mockups, subject to the actual account agreement and integration requirements. Preserve provider-controlled checkout/SDK branding and applicable payment-method button rules. Keep processor identification and data-sharing disclosures in BENMP’s privacy notice; a privacy disclosure and a logo are separate matters.

Sources reviewed 23 September 2026: [Paystack Ghana terms, including privacy policy and Merchant Services Agreement](https://paystack.com/gh/terms); [Stripe official checkout-logo guidance](https://stripe.com/newsroom/information); [Stripe marks terms](https://stripe.com/legal/marks). No account-specific contract was available for review.

## Deliverables and validation

See the accompanying three PNG boards, HTML layouts and DESIGN-NOTES.md. These are design concepts, not a connected donation service. The HTML demonstrates selector changes only; next-step, authentication and payment buttons are illustrative. Monthly individual and organization setup remains specified in section 31. Screen layouts were rendered and checked for clipping, and identity/payment selector switching was checked.
