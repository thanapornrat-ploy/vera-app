# คู่มือติดตั้ง Vera ขึ้นมือถือ (ทำครั้งเดียว)

คู่มือนี้พาไปทีละขั้น ไม่ต้องรู้เรื่องเทคนิคมาก่อน ใช้เวลารวมประมาณ 15-20 นาที
**สำคัญ: ทั้งหมดนี้ใช้บัญชีส่วนตัวของพี่ปุ้ยเท่านั้น ห้ามใช้บัญชี/อีเมลบริษัทเด็ดขาด** เพราะ Vera เป็นแอพส่วนตัว

---

## ขั้นที่ 1 — สร้างบัญชี GitHub ส่วนตัว (ถ้ายังไม่มี)

1. เปิด https://github.com/signup
2. สมัครด้วย **อีเมลส่วนตัว** ของพี่ปุ้ย (ไม่ใช่อีเมลบริษัท)
3. ตั้งชื่อผู้ใช้ (username) อะไรก็ได้ที่ชอบ จำไว้ใช้ตอนหลัง

ถ้ามีบัญชี GitHub ส่วนตัวอยู่แล้ว ข้ามไปขั้นที่ 2 ได้เลย

---

## ขั้นที่ 2 — สร้าง Repository ใหม่บน GitHub

1. ล็อกอิน GitHub แล้วกดปุ่ม **"+"** มุมขวาบน → **"New repository"**
2. ตั้งชื่อ repo เช่น `vera-app`
3. เลือก **Private** (ส่วนตัว มองเห็นได้แค่พี่ปุ้ยคนเดียว) — แนะนำ เพราะมีข้อมูลส่วนตัวเกี่ยวข้อง
4. **ไม่ต้อง** ติ๊กเพิ่มไฟล์ README/gitignore/license ใด ๆ (ปล่อยว่างไว้ ไฟล์ในเครื่องมีครบแล้ว)
5. กด **Create repository**
6. หน้าที่ขึ้นมาจะมีลิงก์ repo หน้าตาประมาณ `https://github.com/<username>/vera-app.git` — เก็บไว้ใช้ขั้นถัดไป

---

## ขั้นที่ 3 — เชื่อมเครื่องกับบัญชี GitHub ส่วนตัว

เครื่องนี้มีโปรแกรมชื่อ `gh` (GitHub CLI) ติดตั้งอยู่แล้ว ใช้ล็อกอินได้เลยโดยไม่ต้องติดตั้งอะไรเพิ่ม

เปิด PowerShell หรือ Terminal แล้วพิมพ์:

```bash
gh auth login
```

ตอบคำถามตามนี้:
- **What account do you want to log into?** → เลือก `GitHub.com`
- **What is your preferred protocol?** → เลือก `HTTPS`
- **Authenticate Git with your GitHub credentials?** → เลือก `Yes`
- **How would you like to authenticate?** → เลือก `Login with a web browser`

จะมีโค้ดขึ้นมาให้ copy แล้วเบราว์เซอร์จะเปิดเอง ให้ **ล็อกอินด้วยบัญชี GitHub ส่วนตัว** ที่สร้างไว้ขั้นที่ 1 แล้วกดยืนยัน

---

## ขั้นที่ 4 — Push โค้ดขึ้น GitHub

โค้ดของ Vera เตรียมพร้อม (git init + commit แรกไว้แล้ว) ในเครื่องนี้ที่โฟลเดอร์ `better-self-app/`

พอทำขั้นที่ 3 เสร็จ (ล็อกอิน gh ด้วยบัญชีส่วนตัวแล้ว) ให้บอก Claude ว่า **"push ได้แล้ว"** เดี๋ยว Claude จะรัน:

```bash
git remote add origin https://github.com/<username>/vera-app.git
git push -u origin master
```

(แทน `<username>` ด้วยชื่อผู้ใช้ GitHub ส่วนตัวจริงของพี่ปุ้ย)

---

## ขั้นที่ 5 — เชื่อมกับ Cloudflare Pages

1. เปิด https://dash.cloudflare.com/ แล้วสมัคร/ล็อกอิน (ใช้อีเมลส่วนตัวได้เช่นกัน)
2. เมนูซ้าย เลือก **Workers & Pages** → กด **Create** → แท็บ **Pages** → **Connect to Git**
3. เลือก **GitHub** แล้วอนุญาตให้ Cloudflare เข้าถึง repo `vera-app` ที่สร้างไว้
4. เลือก repo `vera-app` แล้วกด **Begin setup**
5. ตั้งค่า build (Vera เป็นเว็บล้วน ๆ ไม่ต้อง build อะไรเลย):
   - **Framework preset**: `None`
   - **Build command**: เว้นว่างไว้
   - **Build output directory**: `/` (ค่าเริ่มต้น)
6. กด **Save and Deploy** — รอประมาณ 1 นาที
7. เสร็จแล้วจะได้ลิงก์ประมาณ `https://vera-app-xxx.pages.dev`

---

## ขั้นที่ 6 — เปิดใช้งานบนมือถือ

1. เปิดลิงก์จากขั้นที่ 5 บนมือถือ (ถ้าจำยาก ตั้งค่าใน Cloudflare ให้เป็นโดเมนที่จำง่ายกว่าได้ในภายหลัง)
2. เข้าไปที่หน้า `app.html` โดยตรง (เช่น `https://vera-app-xxx.pages.dev/app.html`) — นี่คือแอพจริงที่ใช้งาน (ส่วน `index.html` เป็นแค่หน้าโชว์ตัวอย่าง)
3. เพิ่มไปหน้าจอโฮม ตามวิธีในแอพ (หน้าตั้งค่า ⚙️ → "วิธีเพิ่มแอพไปหน้าจอโฮม")
4. ทดสอบ: ตั้งเป้าหมาย → เช็คอิน → ดูสรุปเดือน → ดูสรุปอารมณ์ → บันทึกรายจ่าย ให้ครบทุกเมนู

จากนี้ไปทุกครั้งที่ Claude แก้โค้ดแล้ว push ขึ้น GitHub ใหม่ Cloudflare Pages จะ deploy เวอร์ชันใหม่ให้อัตโนมัติ ไม่ต้องทำอะไรเพิ่ม

---

## ถ้าติดปัญหา
- **ลืมรหัส/เข้า GitHub ไม่ได้**: กู้คืนผ่าน https://github.com/password_reset
- **push แล้ว error "remote already exists"**: บอก Claude ให้ตรวจ `git remote -v` ก่อน push ใหม่
- **หน้าเว็บไม่อัพเดทหลัง push**: เข้า Cloudflare Pages dashboard → ดูแท็บ Deployments ว่ามีรอบใหม่วิ่งอยู่ไหม
