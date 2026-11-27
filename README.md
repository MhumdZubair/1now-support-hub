# 1Now Support Hub

A single-file, browser-only support desk for a car-rental operation (Turo-style fleet with direct bookings). Operators create support requests, the app routes each one to a decision, and the Resolver walks the operator through the next steps, the money, the rental period and vehicle options.

**Demo only.** Sample data, simulated payments, unlocks, messages and calendar sync. Nothing is sent or charged. All data stays in the browser's local storage.

---

## Quick start

1. Open `index.html` in any modern browser (Chrome, Edge, Firefox, Safari). No install, no server.
2. Use **New Request** to try an example from the "Try an example" chips, or pick a booking and describe the problem.
3. Open the ticket to work it in the **Resolver**.
4. **Fleet Board** shows all 15 vehicles at once.
5. **Reset Demo Data** restores the sample tickets and bookings.

Data is stored under the key `onenow_support_hub_v3`. Clearing site data resets the app too.

---

## Screens

### 1. New Request
- **Booking picker** lists all bookings with vehicle, customer and rental dates.
- **Duplicate warning**: if the chosen booking overlaps another booking on the same car, a warning appears before you submit.
- **Add a new customer / booking**: choose a vehicle from the 15-car fleet, set the rental start and planned end, deposit (minimum $150) and Turo sync status.
- **Problem types**: Damage Claim, Lockout Mid-Rental, Payment Failed, Late Return / Over-Mileage, Verification Failed, Turo Sync / Double-Booking.
- **When did the damage happen?** (damage claims only): During the rental, Before pickup, After the car came back, or Not sure. "Before pickup" routes the claim to a separate check.
- **Carisma AI assistant** (offline keyword rules) reads the description and suggests the booking, problem type, amounts, photo and dispute answers, and "when it happened". It fills the form only when you click the button.

### 2. Operator Queue
- Summary cards and filter pills: Open, Escalated / Disputes, Needs Evidence, Resolved and each status.
- Search across ticket ID, booking, customer, vehicle, problem, status and priority.
- **Needs attention first** sorts by urgency: expiring deposit holds, unpaid balances, overdue photo replies, overdue returns (car not back past its end time), and booking overlaps.
- Damage claims overview: open claims, amount not yet settled, holds expiring within 48 hours, collected (demo), unpaid balances and written-off amounts.

### 3. Fleet Board (new)
- All 15 vehicles in one table: status (Available, On rent, Returning soon, Booked soon, Overdue, not returned), current or next booking, free until / back by, and overlap count.
- Summary tiles: available now, with renters, booked within 24 hours, overdue and vehicles with overlaps.

### 4. Resolver
- **Carisma AI context** strip: deposit hold status, Turo sync status and the suggested next step, plus a one-click action that always asks first.
- **Progress timeline**: Created, Triaged, Action Taken, Resolved.
- **Smart triage**: the routing decision and the inputs behind it (damage amount, photos, dispute, when it happened, backup vehicle, etc.).
- **Rental period and settlement** (new, see below).
- **Vehicle options for these dates** (new, see below).
- **Claim toolkit** (damage claims): plain-language guide, evidence readiness checklist, before/after photos, itemized estimate, duplicate and damage history, renter appeal, support note, evidence pack.
- **Finish ticket**, **Request details**, **Message to Renter** (generated, copy or record as sent) and the **Activity log**.

---

## Features

### Rental period tracking (new)
Every booking has a pickup time, a planned return time and an optional actual return time. The app works out the state of each rental:

| State | Meaning | What the Resolver shows |
|---|---|---|
| Upcoming | Pickup is in the future | Pickup and planned return |
| On rent | Car is out and within the planned period | Time left until the planned return |
| Overdue | Planned return has passed and the car is still out | Time since the planned end, and a late-fee estimate |
| Returned early | Actual return is before the planned end | Unused full days, refund estimate |
| Returned on time | Actual return equals the planned end | No adjustments |
| Returned late | Actual return is after the planned end | Late hours and late-fee estimate |

A progress bar shows how much of the rental period has been used. Green means early, red means late or overdue.

**Recording a return.** While the car is still with the renter, the Rental card has a return-time field (defaults to now) and a **Record Return** button. The app refuses a return before pickup or in the future. Once recorded, the early or late calculation runs automatically.

**Calculation (demo policy, editable in the code):**
- Early return: unused full days × the vehicle's daily rate × `EARLY_REFUND_PCT` (default 50%). This is an estimate. The operator still approves any refund.
- Late return: late hours × `LATE_PER_HOUR` ($15) plus extra miles × `PER_MILE` ($0.50) from the Late Return ticket.
- Days used so far is shown for every rental, so the operator can see how many days the renter has actually had the car.

**Accident during or outside the rental.** When a damage claim is logged, the app compares the damage time with the rental window:
- During the rental (the car is still with the renter, or was returned after the damage time): logged as during the rental.
- Before pickup: a warning appears, and the claim routes to a pickup-photo check instead of a charge.
- After the car came back: a warning appears asking the operator to check the return inspection before charging.

### Vehicle options when there is a duplicate or conflict (new)
When a booking overlaps another booking on the same car (for example, a Turo trip and a direct booking for the same dates), the Resolver shows the other vehicles in the fleet, sorted by:

1. **Free for the whole rental**: the operator can assign it now. Button: *Assign*.
2. **Free from a later time**: the car is busy now but will be free before the renter needs it to end. Button: *Move pickup and assign*. The pickup and return times move by the same length of time.
3. **Busy**: shows which bookings block it.

Vehicles in the same class as the current car are marked "same class" and listed first among each group. Rates are shown so the operator can compare cost. Assigning a vehicle:
- Updates the booking and every ticket on it.
- Logs the move in the history.
- Shows a warning if an overlap still exists.
- Asks for confirmation first, and tells the operator to tell the renter about any pickup change.

When a sync ticket is marked **Backup vehicle available? Yes**, the options are presented as the available choices. If the backup is **No**, the same list is shown with a note that no backup is marked.

Options are not shown once the renter already has the car. In that case the card explains that the booking cannot be moved and the overlap must be handled another way.

### Damage claims (kept from the original, with additions)
- **Routing rules**: photos missing → request evidence; dispute → freeze charge; under $150 with photos → auto-approve; $150 or more with photos → insurance review; before pickup → pickup-photo check (new).
- **Deposit hold**: 5-day demo hold, expiry warnings, settlement split between capture from hold and balance outside the hold. A hold can only be captured once per booking.
- **Claim lifecycle**: evidence → review and outcome → money settled → close.
- **Evidence**: sample or real before/after photos (shrunk and kept in the browser), a readiness checklist, and an "I checked them" step for the operator.
- **Itemized estimate**: add repair items; using the estimate total as the claim amount re-runs triage.
- **Appeals**: log a renter appeal after settlement, record a decision and a refund.
- **Support note** and **Evidence pack** (print-to-PDF) for handing a case to the insurer or support.

### Other ticket types (kept)
- **Lockout**: check booking, remote unlock (simulated), lockbox code on failure, roadside dispatch.
- **Payment failed**: retry on card on file, or send a payment link (simulated), mark paid.
- **Late return / mileage**: charge calculated from hours and miles.
- **Verification**: re-request ID and insurance, mark verified.
- **Turo sync conflict**: reassign with backup, or escalate with the deposit kept as an authorization hold. Force a calendar re-sync (simulated).

### Carisma AI (simulated)
- Reads the description and fills the New Request form (booking, problem type, amounts, photos, disputes, when it happened, sync source, backup) only after the operator clicks.
- Suggests the next steps for each problem type.
- In the Resolver, offers one next step at a time. Any money or calendar step asks for confirmation first.
- Not a live AI. It uses fixed keyword rules.

### Message to renter
Generated from the ticket state. Updates as the ticket changes. The app never sends messages. The operator copies the text or records it as sent.

### Activity log and history
Every action is logged with a timestamp. Changes to inputs that cancel an earlier action are logged too. Return times, vehicle moves and outcomes all appear in the history and in the claim summary.

---

## Routing reference

| Problem | Decision |
|---|---|
| Damage, marked before pickup | Pickup-photo check. Close with no charge if the damage predates the rental |
| Damage, no photos | Request evidence. No charge |
| Damage, renter disputes | Freeze charge, open a dispute case |
| Damage, under $150, photos, no dispute | Auto-approve deduction |
| Damage, $150 or more, photos, no dispute | Insurance review |
| Lockout | Verify booking, try remote unlock, then lockbox or roadside |
| Payment failed, card on file | Retry; if declined, switch to a payment link |
| Late return / over-mileage | $15 per late hour + $0.50 per extra mile |
| Verification failed | Re-request ID and insurance |
| Sync, backup available | Choose an available vehicle from the options |
| Sync, no backup | Escalate; keep the deposit as an authorization hold, do not capture |

---

## Configuration (edit in the `<script>` block)

| Constant | Default | Meaning |
|---|---|---|
| `HOLD_DAYS` | 5 | Demo length of a deposit hold |
| `REPLY_HOURS` | 48 | Time the renter has to reply to a photo request |
| `MAX_REMINDERS` | 2 | Reminders before a claim can be dropped |
| `EARLY_REFUND_PCT` | 0.5 | Share of unused rent refunded on early return |
| `LATE_PER_HOUR` | 15 | Late fee per hour |
| `PER_MILE` | 0.5 | Fee per extra mile |
| `FLEET` | 15 entries | Vehicle ID, name, class, daily rate |
| `BOOKING_SEED` | 16 entries | Sample bookings with relative start, end and return times |

All policy values are demo values. Replace them with the real rental policy before use.

---

## Limits of this demo

- **Single browser.** Data lives in local storage. Two operators on two computers will not see each other's work. A real version needs a shared database.
- **No real payments, messages, calendar sync or unlocks.** Each is simulated.
- **Photos are shrunk and kept in the browser.** Large volumes can fill local storage. The app warns when saving fails.
- **Timestamps reflect when files were added**, not when photos were taken.
- **Carisma AI** uses keyword rules. It can misread a description. Check every field before submitting.
- **Not legal or financial advice.** Refund, deposit and damage decisions should follow the company's written policy and local law.

---

## Files

- `index.html`: the whole app (HTML, CSS and JavaScript in one file).
- `README.md`: this document.

## Browser support

Any current desktop or mobile browser with local storage and the canvas API. Layout adapts to phone width.
