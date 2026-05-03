# DPFO Filing Reference Template

A reusable form-field mapping for the standard **split srážková + zálohová** employment case (the case this skill is designed for).

## Form-field mapping

| Form field | What goes here | Notes |
|---|---|---|
| ř. 22 (Úhrn příjmů) | Total gross from BOTH regimes | sum of zálohová + srážková gross |
| ř. 24 (Základ daně) | Same as ř. 22 for employee-only filers | |
| ř. 32 (Základ po odpočtech) | ř. 24 rounded DOWN to hundreds | always rounds down |
| ř. 33 / 34 (Daň) | 15% of ř. 32 | |
| ř. 36 / 44 (Sleva na poplatníka) | Full-year amount (verify current year) | TY 2025: 30,840 Kč |
| ř. 45 (Daň po slevách) | max(0, ř. 33 − ř. 36) | If sleva > daň, this is 0 |
| ř. 52 (Sražené zálohy) | Only the ZÁLOHOVÁ withholding | |
| ř. 53 (Srážková daň §36) | Only the SRÁŽKOVÁ withholding | |
| ř. 55 (Zbývá doplatit) | Negative number = refund owed to you | |

Sleva na poplatníka changes yearly. **Always verify the current-year amount** before filing.

## Per-year filing log (suggested KV structure)

Persist this to a KV namespace like `<year>-tax-return/submission` after each filing — useful for next year's reference and as proof in case the FÚ asks:

```
Tax Year:           <YYYY>
Filed date:         <YYYY-MM-DD>
Filing channel:     mojedane.cz EPO
Login method:       Datová schránka | Bankovní identita
Podací číslo:       <number returned by portal>
Heslo:              <STORE PRIVATELY — proof-of-submission secret>
Refund account:     <account / IBAN>
Refund expected by: <filing date + 30 days, or May 2 + 30 if filed earlier>
Totals:
  Gross income:     <Kč>
  Total withheld:   <Kč>
  Refund / owed:    <Kč>  (negative = refund)
```

The **heslo** the portal returns is sensitive — it functions as a submission secret. Don't store it in a public note or chat history.
