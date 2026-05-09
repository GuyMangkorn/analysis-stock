---
name: financial-extractor
description: Ingest investor-relations markdown or CSV files, normalize annual financial data, compute verified ratios, render Obsidian chart blocks, and update entity pages in the financial facts vault without hallucinating missing values.
---

# Financial Extractor

Use this skill for the dedicated financial-facts vault when the task is to ingest `.md` or `.csv` financial source files, normalize annual 5-year data, compute ratios from verified inputs, generate `obsidian-charts` blocks, and publish per-ticker outputs.

## Non-Negotiables

- Never make up financial values, segment labels, or denominator assumptions.
- If a value cannot be verified, write `ไม่พบข้อมูลที่ยืนยันได้`.
- Every number must trace back to a source path, URL, or memo inside the vault.
- Use annual 5-year data as the default scope.

## Read These References

- Read `references/source-hierarchy.md` before choosing between conflicting inputs.
- Read `references/input-mapping.md` when mapping `.md` tables or CSV columns into the canonical schema.
- Read `references/financial-ratios.md` before computing ratios.
- Read `references/chart-blocks.md` before writing chart code blocks.

## Required Workflow

1. Confirm the input file exists in `raw/imports/` or at the provided path.
2. Identify ticker, company name, currency, reporting basis, and fiscal years.
3. Extract annual series into the canonical fields.
4. Record provenance for every extracted block.
5. Compute only the ratios whose inputs are fully supported.
6. Write:
   - `raw/financials/TICKER_fundamentals.md`
   - `raw/financials/TICKER_fundamentals.json`
7. Create or update `wiki/entities/TICKER.md`.
8. Append `log.md`.

## Canonical Output Sections

Markdown outputs should contain:

- `## Snapshot`
- `## Provenance`
- `## Annual Financial Table`
- `## Key Ratios`
- `## Revenue vs Net Profit Chart`
- `## Segment Revenue Chart`
- `## Missing / Unverified Data`

JSON outputs should contain:

- `company`
- `ticker`
- `market`
- `currency`
- `fiscal_years`
- `provenance`
- `annual_series`
- `segment_series`
- `ratios`
- `missing_data`

## Publishing Rules

- `wiki/entities/TICKER.md` must be readable without opening the JSON file.
- Use `chart` code blocks compatible with `obsidian-charts`.
- If segment taxonomy changes across years, use latest-year-only segment chart and say why.
- Keep the entity page idempotent: replace the same sections instead of duplicating them.

## When Not To Force Completion

Stop and report gaps instead of forcing an output when:

- the source file is missing
- the ticker is ambiguous
- the units are unclear
- a ratio input is incomplete
- the source data conflicts and cannot be resolved by the hierarchy rules
