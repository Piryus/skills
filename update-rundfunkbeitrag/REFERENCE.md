# Update Rundfunkbeitrag — reference

Detailed flow, field maps and browser-automation gotchas for the **"Daten ändern"** form on **rundfunkbeitrag.de**. Read alongside [SKILL.md](SKILL.md).

## Entry routes

- **Homepage:** `rundfunkbeitrag.de` → decline Matomo cookie banner (**"Ablehnen"**).
- **Change hub:** `/aendern` ("Daten ändern"). Three audience tabs: **Bürgerinnen und Bürger** (this skill), **Unternehmen und Institutionen**, **Einrichtungen des Gemeinwohls**. Under the citizens tab, the card **"Name, Adresse, Zahlungsweise ändern"** offers two buttons: **"PDF herunterladen"** (print + post) and **"Online ausfüllen"**.
- **Deep link to the online form:** `/buergerinnen-und-buerger/formulare/aendern`. Loads the wizard directly at step 1.
- The footer link **"Bankverbindung"** (`/bankverbindung`) is *not* the change form — it shows the Beitragsservice's own bank details for **outgoing transfers**. Its "Teilnahme SEPA" link points to `/aendern`.

## The 3-step wizard

Tabs: **1. Allgemeine Angaben → 2. Änderung → 3. Prüfen und absenden**. You can click the tabs to jump between steps (no server round-trip); **"Weiter"** / **"Daten prüfen"** also validate the current step and turn its tab green (✓). Nothing is persisted server-side until **"Absenden"** on step 3.

### Step 1 — Allgemeine Angaben (identity / current address)

Heading: *"Geben Sie hier Ihre bisherige Anschrift ein."* Fields, in order:

| Field | Type | Notes |
|---|---|---|
| Beitragsnummer | text | Required. The contribution-account number (see below). |
| Anrede | radio | Frau / Herr / keine Angabe / Firma |
| Vorname | text | |
| Nachname/Firma | text | |
| Geburtsdatum | text | format **TT.MM.JJJJ** |
| PLZ | text | drives Ort + Straße (see gotcha) |
| Ort | combobox | editable; accepts free text |
| Straße | combobox | editable; accepts free text |
| Hausnummer | text | |
| Vorwahl / Telefonnummer / E-Mail | text | all **optional**; add an email only if the user wants a copy |

Use the **currently registered** identity, not the new one. Click **"Weiter"**.

### Step 2 — Änderung (what to change)

Heading: *"Folgende Daten möchte ich ändern:"* — three checkboxes, each reveals its own block:

- **Person/Firma** → new name fields.
- **Wohnung/Betriebsstätte** → new address fields (same PLZ→Ort→Straße pattern as step 1).
- **Zahlungsweise** → payment block:
  - **Zahlungsrhythmus** (radio): `gesetzlich in der Mitte eines Dreimonatszeitraums (zum 15.)` (the statutory default) / `vierteljährlich im Voraus (1.1./1.4./1.7./1.10.)` / `halbjährlich im Voraus (1.1./1.7.)` / `jährlich im Voraus (1.1.)`. A pure bank-account change still forces a choice here — keep the user's current rhythm unless they want it changed.
  - **Zahlungsart** (radio): `durch Lastschrift von meinem/unserem Konto` (direct debit) or `durch Überweisung` (bank transfer).
  - Selecting **Lastschrift** reveals: **IBAN** (user-typed) → **Geldinstitut** (auto-fills, shows *"Wird vom System in einem späteren Verlauf automatisch ergänzt"* until then) → **"Ich bin nicht der Kontoinhaber"** checkbox (tick only if the holder differs; reveals holder name fields) → creditor ID *Gläubiger-Identifikationsnummer DE3000100000001272* (static) → **SEPA-Lastschriftmandat** authorization radio (*"Ich ermächtige den Beitragsservice…"*, user-ticked) → 8-week-refund note.
  - **"Diese Änderung gilt ab"** — date (datepicker; see gotcha).

At the bottom of step 2: the **math CAPTCHA** ("Bitte geben Sie das Ergebnis der Rechenaufgabe ein", e.g. *9 + 5*) with *Neue Aufgabe laden* / *Aufgabe vorlesen*, then **Zurück** / **Daten prüfen**. The CAPTCHA exists *"damit kein automatisiertes Programm die Änderungen durchführt"* — **the user solves it, never the agent**.

### Step 3 — Prüfen und absenden

Full read-back of every entered value under **Allgemeine Angaben** and **Änderung**, each with a **"Korrigieren"** link back to that step, then **Zurück** / **Absenden**. `get_page_text` returns the entire summary cleanly — use it to verify. **Absenden is irreversible and is the user's click.** Afterwards the page shows a confirmation / Eingangsbestätigung.

## Browser-automation gotchas (learned live)

1. **Fields render on scroll.** `read_page` only returns the form inputs once they're scrolled into/near the viewport. Scroll to a field, then re-read to get its `ref`.
2. **PLZ → Ort/Straße is event-driven.** Setting PLZ with `form_input` (programmatic) does **not** trigger the city/street lookup — Ort stays empty and validates red ("Bitte geben Sie einen Ort an"). Fix: **type the PLZ as real keystrokes** (`computer` → click field → `type`). Ort and Straße are **editable comboboxes that accept free text** — type the city ("Berlin") and the full street ("Heilbronner Straße"); no need to pick from a list.
3. **Radios ignore `form_input`.** Setting an Anrede / Zahlungsart radio via `form_input value:true` silently no-ops. **Click radios directly** by coordinate and screenshot-verify the fill.
4. **Datepicker is selection-only.** Typing into "Diese Änderung gilt ab" is ignored; the calendar overlay opens on focus. Scroll so the full month is visible, then **click the day cell** (today is pre-highlighted).
5. **Coordinate drift.** Screenshot viewport size varies (e.g. 1440×693 vs 1568×755); a click computed from one screenshot can land on the wrong field after a re-render (once dropped "10" into Geburtsdatum instead of Hausnummer). Prefer **`form_input` by `ref` for plain text fields**, and re-screenshot before coordinate clicks.
6. **The MCP tab group can drop mid-flow.** Twice the tab/window vanished and `tabs_context_mcp` reported no group. Recreate with `createIfEmpty:true`; the **deep link reloads the wizard but loses entered data**, so re-fill from step 1. Capture the confirmation page promptly — the tab may close right after Absenden.
7. **`form_input` does fire change events for plain text fields** (name, Geburtsdatum, Hausnummer) — those are safe and reliable. The dependency problem is specific to PLG→Ort/Straße and to radios/checkboxes.

## PDF-by-post alternative

The **"PDF herunterladen"** button on the `/aendern` card produces the same change form as a printable PDF (fill online, print, sign, mail). Use only if the online flow is blocked; downloading a file needs explicit user permission.

## Finding the Beitragsnummer

The Beitragsnummer is on any Beitragsservice letter or invoice and on the bank statement line for the debit. The site footer ("Häufig gesucht") and the field's tooltip link to **"Beitragsnummer"** help. It is the only credential required — there is no password/login for this form.

## Language

The site is German. An **"English"** link sits in the footer ("Other Languages"), but the **change form itself is German-only**; field meanings are mapped above. "Leichte Sprache" (top bar) is a plain-German info version, not the form.
