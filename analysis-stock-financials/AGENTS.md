# Financial Facts Vault Agent Schema

This repository is an Obsidian-native vault for extracting, normalizing, and reviewing company financial facts. The agent's job is to preserve verifiable numbers and reusable financial outputs, not to improvise missing values.

## Mission

Maintain a durable financial-facts vault where:

- `raw/imports/` stores original input files such as IR markdown, annual-report extracts, and pasted CSVs.
- `raw/financials/` stores normalized per-ticker outputs in Markdown and JSON.
- `wiki/entities/` stores one financial profile per ticker.
- `wiki/analysis/` stores comparison notes, mapping notes, and validation memos.
- `wiki/reference/` stores human-readable reference material such as ratio formulas and source hierarchy.
- `index.md` stays useful as the vault landing page.
- `log.md` records every ingest, update, regeneration, and maintenance pass.

Chat is transient. Durable outputs belong in the vault.

## Repository Structure

- `raw/imports/`
  - User-supplied `.md` and `.csv` source files.
- `raw/financials/`
  - Final normalized per-ticker artifacts.
  - Main contract:
    - `TICKER_fundamentals.md`
    - `TICKER_fundamentals.json`
- `wiki/entities/`
  - One entity page per ticker, usually `TICKER.md`.
- `wiki/analysis/`
  - Validation notes, parser exceptions, and comparison memos.
- `wiki/reference/`
  - Human-checkable reference pages.
- `index.md`
  - Landing page and workflow dashboard.
- `log.md`
  - Chronological change log.

## Operating Mode

This vault is optimized for manual ingest.

Use it when the human asks to:

- ingest an IR/source markdown file
- ingest a pasted CSV
- normalize annual financial data
- compute ratios from verified inputs
- regenerate charts
- update an entity page
- verify missing or conflicting data

## Core Rules

1. Prefer investor-relations material, annual reports, audited filings, and earnings materials over secondary summaries.
2. Never make up a number, label, denominator, or business segment.
3. If a value cannot be verified, write `ไม่พบข้อมูลที่ยืนยันได้` and record why.
4. Every numeric output must trace back to a source path, URL, or note in the vault.
5. Do not fill gaps with estimates unless the human explicitly asks for a separate estimate workflow.
6. Keep Markdown outputs readable in Obsidian and JSON outputs machine-friendly.

## Naming Rules

- Use uppercase ticker symbols.
- Use:
  - `raw/financials/TICKER_fundamentals.md`
  - `raw/financials/TICKER_fundamentals.json`
- Use:
  - `wiki/entities/TICKER.md`
- For validation or comparison notes in `wiki/analysis/`, prefer concise descriptive names with dates when helpful.

## Output Contract

For each completed ticker job, create or update:

1. `raw/financials/TICKER_fundamentals.md`
2. `raw/financials/TICKER_fundamentals.json`
3. `wiki/entities/TICKER.md`
4. `log.md`

## Entity Page Standard

Each entity page should contain:

- `# TICKER - Company Name`
- `## Snapshot`
- `## Provenance / Sources`
- `## Annual Financial Table`
- `## Key Ratios`
- `## Revenue vs Net Profit Chart`
- `## Segment Revenue Chart`
- `## Missing / Unverified Data`

## Dashboard Standard

`index.md` should help answer:

- what input is ready to ingest
- which tickers already have normalized outputs
- which jobs still have source gaps
- where to check formulas and chart conventions

## Log Standard

Append short dated entries in this form:

```markdown
## YYYY-MM-DD

- `ingest`: Created `raw/financials/MSFT_fundamentals.md`, `raw/financials/MSFT_fundamentals.json`, and updated `[[MSFT]]`.
```

## Scope Boundary

This vault is for financial facts, normalization, and chart-ready outputs. It is not the primary place for full investment thesis writing unless the human explicitly asks for that.
