# วิธีเอาขึ้น GitHub และเปิดใช้งานจริง

## ตอบก่อนเลย: ไม่ต้องวางทั้งหมด

ขึ้นอยู่กับว่าพี่จะเอาไปทำอะไร

| จะทำอะไร | ต้องวางอะไร |
|---|---|
| แค่อยากให้ทีมเปิดใช้ผ่านลิงก์ | โฟลเดอร์ `deploy/` อย่างเดียว |
| อยากแก้ธีม / เพิ่มโมดูลในอนาคตด้วย | `deploy/` + ไฟล์ต้นฉบับ (ดูตารางล่าง) |
| ไม่อยากใช้ GitHub เลย | ส่งไฟล์ `HRM-Trouble-Report.html` ไฟล์เดียวทาง LINE ก็จบ |

`index.html` ในโฟลเดอร์ `deploy/` เป็นไฟล์เดียวจบจริง ๆ — ธีม โค้ดทั้ง 4 โมดูล และไอคอน
ถูกฝังอยู่ข้างในหมดแล้ว มีแค่ฟอนต์ Kanit ที่โหลดจาก Google

---

## แบบเร็วที่สุด — ไม่ต้องลง git ไม่ต้องใช้ command line

**1. สร้าง repository**

เข้า github.com → กดปุ่ม **+** มุมขวาบน → **New repository**

- Repository name: `hrm-report` (หรือชื่ออื่นที่ชอบ)
- เลือก **Public**
- ติ๊ก **Add a README file**
- กด **Create repository**

**2. อัปโหลดไฟล์**

ในหน้า repo กด **Add file** → **Upload files**
ลากไฟล์ **ทุกไฟล์ที่อยู่ในโฟลเดอร์ `deploy/`** เข้าไป (ลากไฟล์ ไม่ใช่ลากโฟลเดอร์)

```
index.html          ← แอปรวม ไฟล์นี้สำคัญที่สุด
manifest.json
apple-touch-icon.png
icon-192.png
icon-512.png
favicon-64.png
report.html  stop.html  qic.html  backup.html   ← ถ้าอยากมีลิงก์แยกรายโมดูล
```

กด **Commit changes**

**3. เปิด GitHub Pages**

ในหน้า repo กด **Settings** → เมนูซ้าย **Pages**

- Source: **Deploy from a branch**
- Branch: **main** / โฟลเดอร์ **/ (root)**
- กด **Save**

รอประมาณ 1–2 นาที แล้วรีเฟรชหน้า Pages จะเห็นลิงก์

```
https://ชื่อบัญชีของพี่.github.io/hrm-report/
```

**4. ติดตั้งลงมือถือ**

เปิดลิงก์บนมือถือ

- **iPhone (Safari)** → กดปุ่มแชร์ → Add to Home Screen
- **Android (Chrome)** → เมนูสามจุด → Add to Home screen / ติดตั้งแอป

จะได้ไอคอน HRM บนหน้าจอ เปิดแล้วเต็มจอ ไม่มีแถบเบราว์เซอร์ เหมือนแอปจริง

ส่งลิงก์นี้ให้ทีมได้เลย ทุกคนติดตั้งเองได้

---

## ถ้าอยากแก้ธีมต่อในอนาคต ให้วางไฟล์ต้นฉบับด้วย

วางเพิ่มจากข้างบนอีก 12 ไฟล์นี้

```
report.html  STOP.html  QIC.html  BACKUP.html    ← โค้ดเดิม 4 โมดูล
uacj-theme.css                                    ← ธีมปัจจุบัน
uacj-theme-v1-blue.css                            ← ธีมน้ำเงินสำรอง
shell-template.html                               ← เปลือกแอป
apply-theme.py  build-app.py  make-icon.py        ← สคริปต์
icon.html  icon-source.png                        ← ต้นแบบไอคอน
README.md
.gitignore
```

**ไม่ต้องวาง** — สร้างใหม่ได้ตลอดจากสคริปต์

```
themed/                      ← สร้างจาก apply-theme.py
HRM-Trouble-Report.html      ← สร้างจาก build-app.py (ตัวเดียวกับ deploy/index.html)
App_icon-v1.png              ← ไอคอนเก่า 2 MB เก็บไว้ในเครื่องพอ
fntest.py fntest2.py shot.py ← สคริปต์ทดสอบ วางก็ได้ ไม่วางก็ได้
```

ไฟล์ `.gitignore` ที่ให้มาจัดการเรื่องนี้ให้แล้ว

### ขั้นตอนเวลาจะอัปเดตแอป

```bash
# แก้ uacj-theme.css หรือโค้ดโมดูล แล้ว
python3 apply-theme.py
python3 build-app.py
cp HRM-Trouble-Report.html deploy/index.html
```

แล้วอัปโหลด `deploy/index.html` ทับของเดิมบน GitHub (Add file → Upload files → ลากทับได้เลย)
Pages จะอัปเดตเองใน 1–2 นาที

---

## เรื่องที่ต้องรู้ก่อนตัดสินใจ

### 1. Public repo = ใครก็เห็นโค้ด

GitHub Pages แบบฟรีใช้ได้เฉพาะ repo ที่เป็น Public แปลว่า

- ใครก็เปิดลิงก์ได้ ถ้าเดาชื่อ repo ถูก
- ใครก็อ่านโค้ดข้างในได้ รวมถึงชื่อเครื่องจักร ตำแหน่ง Alloy รูปแบบข้อความที่ส่งเข้า Line กลุ่ม

ถ้าข้อมูลพวกนี้เป็นเรื่องภายในบริษัท ควรถามหัวหน้าหรือฝ่าย IT ก่อน
ทางเลือกที่ปลอดภัยกว่า:

- **Private repo + GitHub Pro** (ประมาณ 4 USD/เดือน) เปิด Pages บน repo ส่วนตัวได้
- **ไม่ใช้ GitHub เลย** ส่งไฟล์ `HRM-Trouble-Report.html` ทาง LINE ให้แต่ละคนเซฟลงเครื่อง ใช้งานได้เหมือนกันทุกอย่าง แค่ต้องส่งใหม่ทุกครั้งที่อัปเดต
- **วางบนเซิร์ฟเวอร์ภายในของโรงงาน** ถ้ามี

### 2. ประวัติที่บันทึกไว้จะไม่ตามไปด้วย

ทุกโมดูลเก็บประวัติไว้ใน localStorage ของเบราว์เซอร์ ซึ่งผูกกับที่อยู่ของไฟล์
ถ้าตอนนี้พี่เปิดไฟล์จากในเครื่องแล้วมีประวัติเก่าอยู่ พอย้ายไปเปิดผ่านลิงก์ GitHub
มันจะเริ่มนับใหม่จากศูนย์ ประวัติเก่ายังอยู่ที่เดิม ไม่ได้หาย แต่คนละที่กัน

ถ้ามีข้อมูลสำคัญค้างอยู่ ให้ copy เก็บไว้ก่อนย้าย

### 3. เปิดผ่าน https ดีกว่าเปิดไฟล์ตรง ๆ

ปุ่ม Copy ข้อความใช้ Clipboard API ซึ่งบางเบราว์เซอร์บล็อกเมื่อเปิดแบบ `file://`
พอขึ้น GitHub Pages เป็น https แล้วจะทำงานได้เสถียรกว่า

### 4. ครั้งแรกต้องมีเน็ต

ฟอนต์ Kanit โหลดจาก Google ครั้งแรก หลังจากนั้นเบราว์เซอร์แคชไว้แล้วใช้ออฟไลน์ได้
ถ้าอยากให้ออฟไลน์ 100% ตั้งแต่ครั้งแรก ต้องฝังฟอนต์ลงไฟล์ด้วย (ทำให้ไฟล์ใหญ่ขึ้นประมาณ 300 KB)
บอกได้ถ้าอยากให้ทำ

---

## ถ้าถนัด command line

```bash
cd โฟลเดอร์นี้
git init
git add .
git commit -m "HRM Trouble Report"
git branch -M main
git remote add origin https://github.com/ชื่อบัญชี/hrm-report.git
git push -u origin main
```

แล้วไปเปิด Pages ตามขั้นตอนที่ 3 ข้างบน โดยเลือก branch `main` โฟลเดอร์ `/docs`
หรือถ้าอยากใช้ `/ (root)` ให้ย้ายไฟล์ใน `deploy/` ขึ้นมาไว้ระดับบนสุดแทน

อัปเดตครั้งต่อไป

```bash
python3 apply-theme.py && python3 build-app.py
cp HRM-Trouble-Report.html deploy/index.html
git add . && git commit -m "อัปเดตธีม" && git push
```

---

## เช็กลิสต์หลังขึ้นเสร็จ

- เปิดลิงก์บนมือถือแล้วขึ้นหน้า Home พร้อมไอคอน 4 ปุ่ม
- กดเข้าแต่ละโมดูลได้ครบทั้ง 4
- กรอกข้อมูล แล้วกดปุ่ม Copy ข้อความ → ไปวางใน LINE ได้
- Add to Home Screen แล้วไอคอน HRM ขึ้นถูกต้อง
- เปิดจากไอคอนแล้วไม่มีแถบเบราว์เซอร์
