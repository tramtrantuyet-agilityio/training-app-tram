# Decision: How customers pay Lumino merchants — payment link or invoice

**Date:** 2026-11-05 · **Author:** <Tram Tran> · AI-drafted, human-edited

## Question
Which payment flow should be the default for Lumino merchants: a payment link or an invoice?

## Options compared
| | Payment link | Invoice |
|---|---|---|
| Merchant experience | Fastest: amount + share via SMS, Zalo or QR; weak reconciliation, payer often anonymous | Slower to create (name, email, line items, due date); reminders, partial-payment status, invoice number; fits installments ($500+) |
| Fee split | Card = cash price + 4% (customer pays); ACH/cash = cash price; merchant 0%. Fallback 3.5% + $0.30 paid by merchant | Same rules; easier to steer payers to ACH, so less surcharge and fallback exposure |
| Ongoing cost | No subscription assumed; higher card mix and more "unrecognized charge" disputes risk crossing 0.65% → 7-day hold | No subscription assumed; merchant time per invoice and customer-data storage; lower dispute rate expected |
| Main risk | Link forwarding/reuse, phishing look-alikes, amount tampering, card-testing bots | More PII stored (breach impact); invoice spoofing / fake "changed bank details" fraud |

### Fee split, worked (USD)
| Cash price | Card: customer pays | ACH/cash: customer pays | Merchant fee (standard) | Fallback: merchant fee | Fallback: merchant nets |
|---|---|---|---|---|---|
| $80 | $83.20 | $80.00 | $0 | $3.10 | $76.90 |
| $600 | $624.00 | $600.00 | $0 | $21.30 | $578.70 |
| $4,500 | $4,680.00 | $4,500.00 | $0 | $157.80 | $4,342.20 |

- If the $5,000 limit applies to the charged total, the largest card cash price is $4,807.69.
- Two $4,500 invoices paid the same day ($9,000) break the $8,000 daily cap.
- At $20,000/week volume, moving from T+1 to a 7-day hold locks up about $17,100 extra.

## Recommendation
Invoice as the default flow. Payment link stays as a restricted quick-pay option: single-use, fixed amount, no installments. Fees are identical, so the gain is lower chargeback risk (protects T+1 payouts) and more ACH use.

## Reasons against
- Extra friction may lower adoption among merchants who sell through social chat.
- More personal data to store and protect in Vietnam and the US.
- Invoice spoofing is a new fraud surface aimed at payers.
- The benefit rests on a lower chargeback rate that is not yet measured.
- Reminders, partial payments, installments and ACH on invoices cost more to build and support.

## Assumptions
- All amounts in USD; Vietnam merchants use the same percentages in VND.
- The 4% surcharge is kept by Lumino and covers processing; the fallback applies only where a surcharge can't be charged.
- No subscription or per-link/per-invoice fee.
- Payment links are single-use and fixed-amount.
- Chargeback and payout rules apply per merchant, not per flow.

## Unverified (check before deciding)
- Does the $5,000 limit apply to the cash price or the total including the 4%?
- When exactly does the fallback apply (debit cards, regions that ban surcharges, merchant opt-out)?
- Is the 4% surcharge refunded on full and partial refunds?
- Is a 4% surcharge compliant? Visa is believed to cap US surcharges at 3% and debit can't be surcharged — confirm with compliance.
- Chargeback fees charged to the merchant, and how much.
- Should links ever allow customer-entered amounts or installments?
