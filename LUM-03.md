## LUM-03 · Refund button is greyed out
**User story **
As a merchant owner, 
I want the refund options to be enabled or disabled according to the refund windows, with the reason shown, 
so that I know which refund I can still give and why a button is unavailable.
**Acceptance criteria**
- AC1 Given a 1,000,000 VND payment captured on 2026-11-02 10:00 ICT, when the merchant opens it on 2026-11-12 (day 10), then the Full refund button is enabled and refunds 1,000,000 VND without approval.
- AC2 Given the same payment, when the merchant opens it on 2026-11-22 (day 20), then Full refund is disabled and Partial refund is enabled.
- AC3 Given the same payment on 2026-11-22 (day 20), when the merchant submits a partial refund of 300,000 VND, then the refund status is "Pending approval" and no money moves until it is approved.
- AC4 Given the partial refund of 300,000 VND is approved, when the refund is processed, then 300,000 VND is returned to the customer and 700,000 VND remains on the payment.
- AC5 Given the same payment, when the merchant opens it on 2027-01-02 (day 61), then both Full refund and Partial refund are disabled.
- AC6 Given any refund button is disabled (AC2, AC5), when the merchant hovers or taps it, then the reason is shown, e.g. "Full refunds are available for 14 days after payment" or "Refunds are available up to 60 days after payment".
- AC7 Given the payment detail page, when it loads, then it shows the capture date and time (2026-11-02 10:00 ICT) and the last day for a full refund and for a partial refund.
**Scope**: enabling/disabling Full and Partial refund by window (14 / 60 days); approval step for partial refunds; disabled-state reasons; refund deadlines on the payment page.
**Non-scope**: who approves and the approval screen (pending OQ2); chargebacks and disputes; refunding the card surcharge (LUM-01 OQ4); bulk refunds.
**Open questions** (for the client)
1. Is the window counted from capture, authorization, or settlement time, and is day 14 / day 60 inclusive? (E.g. is 2026-11-16 10:30 ICT still a full-refund day for the example payment?)
2. Who approves partial refunds — client-side admin/support, or the merchant owner? What is the approval deadline?
3. Within the first 14 days, is a partial refund allowed, and does it also need approval?
4. Is there a maximum partial-refund amount or a limit on the number of partial refunds per payment?
5. Can the refund button also be greyed out for other reasons (user role without refund permission, payout already sent, payment under dispute, already fully refunded)? Which of these apply?
6. Which payment and date did the merchant report? This tells us whether the cause is the window rule or a bug.
**Spec artifact**: Lumino Ticket Pack — LUM-01 to LUM-05
**Analyst**: Tram Tran · AI-drafted, human-edited
