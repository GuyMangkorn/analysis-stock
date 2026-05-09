# Financial Facts Dashboard

> Use this page as the starting point for ingest, normalization, ratio review, and chart regeneration.

## Quick Links

- [README](README.md)
- [Agent Contract](AGENTS.md)
- [[wiki/reference/entity-template|Entity Template]]
- [[wiki/reference/financial-ratios|Financial Ratio Reference]]
- [[wiki/reference/output-contract|Output Contract]]
- [[wiki/reference/source-hierarchy|Source Hierarchy]]
- [Raw Imports](raw/imports/)
- [Normalized Financial Outputs](raw/financials/)
- [Change Log](log.md)

## Workflow

1. Drop a source file into `raw/imports/`
2. Prompt the `financial-extractor` skill with ticker + file path
3. Review `raw/financials/TICKER_fundamentals.md`
4. Review `wiki/entities/TICKER.md`
5. Check `Missing / Unverified Data` before trusting any ratio table

## Output Contract

Each completed ticker should have:

| Artifact | Required |
|---|---|
| `raw/financials/TICKER_fundamentals.md` | Yes |
| `raw/financials/TICKER_fundamentals.json` | Yes |
| `wiki/entities/TICKER.md` | Yes |
| `log.md` entry | Yes |

## Current Status

| Queue | State |
|---|---|
| `raw/imports/` | Ready for first source files |
| `raw/financials/` | Empty by design |
| `wiki/entities/` | Empty by design |
| `wiki/analysis/` | Empty by design |

## Review Rules

- Do not trust a chart without checking its provenance section.
- Do not trust a ratio if its inputs are missing or inferred.
- Treat `ไม่พบข้อมูลที่ยืนยันได้` as the correct output when source support is incomplete.
