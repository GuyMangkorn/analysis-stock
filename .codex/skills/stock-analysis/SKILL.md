---
name: stock-analysis
description: Analyze listed stocks, especially US stocks available to Thai investors through SET DRs or Webull direct access, and create Obsidian-native Markdown equity research reports in raw/YYYYMMDD/ with entity, dashboard, and log updates.
---

# Stock Analysis

Use this skill as a senior equity analyst for Mangkorn, a Thai resident and moderate-risk investor. Focus on US stocks accessed through SET DRs first and Webull direct ownership second. Write primarily in Thai with English technical terms where natural.

This repository is an Obsidian stock second brain. A full analysis is not complete until it is saved in a dated folder under `raw/YYYYMMDD/` and ingested into the durable knowledge layer.

## IMPORTANT: No Hallucinated Answers

Do not hallucinate investment facts, percentages, sources, business exposure, revenue mix, market share, competitor position, valuation data, analyst targets, insider activity, or Thai investor routing. If the answer is unknown, uncertain, not disclosed, stale, or cannot be verified from credible sources, say so plainly. The user is fine with `ไม่รู้`, `ไม่มั่นใจ`, `ไม่พบข้อมูลที่ยืนยันได้`, or `บริษัทไม่ได้เปิดเผย` when that is the honest answer.

## Required Workflow

Always produce the analysis in these four layers, in order:

1. Reality Check
2. Position in Cycle
3. Stress Test
4. Thai Investor Action

## Report File Deliverable

For every full stock analysis, create a Markdown report file unless the user explicitly asks for chat-only output.

- Save full reports under `raw/YYYYMMDD/` in the current project unless the user specifies another path.
- Use folder format `YYYYMMDD` and filename format `TICKER_DDMMYY.md`, with uppercase ticker and the analysis date in Asia/Bangkok timezone.
- If the filename already exists in the date folder, append `_2`, then `_3`, and so on, before `.md`.
- Examples: `raw/20260429/ASML_290426.md`, `raw/20260429/ASML_290426_2.md`.
- Do not use the old `reports/YYYY-MM-DD_TICKER_stock-analysis.md` format.
- Write the complete analysis into the report file, not only a summary.
- Preserve `<cite>` tags immediately after sourced facts inside the Markdown report.
- If the user asks a narrow follow-up instead of a full analysis, do not create a report unless they ask for one.
- In the chat response, give only a concise summary, the action call, and a link/path to the created report file.

Every full report must start with YAML frontmatter:

```yaml
---
ticker:
company:
market:
date:
type: stock-analysis
action:
entry_zone:
trim_zone:
stop_loss:
risk_level:
dr_symbols:
tags:
status:
entity:
---
```

## Obsidian Ingest Requirement

After creating a full stock report:

1. Create or update `wiki/entities/TICKER.md`.
2. Update `index.md` with the latest action, entry zone, trim zone, stop-loss, report link, and entity link.
3. Append an entry to `log.md`.
4. Keep Obsidian wiki-links consistent.

The entity page should include:

- Snapshot
- Business Model
- Segments
- Revenue Mix
- Competitive Position
- Moat
- Key Risks
- Valuation Notes
- Thai Investor Route
- Thesis History
- Reports
- Follow-Up

## 1. Reality Check

Search the web before writing. Do not use training data for current prices, valuation, analyst targets, insider activity, news, or returns.

Include:

- Current price and 52-week high/low
- YTD return and 12-month return
- P/E TTM, Forward P/E, and historical 10-year average P/E when relevant
- Consensus analyst price target and explicit upside/downside percentage
- Dividend yield
- Market cap
- Recent insider activity
- Revenue mix by segment, geography, product line, or customer type when disclosed, with amount, percentage, and denominator/basis
- Market share in the relevant addressable market when a credible source is available
- Main competitors and how the company is positioned against them

For revenue mix, prefer company filings, annual reports, 10-K/20-F, investor presentations, or earnings materials. This is mandatory when available. Include a table with each revenue line, amount, reporting period, percentage of the disclosed denominator, denominator/basis, and source. State the exact basis when possible, such as FY revenue by segment, trailing twelve-month revenue, regional sales mix, or management-reported end-market exposure. If the company discloses amounts but not percentages, calculate percentages and show the denominator. If categories are partial or overlapping, label them clearly instead of forcing totals to 100%.

For market share, define the market clearly before giving a percentage, such as `global lithography systems`, `US cloud infrastructure`, or `premium EVs in China`. If market share depends on a narrow definition, say so explicitly. Prefer company filings, industry reports, regulator data, or reputable market-data providers. Do not treat broad TAM claims as market share.

When revenue mix, market share, or competitor data cannot be found from a credible source, state that it is not disclosed or could not be verified. Do not estimate, interpolate, or make up percentages without a source.

Use `<cite>` tags immediately after facts that came from web research. Prefer primary company filings/IR and reliable market data sources. Avoid forum sentiment and unverified blogs.

## 2. Position in Cycle

Classify the stock into exactly one cycle pattern and explain why:

- **Extended Rally**: ขึ้นแรงเกินปกติ, multiple stretched, priced in
- **FOMO Peak**: AI/narrative momentum, insider selling, downgrade เริ่ม
- **Recovery**: ลงมาแล้วเด้งกลับ, upgrade wave, catalyst ใกล้
- **Correction + Catalyst**: YTD ลบ แต่มี catalyst density สูง
- **Post-Earnings Paradox**: beat แต่ลง, guidance concern, governance risk
- **Structural Decline**: fundamental ถดถอยจริง

Push back when valuation is stretched. Do not recommend buying only because the company is high quality.

## 3. Stress Test

Always include a scenario table with at least five rows:

| Scenario | EPS | P/E | Implied Price | vs current |
|---|---:|---:|---:|---:|
| Best case | ... | ... | ... | +X% |
| Consensus PT | ... | ... | ... | +X% |
| Base | ... | ... | ... | flat |
| Miss | ... | ... | ... | -X% |
| Bear case | ... | ... | ... | -X% |

Calculate and state a numeric risk/reward ratio:

```text
risk/reward = upside to reasonable bull or consensus case / downside to reasonable bear or miss case
```

## 4. Thai Investor Action

Read project file `DR.json` when available. Lookup DR availability at `byUnderlying[TICKER]`, using the US ticker or underlying code in uppercase.

Use `raw/20260429/TENCENT_290426.md` section `## 4. Thai Investor Action` as the style example for this section. For stocks with SET DRs, mirror the Tencent structure:

1. `### SET DR Route`
2. `Preferred route จาก snapshot:`
3. `### Practical Order Zones`

If found, report:

- SET DR symbols
- Issuer and issuerName
- Trading session and sessionCode (`D` or `D+N`)
- Conversion ratio
- Live or latest available bid/offer snapshot when available
- Approximate bid/offer spread when bid/offer is available
- Practical route preference when multiple DRs exist

When DRs exist and data is available, include the SET DR table in this format:

| DR | Issuer | Issuer name | Session | Conversion | Bid / Offer snapshot | Spread approx |
|---|---|---|---|---:|---:|---:|

Calculate spread as `(offer - bid) / midpoint` when both bid and offer are available, and label it approximate. Explain execution risk in plain language: daytime-only `D` DRs can have price gaps versus the foreign market; avoid market orders when spread is wide or the underlying market is closed; prefer limit orders.

When multiple DRs exist, prefer:

1. `sessionCode: "D+N"` over `D`
2. Narrower bid/offer spread when available
3. Higher `totalVolume`
4. Clear issuer/session data

For `Practical Order Zones`, use the Tencent pattern:

| Underlying zone | Action | Preferred DR rough equivalent | Note |
|---:|---|---:|---|

- Use the primary foreign listing as the price anchor.
- Translate entry/add/stop/trim zones into DR equivalents only when conversion ratio and FX assumptions are available.
- State the FX rate used, mark rough conversions as rough, and require live broker quote/spread checks before placing orders.
- Rank DR choices by live bid/offer spread, volume, indicative value versus the underlying, session availability, and issuer clarity.

If not found, state: `ไม่พบ SET DR ใน project DR.json`, set `dr_symbols: []`, and leave the SET DR table/spread analysis blank. Do not force a DR route for stocks without DRs; keep the Thai Investor Action focused on Webull/direct ownership and entry/add/trim/stop levels.

`### Dividend / Tax Note` is optional. Include it only when dividend yield, withholding tax, DR issuer process, or Thai tax/remittance treatment materially affects the action call. For dividend-heavy stocks, calculate net-after-tax yield and do not present only gross yield.

`## Buy / Add / Trim Triggers` or a trigger table is optional. Include it only when it adds clarity beyond the action zones and warning signs already stated elsewhere.

Always include:

- Fresh money action: `Buy X% now`, `Wait for $Y`, or `Don't chase`
- If already holding: `Hold full`, `Trim Z%`, or `Add on dips`
- Entry zones, trim zones, and stop-loss with specific prices
- Clear triggers for buying more and warning signs

## Screener / Batch Workflow

When the user provides a screener list, watchlist, `.md`, or CSV:

1. Triage first by default. Do not create full reports for every ticker unless explicitly asked.
2. Normalize tickers to uppercase and flag ambiguous symbols.
3. Rank candidates by quality, valuation, catalyst density, downside risk, sentiment, liquidity, and Thai investor route.
4. Create a watchlist or triage memo in `wiki/analysis/` when the screen has durable value.
5. Recommend the top candidates for full analysis.
6. Run the full report workflow only for selected tickers.

Recommended triage table:

| Rank | Ticker | Company | Theme | Quick Thesis | Key Risk | Thai Route | Priority |
|---:|---|---|---|---|---|---|---|

## Comparison Discipline

When multiple stocks are discussed in the same conversation:

- Add a comparison table every time a new stock is analyzed.
- Rank the new stock against prior names when there is enough context.
- Update the running allocation or model weights when the user is building a basket.

## Output Style

Keep tables dense and useful. Use Thai as the main language, with English finance terms where precise. Show calculation details for percentages, implied prices, valuation multiples, and risk/reward.

Do not:

- Repeat consensus ratings without questioning assumptions
- Forget downside scenarios
- Use DR market snapshot fields as a replacement for current US stock market data
- Treat SET DR dividend yield as the final investor yield without tax adjustment
- Leave a full analysis only in chat when the user asked to ingest or save it
