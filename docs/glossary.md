# Glossary — Mobile Money Merchant Collection

Educational terminology for **mobile money merchant collection**, C2B collection, and merchant payment collect. Definitions are industry-oriented; vendor labels vary.

## Core Terms

| Term | Definition |
|------|------------|
| **C2B collection** | Customer-to-business payment into a merchant or till account |
| **Till / short code** | Public identifier that resolves to a merchant collection wallet |
| **Paybill / biller** | Destination that typically requires an account or invoice reference |
| **Static QR** | QR encoding merchant identity; amount entered by the customer |
| **Dynamic QR** | QR encoding merchant, amount, and often an order reference |
| **Collection ID** | Durable platform ID for a collect attempt or success |
| **Business reference** | Merchant-supplied invoice, order, or bill number |
| **Push-to-pay** | Merchant-initiated request the customer must approve |
| **Settlement timing** | When credited value becomes withdrawable for the merchant |
| **Merchant statement** | Export of collects for finance reconciliation |
| **Till suspension** | Temporary block of new collects without deleting history |
| **Callback / webhook** | Signed notify to merchant systems after ledger success |

## Flow States (Typical)

```
intent → till-resolved → amount/ref validated → customer-confirmed → posted → notified / settled
                                              └→ declined (balance, limit, till inactive)
```

## Roles

| Role | Concern |
|------|---------|
| **Customer / payer** | Authenticates and funds the collect |
| **Merchant** | Receives credit; reconciles references |
| **Platform operator** | Runs till registry, ledger, and settlement |
| **Acquirer / aggregator** | May onboard merchants and run settlement cycles |
| **Support** | Matches receipts to till activity on disputes |

## Design Notes

- Treat **collection IDs** as permanent for retries and support lookups.  
- Prefer **amount-locked** dynamic QR for e-commerce to reduce mis-keys.  
- Document whether failed webhooks auto-reverse (they generally should not).  
- Keep till reassignment auditable so historical collects stay attributable.

## Related Reading

Live product reading: [evdsystem.com](https://evdsystem.com/), [electronic voucher management system](https://evdsystem.com/electronic-voucher-management-system/).

MoboGage / EVD System materials on evdsystem.com describe adjacent digital value distribution capabilities that often sit beside mobile money merchant collection in telecom markets.
