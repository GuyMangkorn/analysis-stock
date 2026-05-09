# Financial Ratio Reference

หน้านี้ใช้สำหรับเช็กสูตรก่อนสั่ง agent คำนวณ ratio

## Working Rules

- คำนวณเฉพาะเมื่อ input พอและ trace กลับไปที่ source ได้
- ถ้าขาด input ให้ mark `unavailable`
- อย่าแทนค่าด้วยการเดา

## Formula Set

| Ratio | Formula | Notes |
|---|---|---|
| Current Ratio | `current_assets / current_liabilities` | ใช้ยอดงบดุลสิ้นงวด |
| Quick Ratio | `(cash_and_equivalents + short_term_investments + accounts_receivable) / current_liabilities` | ถ้าส่วนประกอบไม่ครบ ห้ามประมาณ |
| D/E Ratio | `total_liabilities / total_equity` | ใช้มาตรฐานของ vault นี้ |
| Interest Coverage Ratio | `EBIT / interest_expense` | ต้องมี EBIT และดอกเบี้ยจ่ายที่ยืนยันได้ |
| Gross Profit Margin | `gross_profit / revenue` | แสดงเป็น % |
| Net Profit Margin | `net_income / revenue` | แสดงเป็น % |
| ROE | `net_income / average_equity` | average equity = `(beginning_equity + ending_equity) / 2` |
| P/E Ratio | `current_price / trailing_diluted_EPS` | ต้องมี source ของ price และ EPS |
| P/BV Ratio | `current_price / book_value_per_share` | ต้องมี book value per share หรือ input พอคำนวณ |
| Dividend Yield | `annual_dividend_per_share / current_price` | ต้องใช้ price และ DPS ในช่วงเวลา compatible กัน |
| Dividend Payout Ratio | `dividend_per_share / EPS` | ใช้ per-share approach เป็นค่าเริ่มต้น |

## Data Availability Notes

- `Interest Coverage Ratio` มักขาดเพราะบางบริษัทไม่เปิดเผยดอกเบี้ยจ่ายแยกชัด
- `Quick Ratio` มักขาดเพราะ source note ไม่แยก short-term investments หรือ receivables
- market ratios ทั้งหมดควรถูกแยกชัดจาก ratios ที่มาจากงบการเงินโดยตรง
