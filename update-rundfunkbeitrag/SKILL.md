---
name: update-rundfunkbeitrag
description: Change your German broadcasting-fee (Rundfunkbeitrag) account details on rundfunkbeitrag.de via Claude in Chrome — bank account / IBAN and SEPA direct-debit mandate, postal address, name, payment method and payment frequency. Use when the user wants to change the bank account the Rundfunkbeitrag / "GEZ" / "ARD ZDF Deutschlandradio Beitragsservice" is debited from, update their SEPA-Lastschriftmandat or IBAN, switch between direct debit and bank transfer, change the billing rhythm, or report a new address or name to the Beitragsservice.
---

# Update Rundfunkbeitrag details

Reports a change to a Rundfunkbeitrag account (the German broadcasting fee, collected by the **ARD ZDF Deutschlandradio Beitragsservice**; formerly nicknamed "GEZ") through the **"Daten ändern"** online form on **rundfunkbeitrag.de**, driven with **Claude in Chrome**. One form covers three kinds of change. **No login is needed** — the user is identified by their **Beitragsnummer** plus current name and address.

## What it can change

- **Zahlungsweise** — bank account (**IBAN** + **SEPA-Lastschriftmandat**), payment method (Lastschrift ↔ Überweisung), and payment frequency (**Zahlungsrhythmus**: statutory quarterly / quarterly / half-yearly / yearly in advance).
- **Wohnung/Betriebsstätte** — postal address.
- **Person/Firma** — name.

## Hard safety rules (never violate)

- **Never type the user's IBAN, bank details, or any password.** Make the structural selections, then hand off — the **user types the IBAN** directly in the browser.
- **Never solve the math CAPTCHA.** Step 2 has an arithmetic task ("X + Y") whose stated purpose is to ensure no automated program submits the change. The **user enters the answer**.
- **Never tick the SEPA-Lastschriftmandat authorization radio** — that is the user authorizing the new direct debit. Hand it off.
- **Never click the final "Absenden".** It is irreversible. Pause, show the full review summary, and let the **user submit** after explicit confirmation.
- **Verify every value against the screen** before advancing a step; the form pre-fills nothing.
- Do **not** record the user's PII (Beitragsnummer, address, IBAN) into a saved GIF.

## Workflow

1. **Open Claude in Chrome**, get tab context, create a fresh tab, navigate to `rundfunkbeitrag.de`. Decline the Matomo analytics cookie banner (**"Ablehnen"**).
2. **Open the form:** `/aendern` → tab **"Bürgerinnen und Bürger"** → card **"Name, Adresse, Zahlungsweise ändern"** → **"Online ausfüllen"**. Deep link straight to it: `/buergerinnen-und-buerger/formulare/aendern`. It is a 3-step wizard: **1. Allgemeine Angaben → 2. Änderung → 3. Prüfen und absenden**.
3. **Step 1 — Allgemeine Angaben (identity):** fill Beitragsnummer · Anrede · Vorname · Nachname/Firma · Geburtsdatum · PLZ · Ort · Straße · Hausnummer (phone/email optional). Use the **currently registered** name and address. ⚠️ The form has automation quirks (dependent PLZ→Ort/Straße dropdowns, radios that ignore programmatic input, fields that only render once scrolled into view) — see [REFERENCE.md](REFERENCE.md). Click **"Weiter"** (validates step 1; tab turns green).
4. **Step 2 — Änderung:** tick the box(es) for what to change — **Zahlungsweise** / **Wohnung/Betriebsstätte** / **Person/Firma** — then fill the revealed sub-fields. For a **bank-account change**:
   - Tick **Zahlungsweise** → pick **Zahlungsrhythmus** (keep the user's current one unless they want it changed) → **Zahlungsart = "durch Lastschrift von meinem/unserem Konto"** → set the **"Diese Änderung gilt ab"** date.
   - Then **hand off**: the user types the **IBAN** (Geldinstitut auto-fills), ticks the **SEPA-Lastschriftmandat**, and solves the **math CAPTCHA**. Tick **"Ich bin nicht der Kontoinhaber"** only if the account holder differs.
   - Click **"Daten prüfen"**.
5. **Step 3 — Prüfen und absenden:** read the whole summary with `get_page_text`, verify every field against the user's data, present it (confirm the **IBAN** explicitly). **The user clicks "Absenden."**
6. **Capture the confirmation** (success message / Eingangsbestätigung, reference if shown) and report it back.

## More detail

See [REFERENCE.md](REFERENCE.md): exact field list per step, the browser-automation gotchas, the address/name-change branches, the PDF-by-post alternative, where to find the Beitragsnummer, and English/German notes.
