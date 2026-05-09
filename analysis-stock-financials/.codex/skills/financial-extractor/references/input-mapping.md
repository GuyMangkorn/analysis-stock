# Input Mapping

Map extracted fields into the canonical schema below.

## Annual Series Keys

- `revenue`
- `gross_profit`
- `net_income`
- `current_assets`
- `current_liabilities`
- `cash_and_equivalents`
- `short_term_investments`
- `accounts_receivable`
- `total_liabilities`
- `total_equity`
- `interest_expense`
- `pretax_income`
- `tax_expense`
- `diluted_eps`
- `dividend_per_share`

## Mapping Rules

- Keep original units in provenance notes and normalize units before writing final series.
- Preserve the exact reporting basis: annual, TTM, or latest-year-only.
- Do not merge overlapping rows into one canonical field without an explicit reason.
- If a CSV column name is ambiguous, record the ambiguity in `missing_data` instead of guessing.

## Markdown Table Parsing

- Capture header row exactly before normalizing.
- Detect whether years run left-to-right or right-to-left.
- Check whether numbers are in millions, billions, or local currency units.
- Record whether negatives are shown with minus signs or parentheses.

## CSV Parsing

- Preserve the original header names in provenance.
- Normalize dates or year labels into consistent fiscal-year labels.
- Reject mixed currencies unless the source explicitly provides a normalized reporting currency.
