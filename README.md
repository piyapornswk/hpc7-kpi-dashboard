# Dashboard ตัวชี้วัดสำคัญ · ศูนย์อนามัยที่ 7 ขอนแก่น

เว็บแสดงผล (Dashboard) ตัวชี้วัดสำคัญ เขตสุขภาพที่ 7 — **เวอร์ชันเผยแพร่ออนไลน์ (static, อ่านอย่างเดียว)**
แยกออกมาจากระบบ "งานข้อมูลและติดตามประเมินผล" เฉพาะส่วน Dashboard เพื่อโฮสต์บน Cloudflare Pages

## โครงสร้าง
```
index.html          หน้า Dashboard (HTML+JS ล้วน ไม่มี backend)
data/years.json     รายการปีงบประมาณ
data/kpi-<ปี>.json  ข้อมูลตัวชี้วัด/ผลรายจังหวัดของแต่ละปี
logo.png            โลโก้ศูนย์อนามัยที่ 7
```

## ที่มาของข้อมูล
ข้อมูลเป็น **snapshot** ส่งออกจากระบบหลัง (ASP.NET Core + SQL Server `MneDb`)
ซึ่งดึงจาก MOPH opendata HDC API — เวอร์ชันออนไลน์นี้ **ไม่ดึง API สด** และ **แก้ไขไม่ได้**
(ระบบเต็ม + การแก้ไข/ดึง API อัตโนมัติ ยังอยู่ที่เครื่องภายในเท่านั้น)

## วิธีอัปเดตข้อมูล (เมื่อข้อมูลในระบบหลักเปลี่ยน)
1. รันระบบหลัก (เปิด `เปิดเว็บงานข้อมูล.bat`) ให้ API ที่ `http://localhost:5080` ทำงาน
2. ส่งออก JSON ใหม่ทับไฟล์เดิม:
   ```powershell
   $r = "<โฟลเดอร์นี้>"
   irm "http://localhost:5080/api/kpi/years"      -OutFile "$r\data\years.json"
   irm "http://localhost:5080/api/kpi/data?fy=2569" -OutFile "$r\data\kpi-2569.json"
   ```
3. `git add . ; git commit -m "update data" ; git push` → Cloudflare Pages จะ deploy ใหม่อัตโนมัติ

## Deploy
โฮสต์เป็น static site บน **Cloudflare Pages** (เชื่อมกับ GitHub repo นี้)
- Build command: *(ไม่มี — static ล้วน)*
- Build output directory: `/` (root)
