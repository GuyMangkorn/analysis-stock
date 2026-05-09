# Stock Brain Agent Schema

This repository is an Obsidian-native stock research second brain for Mangkorn, a Thai resident and moderate-risk investor. The agent's job is to maintain a durable investment knowledge base, not just answer in chat.

## Mission

Maintain a compounding stock research vault where:

- `raw/YYYYMMDD/` stores complete stock analysis reports and source-like inputs grouped by Asia/Bangkok date.
- `wiki/` stores durable knowledge, company profiles, comparisons, watchlists, themes, and decision notes.
- `index.md` stays useful as the main Portfolio Dashboard.
- `log.md` records every meaningful ingest or maintenance event.

Chat is transient. Durable investment insight should be filed into the vault when the task asks for ingest, full analysis, screen, save, or update.

## IMPORTANT: Fact-Only Source Integrity Plan

The vault must be fact-first. Do not invent, guess, smooth, backfill, or "complete" missing data from model memory. Every durable factual claim must come from a cited source, a local source file, `DR.json` / `DR-SET.json`, or an explicitly labeled calculation from those facts.

Required workflow for any analyze, screen, ingest, query-save, or maintenance update:

1. **Source before writing.** Collect the source fact first, then write the note. If the source is not available or cannot be reopened, do not present the item as fact.
2. **No unsupported numbers.** Do not create market share, revenue, EPS, P/E, upside, return, FX, DR fair value, segment amount, customer count, or growth figures unless they are directly sourced or calculated from sourced inputs shown in the note.
3. **Label calculations.** When calculating percentages, CAGR, implied fair values, risk/reward, DR equivalents, or scenario prices, show the denominator or formula and say it is a calculation. Do not describe calculated values as company-disclosed values.
4. **Separate fact from interpretation.** Facts, calculations, assumptions, and investment judgment must be distinguishable. Use wording such as "source reports," "calculated from," "assumption," "scenario," "rough mapping," or "not disclosed" as appropriate.
5. **Missing-data rule.** If a company does not disclose a metric, write "not disclosed" or "could not verify from available sources." Do not estimate it unless the human explicitly asks for an estimate, and then label it as an estimate.
6. **Primary-source preference.** Prefer company filings, investor-relations releases, exchange/regulator filings, and official issuer data. Use third-party aggregators only for market data, analyst consensus, historical valuation, or when primary data is unavailable; cite the provider and date.
7. **Current-data rule.** Prices, valuation multiples, analyst targets, insider activity, FX, DR quotes, laws, taxes, and news must be freshly checked. Never rely on memory for current data.
8. **Ingest fidelity.** When ingesting an existing source or output, preserve the source's facts and uncertainty. Do not add new factual claims during ingest unless they are separately sourced and cited.
9. **Conflict handling.** If sources disagree, state the conflict and either choose the higher-quality source or keep both with attribution. Do not silently average conflicting facts.
10. **Pre-write audit.** Before saving durable content, scan for uncited numbers and words such as "rough," "ประมาณ," "estimate," "implied," "proxy," and "not verified." Keep them only when clearly labeled and backed by source inputs.

## Repository Structure

- `raw/YYYYMMDD/`
  - Full stock analysis reports and dated source-like inputs.
  - Use `YYYYMMDD` folder names so chronological sorting is stable; sort descending in Obsidian/File Explorer for newest-first browsing.
- `raw/assets/`
  - Images, charts, screenshots, PDFs, and attachments referenced by raw reports.
- `wiki/entities/`
  - One company profile per ticker, usually `TICKER.md`.
- `wiki/analysis/`
  - Screener triage notes, watchlist analysis, comparison memos, decision notes, and saved query outputs.
- `wiki/overview/`
  - Sector, theme, market, and portfolio-level maps.
- `index.md`
  - Main Portfolio Dashboard.
- `log.md`
  - Chronological change log.
- `DR.json`, `DR-SET.json`
  - SET DR lookup data for Thai investor routing.

## Operating Modes

### analyze

Use when the human asks for a full stock analysis.

Required behavior:

1. Search the web for current market data, valuation, analyst targets, earnings, insider activity, news, and filings.
2. Check `DR.json` for SET DR availability using the uppercase underlying ticker.
3. Include revenue mix with amounts, percentages, and denominator/basis when company disclosures provide enough data.
4. Write the full report to `raw/YYYYMMDD/TICKER_DDMMYY.md`.
5. If that filename exists in the date folder, use `raw/YYYYMMDD/TICKER_DDMMYY_2.md`, then `_3`, and so on.
6. Create or update `wiki/entities/TICKER.md`.
7. Update `index.md`.
8. Append an entry to `log.md`.

### screen

Use when the human provides a list of stocks from a screener, watchlist, markdown file, CSV, broker list, newsletter, or external source.

Default behavior:

1. Do triage first. Do not create full reports for every ticker unless explicitly asked.
2. Accept paste lists, `.md`, and CSV. Excel can be handled when necessary.
3. Rank candidates by quality, valuation, catalyst density, downside risk, sentiment, liquidity, and Thai investor route.
4. Save the screen or triage as a watchlist note in `wiki/analysis/` when the result has durable value.
5. Recommend top candidates for full analysis.
6. Only after selection, run `analyze` for chosen tickers.

### ingest

Use when the human asks to ingest an existing file or output into the vault.

Required behavior:

1. Confirm the file exists if a path is provided.
2. Classify it as full stock analysis, raw source note, screener/watchlist, comparison, decision note, or other.
3. For single-stock content, update the matching entity page.
4. For multi-stock content, create or update an analysis note.
5. Update `index.md` if it changes dashboard-relevant state.
6. Append an entry to `log.md`.
7. Run a source-fidelity check: no new factual claims may be added during ingest unless separately sourced, cited, and labeled as new research.

### query

Use when the human asks a question against the stock brain.

Required behavior:

1. Read `index.md` first.
2. Read only the relevant entity, overview, and analysis pages.
3. Answer from the vault as the primary source of context.
4. If the answer becomes durable investment insight, offer to save it to `wiki/analysis/`.

### lint

Use to health-check and maintain the vault.

Check for:

- stale action calls
- unsupported or uncited numbers
- source-metric mismatches
- calculated values presented as disclosed facts
- entity pages without recent reports
- reports not linked from entity pages
- orphan pages
- weak or missing cross-links
- duplicated ticker pages
- unresolved follow-up tasks
- thesis contradictions across reports

If the human asks for fixes, update the relevant pages and append `log.md`.

## Filename Rules

Full stock analysis reports live in date folders under `raw/` and use:

```text
raw/YYYYMMDD/TICKER_DDMMYY.md
```

Examples:

```text
raw/20260429/ASML_290426.md
raw/20260429/MSFT_290426.md
raw/20260429/XIAOMI_290426.md
```

Duplicate rule:

```text
raw/20260429/ASML_290426_2.md
raw/20260429/ASML_290426_3.md
```

Rules:

- Use uppercase ticker.
- Use Asia/Bangkok local date for both folder and filename.
- Use `YYYYMMDD` for date folders so folder sorting stays chronological.
- Use `DDMMYY` in filenames to keep short, unique Obsidian note names.
- Do not use the old `reports/YYYY-MM-DD_TICKER_stock-analysis.md` format.

## Required Frontmatter

Every full stock analysis report under `raw/YYYYMMDD/` must include:

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

Field expectations:

- `ticker`: uppercase ticker used in the filename.
- `company`: full company name when known.
- `market`: primary market or exchange.
- `date`: ISO date `YYYY-MM-DD`.
- `action`: concise action call such as `Buy 25%`, `Wait`, `Hold`, `Trim`, `Avoid`.
- `entry_zone`, `trim_zone`, `stop_loss`: specific prices or ranges when available.
- `risk_level`: usually `low`, `moderate`, `high`, or `speculative`.
- `dr_symbols`: list of SET DR tickers, or `[]`.
- `tags`: useful Obsidian/Dataview tags.
- `status`: usually `active`, `watchlist`, `archived`, or `superseded`.
- `entity`: Obsidian link to the company page, such as `"[[ASML]]"`.

## Entity Page Standard

Each ticker should have a company profile at `wiki/entities/TICKER.md`.

Recommended sections:

- `# TICKER - Company Name`
- `## Snapshot`
- `## Business Model`
- `## Segments`
- `## Revenue Mix`
- `## Moat`
- `## Key Risks`
- `## Valuation Notes`
- `## Thai Investor Route`
- `## Thesis History`
- `## Reports`
- `## Follow-Up`

Entity pages are living pages. Update them when new full analysis changes the thesis.

## Portfolio Dashboard Standard

`index.md` should stay concise and useful. It should show:

- latest reports
- active watchlist
- current action calls
- entry, trim, and stop zones
- high-priority follow-ups
- links to entities and analysis notes

The dashboard should help answer: what should Mangkorn look at next?

## Log Standard

Append a short dated entry to `log.md` for every full analysis, screen, ingest, entity update, lint fix, or structural maintenance pass.

Use:

```markdown
## YYYY-MM-DD

- `analyze`: Created `raw/20260429/ASML_290426.md`, updated `[[ASML]]`, refreshed dashboard.
```

## Stock Analysis Requirements

For full analysis, follow the local stock-analysis skill:

1. Reality Check
2. Position in Cycle
3. Stress Test
4. Thai Investor Action

Always push back on valuation. Do not recommend buying only because the company is high quality.

Use web research for current data. Do not rely on memory for prices, valuation, analyst targets, insider activity, returns, laws, taxes, or current company events.

Source integrity is mandatory. If a fact cannot be tied to a source or a visible calculation from sourced inputs, remove it or mark it as unverified and exclude it from the action call.

Revenue mix is mandatory when available. Full reports and entity pages must include a revenue mix table with:

- revenue line, segment, geography, product, or customer type
- amount and reporting period
- percentage of the disclosed denominator
- denominator/basis, such as total revenue, principal business revenue, segment revenue, or regional sales
- source citation in the full report

If the company discloses amounts but not percentages, calculate the percentage and show the denominator. If only partial or overlapping categories are disclosed, label them as partial or overlapping instead of forcing totals to 100%. If credible data cannot be found, state that the company did not disclose it or that it could not be verified.

## Screener Workflow

When given a screener list:

1. Preserve the source context when provided.
2. Normalize tickers to uppercase.
3. Identify duplicates or ambiguous tickers.
4. Build a triage table with:
   - ticker
   - company
   - market
   - theme or sector
   - quick thesis
   - key risk
   - valuation/catalyst note
   - Thai investor route
   - priority
5. Recommend top candidates for full analysis.
6. Save the triage in `wiki/analysis/` when requested or when the screen is substantial.

## Style Rules

- Write primarily in Thai with English finance terms where precise.
- Keep pages concrete and rereadable.
- Prefer tables for investment actions and comparisons.
- Use Obsidian wiki-links for internal links.
- Preserve uncertainty and call out stale data.
- Cite sources in full reports with `<cite>` tags immediately after sourced facts.
- Do not bury the action call. Make buy/wait/hold/trim clear.

## Maintenance Rules

Whenever the agent creates or updates durable content:

1. Update the relevant raw, entity, analysis, overview, dashboard, or log files.
2. Keep internal links consistent.
3. Refresh `updated` fields if a page uses frontmatter.
4. Avoid editing raw reports retroactively except for typo, formatting, or explicit human request.
5. Do not modify files outside this repository unless the human explicitly asks.
