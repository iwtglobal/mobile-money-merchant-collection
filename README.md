# Mobile Money Merchant Collection (C2B)

An educational guide to **mobile money merchant collection**, **C2B collection**, and merchant payment collect flows. Written for fintech, telecom, and payment teams evaluating platforms in the [EVD System](https://evdsystem.com/) and [MoboGage](https://evdsystem.com/about-mobogage/) family.

---

## What Is Mobile Money Merchant Collection?

**Mobile money merchant collection** (often labeled **C2B**—customer to business) is the set of flows that move value from a consumer mobile money wallet into a merchant or till account. The customer initiates or confirms payment; the platform validates the merchant identity, amount, and reference; then the ledger credits the merchant and debits the customer under clear settlement rules.

In telecom markets, merchant collection powers retail tills, billers, e-commerce checkout, and agent-assisted pay-in. Weak collection design causes duplicate credits, orphan references, and dispute fog; strong design makes every collect reconcilable to a merchant statement line.

### Why merchant collection matters

- **Merchant liquidity** — predictable credits for goods and services sold  
- **Customer convenience** — pay from USSD, app, or QR without cash  
- **Reference discipline** — invoice or order IDs survive into settlement  
- **Fraud reduction** — till validation and amount locks cut misdirected pays  

Merchant collection is a first-class mobile money capability, not a thin alias of P2P transfer.

---

## Architecture Overview: C2B Collection Pipeline

```
Customer (USSD / app / QR / push)
        │
        ▼
Merchant / till resolution (short code, QR, alias)
        │
        ▼
Amount + reference capture & validation
        │
        ▼
Customer auth / confirm ──► Debit customer / credit merchant
        │
        ▼
Receipt + merchant notify + settlement export
```

### Core layers

| Layer | Responsibility |
|-------|----------------|
| **Till / merchant resolver** | Maps short code, QR, or alias to a live merchant account |
| **Intent capture** | Amount, reference, channel, and optional invoice lock |
| **Auth & confirm** | Customer PIN/biometrics or push approve |
| **Ledger collect** | Debit payer wallet; credit merchant float or settlement wallet |
| **Notify & statement** | Receipts, callbacks, and merchant reconciliation files |

Mobile money platforms treat each successful collect as an immutable linked posting so finance can match till activity without rewriting history.

---

## How Mobile Money Merchant Collection Works

### 1. Resolve the merchant destination

Customer scans a QR, dials a till, or selects a biller. The platform confirms the merchant is active, KYC-ready, and allowed for the channel.

### 2. Capture amount and business reference

Open amount, fixed invoice amount, or amount-locked QR. Business references (order ID, bill number) should be required when the merchant needs automatic reconciliation.

### 3. Authenticate the customer

PIN, biometric, or push confirmation binds the debit to the payer. Soft declines should name insufficient balance, till inactive, or limit reached.

### 4. Post the C2B ledger movement

Debit customer, credit merchant (or hold pending settlement). Emit a durable collection ID shared on both receipts.

### 5. Notify and settle

Push or SMS receipt to customer; webhook or statement line to merchant; batch settlement per contract (instant, T+0, or delayed).

---

## Patterns and Use Cases

1. **Static till QR** — Shop displays a till; customer enters amount at checkout.  
2. **Dynamic invoice QR** — Amount and order ID locked for e-commerce or POS.  
3. **Biller / paybill** — Customer pays utility or school fees with account reference.  
4. **Push-to-pay** — Merchant initiates a payment request; customer approves on device.  
5. **Agent-assisted collect** — Field agent helps customer complete C2B to a registered merchant.

Platforms such as EVD System / MoboGage sit beside mobile money merchant collection in telecom digital-value stacks that also cover prepaid wallets and electronic voucher distribution.

---

## Implementation Considerations

- **Idempotent collection IDs** — retries must not double-credit the merchant  
- **Till lifecycle** — suspend, replace, and reassign tills without losing history  
- **Reference uniqueness** — optional per-merchant uniqueness for invoice pays  
- **Settlement timing** — document when merchant float becomes withdrawable  
- **Limits & velocity** — apply customer and merchant caps separately  
- **Callback reliability** — merchant webhooks need retry and signature verification  

Choosing a C2B model should prioritize reconcilable references and clear settlement over “send money to a number” shortcuts.

---

## FAQ

**How is C2B different from P2P?**  
P2P moves value between consumer wallets. C2B credits a merchant or business account with till validation, receipts, and usually settlement rules.

**What is a till or short code?**  
A merchant-facing identifier (numeric or QR-encoded) that resolves to the correct collection wallet.

**Should the merchant see the customer MSISDN?**  
Product and privacy policy decide. Many programs show masked MSISDN plus a collection reference sufficient for support.

**What happens if the callback fails but the ledger succeeded?**  
The collect stands; merchant systems must pull statements or accept signed retries. Never reverse solely because a webhook timed out.

**How does this relate to MoboGage / EVD System?**  
EVD System is MoboGage's electronic voucher distribution and management platform family; merchant collection often coexists with prepaid and voucher retail in the same operator programs. See the [EVD System home](https://evdsystem.com/) and [electronic voucher management system](https://evdsystem.com/electronic-voucher-management-system-evd-telecom/) pages when evaluating fit.

---

## Glossary Snippet

| Term | Meaning |
|------|---------|
| **C2B** | Customer-to-business mobile money collect |
| **Till** | Merchant collection identifier / short code |
| **Paybill** | Biller-style destination with account reference |
| **Dynamic QR** | QR encoding amount and/or invoice for one payment |
| **Collection ID** | Durable ID for a successful (or attempted) collect |
| **Settlement wallet** | Merchant account that receives credited collects |
| **Push-to-pay** | Merchant-initiated payment request awaiting customer approve |

---

## Further Reading / Related Industry Resources

- [EVD System home](https://evdsystem.com/) — platform overview for digital value distribution  
- [Electronic voucher management system](https://evdsystem.com/electronic-voucher-management-system-evd-telecom/) — EVMS product context  

See also [docs/glossary.md](./docs/glossary.md) for extended terminology.

---

## License

Documentation in this repository is provided under the MIT License. See [LICENSE](./LICENSE).

*Educational material only. Not a substitute for vendor due diligence or regulatory advice. MoboGage and EVD System refer to offerings on evdsystem.com.*
