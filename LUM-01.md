## LUM-01 · Customer charged more than the listed price
**User story**
As a paying customer, 
I want to see the card price and the cash/ACH price before I confirm payment, 
so that the amount I am charged matches the price I agreed to.

**Acceptance criteria**
- AC1 Given an item listed at a cash price of 1,000,000 VND, when the customer selects card at checkout, then the checkout shows a card price of 1,040,000 VND (1,000,000 + 4% = 40,000) before the Pay button.
- AC2 Given the same 1,000,000 VND item, when the customer pays by ACH or cash, then the charged amount is exactly 1,000,000 VND with no surcharge line.
- AC3 Given a card payment of a 1,000,000 VND item is completed, when the customer opens the receipt, then it shows "Cash price 1,000,000 VND", "Card surcharge (4%) 40,000 VND" and "Total charged 1,040,000 VND".
- AC4 Given a card payment of a 1,000,000 VND item is completed, when the merchant views the transaction, then the merchant receives 1,000,000 VND before payout and is charged 0 VND in processing fees.
- AC5 Given the checkout shows a card total of 1,040,000 VND, when the processor returns any other amount for authorization, then the payment is not captured and the customer sees "Amount changed — please review" with the new total.
- AC6 Given the merchant's catalogue lists 2,500,000 VND as the cash price, when the product page loads, then it shows both "2,500,000 VND cash/ACH" and "2,600,000 VND by card".

**Scope**: card surcharge calculation (+4%); price display on product page, checkout and receipt; merchant-side fee of 0%; blocking capture when the authorized amount differs from the displayed total. 
**Non-scope**: the fallback fee (3.5% + 7,500 VND) logic until its trigger is confirmed; changing the 4% rate; currency conversion; tax/VAT display; refunds of surcharges (see LUM-03).

**Open questions** (for the client)
1. When does the fallback "flat 3.5% + $0.30" apply instead of the +4% card price, and who pays it — the customer or the merchant? (E.g. 1,000,000 VND would give 42,500 VND fallback vs 40,000 VND surcharge.)
2. Is $0.30 fixed at 7,500 VND, or should a native VND amount be set?
3. How should the 4% be rounded when the result is not a whole VND (e.g. 1,234,567 × 4% = 49,382.68)? Up, down, or nearest?
4. Is the surcharge refunded when the payment is refunded (fully or partially)?
5. Are there legal limits on card surcharging for Vietnamese or US merchants that the display must reflect?
6. Which complaint triggered this ticket — a missing price display, a wrong calculation, or the fallback fee being applied?
 
**Analyst**: Tram Tran · AI-drafted, human-edited
