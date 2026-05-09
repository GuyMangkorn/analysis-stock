# Financial Facts Vault

Obsidian vault นี้ใช้สำหรับแปลง source ทางการเงินให้กลายเป็น output ที่ตรวจสอบย้อนกลับได้ เช่น annual financial tables, ratio tables, chart blocks, และ entity pages ที่อัปเดตซ้ำได้ง่าย

## What This Vault Is For

- ingest ไฟล์ `.md` จาก investor relations, annual report extracts, หรือ earnings materials
- ingest ไฟล์ `.csv` ที่คุณแปะเอง
- normalize ข้อมูลงบการเงินแบบ annual 5 ปี
- คำนวณ ratios เฉพาะเมื่อมี source ยืนยันได้
- render chart block สำหรับ `obsidian-charts`
- สร้างหรืออัปเดต `wiki/entities/TICKER.md`

vault นี้ไม่ควร make ข้อมูล และไม่ควรเติมค่าที่หาไม่ได้

## Structure

- `raw/imports/` วางไฟล์ต้นทาง `.md` และ `.csv`
- `raw/financials/` เก็บ output ต่อ ticker ทั้ง Markdown และ JSON
- `wiki/entities/` หน้าอ่านจริงของแต่ละบริษัท
- `wiki/analysis/` memo สำหรับ validation, exceptions, และ comparison
- `wiki/reference/` เอกสารอ้างอิงสำหรับมนุษย์
- `.codex/skills/financial-extractor/` local skill สำหรับ AI workflow

## Setup Notes

- vault นี้เตรียม `obsidian-charts` plugin ไว้ใน `.obsidian/plugins/obsidian-charts`
- เปิด Community Plugins ใน Obsidian หากยังไม่เปิด
- chart block ทั้งหมดใน vault นี้ออกแบบให้ใช้ code block ชนิด `chart`

## Standard Workflow

1. วาง source file ใน `raw/imports/`
2. prompt ให้ agent ingest ไฟล์นั้น
3. agent อ่าน source และสรุป provenance
4. agent normalize annual financial data เป็น output กลาง
5. agent คำนวณ ratios เฉพาะตัวที่มี input พอ
6. agent สร้างหรืออัปเดต:
   - `raw/financials/TICKER_fundamentals.md`
   - `raw/financials/TICKER_fundamentals.json`
   - `wiki/entities/TICKER.md`
   - `log.md`

## Prompting Guide

ใช้ prompt ให้ชัดเจนเรื่อง ticker, source file, และสิ่งที่ต้องการให้ update

หลักสำคัญ:

- ระบุ path ของ input file ให้ชัด
- ถ้ารู้ ticker แล้ว ให้ใส่ ticker
- ถ้าต้องการแค่ recompute charts หรือ verify source gap ให้บอกตรง ๆ
- ถ้าข้อมูลใดหาไม่ได้ ให้ agent เขียน `ไม่พบข้อมูลที่ยืนยันได้` พร้อม source ที่ตรวจแล้ว

## Prompt Library

### 1. Ingest IR Markdown

```text
ใช้ skill financial-extractor ingest ไฟล์ raw/imports/MSFT_ir.md สำหรับ ticker MSFT
สร้าง annual financial table 5 ปี, ratio table, revenue/net profit chart, segment revenue chart
ถ้าข้อมูลไหนยืนยันไม่ได้ให้เขียนว่าไม่พบข้อมูลที่ยืนยันได้และใส่ source/path ที่ตรวจแล้ว
```

### 2. Ingest CSV

```text
ใช้ skill financial-extractor ingest ไฟล์ raw/imports/MSFT_financials.csv สำหรับ ticker MSFT
normalize field names ให้ตรง schema ของ vault นี้
สร้าง raw/financials/MSFT_fundamentals.md, raw/financials/MSFT_fundamentals.json และอัปเดต wiki/entities/MSFT.md
```

### 3. Update Existing Entity

```text
อัปเดต wiki/entities/MSFT.md จาก raw/financials/MSFT_fundamentals.json
regen เฉพาะตาราง ratios และ chart blocks โดยไม่แก้ source facts ที่ยืนยันไม่ได้
```

### 4. Regenerate Charts

```text
regen charts ของ MSFT จาก raw/financials/MSFT_fundamentals.json
ใช้ obsidian-charts bar chart สำหรับ revenue/net profit 5 ปี และ segment revenue
ถ้า segment taxonomy เปลี่ยนข้ามปี ให้ใช้ latest-year only และใส่ warning
```

### 5. Verify Source Gaps

```text
ตรวจ source gaps ของ AAPL จาก raw/imports/AAPL_ir.md
บอกชัดว่าค่าไหนยืนยันได้ ค่าไหนยืนยันไม่ได้ และต้องการ source เพิ่มตรงไหน
อย่าคำนวณค่าที่ input ไม่ครบ
```

## Output Expectations

ต่อ 1 ticker ต้องได้อย่างน้อย:

- `raw/financials/TICKER_fundamentals.md`
- `raw/financials/TICKER_fundamentals.json`
- `wiki/entities/TICKER.md`
- log entry ใหม่ใน `log.md`

Markdown output ควรมี:

- Provenance
- Annual Financial Table
- Key Ratios
- Revenue vs Net Profit Chart
- Segment Revenue Chart
- Missing / Unverified Data

JSON output ควรมี:

- company metadata
- provenance list
- annual series 5 ปี
- segment series
- ratio results
- missing-data registry

## Failure Rules

- ถ้าไม่มี source ยืนยัน ให้เขียน `ไม่พบข้อมูลที่ยืนยันได้`
- ทุกตัวเลขต้อง trace กลับไปที่ source path, URL, หรือ note ได้
- ห้ามเดา
- ห้าม fill gap
- ห้าม derive แบบย้อนตรวจไม่ได้
- ถ้า source conflict กัน ให้เก็บ note ความขัดแย้งและอย่าฝืนเลือกตัวเลขโดยไม่มีเหตุผล

## Expected Ratios

ค่าเริ่มต้นของ vault นี้รองรับ:

- Current Ratio
- Quick Ratio
- D/E Ratio
- Interest Coverage Ratio
- Gross Profit Margin
- Net Profit Margin
- ROE
- P/E Ratio
- P/BV Ratio
- Dividend Yield
- Dividend Payout Ratio

ดูรายละเอียดสูตรใน [wiki/reference/financial-ratios.md](wiki/reference/financial-ratios.md) และ skill reference files
