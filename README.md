# Obsidian Stock Second Brain

โปรเจกต์นี้คือ Obsidian vault สำหรับสะสมบทวิเคราะห์หุ้นแบบใช้งานจริง โดยเก็บรายงานฉบับเต็มไว้ใน `raw/YYYYMMDD/` และให้ `wiki/` เป็นสมองที่สองสำหรับสรุป thesis, company profile, watchlist, decision notes และการค้นย้อนหลัง

## โครงสร้างหลัก

- `raw/YYYYMMDD/` - รายงานหุ้นฉบับเต็มและ dated source note ที่ agent เขียนหรือ ingest เข้ามา
- `raw/assets/` - รูป chart, screenshot, source attachments หรือไฟล์ประกอบรายงาน
- `wiki/entities/` - หน้า company profile ต่อหุ้น เช่น `ASML.md`, `MSFT.md`
- `wiki/analysis/` - screener triage, comparison, watchlist memo, decision notes
- `wiki/overview/` - ภาพรวม sector, theme, portfolio map หรือ dashboard ย่อย
- `index.md` - Portfolio Dashboard หลัก
- `log.md` - บันทึกว่า ingest/update อะไร เมื่อไร
- `AGENTS.md` - operating rules สำหรับ Codex/agent
- `DR.json`, `DR-SET.json` - ฐานข้อมูล SET DR สำหรับนักลงทุนไทย

## กติกาชื่อไฟล์รายงานหุ้น

รายงานหุ้นฉบับเต็มทุกไฟล์ต้องอยู่ในโฟลเดอร์วันที่ใต้ `raw/` และใช้รูปแบบ:

```text
raw/YYYYMMDD/TICKER_DDMMYY.md
```

ตัวอย่าง:

```text
raw/20260429/ASML_290426.md
raw/20260429/MSFT_290426.md
raw/20260429/XIAOMI_290426.md
```

ถ้าวิเคราะห์หุ้นตัวเดิมในวันเดียวกันและชื่อซ้ำ ให้เติม suffix:

```text
raw/20260429/ASML_290426_2.md
raw/20260429/ASML_290426_3.md
```

ใช้ ticker uppercase เป็นชื่อหลักเสมอ วันที่ใช้ timezone Asia/Bangkok โฟลเดอร์ใช้ `YYYYMMDD` เพื่อให้เรียงตามเวลาได้ตรงเสมอ; ถ้าอยากดูใหม่ไปเก่า ให้ sort descending ใน Obsidian หรือ file explorer

## วิธีใช้ให้เกิดประสิทธิภาพสูงสุด

### 1. วิเคราะห์หุ้นเดี่ยวแบบเต็ม

ใช้เมื่ออยากได้รายงานลงทุนจริงสำหรับหุ้นหนึ่งตัว

ตัวอย่าง prompt:

```text
วิเคราะห์ ASML full analysis และ ingest เข้า Obsidian
```

สิ่งที่ agent ต้องทำ:

1. ค้นข้อมูลล่าสุดจากเว็บ เช่น ราคา, valuation, analyst target, earnings, insider activity, news และ filings
2. อ่าน `DR.json` เพื่อเช็กว่ามี SET DR หรือไม่
3. เขียนรายงานเต็มลง `raw/YYYYMMDD/TICKER_DDMMYY.md`
4. สร้างหรืออัปเดต `wiki/entities/TICKER.md`
5. อัปเดต `index.md` ให้เห็น action ล่าสุด, entry zone, trim zone, stop-loss และลิงก์รายงาน
6. เพิ่มรายการใน `log.md`

ผลลัพธ์ที่ควรคาดหวัง:

- ได้รายงานฉบับเต็มพร้อม sources
- ได้หน้า company profile ที่สะสม thesis ระยะยาว
- dashboard เห็นทันทีว่าหุ้นนี้ควร buy, wait, hold, trim หรือ avoid

### 2. Ingest รายงานหรือข้อมูลดิบที่มีอยู่แล้ว

ใช้เมื่อคุณมีไฟล์ markdown, note, source summary หรือรายงานที่ทำมาก่อนแล้ว

ตัวอย่าง prompt:

```text
ingest raw/20260429/ASML_290426.md เข้า stock brain
```

หรือ:

```text
อ่านไฟล์ raw/20260429/my-note.md แล้ว ingest เข้า Obsidian
```

ขั้นตอนที่ดีที่สุด:

1. วางไฟล์ไว้ใน `raw/YYYYMMDD/` หรือส่ง path ให้ agent
2. บอกชัดว่าเป็น full stock analysis, raw source note, watchlist memo หรือ comparison
3. ให้ agent อ่านไฟล์และแยกประเภท
4. ถ้าเป็นหุ้นเดี่ยว ให้ update `wiki/entities/TICKER.md`
5. ถ้าเป็นหลายหุ้น ให้สร้างหรืออัปเดต note ใน `wiki/analysis/`
6. ให้ agent update `index.md` และ `log.md`

ข้อแนะนำ:

- อย่าให้ chat answer ที่มีสาระสำคัญหายไปในแชท ถ้ามี durable insight ให้สั่งว่า “save เป็น analysis note”
- ถ้าไฟล์ยังเป็นข้อมูลดิบมาก ให้บอก agent ว่า “สรุปก่อน ingest” เพื่อแยก facts, thesis, risk และ follow-up

### 3. ใส่รายชื่อหุ้นจาก screener

ใช้เมื่อได้ stock ideas จาก Finviz, TradingView, Koyfin, broker screen, YouTube, newsletter หรือแหล่งอื่น

รองรับ input:

- paste list เช่น `ASML, TSM, MSFT, AMZN`
- markdown เช่น `raw/20260429/screener_290426.md`
- CSV เช่น `raw/20260429/screener_290426.csv`

ตัวอย่าง prompt แบบเร็ว:

```text
นี่คือ screener list: ASML, TSM, MSFT, AMZN, NOW ช่วย triage และเลือก top 3 ที่ควร full analysis
```

ตัวอย่าง prompt จากไฟล์:

```text
อ่านไฟล์ raw/20260429/screener_290426.csv แล้วทำ watchlist triage
```

workflow ค่าเริ่มต้น:

1. Agent บันทึกหรืออ้างอิง screener input เป็น watchlist memo ใน `wiki/analysis/`
2. Agent ทำ triage ก่อน ไม่สร้าง full report ทุกตัวทันที
3. Triage ต้องดู valuation, catalyst, risk, quality, sentiment, liquidity และ DR/Webull route
4. Agent จัดอันดับ top candidates พร้อมเหตุผลว่าตัวไหนควรขุดต่อ
5. คุณเลือกตัวที่อยากทำ full analysis หรือสั่งให้ทำ top N
6. Agent สร้าง full report แยกเป็น `raw/YYYYMMDD/TICKER_DDMMYY.md`
7. Full report แต่ละตัวต้อง ingest เข้า entity, index และ log

ผลลัพธ์ที่ดี:

- Screener list ไม่กลายเป็นกอง ticker ที่ไม่รู้จะเริ่มตรงไหน
- คุณได้ shortlist ที่มีเหตุผลก่อนเสียเวลากับ full report
- หุ้นที่ผ่านการคัดจะมี history ใน Obsidian ต่อทันที

### 4. Query จาก Stock Brain

ใช้เมื่ออยากถามจากข้อมูลที่สะสมไว้แล้ว

ตัวอย่าง prompt:

```text
ตอนนี้ใน stock brain มีหุ้น AI infrastructure ตัวไหนน่าสนใจสุด
```

หรือ:

```text
เทียบ ASML กับ TSM จาก thesis ล่าสุดใน vault
```

สิ่งที่ agent ต้องทำ:

1. อ่าน `index.md` ก่อน
2. หา entity, analysis note และ overview ที่เกี่ยวข้อง
3. ตอบโดยอ้างอิงไฟล์ใน vault เป็นหลัก
4. ถ้าคำตอบมีคุณค่าระยะยาว ให้ถามหรือเสนอให้ save เข้า `wiki/analysis/`

### 5. Lint และบำรุง Stock Brain

ใช้เป็นรอบ ๆ เพื่อไม่ให้ vault กลายเป็นคลังไฟล์รก

ตัวอย่าง prompt:

```text
lint stock brain ให้หน่อยว่ามีหุ้นไหนข้อมูลเก่า ขาด follow-up หรือ thesis ขัดกัน
```

สิ่งที่ agent ควรตรวจ:

- entity ไหนไม่มีรายงานล่าสุด
- หุ้นไหน action call เก่าเกินไป
- thesis ไหนมีข่าวใหม่ที่อาจ invalidate
- หน้าไหน orphan ไม่มี internal links
- screener/watchlist ไหนยังไม่ได้ follow-up
- stock ไหนควร merge, split หรือ archive

## Frontmatter มาตรฐานของรายงานหุ้น

รายงาน full analysis ใน `raw/YYYYMMDD/` ควรเริ่มด้วย:

```yaml
---
ticker: ASML
company: ASML Holding N.V.
market: NASDAQ
date: 2026-04-29
type: stock-analysis
action: Wait
entry_zone: ""
trim_zone: ""
stop_loss: ""
risk_level: moderate
dr_symbols: []
tags: [stock, semiconductor]
status: active
entity: "[[ASML]]"
---
```

ค่าเหล่านี้ช่วยให้ Obsidian Dataview กรองหุ้นตาม action, market, sector, risk และ follow-up ได้ง่าย

## Prompt Library

```text
วิเคราะห์ ASML full analysis และ ingest เข้า Obsidian
```

```text
นี่คือ screener list จาก TradingView: ASML, TSM, MSFT, AMZN ช่วย triage และเลือก top 3
```

```text
อ่านไฟล์ raw/20260429/screener_290426.md แล้วทำ watchlist analysis
```

```text
อัปเดต entity ของ MSFT จากรายงานล่าสุด
```

```text
สร้าง comparison note ระหว่าง ASML, TSM, NVDA จากข้อมูลล่าสุดใน vault
```

```text
lint stock brain ให้หน่อยว่ามีหุ้นไหนข้อมูลเก่า/ขาด follow-up
```

## หลักการใช้งาน

- `raw/YYYYMMDD/` คือรายงานฉบับเต็มและ dated source notes อย่าแก้ย้อนหลังแบบลบประวัติ ยกเว้นแก้ typo หรือ format
- `wiki/entities/` คือ thesis ที่อัปเดตได้เรื่อย ๆ
- `wiki/analysis/` คือพื้นที่คิด เช่น comparison, screener triage, decision memo
- `index.md` ต้องตอบได้เร็วว่า “ตอนนี้ควรทำอะไรกับหุ้นตัวไหน”
- `log.md` ต้องบอกได้ว่า vault เปลี่ยนอะไรไปบ้าง
