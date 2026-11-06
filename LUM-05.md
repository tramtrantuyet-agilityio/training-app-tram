## LUM-05 · Why is my payout on hold?
**User story**
As a merchant owner, 
I want to see why a payout is on hold, my current chargeback rate and when the money will be released, 
so that I can plan my cash flow and act to lower my chargeback rate.
**Acceptance criteria**
- AC1 Given a merchant with a chargeback rate of 0.50% and 50,000,000 VND of sales on Monday 2026-11-02, when payouts run, then 50,000,000 VND is paid out on Tuesday 2026-11-03 (T+1) with status "Paid".
- AC2 Given a merchant with a chargeback rate of 0.70% and 50,000,000 VND of sales on 2026-11-02, when payouts run, then the payout is not sent on 2026-11-03, its status is "On hold", and the hold length is 7 days.
- AC3 Given a merchant with a chargeback rate of exactly 0.65%, when payouts run, then the payout is put on hold (threshold is inclusive: 0.65%+).
- AC4 Given a merchant with a chargeback rate of 0.64%, when payouts run, then the payout is paid at T+1 with no hold.
- AC5 Given a payout with status "On hold", when the merchant opens it in the web or mobile app, then they see the reason "Chargeback rate 0.70% is at or above the 0.65% limit", the held amount (50,000,000 VND), the hold length (7 days) and the expected release date.
- AC6 Given a payout held under AC2, when the 7-day hold ends, then the held 50,000,000 VND is paid out and the status changes from "On hold" to "Paid".
- AC7 Given a payout with status "On hold", when client-side support looks up the merchant, then support sees the same reason, rate, threshold and release date as the merchant.
**Scope**: applying the 7-day hold at a chargeback rate of 0.65% or more; T+1 payouts otherwise; hold reason, rate and release date for merchants and support. Non-scope: chargeback dispute handling; changing the 0.65% threshold or 7-day length; holds for other reasons (KYC, fraud review) until confirmed in OQ6; reserve accounts.
**Open questions** (for the client)
1. How is the chargeback rate calculated — by count or by value, and over which window (e.g. last 30 or 90 days)?
2. Do the 7 days run from the sale date, from the normal T+1 payout date, or from when the rate first crossed 0.65%? (For sales on 2026-11-02 the release date could be 2026-11-09 or 2026-11-10.)
3. When the rate drops below 0.65%, are holds already in place released early, or do they run the full 7 days?
4. Does T+1 mean the next calendar day or the next business day? What is the daily cut-off time in ICT, and how are weekends and Vietnamese/US public holidays handled?
5. Is the hold applied to all payouts of the merchant or only to sales made while the rate is 0.65%+?
6. Are there other hold reasons (KYC, fraud review, negative balance) that should be shown on the same screen?
7. Should merchants be notified (email, push) when a hold starts and ends?
**Spec artifact**: Lumino Ticket Pack — LUM-01 to LUM-05
**Analyst**: Tram Tran · AI-drafted, human-edited
