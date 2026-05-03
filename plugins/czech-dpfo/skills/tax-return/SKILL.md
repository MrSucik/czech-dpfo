---
name: tax-return
description: Use when filing annual Czech tax return (daňové přiznání DPFO), preparing tax documents, checking tax deadlines, or when user says /tax-return. Covers Czech personal income tax for employment income — especially the split srážková / zálohová case that disqualifies the employer from doing roční zúčtování (RZD) on the employee's behalf.
---

# Annual Tax Return (Daňové přiznání DPFO)

## Overview

Walk a Czech employee through filing their annual personal income-tax return (DPFO) on **mojedane.cz**. Designed for the case the employer cannot do roční zúčtování daně (RZD) for the employee — most commonly when the employee had a split between **srážková daň** (unsigned prohlášení period) and **zálohová daň** (signed prohlášení period) at the same employer in one year. This combination usually results in a refund.

## When to Use

- "Help me file my Czech tax return"
- "Daňové přiznání"
- "DPFO"
- January–April annually
- `/tax-return`

## Step 1: Gather user data (store in KV)

Before anything else, ask the user for the data below and persist it to a Jarvis-style KV namespace `<year>-tax-return/personal_data` (or wherever you store reusable personal data). Once captured, reuse it next year — only income figures change.

| Field | Notes |
|---|---|
| Full legal name | As on OP |
| Rodné číslo | Required on most form fields |
| Date of birth | |
| Permanent address | Trvalé bydliště — street, house number, district, ZIP, city (combobox-driven on the form) |
| Employer name + DIČ | From the potvrzení |
| Employer address | From the potvrzení |
| Tax office (FÚ) | Územní pracoviště — derives from permanent address |
| Datová schránka ID (Fyzická osoba) | **Personal** DS, NOT a company DS. Required for electronic filing if you have one. |
| Bank account for refund | Account number + bank code, or IBAN |
| Accountant / payroll contact | Email of whoever at the employer issues the potvrzení |

If any field is missing, ask the user — do NOT invent values for tax-form fields. Do NOT use a company datová schránka for personal DPFO; that's a different legal entity.

## Why manual filing may be needed

The employer can't do RZD (roční zúčtování) on the employee's behalf when:

- The employee had a **split prohlášení** at the same employer (signed for part of the year, unsigned for part) — produces a srážková + zálohová split
- The employee had **multiple employers** in the year
- The employee has **other §7/§9/§10 income** beyond employment (freelance, rentals, capital gains)

If the user has a **datová schránka**, electronic filing is **mandatory** — paper option is no longer available.

## Key deadlines (each year)

| Method | Deadline |
|---|---|
| Paper filing | April 1 |
| Electronic filing | May 2 |
| Via tax advisor | July 1 |

## Annual workflow

### Step 2: Collect documents (February)

Ask the employer's payroll contact (from KV) for the **Potvrzení o zdanitelných příjmech** for the relevant tax year.

For the split-prohlášení case, expect **two pages** — one per regime (srážková + zálohová). If the contact only sends one, ask explicitly for both.

### Step 3: Prepare data

From the potvrzení(s), extract:

- **Zálohová daň period**: gross income, months covered, tax withheld (zálohy)
- **Srážková daň period**: gross income, months covered, tax withheld (srážková)
- Total gross = sum of both
- Total withheld = sum of both

### Step 4: File online (March–April)

1. Go to **mojedane.cz** → EPO → Daň z příjmů fyzických osob
2. Choose: *"DPFO pro poplatníky mající pouze příjmy ze závislé činnosti ze zdrojů na území ČR"*
3. Log in via **datová schránka** or **bankovní identita** (NOT eID — incompatible with this form)
4. Fill the form using data from the user's KV + the potvrzení:
   - **Oddíl 1**: Personal info (auto-fills if logged in via DS)
   - **Příloha 1 / Výpočtová tabulka k §6**: ONE row per employer, combining BOTH regimes
   - **Slevy na dani**: Sleva na poplatníka — FULL year, regardless of months worked
   - **Bank account for refund**: from KV
5. Attach PDF(s) of the potvrzení
6. Submit electronically (Odeslat a podepsat → Datová schránka, or bankovní identita)
7. Save the **podací číslo** and **heslo** that the portal returns — they're proof of submission

### Step 5: Claim refund

Refund typically arrives within **30 days of the filing deadline** (e.g., file by May 2 → refund by ~June 2). Refund goes to the bank account specified in the form.

## Form-field mapping

See [TEMPLATE.md](TEMPLATE.md) for the row-by-row mapping for the standard split srážková + zálohová case.

## Lessons learned (built into the skill)

- Form auto-fills "sleva na invaliditu" — clear it unless the user actually qualifies
- Attachments (potvrzení) are *propustné chyby* — form submits without them, but attach anyway for the FÚ's audit trail
- Address fields use combobox dropdowns — fill street/number separately, not combined into one string
- Sleva na poplatníka is **full year regardless of months worked** — don't pro-rate
- Verify the **current-year sleva amount** before filing (it changes; e.g., 30,840 Kč for TY 2025)

## Reminders to set each year

In January, suggest the user create reminders/tasks:

1. "Ask payroll for **potvrzení o zdanitelných příjmech** for tax year [YEAR]" — due Feb 15
2. "Submit DPFO for tax year [YEAR]" — due March 31 (one month before electronic deadline)

## Portal URLs

- EPO formuláře: https://adisspr.mfcr.cz/pmd/epo/formulare
- DPFO (zaměstnanci): https://adisspr.mfcr.cz/pmd/epo/novy/DPF_ZC2
- MojeDaně DIS+: https://adisspr.mfcr.cz/pmd/dis

## Disclaimer

This skill is **not tax advice**. Edge cases out of scope:

- Foreign income (§ 38f credits, double-taxation treaties)
- §7 / §9 / §10 income beyond employment
- ZTP/P credit, children's tax credit, spouse credit
- Mortgage interest / pension contribution deductions
- Multi-year corrections (dodatečné DPP)

For any of these, consult a tax advisor or extend the skill for your specific situation.
