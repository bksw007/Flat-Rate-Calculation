# โปรแกรมคำนวนค่างวด Loan Rate Calculation

เว็บแอพคำนวณค่างวดแบบมืออาชีพสำหรับ
- สินเชื่อรถยนต์/มอเตอร์ไซค์ (Flat Rate)
- สินเชื่อบ้าน (Effective Rate + Annuity/PMT)

พัฒนาด้วย Next.js App Router + TypeScript พร้อม UI responsive, Dark/Light mode, และบันทึกอินโฟกราฟฟิกเป็นไฟล์ภาพ

---

## Highlights

- คำนวณค่างวดรถแบบ Flat Rate
  - รองรับเงินดาวน์, ดอกเบี้ย, ระยะเวลาผ่อน
  - รองรับกรณีรถมือสอง (+VAT 7%)
  - มีกราฟโครงสร้างต้นทุนและกราฟเปรียบเทียบระยะเวลาผ่อน

- คำนวณค่างวดบ้านแบบลดต้นลดดอก (Effective Rate)
  - ค่างวดคงที่ด้วยสูตร Annuity/PMT
  - แสดงเงินดาวน์, ยอดจัดกู้, ค่างวด, ดอกเบี้ยรวม, ดอกเบี้ยเดือนแรก
  - มีกราฟโครงสร้างต้นทุนและกราฟเทียบระยะเวลากู้

- ประสบการณ์ใช้งาน
  - Responsive สำหรับ Desktop/Mobile
  - Dark/Light toggle
  - Export หน้าผลลัพธ์เป็น PNG (พร้อม timestamp กันชื่อไฟล์ซ้ำ)
  - ไอคอนเว็บ/แอพ (favicon + manifest + apple touch icon)

---

## หลักการคำนวณ

### 1) รถยนต์/มอเตอร์ไซค์ (Flat Rate)

- เงินดาวน์ = ราคารถ x %เงินดาวน์
- ยอดจัด = ราคารถ - เงินดาวน์
- ดอกเบี้ยรวม = ยอดจัด x อัตราดอกเบี้ยต่อปี x จำนวนปี
- จำนวนงวด = ปี x 12
- ค่างวด = (ยอดจัด + ดอกเบี้ยรวม) / จำนวนงวด

หมายเหตุ: เงื่อนไขจริงขึ้นกับไฟแนนซ์ (ดอกเบี้ย, ค่าธรรมเนียม, VAT, ระยะเวลา)

### 2) บ้าน (Effective Rate + Annuity)

- เงินดาวน์ = ราคาบ้าน x %เงินดาวน์
- ยอดจัดกู้ = ราคาบ้าน - เงินดาวน์
- จำนวนงวด = ปี x 12
- อัตราดอกเบี้ยต่อเดือน r = ดอกเบี้ยต่อปี / 12
- ค่างวดคงที่:

\[
A = P \times \frac{r(1+r)^n}{(1+r)^n - 1}
\]

โดย
- `A` = ค่างวดรายเดือน
- `P` = เงินต้นที่กู้
- `r` = อัตราดอกเบี้ยรายเดือน
- `n` = จำนวนงวดทั้งหมด

ดอกเบี้ยเดือนแรกตัวอย่าง:
\[
\text{Interest}_{month} = \frac{\text{เงินต้นคงเหลือ} \times \text{ดอกเบี้ยต่อปี} \times \text{จำนวนวัน}}{365}
\]

---

## Tech Stack

- Next.js 16 (App Router)
- React 19
- TypeScript
- Chart.js + react-chartjs-2
- html2canvas
- ESLint (Flat Config)

---

## Getting Started

### Prerequisites

- Node.js 20+ (แนะนำ LTS)
- npm 10+

### Install & Run

```bash
npm install
npm run dev
```

เปิดใช้งานที่: [http://localhost:3000](http://localhost:3000)

### Lint / Build / Start

```bash
npm run lint
npm run build
npm run start
```

---

## Deploy (Vercel)

1. Push โค้ดขึ้น GitHub
2. ไปที่ Vercel > **Add New Project**
3. เลือก repository นี้
4. Framework จะถูกตรวจจับเป็น **Next.js** อัตโนมัติ
5. กด Deploy

---

## Project Structure

```text
app/
  globals.css          # global styles + responsive + animation
  layout.tsx           # metadata, favicon/app icons, manifest
  page.tsx             # main app (car/home tabs + calculations + charts)
public/
  icons/               # app icons (192/512, apple touch icon, svg)
  favicon*.png/.ico    # browser favicon set
  site.webmanifest     # web app manifest
```

---

## Important Notes

- ผลลัพธ์เป็นเครื่องมือช่วยวางแผนทางการเงินเบื้องต้น
- ตัวเลขจริงอาจต่างจากข้อเสนอธนาคาร/ไฟแนนซ์ (LTV, ค่าธรรมเนียม, ประกัน, โปรโมชัน, อัตราลอยตัว)
- ควรตรวจสอบเงื่อนไขสัญญาจริงก่อนตัดสินใจ

---

## License

For internal/project use. Add a dedicated LICENSE file if you plan public open-source distribution.
