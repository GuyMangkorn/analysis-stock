# Source Hierarchy

หน้านี้ใช้เป็นลำดับความเชื่อถือของ source ใน vault นี้

## Preferred Order

1. Investor relations pages
2. Annual reports / audited financial statements
3. Official filings
4. Earnings releases / earnings presentations
5. Company-provided data tables or downloadable CSV
6. User-supplied CSV derived from official source
7. Secondary market-data sites for market ratios only

## Rules

- ถ้า source หลักกับ source รองขัดกัน ให้ยึด source หลักก่อน
- ถ้าเลือกใช้ source รอง ต้องอธิบายว่าทำไม source หลักไม่พอ
- ถ้า source หลายตัวไม่สอดคล้องกันและยังตัดสินไม่ได้ ให้บันทึก conflict แทนการฝืนเลือก
- ตัวเลขทุกตัวใน output ควรมี path, URL, หรือ reference note กลับไปหาได้

## When To Stop

หยุดแล้วรายงาน `ไม่พบข้อมูลที่ยืนยันได้` เมื่อ:

- บริษัทไม่ได้เปิดเผยข้อมูลนั้น
- input ขาดองค์ประกอบสำคัญของสูตร
- taxonomy ของ segment ทำให้เทียบข้ามปีไม่ได้อย่างปลอดภัย
- source ที่มีอยู่เป็นเพียงสรุปต่อ ๆ กันและย้อนไปหา primary source ไม่ได้
