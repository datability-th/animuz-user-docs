---
sidebar_position: 13
sidebar_label: How to embed on Shopify
title: How to embed on Shopify
---

# How to embed on Shopify 🛍️

ติดตั้ง **Rudi Chat Embed** ในร้านค้า **Shopify** ของคุณ เพื่อช่วยตอบลูกค้าและปิดการขายได้อัตโนมัติ 💬

### วิธีการติดตั้ง Rudi Chat Embed ในร้านค้าของ Shopify 🟡

1. ไปที่เมนู **Online Store > Themes** และหาปุ่ม **Edit theme**

![Online Store Themes](../static/img/Shopify/01-online-store-themes.png)

---

2. เมื่อเข้ามาในหน้า Edit theme แล้ว ให้มองไปที่แถบด้านบน ". . ." และกด **Edit code**

![Edit Code](../static/img/Shopify/02-edit-code.png)

---

3. ทำการหาไฟล์ที่มีชื่อว่า **theme.liquid** หาได้ 2 วิธี

![Find theme.liquid](../static/img/Shopify/03-find-theme-liquid.png)

    3.1 แถบด้านซ้าย **layout > theme.liquid**

![Layout theme.liquid](../static/img/Shopify/03-1-layout-theme-liquid.png)

    3.2 ค้นหาจากช่องด้านบน

![Search theme.liquid](../static/img/Shopify/03-2-search-theme-liquid.png)

---

4. เมื่อเปิดไฟล์ **theme.liquid** แล้ว ให้เลื่อนลงมีที่ล่างสุด :point_right: ทำการวาง script ให้อยู่เหนือ tag `</body>`, แล้วทำการกด **Save** ในกรอบ วงกลมสีฟ้า

![Paste Script Before Body Tag](../static/img/Shopify/04-paste-script-before-body-tag.png)

---

5. เมื่อทำการ Preview หน้าร้านค้าของเรา จะเห็นว่า Rudi Chat Embed มาแล้วเป็นที่เรียบร้อย

![Preview Rudi Chat Embed](../static/img/Shopify/05-preview-rudi-chat-embed.png)

:::info["เสริม"]
หากว่าหลังจากนี้ ร้านค้า จะต้องทำการเข้ามาแก้ไขเนื้อหาใดๆ จะพบกับ Rudi Chat Embed แสดงขึ้นมาเสมอ (หากไม่อยากพบเจอ) จะต้องทำการ ปิด Tag ชั่วคราวก่อน

จะเห็นตามภาพว่า Rudi Chat Embed แสดงขึ้นมาเสมอ ให้ทำการ ให้มองไปที่แถบด้านบน ". . ." และกด **Edit code**
ทำการใส่ Tag ปิด code script ชั่วคราวก่อน (ตามภาพ) หรือทำการเอาเมาส์คลุม code script ทั้งหมดและกด ( `ctrl` + `/` ) ระบบจะ ปิดให้อัตโนมัติ
และทำการ กด **Save**
:::
