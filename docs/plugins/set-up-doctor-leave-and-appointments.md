---
sidebar_position: 10
sidebar_label: Set up Doctor Leave + Appointments (EN)
title: Set up Doctor Leave and Appointments
---

# Set up Doctor Leave and Appointments

Goal: your assistant answers **"When is Dr. X free?"** from your clinic-hours file and your doctor-leave feed.
Time: about 20 minutes. Who: a team admin. ภาษาไทย: [ตั้งค่า Doctor Leave + Appointments](./set-up-doctor-leave-and-appointments-th.md)

:::note Preview
The **Plugins** menu is still rolling out. If you do not see the bag icon in the left bar, ask Datability to switch it on for your team.
Screenshots below are from a demo team with made-up data.
:::

## Before you start

| You need | Where it comes from |
|---|---|
| Doctor Leave and Appointments **shared with your team** | Datability shares them (both are private plugins, not in the public catalogue) |
| Your doctor schedule file (`.xlsx` or `.csv`) | Your hospital, in the [format below](#2-prepare-the-doctor-schedule-file) |
| The doctor-feed token | Your hospital IT (the owner of the leave feed) |
| An assistant that already exists | Create it in **My Rudi** |

## 1. Sign in and pick your team

Sign in to the Rudi dashboard and choose your team in the top bar. Everything below happens inside that team. Open **Plugins** (bag icon, left bar).

![Step 1](../../static/img/plugins/self-serve/01-marketplace.png)

## 2. Prepare the doctor schedule file

One row per doctor. Row 1 must be the headers, **spelled exactly** like this (other columns are ignored):

| รายชื่อแพทย์ | แผนกการรักษา | ตารางเวลาออกตรวจ |
|---|---|---|
| นพ.สมศักดิ์ ทดสอบวงศ์ | อายุรกรรม | วันจันทร์ \| 09.00-12.00 น. \| วันพุธ \| 13.00-16.00 น. |
| พญ.วิไล ตัวอย่างดี | กุมารเวชกรรม | วันอังคาร 08.00-12.00 น. ⏎ วันศุกร์ 08.00-12.00 น. |
| ทพ.ประเสริฐ สมมติ | ทันตกรรม | ทันตกรรมวันพุธ \| 09.00-18.00 น. \| ทันตกรรมวันพฤหัสบดี \| 09.00-18.00 น. |

(All names are invented. ⏎ = a line break inside the cell.)

Rules the system follows:

- **Doctor name** (`รายชื่อแพทย์`): titles such as นพ. พญ. ทพ. ทพญ. นายแพทย์ แพทย์หญิง Dr. are ignored when matching. The name must match your leave feed's name once titles are removed.
- **Schedule** (`ตารางเวลาออกตรวจ`): **Thai weekday names** (จันทร์ อังคาร พุธ พฤหัสบดี ศุกร์ เสาร์ อาทิตย์), each followed by a time range `HH.MM-HH.MM` (`:` also works). Separate with `|`, `,`, `/` or a new line. Several ranges per day are fine. Add `(สัปดาห์ที่ 1 3 5)` to limit a line to those weeks of the month.
- **Department**: written after the time, in front of the weekday (`ทันตกรรมวันพุธ`), or taken from `แผนกการรักษา`.
- A row with an empty name or empty schedule is skipped. A schedule written in English weekdays is **not** read.
- `.xlsx`: only the **first sheet** is read. `.csv`: UTF-8.

Upload the file in **Feed Datasource** (step 1 of your assistant) and note its path: folder name, then file name, e.g. `doctors/schedule.xlsx`.

## 3. Add Doctor Leave

1. Open **Plugins → Marketplace**. Under **Shared with you**, open **Kasemrad Doctor Leave** (called Doctor Leave below). It reads Kasemrad's leave feed; another hospital needs its own leave plugin from Datability.
2. Read the **Tools** list: `get_doctor_leave` is read-only (it only reads leave dates).
3. Paste the **Doctor feed token** from your hospital IT. It is stored encrypted and never shown again.
4. Under **Use in**, tick the assistant(s) that should use it. **All assistants** also covers assistants you create later (**Also new assistants**).
5. Click **Add**. If the token is wrong the page shows **Couldn't connect** with the reason; nothing is saved.

![Step 3](../../static/img/plugins/self-serve/02-add-doctor-leave.png)

Leave data is refreshed twice a day (06:00 and 18:00, Bangkok time), so a leave entered now can take up to 12 hours to appear.

## 4. Add Appointments

1. Open **Appointments** under **Shared with you**. Its tools:
   - `get_doctor_schedule` (Read only): free windows for a date range (max 31 days), per doctor or department.
   - `get_my_bookings` (Read only): this chat's own booking requests.
   - `request_booking` (**Mutation**): sends a **pending** request to the hospital. It never confirms a booking; staff confirm afterwards.
2. Fill the two fields (see [Known limitations](#known-limitations-today)):
   - **Clinic hours file**: the path from step 2, e.g. `doctors/schedule.xlsx`.
   - **Doctor leave source**: `<plugin id>.get_doctor_leave`. The plugin id is the last part of the Doctor Leave page address, `…/plugins/detail/<plugin id>`.
3. Choose **Use in**, then **Add**.

![Step 4](../../static/img/plugins/self-serve/03-add-appointments.png)

## 5. Wait for Datability to verify

Appointments is a hosted plugin: after **Add** it shows **Waiting for verification** and its tools stay off. Datability finishes a one-time secure setup, then you press **Check again**. Once it passes, the badge changes to **Connected**.

![Step 5](../../static/img/plugins/self-serve/06-pending-verification.png)

Check both plugins any time in **Plugins → My plugins** (version, health, which assistants use them).

![My plugins](../../static/img/plugins/self-serve/05-my-plugins.png)

## 6. Turn them on for the assistant

Open your assistant → **Step 3: Play & Design** → **Equipped tools → Plugins**. Switch **Doctor Leave** and **Appointments** on. Click a plugin's name to see its tools and risk.

![Step 6](../../static/img/plugins/self-serve/07-step3-plugins.png)

- The switch is per plugin. Individual tools cannot be turned off here, so `request_booking` comes with Appointments.
- In the **Custom Prompt**, add instructions in plain words, e.g. *"Never say a booking is confirmed. Tell the patient the hospital will confirm."* Typing `@` only lists built-in tools (RAG, CSV, Web Search) today; plugin tools are used automatically when relevant.

![Step 6b](../../static/img/plugins/self-serve/08-step3-at-menu.png)

## 7. Test in the Playground

In Step 3, use the **Playground** tab on the right. Try:

| Ask | Expect |
|---|---|
| "Is Dr. Wilai free this Friday?" / "พญ.วิไลว่างวันศุกร์นี้ไหม" | Available windows, e.g. 08:00-12:00 |
| "What is on for the Dentistry department next week?" | Windows per dentist |
| "Is Dr. Somsak working on Wednesday 14 October?" | On leave if the feed lists it; the answer separates *no clinic that day* from *on leave* |
| "Book Dr. Wilai on Friday morning" | Asks for the patient's ID number, then says the request was **received** and the hospital will confirm |
| "What are my bookings?" | Lists this chat's requests as pending/confirmed/rejected |

Only the last 4 characters of an ID number are stored. Use test data, not a real patient's.

## Troubleshooting

| You see | Meaning | Fix |
|---|---|---|
| **Couldn't connect** when adding | The upstream rejected the token | Re-check the token with hospital IT |
| **Waiting for verification** for long | Datability has not set the secure key yet | Send the support details below |
| Plugin badge **Down** (red) | Token rejected or service stopped; tools are hidden | Press the red button on the plugin page and re-enter the connection |
| **Blocked** | Datability recalled that version | Upgrade as the banner says |
| "Appointments is not configured" | Clinic hours file or leave source field is empty | Fill both fields |
| "could not fetch clinic hours" | Wrong path, or the file is not in your documents | Re-check the path in Feed Datasource |
| "could not fetch doctor leave" | Doctor Leave is not added, is Down, or the id is wrong | Check **My plugins**; re-enter `<plugin id>.get_doctor_leave` |
| "No doctor matching … Did you mean" | Name differs from the file | Use a suggested name or fix the file |
| "date range too long" | More than 31 days | Ask for a shorter range |
| Everyone shows *no clinic hours* | Schedule text not understood (e.g. English weekdays) | Rewrite in the [format above](#2-prepare-the-doctor-schedule-file) |
| Doctor on leave still shown free | Leave feed is cached up to 12 hours; or names differ | Wait for 06:00/18:00; compare the names |

## Known limitations (today)

- No file or tool **picker** yet: the two Appointments fields are plain text boxes (paths and ids typed by hand).
- Plugins cannot be added from a public catalogue; Datability shares them first.
- Appointments needs Datability's one-time verification.
- Tools cannot be switched off one by one; `@` does not list plugin tools.

## If you are stuck, send Datability

1. Team name and assistant name.
2. Plugin name and the address of its page in **My plugins**.
3. The exact message, the time it happened, and a screenshot.
4. The file name and path (never patient data).
5. What you asked in the Playground and what it answered.

Send it to your Datability contact.
