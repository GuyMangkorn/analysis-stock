# Financial Ratios

## Canonical Ratios

| Ratio | Formula | Required Inputs |
|---|---|---|
| Current Ratio | `current_assets / current_liabilities` | current assets, current liabilities |
| Quick Ratio | `(cash_and_equivalents + short_term_investments + accounts_receivable) / current_liabilities` | cash, short-term investments, receivables, current liabilities |
| D/E Ratio | `total_liabilities / total_equity` | total liabilities, total equity |
| Interest Coverage Ratio | `EBIT / interest_expense` | EBIT, interest expense |
| Gross Profit Margin | `gross_profit / revenue` | gross profit, revenue |
| Net Profit Margin | `net_income / revenue` | net income, revenue |
| ROE | `net_income / average_equity` | net income, beginning equity, ending equity |
| P/E Ratio | `current_price / trailing_diluted_EPS` | price, trailing diluted EPS |
| P/BV Ratio | `current_price / book_value_per_share` | price, book value per share |
| Dividend Yield | `annual_dividend_per_share / current_price` | dividend per share, price |
| Dividend Payout Ratio | `dividend_per_share / EPS` | dividend per share, EPS |

## Availability Policy

- If any required input is missing, mark the ratio `unavailable`.
- Do not backfill missing inputs from memory.
- Do not convert overlapping or partial disclosures into a full ratio unless the denominator is explicit.

## Presentation Policy

- Use percentages for margins, ROE, yield, and payout.
- Use `x` style where natural for leverage or coverage if the vault page prefers it, but keep the underlying numeric value in JSON.
