## LUM-02 · Payment for our 12-week program was declined
**User story**
As a paying customer buying a high-value program, 
I want to know exactly why my payment was declined and what I can do next, 
so that I can complete the purchase without contacting support.
**Acceptance criteria**
- AC1 Given an ACH payment of 125,000,000 VND ($5,000), when the customer confirms payment, then the payment is accepted (upper limit is inclusive).
- AC2 Given an ACH payment of 130,000,000 VND ($5,200), when the customer confirms payment, then it is declined before submission with "This amount is above the 125,000,000 VND per-payment limit".
- AC3 Given an ACH payment of 100,000 VND ($4), when the customer confirms payment, then it is declined with "The minimum payment is 125,000 VND".
- AC4 Given the same customer, on the same card, at the same merchant, has had 150,000,000 VND accepted between 09:00 and 10:00 ICT, when they submit at 10:05 ICT a further payment of 60,000,000 VND, then it is declined with "This payment would exceed the 200,000,000 VND daily limit" and shows 50,000,000 VND remaining today.
- AC5 Given a card payment of 50,000,000 VND where the CVV does not match, when the customer submits, then it is declined with "Security code (CVV) does not match" and the customer can re-enter the CVV without re-entering other card details.
- AC6 Given a card payment of 50,000,000 VND where the ZIP/postal code does not match, when the customer submits, then it is declined with "Billing ZIP/postal code does not match".
- AC7 Given any payment declined under AC2–AC6, when the merchant opens the transaction in the dashboard, then the decline reason code (limit_per_txn, limit_min, limit_daily, cvv_mismatch, zip_mismatch) and attempted amount are shown.
- AC8 Given a decline under AC2 for an order of 130,000,000 VND, when the decline message appears, then a link to the payment plan option is shown, because the order is above 12,500,000 VND (see LUM-04).
**Scope**: limit checks ($5 / $5,000 per transaction, $8,000 daily); CVV and ZIP match checks; customer-facing decline messages; decline reason codes for merchant and support.
**Non-scope**: raising limits for specific merchants; issuer/bank-side declines (insufficient funds, fraud) beyond passing through the issuer reason; installment mechanics (LUM-04).
**Open questions** (for the client)
1. What is the price of the 12-week program and which payment method was used? This decides which rule caused the decline.
2. Does the per-transaction limit apply to the card price including the 4% surcharge? (E.g. cash price 121,000,000 VND becomes 125,840,000 VND by card.)
3. Is the 200,000,000 VND daily cap per customer, per card, or per merchant? Does it reset at 00:00 ICT or on a rolling 24 hours?
4. Are the VND limits exact conversions (125,000 / 125,000,000 / 200,000,000 VND) or rounded native VND values?
5. Do declined attempts count toward the daily cap?
6. How is the ZIP check applied to Vietnamese cards and addresses, which may not carry a postal code?
7. After how many CVV/ZIP failures should the card be blocked for a cooling-off period?

**Spec artifact**: Lumino Ticket Pack — LUM-01 to LUM-05 
**Analyst**: Tram Tran · AI-drafted, human-edited
