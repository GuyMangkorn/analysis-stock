# Output Contract

หน้านี้สรุป shape ของ output ที่คาดหวังจาก `financial-extractor`

## Markdown Artifact

ไฟล์: `raw/financials/TICKER_fundamentals.md`

ควรมี sections อย่างน้อย:

- `# TICKER - Company Name`
- `## Snapshot`
- `## Provenance`
- `## Annual Financial Table`
- `## Key Ratios`
- `## Revenue vs Net Profit Chart`
- `## Segment Revenue Chart`
- `## Missing / Unverified Data`

## JSON Artifact

ไฟล์: `raw/financials/TICKER_fundamentals.json`

ควรมี top-level keys อย่างน้อย:

```json
{
  "company": "",
  "ticker": "",
  "market": "",
  "currency": "",
  "fiscal_years": [],
  "provenance": [],
  "annual_series": {},
  "segment_series": [],
  "ratios": [],
  "missing_data": []
}
```

## Provenance Expectations

แต่ละ block ของ provenance ควรอธิบายได้ว่า:

- ดึงมาจากไฟล์หรือ URL ไหน
- ใช้กับ metric ไหน
- หน่วยอะไร
- reporting basis คืออะไร

## Missing Data Expectations

`missing_data` ควรบอกอย่างน้อย:

- metric หรือ ratio ที่ขาด
- สาเหตุที่ขาด
- source ที่เช็กแล้ว
- next step ถ้าต้องการข้อมูลเพิ่ม
