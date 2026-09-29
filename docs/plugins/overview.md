---
sidebar_position: 1
sidebar_label: Plugins คืออะไร
title: Plugins คืออะไร
---

# Plugins คืออะไร 🔌

**Plugin** คือชุดเครื่องมือ (tools) ที่ให้ Rudi เรียกใช้ได้ระหว่างสนทนา เช่น ค้นข้อมูลในเอกสาร ค้นเว็บ ส่งอีเมล หรือต่อเข้าระบบของบริษัทคุณเองอย่างการจองคิว ทุกเครื่องมือที่ Rudi เรียกได้มาจาก plugin เสมอ — ไม่มีทางอื่น

![Marketplace](../../static/img/plugins/marketplace.png)

## Plugin มี 4 แบบ

### 1. Built-in 🏠

มาจาก Rudi เอง เช่น **Knowledge base (RAG)**, **Data analysis (pandas)**, **Web search (Tavily)**, **Send email** — ติดตั้งให้ทุกทีมอัตโนมัติตั้งแต่แรก ไม่ต้องกด Add

### 2. Marketplace 🛒

Plugin ที่ทีมอื่นหรือ Rudi เผยแพร่เป็น public เช่น **Microsoft Learn** เปิดให้ทุกทีมกด Add เข้าไปใช้ได้เอง ดูวิธีค้นหาและเพิ่มได้ใน [ค้นหาและเพิ่ม Plugin](./find-and-add-a-plugin.md)

### 3. Shared with you 🤝

Plugin แบบ private ที่ทีมอื่น (เช่นผู้ให้บริการที่ทำระบบเชื่อมต่อให้คุณโดยเฉพาะ) แชร์มาให้ทีมคุณเท่านั้น จะขึ้นในหมวด **Shared with you** ด้านบนสุดของ Marketplace เสมอ อย่างตัวอย่าง Kasemrad HIS ด้านบน

### 4. ของทีมคุณเอง 🛠️

ทีมคุณสร้าง plugin เองได้ ทั้งแบบวางลิงก์ MCP/OpenAPI ตรง ๆ (ดู [เพิ่ม Custom connector](./custom-connector.md)) หรือเขียน manifest เต็มรูปแบบด้วย CLI (ดู [สำหรับนักพัฒนา: เผยแพร่ Plugin ของคุณเอง](./publish-your-own-plugin.md))

---

## หลังจาก Add แล้ว ใช้ยังไง

- **Add** พลักอินเข้าทีมครั้งเดียว แล้วเลือกว่าจะ **Use in** ผู้ช่วย (assistant) ตัวไหนบ้าง — ดูรายละเอียดใน [ค้นหาและเพิ่ม Plugin](./find-and-add-a-plugin.md)
- เข้าไปเปิด/ปิดเครื่องมือแยกทีละตัวต่อผู้ช่วยได้ทีหลังเสมอ — ดู [เลือกเครื่องมือให้แต่ละผู้ช่วย](./choose-tools-for-an-assistant.md)
- Plugin แต่ละตัวมีหน้าของตัวเอง แสดงสถานะการทำงาน (Healthy/Degraded/Down) และประวัติการเรียก — ดู [หน้า Plugin](./the-plugin-page.md)

:::info
Plugin ที่ทำงานกับข้อมูลจริง (นัดคิว สร้างเคส ส่งอีเมล ฯลฯ) จะให้ Rudi **ถามยืนยันกับลูกค้าในแชทก่อนทุกครั้ง** ไม่มีการยิงคำสั่งแบบเงียบ ๆ
:::
