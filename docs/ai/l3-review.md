# Review of docs/ai/l3-draft.md
Reviewers: M.Kamol Deeyeangyang and Naruechit Boonyoung. Checked against docs/intent/movie-filter.md and SMART.

| บรรทัดในร่าง | ปัญหา | ตัดสิน: แก้ ตัด หรือเก็บ |
|---|---|---|
| 11: "The system shall offer exactly five ratings to choose from: G, PG, PG-13, R and NC-17." (REQ-1) | ตรงกับ Constraints ใน intent ที่จำกัดค่า rating ไว้เพียง 5 ค่านี้เท่านั้น | เก็บ |
| 12: "When the user chooses one rating, the system shall show every movie in the catalog that has that rating." (REQ-2) | ตรงกับ Proposed outcome ที่ผู้ใช้สามารถเลือก rating เพื่อคัดกรองหนังเองได้ | เก็บ |
| 13: "The movies shown after choosing a rating shall not include any movie with a different rating." (REQ-3) | ตรงกับ Proposed outcome และชัดเจนในการทดสอบ (Testable) ว่าต้องไม่แสดงหนังเรตอื่นปนมา | เก็บ |
| 14: "The movies shown after choosing a rating shall be ordered by release date, newest first." (REQ-4) | ตรงกับ Proposed outcome ที่ให้เรียงลำดับผลลัพธ์การคัดกรองโดยเอาหนังใหม่ขึ้นก่อน | เก็บ |
| 15: "The full, unfiltered movie list shall also be ordered by release date, newest first." (REQ-5) | ลูกค้าไม่ได้ขอให้เรียงลำดับรายการทั้งหมดตอนที่ยังไม่ได้กรอง (ขอแค่เรียงรายการที่ผ่านการกรองแล้ว) | ตัด |
| 16: "When two movies have the same release date, they shall be ordered by title, A to Z, ignoring case." (REQ-6) | AI คิดกฎการเรียงชื่อเรื่อง (tie-break) ขึ้นมาเองโดยไม่มีใน intent | แก้: ตัดออกหรือนำไปถาม customer proxy ก่อน |
| 17: "The user shall be able to choose only one rating at a time." (REQ-7) | ไปตอบ Open question ข้อ 1 แทนลูกค้า (ลูกค้ายังไม่ตัดสินใจว่าจะเลือกทีละค่าหรือหลายค่าพร้อมกัน) | แก้: ถาม customer proxy |
| 18: "When no movie has the chosen rating, the system shall show an empty list and the message \"No movies with this rating\"." (REQ-8) | ไปตอบ Open question ข้อ 2 แทนลูกค้า และคิดข้อความแจ้งเตือนขึ้นมาเองทั้งที่ยังไม่มีหน้าเว็บ | แก้: ถาม customer proxy |
| 19: "A rating label that is not one of the five in REQ-1 (for example \"X\" or \"pg-13\") shall be rejected with an error that lists the five allowed ratings." (REQ-9) | ผู้ใช้เลือกจาก 5 ค่าที่มีอยู่ ไม่ได้พิมพ์ข้อความเอง ลูกค้าไม่ได้ขอให้จัดการกรณีพิมพ์ผิด | ตัด |
| 20: "Choosing a rating shall not change, add or remove any movie in the catalog." (REQ-10) | ตรงกับบริบทการอ่าน/คัดกรองข้อมูลใน MovieCatalog โดยไม่แก้ไขข้อมูลต้นฉบับ และทดสอบได้ | เก็บ |
| 21: "The filter shall respond within 1 second for a catalog of up to 10,000 movies." (REQ-11) | ตัวเลข 1 วินาที และ 10,000 เรื่องเป็นสิ่งที่ AI เดาขึ้นเอง ไม่มีใน intent หรือคำของลูกค้า | ตัด |
| 22: "The system shall not ask for a login and shall not store any personal data about the user." (REQ-12) | ตรงกับ Constraints ใน intent ที่ระบุว่าไม่ต้อง login และไม่เก็บข้อมูลส่วนตัว | เก็บ |
| 26-29: "Story 1: Filter movies by rating" | ตรงกับเป้าหมายหลักใน intent ในมุมมองผู้ปกครองที่ต้องการคัดกรองหนังตาม rating เอง | เก็บ |
| 32: "AC-1.1: Choosing PG-13 shows every PG-13 movie in the catalog and no movie of any other rating." | ตรงกับ REQ-2, REQ-3 และตรวจสอบพฤติกรรมผ่าน domain classes ได้ชัดเจน | เก็บ |
| 33: "AC-1.2: The movies shown are ordered newest first." | ตรงกับ Proposed outcome ใน intent ที่ต้องการให้แสดงหนังใหม่ล่าสุดก่อนหลังกรอง | เก็บ |
| 34: "AC-1.3: Choosing a rating that no movie has shows an empty list and the message \"No movies with this rating\"." | ตอบ Open question ข้อ 2 แทนลูกค้า และสมมติข้อความ UI ขึ้นมาเองทั้งที่ยังไม่มีหน้าเว็บ | แก้: ถาม customer proxy |
| 35: "AC-1.4: Only one rating can be chosen at a time." | ตอบ Open question ข้อ 1 แทนลูกค้า (ยังไม่ยืนยันว่าเลือกได้ทีละ 1 ค่าหรือหลายค่า) | แก้: ถาม customer proxy |
| 37-40: "Story 2: See the newest movies first" | แยกการเรียงลำดับออกมาเป็น Story ใหม่สำหรับรายการทั้งหมด ทั้งที่ใน intent การเรียงหนังใหม่เป็นเงื่อนไขของผลลัพธ์หลังกรอง | แก้: ย้ายไปรวมใน Story 1 และตัดเรื่องเรียงรายการทั้งหมดออก |
| 43: "AC-2.1: In the full list, a movie released in 2011 appears before one released in 1992." | อ้างอิงถึงรายการทั้งหมด (full list) ซึ่งลูกค้าไม่ได้ขอให้เรียงลำดับ | แก้: ปรับบริบทให้เป็นการเรียงเฉพาะในรายการที่ผ่านการกรอง rating แล้ว |
| 44: "AC-2.2: Movies released on the same date appear in title order, A to Z." | เป็นกฎ tie-break ที่ AI คิดขึ้นเอง ไม่มีใน intent | แก้: ตัดออกหรือนำไปถาม customer proxy |
| 46-53: "Story 3: Save a preferred rating" (รวม AC-3.1 และ AC-3.2) | ขัดแย้งกับ Constraints ใน intent โดยตรงที่ห้ามมีการ login และห้ามเก็บข้อมูลส่วนตัวของผู้ใช้ | ตัด |
| 71-75: "Scenario: Happy path - only the chosen rating is shown, newest first" | ทดสอบการคัดกรองตาม rating และการเรียงลำดับหนังใหม่ก่อนได้ตรงตาม intent | เก็บ |
| 77-80: "Scenario: Sad path - no movie has the chosen rating" | ทดสอบกรณีไม่พบหนังโดยกำหนดข้อความแจ้งเตือนเองซึ่งเป็นการตอบ Open question ข้อ 2 แทนลูกค้า | แก้: ถาม customer proxy เรื่องผลลัพธ์และข้อความเมื่อไม่พบหนัง |
| 82-84: "Scenario: Sad path - unknown rating label" | ทดสอบกรณีพิมพ์ rating ผิด ("X") ซึ่งอ้างอิงจาก REQ-9 ที่ลูกค้าไม่ได้ขอ | ตัด |
| 90: "1. REQ-6 and AC-2.2: the tie-break by title for equal release dates is my own choice." | ยอมรับเองว่าคิดเงื่อนไขเพิ่มนอกเหนือจาก intent (ขัดกับคำสั่ง Keep to what the intent asks for) | แก้: นำไปถาม customer proxy แทนการกำหนดเอง |
| 91: "2. REQ-7 and AC-1.4: one rating at a time is assumed; the intent lists this as an open question." | AI ตัดสินใจเลือกคำตอบให้ Open question ข้อ 1 แทนลูกค้าเอง | แก้: ปล่อยเป็นคำถามให้ customer proxy ตัดสินใจ |
| 92: "3. REQ-8 and AC-1.3: the empty list and the exact message text are assumed; the intent lists this as an open question." | AI ตัดสินใจเลือกคำตอบให้ Open question ข้อ 2 แทนลูกค้าเอง | แก้: ปล่อยเป็นคำถามให้ customer proxy ตัดสินใจ |
| 93: "4. REQ-9: users might type a label, so invalid labels are handled; the intent does not say how a rating is chosen." | กลุ่มค่า rating ถูกจำกัดตายตัว 5 ค่าตาม Constraints อยู่แล้ว การสมมติว่าผู้ใช้พิมพ์เองทำให้เกิด requirement เกินจำเป็น | ตัด |
| 94: "5. REQ-11: the 1-second limit and the 10,000-movie size are guesses; the intent gives no performance target." | ยอมรับเองว่าเดาตัวเลขประสิทธิภาพขึ้นเองโดยไม่มีใน intent | ตัด |
| 95: "6. Story 3 assumes the site can remember a preference between visits." | สมมติฐานนี้ขัดแย้งกับ Constraints เรื่องการไม่เก็บข้อมูลส่วนตัวและไม่ใช้ฐานข้อมูล | ตัด |
| 96: "7. \"Newest\" is taken to mean the release date, not the date the movie was added to the catalog." | เป็นสมมติฐานที่สมเหตุสมผลและสอดคล้องกับฟิลด์ข้อมูลภาพยนตร์ที่มีอยู่ แต่ควรยืนยันความเข้าใจให้ตรงกัน | เก็บ (และยืนยันกับ customer proxy) |
| 99: "1. Story 3 needs some way to remember the user, which conflicts with the constraint of no login and no personal data. It should be confirmed or dropped." | วิเคราะห์ข้อขัดแย้งได้ถูกต้อง แต่เมื่อตัด Story 3 ทิ้งตาม Constraints แล้ว ข้อกังวลนี้ก็หมดความจำเป็น | ตัด (หลังจากตัด Story 3 ออกแล้ว) |
| 100: "2. A rating is not the same as suitability: a parent may still need more than the rating to judge a movie for an 8-year-old." | เป็นข้อกังวลที่ถูกต้องและสอดคล้องกับ Problem ใน intent ที่ผู้ปกครองต้องเป็นคนตัดสินใจเลือกเอง | เก็บ |
| 101: "3. The Movie record from L2 requires a rating, so movies without a rating cannot exist in the catalog today; if unrated movies are added later, the filter must decide where they go." | สอดคล้องกับระบบ Movie ใน L2 ที่บังคับให้มี rating อยู่แล้ว ช่วยป้องกันการเพิ่มเงื่อนไขเรื่องหนังไม่มีเรตเกินจำเป็นในสัปดาห์นี้ | เก็บ |
| 102: "4. There is no web page yet, so \"show\" and \"message\" can only be checked through the domain classes this week." | ตรงกับ Affected users and systems ใน intent ที่ระบุว่ายังไม่มีหน้าเว็บและตรวจสอบผ่าน domain classes เท่านั้น | เก็บ |

Kept: การคงข้อจำกัดหลักเรื่องเรตติ้ง 5 ค่า (`REQ-1`) และการไม่เก็บข้อมูลส่วนตัวหรือบังคับเข้าสู่ระบบ (`REQ-12`) รวมถึงการระบุข้อกังวลที่ถูกต้องว่าระดับเรตติ้งไม่ใช่สิ่งเดียวกับความเหมาะสมของเนื้อหา หนังใน L2 มีเรตติ้งครบทุกเรื่อง และสัปดาห์นี้ตรวจสอบพฤติกรรมผ่าน domain classes เท่านั้น เพราะทั้งหมดตรงกับ `movie-filter.md` และสามารถตรวจสอบได้จริง

Counts: แก้ 12 ตัด 9 เก็บ 14

Customer proxy: ผศ.ดร.ดำรงค์ฤทธิ์ เศรษฐ์ศิริโชค confirmed all three cards on 27/11/2026.
