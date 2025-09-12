---
sidebar_position: 10
---

# How to embed on SharePoint Site

## ขั้นตอนการฝังตัว (Embed) Rudi ลงในเว็บไซต์ SharePoint

### 1. เริ่มต้นสร้างเว็บไซต์ใน SharePoint

![Create Site SharePoint](../static/img/SharePoint/create_site_SharePoint.png)

### 2. ตั้งค่า Custom Script

เลือกไซต์ที่ต้องการใช้งาน จากนั้นเข้าสู่หน้าการตั้งค่า

![Site](../static/img/SharePoint/setting_site.png)

- เปิดใช้งาน สคริปต์แบบกำหนดเอง (Custom Script)

![Custom Script](../static/img/SharePoint/custom_script.png)

### 3. ตั้งค่า Custom Script อีกครั้ง

เข้าสู่หน้าการตั้งค่าขั้นสูงโดยใช้ URL ต่อไปนี้:  
`https://[Tenant_name].sharepoint.com/sites/[Site_name]/_layouts/15/HtmlFieldSecurity.aspx`

โดยนำ **Tenant name** และ **Site name** จาก URL ปัจจุบันของคุณมาแทนที่ เช่น:

https://databilitycoltd.sharepoint.com/sites/rudi-test-02/SitePages/CollabHome.aspx

- เลือกที่ Allow contributors to insert iframes from any domain.

![Custom Script HtmlFieldSecurity](../static/img/SharePoint/html_field_security.png)

:::success
เมื่อกำหนดค่าเรียบร้อยแล้ว สามารถปิดหน้านี้ได้ทันที
:::

### 4. เพิ่มเนื้อหาสำหรับ Embed ลงในหน้า SharePoint

- คลิก แก้ไข (Edit) ที่มุมขวาบนของหน้า SharePoint

![Edit](../static/img/SharePoint/edit.png)

- เลือก ฝังตัว (Embed) จากเมนูเครื่องมือ

![Embed](../static/img/SharePoint/embed.png)

- คัดลอกโค้ดสำหรับการฝังโค้ดจาก [Rudi Website](https://app.rudi.animuz.ai/app/rudi) แล้ววางลงในช่องสำหรับฝังโค้ด

![Embed Script](../static/img/embed_script.png)

![Code For Embed](../static/img/SharePoint/code.png)

:::info[วิดีโอแนะนำการ Embed บน SharePoint]

<iframe width="560" height="315" src="https://www.youtube.com/embed/xIAuhjdkV6M?si=41fKYMj7W3sWmT-1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
:::

:::success[]
ระบบจะแสดงผล Embed ตามตัวอย่างด้านล่าง :

![Embed Success](../static/img/SharePoint/embed_success.png)
:::
