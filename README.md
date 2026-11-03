# training-app-tram
# Lumino payment – BA/QA practice

Fictional client for the AI Tools training. Training data only – no real client data.

## Client rules for the payment page
- R1: Card price = cash price + 4%; ACH and cash pay the cash price.
- R2: Payments: $5 to $5,000 per transaction, $8,000 daily cap, CVV and ZIP must match; installments only on orders of $500+.
- R3: full refund within 14 days, partial up to 60 days with approval.
- R4: Chargebacks at 0.65%+ hold payouts 7 days.

## Folders
- app/      the payment page we test
- tickets/  ticket analyses and ticket packs
- specs/    specs written by BA
- tests/    test matrices (.md) and Playwright tests (.spec.js)
- notes/    personal notes, decision docs
