# LUM-02: Per-transaction limit applies to the cash price, not the card surcharge

| | |
|---|---|
| **Ticket** | [Issue #3](https://github.com/tramtrantuyet-agilityio/training-app-tram/issues/3) |
| **Rule** | R2 (Payment limits) |
| **Status** | Draft, pending sign-off from the client rule owner |
| **Affects** | `app/index.html` (checkout validation and messages) |

## 1. Problem

A new client tried to pay for a 12-week program plus a bonus session by card. The payment was blocked with the red message **"Maximum transaction is $5,000.00."** The card was valid.

| Item | Amount |
|---|---|
| 12-week program | $4,750.00 |
| Bonus session | $250.00 |
| **Cash price** | **$5,000.00** |
| Card surcharge (4%, R1) | $200.00 |
| **Charged total (card)** | **$5,200.00** |

### Root cause

The checkout compares the **charged total** ($5,200.00) to the $5,000 limit. R2 does not say whether the limit applies to the cash price or to the total after the surcharge. The current page assumed the charged total.

The merchant priced the bundle at exactly $5,000 to fit under the limit and did not expect the surcharge to count. The message also did not mention the surcharge or the card, so the client could not tell why the payment failed.

### Evidence

- Merchant confirmation on Issue #3 (cash price $5,000.00, card total $5,200.00, message seen).
- Reproduction on Issue #3: amount `5000`, card `4242 4242 4242 4242`, `12/30`, CVV `123`, ZIP `94103`. The summary shows "Total charged $5,200.00" and the page blocks the payment.

## 2. Rule

### 2.1 Decision

The R2 limits apply to the **cash price** (the order amount before any surcharge). The surcharge never counts toward them.

### 2.2 Updated R2

| Limit | Applies to | Value |
|---|---|---|
| Minimum per transaction | Cash price | $5.00 (inclusive) |
| Maximum per transaction | Cash price | $5,000.00 (inclusive) |
| Daily cap | Sum of cash prices paid that day | $8,000.00 (inclusive) |
| Installments | Cash price of the order | Available at $500.00 or more |

### 2.3 Interaction with other rules

- **R1 (pricing) is unchanged.** Card payments are charged cash price + 4%, rounded to the cent. ACH and cash are charged the cash price.
- **The card is charged more than the limit.** A card payment can be up to $5,200.00 (a $5,000.00 cash price + $200.00 surcharge). This is intended.
- **Installments.** The limit applies to each installment, since each payment is a separate transaction. An order's total may exceed $5,000 if every installment is within $5.00 to $5,000.00.
- **Daily cap** uses the same basis as the per-transaction limit (cash price). It is not counted on the charged total.

### 2.4 Message change

The page should state which amount was checked, so the client can see why a payment was blocked.

| Condition | Message |
|---|---|
| Cash price above $5,000.00 | "The order amount of {cash price} is above the $5,000.00 per-transaction limit." |
| Cash price below $5.00 | "The order amount of {cash price} is below the $5.00 minimum." |
| Installment above $5,000.00 | "Each installment of {installment} is above the $5,000.00 per-transaction limit." |
| Daily cap exceeded | "This order would bring today's total to {new total}, above the $8,000.00 daily limit." |

When paying by card, the order summary keeps showing the cash price, the 4% surcharge, and the total charged.

## 3. Acceptance criteria

Amounts are the cash price entered in the Order amount field. Use test card `4242 4242 4242 4242`, expiry `12/30`, CVV `123`, ZIP `94103` for card cases. "Daily total" starts at $0.00.

| ID | Given | When | Then |
|---|---|---|---|
| AC-1 | Method = Card, amount $5,000.00 | Pay now | Approved. Summary shows cash $5,000.00, surcharge $200.00, total charged $5,200.00. (The original failing case.) |
| AC-2 | Method = ACH or Cash, amount $5,000.00 | Pay now | Approved. Charged $5,000.00. |
| AC-3 | Method = Card, amount $5,000.01 | Pay now | Blocked with the maximum message. |
| AC-4 | Method = ACH, amount $5,000.01 | Pay now | Blocked with the maximum message. |
| AC-5 | Method = Card, amount $4,999.99 | Pay now | Approved. Charged $5,199.99 (surcharge $200.00 after rounding to the cent: 4% of 4,999.99 = 199.9996). |
| AC-6 | Any method, amount $5.00 | Pay now | Approved. (Card total $5.20.) |
| AC-7 | Any method, amount $4.99 | Pay now | Blocked with the minimum message. |
| AC-8 | Method = Card, amount $5,000.00 | Pay now (twice, in one day) | First approved. Second approved only if the daily total stays within $8,000.00. Otherwise blocked with the daily-cap message. |
| AC-9 | Daily total is $3,000.00 (cash price basis) | Pay $5,000.00 by card | Approved. Daily total becomes $8,000.00. |
| AC-10 | Daily total is $3,000.01 | Pay $5,000.00 by any method | Blocked with the daily-cap message. |
| AC-11 | Daily total is $3,000.00 | Pay $5,000.00 by card | The cap counts $5,000.00, not $5,200.00, so the daily total becomes $8,000.00. |
| AC-12 | Method = Card, amount $5,000.00, installments = 3 months | Pay now | Approved. Total charged $5,200.00 in 3 payments (about $1,733.33 each, last payment takes the remaining cents). Each installment is within $5.00 to $5,000.00. |
| AC-13 | Amount $499.99 | Open installments | Installments are not available. |
| AC-14 | Amount $500.00 | Open installments | Installments are available. |
| AC-16 | Method = Card, amount $6,000.00, installments = 3 months | Pay now | Approved. Total charged $6,240.00 in 3 payments of $2,080.00. |
| AC-17 | Method = Card, amount $6,000.00, installments off | Pay now | Blocked with the maximum message. |
| AC-18 | Method = Card, amount $16,000.00, installments = 3 months | Pay now | Blocked: each installment ($5,333.34) is above $5,000.00. |
| AC-15 | Any blocked payment | View the message | The message names the checked amount (cash price) and the limit, and does not read as a card decline. |

## 4. Out of scope

- **Refunds (R3) and chargebacks (R4)** are unchanged.
- **Splitting one order into several payments** to get under the limit. The product does not support a combined receipt across payments.
- **Workaround for the current client** (ACH, separate program and bonus charges, installments). Handled on Issue #3, not in this spec.
- **Installment edge cases**, such as how the daily cap applies to a later installment charged on another day. Needs a follow-up ticket.
- **Price or surcharge changes** (the 4% rate in R1).
- **Whether the surcharge may be shown or collected separately** from the order amount, which is a policy question for the client.

## 5. Open questions for the client

1. Confirm the limit applies to the cash price (this spec's decision).
2. Confirm the minimum ($5.00) and daily cap ($8,000.00) use the same basis.
3. Should the installment limit be checked on each payment's cash share or its charged amount (with surcharge)? This spec assumes cash share.
