## LUM-04 · Payment plan option missing at checkout
**User story **
As a paying customer, 
I want to see the payment plan option whenever my order qualifies, and a clear reason when it does not, 
so that I can spread the cost of large orders without contacting the merchant.
**Acceptance criteria**
- AC1 Given an order of 15,000,000 VND (cash price), when the customer reaches checkout, then "Pay in installments" is shown as a payment option.
- AC2 Given an order of exactly 12,500,000 VND ($500, cash price), when the customer reaches checkout, then "Pay in installments" is shown (threshold is inclusive).
- AC3 Given an order of 10,000,000 VND (cash price), when the customer reaches checkout, then "Pay in installments" is not selectable and the text "Payment plans are available for orders from 12,500,000 VND" is shown.
- AC4 Given an order of 10,000,000 VND at checkout, when the customer adds an item of 3,000,000 VND (new total 13,000,000 VND), then "Pay in installments" becomes available without reloading the page.
- AC5 Given an order of 13,000,000 VND with "Pay in installments" selected, when the customer removes an item of 3,000,000 VND (new total 10,000,000 VND), then the installment selection is cleared and the AC3 message is shown.
- AC6 Given an order of 15,000,000 VND, when checkout loads on the mobile app and on the web, then "Pay in installments" is shown on both.
**Scope**: showing/hiding the installment option at the 12,500,000 VND ($500) threshold; live update when the cart total changes; explanation when the option is unavailable; parity between mobile and web. Non-scope: installment schedule, number of installments, interest or fees; credit checks; merchant opt-in settings (pending OQ4); collecting failed installments.
**Open questions** (for the client)
1. Is the 12,500,000 VND threshold checked against the cash price or the card price? (E.g. cash 12,100,000 VND is 12,584,000 VND by card — should it qualify?)
2. Which payment methods can be used for installments — card only, or ACH too?
3. How do installments interact with the 125,000,000 VND per-transaction limit and the 200,000,000 VND daily cap? Is the limit checked against the order total or each installment? (Relevant to the 12-week program in LUM-02.)
4. Can a merchant switch installments off, and is the option missing because of a merchant setting?
5. Is the threshold before or after discounts, shipping and tax?
6. What is the native VND threshold, if not exactly 12,500,000 VND?
7. What order total did the reporting customer have?
**Spec artifact**: Lumino Ticket Pack — LUM-01 to LUM-05
**Analyst**: Tram Tran · AI-drafted, human-edited
