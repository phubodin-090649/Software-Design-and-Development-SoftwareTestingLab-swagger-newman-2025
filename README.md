# Lab02A — API Documentation & Automation Testing
## Swagger/OpenAPI + Newman (Postman CLI)
### ระบบจองห้องพักออนไลน์ (Hotel Booking System)

> **วิชา:** การออกแบบและพัฒนาซอฟต์แวร์ (Software Design and Development)  
> **Repository:** `Software-Design-and-Development-Booking-App-Demo-2025`

[![Node.js](https://img.shields.io/badge/Node.js-22.x-339933?logo=node.js&logoColor=white)](https://nodejs.org)
[![Swagger](https://img.shields.io/badge/Swagger-OpenAPI_3.0-85EA2D?logo=swagger&logoColor=black)](https://swagger.io)
[![Newman](https://img.shields.io/badge/Newman-6.x-FF6C37?logo=postman&logoColor=white)](https://www.npmjs.com/package/newman)

---

## 📋 สารบัญ

- [ภาพรวม](#-ภาพรวม)
- [เครื่องมือที่ใช้](#-เครื่องมือที่ใช้)
- [ข้อกำหนดเบื้องต้น](#-ข้อกำหนดเบื้องต้น)
- [โครงสร้างไฟล์](#-โครงสร้างไฟล์)
- [การติดตั้งและรัน](#-การติดตั้งและรัน)
- [สรุปเนื้อหา](#-สรุปเนื้อหา)
- [ไฟล์ที่เกี่ยวข้อง](#-ไฟล์ที่เกี่ยวข้อง)

---

## 🎯 ภาพรวม

Lab02A สอนการสร้าง **API Documentation** อัตโนมัติด้วย Swagger/OpenAPI และการรัน **Automated Test** ผ่าน Newman (Postman CLI) โดยทดสอบกับ Hotel Booking API จริงที่รันอยู่บน `localhost:3001`

**API ที่ครอบคลุม:**

| Method | Endpoint | Auth | คำอธิบาย |
|--------|----------|:---:|---------|
| POST | `/api/login` | ❌ | เข้าสู่ระบบ รับ JWT Token |
| POST | `/api/bookings` | ❌ | สร้างการจองใหม่ |
| GET | `/api/bookings` | ✅ JWT | ดึงรายการจองทั้งหมด |
| GET | `/api/bookings/:id` | ✅ JWT | ดึงการจองตาม ID |
| PUT | `/api/bookings/:id` | ✅ JWT | แก้ไขการจอง |
| DELETE | `/api/bookings/:id` | ✅ JWT | ลบการจอง |

> **Admin:** username: `admin` / password: `admin123`

---

## 🛠 เครื่องมือที่ใช้

| เครื่องมือ | Package | หน้าที่ |
|-----------|---------|---------|
| **swagger-jsdoc** | `swagger-jsdoc` | อ่าน JSDoc Comment ใน `server.js` → สร้าง OpenAPI 3.0 Spec อัตโนมัติ |
| **swagger-ui-express** | `swagger-ui-express` | Serve Swagger UI ที่ `/api-docs` ให้ทดสอบ API ในเบราว์เซอร์ได้ |
| **Newman** | `newman` | รัน Postman Collection ผ่าน Command Line (CLI) |
| **newman-reporter-htmlextra** | `newman-reporter-htmlextra` | สร้าง HTML Report ของผล Newman |

---

## 📦 ข้อกำหนดเบื้องต้น

- Node.js v18 ขึ้นไป (แนะนำ v22 LTS)
- npm v9 ขึ้นไป
- Backend รันอยู่ที่ `http://localhost:3001` (ทำ Lab01 ก่อน)
- Git

---

## 📁 โครงสร้างไฟล์

```
Software-Design-and-Development-Booking-App-Demo-2025/
│
├── backend/
│   ├── server.js              ← เพิ่ม: swagger-jsdoc config + JSDoc comments
│   ├── database.js
│   ├── package.json           ← เพิ่ม: swagger-jsdoc, swagger-ui-express, newman
│   └── newman/
│       ├── hotel-booking-collection.json   ← Postman Collection (สร้างจาก script)
│       └── hotel-booking-env.json          ← Environment Variables ของ Newman
│
├── reports/
│   └── api-test-report.html   ← Newman HTML Report (สร้างอัตโนมัติเมื่อรัน)
│
└── create-newman-files.js     ← Script สร้าง newman/ files
```

---

## 🚀 การติดตั้งและรัน

### 1. ติดตั้ง Swagger packages

```bash
cd backend
npm install swagger-jsdoc swagger-ui-express
```

### 2. ตั้งค่า Swagger ใน `server.js`

เพิ่ม `swaggerJsdoc` และ `swaggerUi` ใน `server.js` ตามใบงาน แล้วรัน Backend:

```bash
npm run dev
```

เปิดเบราว์เซอร์ไปที่:

```
http://localhost:3001/api-docs
```

ควรเห็น Swagger UI พร้อม Endpoint ทั้ง 6 รายการ

### 3. ติดตั้ง Newman

```bash
cd backend
npm install --save-dev newman newman-reporter-htmlextra
```

ตรวจสอบ:

```bash
npx newman --version
```

### 4. สร้างไฟล์ Newman Collection

```bash
# รัน script สร้างไฟล์ Collection และ Environment
node create-newman-files.js
```

จะสร้างไฟล์ใหม่ 2 ไฟล์:
- `backend/newman/hotel-booking-collection.json`
- `backend/newman/hotel-booking-env.json`

### 5. รัน Newman

```bash
# รันพื้นฐาน (แสดงผลใน Terminal)
npx newman run backend/newman/hotel-booking-collection.json \
  -e backend/newman/hotel-booking-env.json

# รันพร้อมสร้าง HTML Report
npx newman run backend/newman/hotel-booking-collection.json \
  -e backend/newman/hotel-booking-env.json \
  -r cli,htmlextra \
  --reporter-htmlextra-export ./reports/api-test-report.html
```

เปิด `reports/api-test-report.html` ในเบราว์เซอร์เพื่อดูผลแบบ HTML

---

## 📖 สรุปเนื้อหา

### ส่วนที่ 1 — API Documentation ด้วย Swagger

**ทฤษฎีที่เรียน:**
- Swagger คืออะไร และวิวัฒนาการสู่ OpenAPI 3.0 Specification
- ประโยชน์ของ API Documentation (Single Source of Truth)
- โครงสร้าง OpenAPI Spec: `info`, `servers`, `paths`, `components/schemas`
- ความสัมพันธ์ระหว่าง Swagger UI ↔ Newman ↔ Postman

**กิจกรรมหลัก:**

| กิจกรรม | สิ่งที่ทำ |
|---------|---------|
| กิจกรรมที่ 1 | ตั้งค่า `swaggerOptions` และเพิ่ม JSDoc `@swagger` comment ให้ครบ 6 Endpoints |
| กิจกรรมที่ 2 | ทดลองใช้ Swagger UI — Authorize ด้วย JWT, ทดสอบ GET/POST/PUT/DELETE |
| แบบฝึกหัดที่ 1 | เพิ่ม Response Schema, `GET /api/health` endpoint, และ Token Expiry |

**ตัวอย่าง JSDoc Comment:**

```javascript
/**
 * @swagger
 * /api/login:
 *   post:
 *     summary: เข้าสู่ระบบและรับ JWT Token
 *     tags: [Authentication]
 *     requestBody:
 *       required: true
 *       content:
 *         application/json:
 *           schema:
 *             $ref: '#/components/schemas/LoginRequest'
 *     responses:
 *       200:
 *         description: Login สำเร็จ
 */
```

---

### ส่วนที่ 2 — API Automation ด้วย Newman

**ทฤษฎีที่เรียน:**
- สถาปัตยกรรม Newman: Collection → Runner → Reporter
- Chai Assertion Library: `pm.expect(val).to.have.status(200)`
- การส่งต่อข้อมูลระหว่าง Requests ด้วย `pm.environment.set()` / `pm.environment.get()`
- ความแตกต่าง `include.all.keys` vs `have.all.keys`

**Collection มี 8 Requests:**

| ลำดับ | Request | Test Script |
|-------|---------|------------|
| 1 | POST /api/login | ตรวจ status 200, บันทึก `token` ใน environment |
| 2 | POST /api/bookings | ตรวจ status 201, บันทึก `bookingId` |
| 3 | GET /api/bookings | ตรวจ status 200, ตรวจว่า response เป็น Array |
| 4 | GET /api/bookings (ไม่มี Token) | ตรวจ status 401 |
| 5 | GET /api/bookings/:id | ตรวจ status 200, id ตรงกัน |
| 6 | PUT /api/bookings/:id | ตรวจ status 200, fullname อัปเดตแล้ว |
| 7 | DELETE /api/bookings/:id | ตรวจ status 200 และ message |
| 8 | GET /api/bookings/:id (หลัง DELETE) | ตรวจ status 404 (ยืนยันลบจริง) |

**กิจกรรมหลัก:**

| กิจกรรม | สิ่งที่ทำ |
|---------|---------|
| กิจกรรมที่ 3 | รัน `create-newman-files.js` สร้าง Collection และ Environment |
| กิจกรรมที่ 4 | วิเคราะห์ Test Scripts ของแต่ละ Request |
| กิจกรรมที่ 5 | รัน Newman + สร้าง HTML Report |
| แบบฝึกหัดที่ 2 | แก้ไข Collection: เพิ่ม Assertion, ทดสอบ Error Cases, เพิ่ม Negative Test |

---

## 📂 ไฟล์ที่เกี่ยวข้อง

| ไฟล์ | คำอธิบาย |
|------|---------|
| `Lab02A_Swagger_Newman.md` | ใบงานหลัก — เนื้อหาทั้งหมด ทฤษฎี กิจกรรม แบบฝึกหัด |
| `create-newman-files.js` | Script Node.js สำหรับสร้าง Collection + Environment files |
| `backend/newman/hotel-booking-collection.json` | Postman Collection (สร้างจาก script) |
| `backend/newman/hotel-booking-env.json` | Newman Environment Variables |
| `reports/api-test-report.html` | Newman HTML Report (สร้างเมื่อรัน Newman) |

### ความสัมพันธ์ระหว่าง Lab

```
Lab01  →  Lab02A  →  Lab02B  →  Lab02C
Manual     Swagger     UI Auto    Security
Testing  + Newman    Robot/Cy   Performance
                     Playwright  Jest + CI/CD
```

Lab02A ต่อเนื่องจาก Lab01 (Backend ต้องรันอยู่) และเป็นพื้นฐานของ Lab02C ในส่วน Newman

---

## ❓ คำถามทบทวน (6 ข้อ)

1. Swagger UI และ Newman ต่างกันอย่างไร? ควรใช้เครื่องมือใดในสถานการณ์ใด?
2. `$ref: '#/components/schemas/Booking'` หมายความว่าอะไร?
3. Flag ใดที่ใช้รัน Newman ซ้ำ 5 รอบ?
4. ควรเขียน Swagger Documentation ก่อนหรือหลัง Code API?

*(คำตอบอยู่ในใบงาน `Lab02A_Swagger_Newman.md`)*

---

## 📊 เกณฑ์การให้คะแนน

| หัวข้อ | คะแนน |
|--------|:-----:|
| ติดตั้งและตั้งค่า Swagger | 5 |
| JSDoc Comment ครบทุก Endpoint | 10 |
| ทดลอง Swagger UI + Screenshot | 10 |
| แบบฝึกหัดที่ 1 (Schema, Health, Token Expiry) | 15 |
| สร้าง Newman Collection และรัน | 10 |
| เขียน `pm.test()` เพิ่มเติม | 10 |
| แบบฝึกหัดที่ 2 (แก้ข้อมูล, Error, Assertion, Negative) | 15 |
| คำถามทบทวน 4 ข้อ | 25 |
| **รวม** | **100** |

---

*วิชาการออกแบบและพัฒนาซอฟต์แวร์ — Software Design and Development*
