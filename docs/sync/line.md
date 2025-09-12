---
sidebar_position: 1
---

# LINE Official Account

### ขั้นตอนการสร้าง Provider บน LINE Developers

1. ไปที่เว็บไซต์ [LINE Developers](https://developers.line.biz)
2. คลิกปุ่ม Console

![Line Developers](../../static/img/LineOA/line_developers.png)

:::info["Note"]
หากยังไม่เคยใช้งานระบบ จะต้องกรอกข้อมูลLine Developers Console ก่อน

![Line Developers Console](../../static/img/LineOA/info_console.png)
:::

3. คลิก Create a new provider เพื่อสร้าง Provider ใหม่

![Create A New Provider](../../static/img/LineOA/create_new_provider.png)

- กรอกชื่อ Provider

![Set Name Provider](../../static/img/LineOA/provider_name.png)

4. เลือก Create a new channel แล้วเลือกประเภท Messaging API

![Create Channel](../../static/img/LineOA/create_channel.png)

:::info[]
หากยังไม่เคยสร้าง Channel มาก่อน ระบบจะให้เลือกประเภทของ Channel  
ให้เลือกเป็น Messaging API เพื่อใช้เชื่อมต่อกับแชทบอท

![Create A New Channel](../../static/img/LineOA/create_new_channel.png)
:::

1. คลิก Create LINE Official Account

![Create A New Channel](../../static/img/LineOA/create_line_oa.png)

### ขั้นตอนการสร้าง LINE Official Account

6. กรอกข้อมูลของ LINE Official Account ผ่านเว็บไซต์ [LINE Business](https://manager.line.biz/)

![Create Info Line OA](../../static/img/LineOA/create_info_line_oa.png)

![Success Create Line OA](../../static/img/LineOA/success_create_line_oa.png)

### ขั้นตอนการเชื่อมต่อ Provider กับ Line Official Account

7. กดเลือกบัญชี Line Official Account ที่สร้าง

![Select Line OA](../../static/img/LineOA/select_account.png)

คลิกที่ปุ่ม ตั้งค่า

![Select Setting Line OA](../../static/img/LineOA/select_setting.png)

8. ไปที่เมนู Messaging API แล้วคลิก ใช้งาน Messaging API

![Messaging API](../../static/img/LineOA/messaging_api.png)

9. เลือก Provider ที่สร้างไว้ก่อนหน้านี้

![Create Messaging API](../../static/img/LineOA/create_messaging_api.png)

10. เมื่อตั้งค่า Provider เสร็จแล้ว ระบบจะให้กรอกลิงก์ Webhook  
    หากคุณยังไม่มีระบบปลายทาง สามารถใส่ลิงก์ ตัวอย่างไปก่อน เช่น `https://example.com`  
    เพียงเพื่อให้สามารถเปิดใช้งาน Webhook ได้ในขั้นตอนถัดไป จากนั้นคลิก บันทึก

![Create Webhook](../../static/img/LineOA/create_webhook.png)

11. ไปที่เมนู ตั้งค่าการตอบกลับ แล้วคลิกเพื่อเปิดใช้งาน Webhook

![Open Webhook](../../static/img/LineOA/open_webhook.png)

### ขั้นตอนการสร้าง Access Token

12. กลับไปที่เว็บไซต์ LINE Developers Console แล้วเลือก Channel ที่สร้างไว้

![Select Your Channel](../../static/img/LineOA/select_channel.png)

13. ไปที่เมนู Messaging API

![Select Messaging API](../../static/img/LineOA/select_messaging_api.png)

14. เลื่อนลงไปด้านล่างสุด แล้วคลิกปุ่ม Issue เพื่อสร้าง Access Token

![Issue For Generate Access Token](../../static/img/LineOA/generate_access_token.png)

:::info[]
หลังจากคลิกแล้ว ระบบจะแสดง Access Token ขึ้นมา  
ให้คลิกปุ่มคัดลอก เพื่อเก็บรหัสนี้ไว้ใช้งาน

![Get Access Token](../../static/img/LineOA/coppied_access_token.png)  
:::

### ขั้นตอนการซิงค์กับ Rudi

15. ไปที่เว็บไซต์ [Rudi Website](https://app.rudi.animuz.ai/app/rudi)
16. ไปที่เมนู Sync

![Go To Sync Menu](../../static/img/go_sync.png)

![Sync Page](../../static/img/sync_page.png)

17. คลิกปุ่ม New Social Media

![New Social Media](../../static/img/new_social_media.png)

18. เลือก Assistant

![Select Assistant](../../static/img/select_assistant.png)

19. เลือก LINE Platform
20. วาง Access Token ลงในช่องที่กำหนด แล้วคลิก Connect to LINE

![Filled Access Token To Line Provider](../../static/img/LineOA/filled_access_token.png)

:::success
หากขั้นตอนถูกต้องทั้งหมด ระบบจะแสดงผลการเชื่อมต่อกับ Line Official Account ดังภาพ:
:::

![Connect with Line Official Account](../../static/img/LineOA/sync_result.png)

## คลิปวิดีโอ สำหรับการทำงานทั้งหมด

<iframe width="560" height="315" src="https://www.youtube.com/embed/aU_I4tSANyw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
