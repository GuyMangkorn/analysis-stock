---
date: 2026-05-05
type: source-integrity-audit
status: active
tags:
  - maintenance
  - source-integrity
  - fact-only
---

# Source Integrity Audit 050526

## Scope

ตรวจ source integrity รอบนี้จากส่วนที่เสี่ยงที่สุดต่อการ "เดาเอง":

- `[[MSFT_290426]]`, `[[MSFT_300426]]`, และ `[[MSFT FY26 Q3 Earnings Update 300426]]`
- `[[V_050526]]` และ `[[MA_050526]]`
- `[[MSFT]]`, `[[V]]`, `[[MA]]`
- `AGENTS.md` operating rules

## Verdict

**ไม่พบหลักฐานว่า operating data / revenue mix หลักถูกสร้างขึ้นมาเองใน scope ที่ตรวจ.** MSFT, Visa และ Mastercard ใช้ตัวเลขหลักจาก company IR / SEC filings / cited market-data providers และหลายจุดมีการบอกแล้วว่าเป็น calculation หรือ rough mapping.

อย่างไรก็ตามพบจุดที่ควร tighten เพื่อไม่ให้ข้อมูลกึ่งประมาณกลายเป็น fact ถาวร:

| File | Finding | Action |
|---|---|---|
| `[[V_050526]]`, `[[V]]` | FullRatio P/E history เปิดซ้ำได้เป็น current P/E 28.45x, 10-year average 33.58x, median 32.64x; report เดิมใช้ 33.77x / 32.9x | ปรับ raw report และ entity ให้ตรงกับ source ที่เปิดซ้ำได้ |
| `[[MA_050526]]` | 12-month return เขียนชัดว่า `not cleanly verified` จาก Yahoo quote card | เปลี่ยนเป็น `Not used; clean source not captured` และ exclude จาก action call |
| หลาย reports | DR fair value, risk/reward, implied EPS/P/E, market-share percentages เป็น calculations จาก source inputs | เพิ่ม IMPORTANT fact-only plan ใน `AGENTS.md` ให้ต้อง label calculations และห้ามเขียนเหมือน company-disclosed fact |

## Verified Checks

| Area | Result |
|---|---|
| MSFT Q3 FY2026 headline | Revenue $82.9B, operating income $38.4B, EPS $4.27, Microsoft Cloud $54.5B, cRPO $627B, Azure + other cloud services +40% reported / +39% CC, AI ARR >$37B +123% YoY all match Microsoft IR. <cite>[Microsoft FY26 Q3 press release](https://www.microsoft.com/en-us/Investor/earnings/FY-2026-Q3/press-release-webcast)</cite> |
| MSFT revenue mix | Segment amounts are from Microsoft segment results; percentages are calculations from disclosed total revenue and should remain labeled as calculated mix. <cite>[Microsoft FY26 Q3 segment revenues](https://www.microsoft.com/en-us/Investor/earnings/FY-2026-Q3/segment-revenues)</cite> |
| Visa Q2 FY2026 | Net revenue $11.23B, EPS $3.14 / $3.31 non-GAAP, payments volume +9%, cross-border total +12%, processed transactions +9%, and $20B buyback authorization match SEC exhibit. <cite>[Visa Q2 FY2026 earnings release, SEC](https://www.sec.gov/Archives/edgar/data/1403161/000140316126000077/q22026earningsrelease.htm)</cite> |
| Visa revenue mix | FY2025 revenue category/geography values are tied to Visa 10-K; percentages are calculated against explicitly stated denominators. <cite>[Visa FY2025 Form 10-K, SEC](https://www.sec.gov/Archives/edgar/data/1403161/000140316125000089/v-20250930.htm)</cite> |
| Mastercard quote/valuation | MA close $504.74, market cap $445.98B, TTM P/E 29.20x, forward P/E 24.94x, dividend $3.48 / 0.69%, and target $648.04 match StockAnalysis as of the checked page. <cite>[StockAnalysis MA quote](https://stockanalysis.com/stocks/ma/)</cite> <cite>[StockAnalysis MA forecast](https://stockanalysis.com/stocks/ma/forecast/)</cite> |
| Mastercard revenue mix | FY2025 and Q1 2026 Payment network / Value-added services and solutions amounts match SEC 10-K / 10-Q; percentages are calculated from total net revenue. <cite>[Mastercard 2025 Form 10-K](https://www.sec.gov/Archives/edgar/data/1141391/000114139126000013/ma-20251231.htm)</cite> <cite>[Mastercard Q1 2026 Form 10-Q](https://www.sec.gov/Archives/edgar/data/1141391/000114139126000031/ma-20260331.htm)</cite> |

## Rules Added

Added `## IMPORTANT: Fact-Only Source Integrity Plan` to `AGENTS.md`. Key behavior now required:

- source before writing
- no unsupported numbers
- label calculations and assumptions
- separate facts from interpretation
- write `not disclosed` / `could not verify` instead of estimating silently
- prefer primary sources
- fresh-check current market data
- ingest without adding unsourced facts
- state source conflicts instead of smoothing them
- pre-write audit for uncited numbers and rough/estimated language

## Follow-Up

- Future maintenance pass should scan older reports for uncited rough DR mappings and convert them to explicitly labeled calculations where needed.
- Any future screen/analyze task should keep a small source ledger before writing the final durable note.
