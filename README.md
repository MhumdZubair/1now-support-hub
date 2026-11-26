# 1Now Support Hub: Damage Claims, start to finish (prototype)

## The problem
A damage claim is not finished when it is routed. Photos may be missing, the renter may not reply or may dispute,
insurance may approve only part, the claim may exceed the deposit, and the card hold may expire during review.

## Core flow
1. **Evidence:** no photos means no charge. Request them, follow up after 48 hours, two reminders, then the operator can drop the claim.
2. **Route:** dispute freezes the charge; under $150 can be auto-approved; $150 or more goes to insurance review.
3. **Outcome:** record approved, partly approved or denied, with the amount the renter owes and a reason.
4. **Money:** preview and approve the capture and release, then collect any balance by link or write it off with a reason.
5. **Close:** accurate renter message, full history, downloadable claim file. Closing early requires a reason.

Lockout, payment, late return, verification and Turo sync tickets are also included as secondary flows.

## Claim toolkit
- Before and after photo files, shrunk and stored in the browser, with labelled sample images for demos
- Itemized repair estimate that can set the claim amount
- Evidence readiness checklist and plain-language guidance for each route
- Duplicate claim warning and same-vehicle damage history
- One deposit hold per booking: once one claim uses it, another cannot capture it
- Renter appeal after settlement, with decision and simulated refund
- Record of messages the operator marks as sent
- Support note and downloadable evidence pack (HTML, printable to PDF)
- Damage claims overview in the queue

## Run it
Download `index.html` and open it in any browser. No install, server or internet needed.
If you used an earlier version, this one starts with fresh sample data.

## Try it
- **Operator Queue:** tick *Needs attention first*.
- **New Request:** tap *Claim above deposit, hold expiring*, submit, send to review, record a partial approval of $500, settle, collect the balance, close.
- **Carisma AI:** on New Request type `Ali Raza scratched the bumper, $90, photos attached, he does not dispute it.`

## Demo rules and what is simulated
- The $150 limit, late fees, 48-hour follow-up, two reminders and 5-day hold are demo assumptions, not 1Now policy.
- Real hold windows vary by card and provider. Stripe documents typical online windows of about 5 to 7 days, and
  one capture for most payments, where a partial capture releases the rest:
  https://docs.stripe.com/payments/place-a-hold-on-a-payment-method
- Not connected to 1Now, Turo, Stripe or any insurer. No money moves and no messages are sent.
- Carisma AI here is offline keyword rules, not a live AI model.
- Data is stored in this browser's localStorage.

## Known limits
- Photo files stay in this browser. Nothing inspects them or verifies when they were taken. The Yes/No answer drives routing.
- Browser storage is small, so many photos can fill it.
- Appeals can only be logged after the claim's money is settled, once per claim, and can only keep or lower the amount.
- Shared-hold protection covers claims within this app only.
- Follow-ups appear as on-screen flags only. There are no notifications.
- In the secondary lockout flow the booking check always passes, and failed roadside dispatch is not handled.
- The assistant reads simple English and can misread it, so the operator must check suggestions.
- No login, roles or separate renter view.
