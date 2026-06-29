# DB Compensation — Reference

Detailed reference for [SKILL.md](SKILL.md). Everything here was validated against the live `int.bahn.de/en` flow (logged-in private account).

## The four ways to claim

DB offers several routes; the skill uses the first one because it pre-fills everything.

1. **DB account (Kundenkonto)** — *preferred.* Ticket is in the account → My trips → Previous trips → Trip details → **Submit compensation request**. Journey, route and times are pre-filled.
2. **Order search (Auftragssuche)** — for digital tickets *not* in an account. Needs the **Auftragsnummer (order number)** + the **traveller's surname**. Pull up the order, then the same "Submit compensation request" button.
3. **Fahrgastrechte PDF form** — download, fill (ideally in Adobe Acrobat), attach receipts, and post or hand in at a DB Reisezentrum / on board.
4. **EU passenger-rights form** — for journeys under EU rules / other railways.

Entry point for all of them: `bahn.de` → Info & Services → **Fahrgastrechte**.

## Entitlement rules

| Delay at destination | Refund of ticket price |
| --- | --- |
| < 60 min | none (under DB Fahrgastrechte) |
| ≥ 60 min | **25%** |
| ≥ 120 min | **50%** |

- Percentage is of the **price paid for the affected portion** of the ticket. DB assesses the exact figure after submission.
- **Extra costs** from the disruption are reimbursed separately with receipts: replacement seat reservation, additional ticket, taxi/bus where no train ran, and an overnight hotel where required.
- A **separately purchased seat reservation that went unused** can also be reimbursed.
- Deadline: **within 1 year** of the end of the ticket's validity.
- Covered services: S-Bahn, RB, RE, IRE, IC/EC, ICE, night trains. **Not** the underground/tram/bus legs — those go to the local operator.

## Online form — step by step

After **Submit compensation request**:

1. **What happened?** — `Delayed arrival at destination` / `I did not start the trip` / `Discontinued train journey and returned home`.
2. **How long was the delay?** — `Under 60 minutes` / `At least 60 minutes`. Picking "at least 60" shows the entitlement summary ("Compensation for the delay and unused seat reservation; Refunds for additional costs") → **OK, continue**.
3. **What connection did you plan to use?** — pre-filled from the booked ticket (from/to, planned dep/arr date+time). Verify.
4. **When did you actually arrive?** — date is pre-filled, **time defaults to the planned time → change it to the real arrival**. This single field determines which tier applies, so confirm it with the user.
5. **Did you use your ticket for the entire journey?**
   - **Yes** — "I travelled the entire journey with my ticket (**also applies if a different train was used than planned**)." Use this when the user stayed on rail.
   - **No** — only if they "switched to another mode of transport (taxi, car, bus)."
6. **Did you incur additional expenses?** — `Yes` / `No`. Valid extras include "costs for another means of transport, an additional train ticket, a hotel stay, or a separately purchased and unused reservation." Receipts required; the ticket itself is not.
7. **How do you want to submit the invoices?** — `upload digitally` (recommended, faster) / `by post`.
8. **Submit receipts online** — up to 5 receipts (or one combined PDF). Each receipt row needs:
   - the **file**,
   - a **Document category**: `Other modes of transport` (taxi/bus), `Accommodation` (hotel), or **`Other expenses`** (use this for a **seat reservation** or extra ticket),
   - the **Amount in EUR** (comma decimal, e.g. `5,50`),
   then click **Submit receipt** (registers the row) before **OK, continue**.
9. **Your personal details** — pre-filled from the account; verify, then **OK, continue**.
10. **Your compensation** — `Voucher` (redeemable on bahn.de / DB Navigator / DB Reisezentrum, valid 3 years) or `Bank transfer` (account holder pre-filled; **IBAN entered by the user**; BIC optional/auto for German IBANs).
11. **Two more questions** — optionally tick *receive a copy by e-mail*; the customer survey is optional.
12. **Send request now** — final submit → **Confirmation of request** page with a **reference number** (e.g. `26V98220962`). Capture it. DB responds within ~1 month by post or e-mail.

## Getting the receipt (invoice) for an extra seat/ticket

On the extra order's **Trip details** page → **Create invoice** → billing address is pre-filled from the account → **Download invoice as PDF**. The PDF lands in `~/Downloads` (e.g. `DB_Invoice_<order>.pdf`) and shows the line items + price (a 2nd-class seat reservation is typically a few euros). Always ask the user before downloading.

Alternative: **Reservation info as a PDF** on the same page (reservation details, may not show the price). The invoice is the stronger proof of the amount paid.

## Gotchas

- **Browser file-upload is unreliable.** The Claude-in-Chrome `file_upload` tool may reject host paths (`"no longer accepts host filesystem paths"`) for both `~/Downloads` and session folders. Workaround: **ask the user to attach the file manually** — have them click the "Select receipts" drop zone (or drag the PDF onto it) in their own browser, then continue once it shows as attached.
- **Language flips to English.** Logging in often redirects to `int.bahn.de/en`; labels become English while a few inline strings stay German. The flow is identical.
- **Trip status labels:** `Trip expired` = the trip happened/was used; `Cancelled` = the booking was cancelled (often an unrelated artifact — confirm with the user before treating it as relevant).
- **Multiple same-day orders** are common after a disruption (original ticket, replacement ticket, standalone seat reservation). Each is a separate **Auftragsnummer** with its own Trip details. Claim against the *ticket actually used*, and add the *extra seat/ticket* as an additional expense.
- **The form is one long page** with sectioned steps; each "OK, continue" turns that section green ("Saved") and reveals the next. Use the per-section **Change** pencil to edit a saved section.

## Quick checklist

- [ ] Logged in (or order number + surname for guest)
- [ ] Correct journey identified (the one actually taken)
- [ ] Actual arrival time set and confirmed with the user
- [ ] Extra costs added with receipt + correct category + amount
- [ ] Payout method chosen; IBAN entered **by the user**
- [ ] Final summary reviewed and explicitly approved before **Send request now**
- [ ] Reference number captured and reported
