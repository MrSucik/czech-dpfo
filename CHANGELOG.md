# Changelog

All notable changes to the `czech-dpfo` skill are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-05-03

### Added

- Initial release of the `czech-dpfo` skill.
- 5-step annual workflow: gather user data → identify why manual filing is needed → collect potvrzení → file via mojedane.cz EPO → claim refund.
- Personal-data prompt with KV persistence under `<year>-tax-return/personal_data` for cross-year reuse.
- Targeted at the **split srážková + zálohová** prohlášení case at one employer (the most common reason RZD is impossible).
- Form-field mapping in TEMPLATE.md covering rows ř. 22, 24, 32, 33/34, 36/44, 45, 52, 53, 55 for the split case.
- Lessons-learned table: auto-filled sleva na invaliditu cleanup, address combobox quirks, full-year sleva na poplatníka rule, attachment-as-propustná-chyba behavior.
- Per-year submission log structure for podací číslo / heslo / refund tracking.
- Explicit disclaimer covering out-of-scope cases (§7/§9/§10, foreign income, ZTP/P, children/spouse credits, mortgage/pension deductions).
