---
name: claim-db-compensation
description: Claim Deutsche Bahn (DB) passenger-rights compensation (Fahrgastrechte) for a delayed or cancelled train on bahn.de, including reimbursement of extra costs such as a replacement seat reservation or ticket. Use when the user wants to file or submit a DB delay/cancellation compensation claim, get money back for a late or cancelled DB train, or claim back an extra seat reservation or ticket they had to buy because of a disruption.
---

# Claim DB Compensation (Fahrgastrechte)

Files a Deutsche Bahn passenger-rights claim through the **bahn.de** online flow using **Claude in Chrome** browser automation. Works best when the user is logged into their DB account — the account pre-fills the journey, route and times. For guests (no account) see [REFERENCE.md](REFERENCE.md).

## Entitlement at a glance

- **≥ 60 min late** at the destination → **25%** of the ticket price refunded
- **≥ 120 min late** → **50%**
- Plus refunds for **extra costs** caused by the disruption: replacement seat reservation, extra ticket, taxi/bus, hotel — the user needs the **receipts**.
- Claim within **1 year**. Covered: S-Bahn, RB, RE, IRE, IC/EC, ICE and night trains.

## Hard safety rules (never violate)

- **Never type the user's password or IBAN/bank details.** Navigate to the field, then hand off — the user types these in themselves.
- **Pause for explicit confirmation before the final "Send request now"** submit. It is irreversible.
- **Ask before downloading any file** (e.g. the receipt invoice).

## Workflow

1. **Open Claude in Chrome**, get tab context, navigate to `bahn.de` (it may redirect to `int.bahn.de/en`). Confirm the user is logged in (name shows top-right). If not, send them to log in — do **not** enter the password.
2. **Find the journey:** My trips → **Previous trips** → the trip actually taken (`Trip expired` = used; ignore `Cancelled` artifacts unless relevant). Open **Trip details**; confirm route, date and *scheduled* arrival.
3. **Open the claim:** scroll to the **Passenger rights** box → **Submit compensation request**.
4. **Fill the form** (verify each value against the screen):
   - What happened → usually **Delayed arrival at destination**
   - Delay length → **At least 60 minutes** (if true)
   - Planned connection → pre-filled, verify it
   - **Actual arrival time** → set the *real* arrival (this drives the payout — confirm it with the user)
   - Used ticket for whole journey → **Yes** if completed by rail (even on a different train); **No** only if they switched to taxi/car/bus
5. **Extra costs:** if a replacement seat/ticket was bought, answer **Yes** to additional expenses → *upload digitally* → category **Other expenses** → enter the **amount** → **Submit receipt**. Obtain the receipt via the order's **Create invoice** (it shows the price). ⚠️ Browser file-upload often fails — see the upload gotcha in [REFERENCE.md](REFERENCE.md).
6. **Personal details** → pre-filled from the account, verify.
7. **Payout:** ask the user — **Voucher** (3-year) or **Bank transfer**. For bank transfer the **user enters the IBAN**; account holder is pre-filled.
8. **Final page:** optionally tick *email me a copy*; leave the survey off unless asked. **Confirm with the user, then click Send request now.**
9. **Capture the reference number** from the confirmation page and report it back.

## More detail

See [REFERENCE.md](REFERENCE.md): the three claim routes (account / order-search / PDF / EU form), guest claims, the file-upload gotcha and workaround, field-by-field option meanings, English/German switching, and the receipt/invoice flow.
