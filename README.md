# czech-dpfo

An agent skill that walks a Czech employee through filing their **annual personal income-tax return (DPFO)** on [mojedane.cz](https://adisspr.mfcr.cz/pmd/dis).

Designed for the case the employer **cannot do roční zúčtování daně (RZD)** on the employee's behalf — most often a **split srážková + zálohová prohlášení** at the same employer in one year, which the payroll system can't reconcile. This combination almost always produces a refund.

## What it does

1. **Asks for personal data** — full name, RČ, address, employer details, datová schránka ID, refund account — and persists to a KV namespace `<year>-tax-return/personal_data` for reuse next year.
2. **Identifies why manual filing is needed** — split prohlášení / multiple employers / non-employment income.
3. **Walks the document collection** — ask the employer's payroll contact for the *Potvrzení o zdanitelných příjmech* (two pages for the split case).
4. **Walks the mojedane.cz EPO form** — login via datová schránka or bankovní identita, fill *Příloha 1 / Výpočtová tabulka k §6* with one row per employer combining both regimes, claim full-year sleva na poplatníka.
5. **Submits + records** — saves the *podací číslo* and *heslo* (proof-of-submission secret) so the user has audit trail.
6. **Includes a form-field mapping** — see [TEMPLATE.md](./plugins/czech-dpfo/skills/tax-return/TEMPLATE.md) for the row-by-row guide on the split srážková + zálohová case.

## Why this skill exists

Czech employment income with a split prohlášení mid-year is a common scenario (changing roles, contract type changes, missed prohlášení signing) but every year people pay an accountant a few thousand Kč to fill what is essentially a deterministic form. This skill encodes the form filling so the user can do it themselves in ~20 minutes.

## Built-in lessons learned

- Auto-filled *sleva na invaliditu* — clear it unless qualified.
- Attachments are *propustné chyby* — form submits without them but attach anyway.
- Address combobox dropdowns — fill street/number separately, not as one string.
- Sleva na poplatníka is **full year regardless of months worked** — don't pro-rate.
- Verify the current-year sleva amount before filing (it changes year over year).

## Constraints

- **No tax advice.** This skill encodes a specific filing workflow, not tax planning.
- **Out of scope**: foreign income (§ 38f), §7/§9/§10 income, ZTP/P credit, children/spouse credits, mortgage/pension deductions, dodatečné DPP corrections. See the disclaimer in SKILL.md.
- **Refuses to fabricate data** — if a required field is missing, asks the user instead of estimating.

## Install

In an active agent session that supports the plugin format (e.g. Claude Code):

```
/plugin marketplace add MrSucik/czech-dpfo
/plugin install czech-dpfo@czech-dpfo
```

Or from the terminal:

```bash
claude plugin marketplace add MrSucik/czech-dpfo
claude plugin install czech-dpfo@czech-dpfo
```

Pin to a release tag:

```
/plugin marketplace add MrSucik/czech-dpfo@v1.0.0
```

## Use

```
/tax-return
```

Or describe it: `"help me file my Czech tax return"`, `"daňové přiznání"`, `"DPFO za rok 2025"`.

## Key deadlines

| Method | Deadline |
|---|---|
| Paper filing | April 1 |
| Electronic filing | May 2 |
| Via tax advisor | July 1 |

If you have a *datová schránka*, electronic filing is **mandatory** — no paper option.

## Disclaimer

This skill is **not tax advice** and **not a substitute for a tax advisor** for complex situations. Tax law changes; verify the current-year sleva and any new credits / deductions with the FÚ or a daňový poradce before filing.

## License

MIT — see [LICENSE](./LICENSE).
