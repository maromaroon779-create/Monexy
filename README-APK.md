# Monexy — วิธีทำ APK

โครงสร้างโปรเจกต์ (พร้อมใช้กับ Capacitor):
- `www/index.html` — ตัวแอป (ไฟล์เดียว)
- `resources/` — ไอคอนและภาพ splash ต้นฉบับ
- `capacitor.config.json`, `package.json` — ตั้งค่าโปรเจกต์ (appId: com.monexy.app)
- `.github/workflows/build-apk.yml` — สคริปต์ให้ GitHub build APK ให้ (วิธี B)

---------------------------------------------------------------
## วิธี A — Build บนคอมพิวเตอร์ (Capacitor + Android Studio)

ติดตั้งก่อน: Node.js 18+ (หรือ 20), JDK 17, Android Studio

1. แตกไฟล์ zip แล้วเปิด Terminal ในโฟลเดอร์ `monexy`
2. `npm install`
3. `npx cap add android`
4. `npx capacitor-assets generate --android`   (สร้างไอคอน + splash ทุกขนาด)
5. `npx cap sync android`
6. `npx cap open android`  → เปิดใน Android Studio รอ Gradle sync เสร็จ
7. เมนู **Build > Build Bundle(s) / APK(s) > Build APK(s)**
8. ไฟล์อยู่ที่ `android/app/build/outputs/apk/debug/app-debug.apk` → ส่งเข้ามือถือแล้วติดตั้ง
   (ต้องเปิด "ติดตั้งแอปจากแหล่งที่ไม่รู้จัก")

ทำ APK สำหรับเผยแพร่ (Release): Android Studio > Build > Generate Signed Bundle / APK > เลือก APK > สร้าง keystore ใหม่
แล้วเก็บไฟล์ keystore + รหัสผ่านไว้ให้ดี (หายแล้วอัปเดตแอปเดิมไม่ได้)

เมื่อแก้ `www/index.html` ภายหลัง: รัน `npx cap sync android` แล้ว Build ใหม่

---------------------------------------------------------------
## วิธี B — ไม่ต้องลงโปรแกรมอะไร (ใช้ GitHub สร้างให้ ฟรี)

1. สมัคร github.com แล้วสร้าง repository ใหม่ (Private ก็ได้)
2. อัปโหลดไฟล์ทั้งหมดในโฟลเดอร์นี้ขึ้นไป (รวมโฟลเดอร์ `.github`)
3. แท็บ **Actions > Build APK > Run workflow**
4. รอ 4–8 นาที แล้วเข้าไปในรันที่เสร็จ ดาวน์โหลด **Monexy-debug-apk** ใน Artifacts
5. แตก zip จะได้ `app-debug.apk` ติดตั้งในมือถือได้เลย

---------------------------------------------------------------
## วิธี C — เร็วสุดแต่ปรับแต่งได้น้อย
เว็บแปลงเว็บเป็นแอป เช่น WebIntoApp / Median.co — อัปโหลดโฟลเดอร์ `www` เป็น zip แล้วโหลด APK

---------------------------------------------------------------
## หมายเหตุ
- ข้อมูลเก็บใน localStorage ของแอป ถ้าถอนการติดตั้งข้อมูลจะหาย — ใช้ปุ่ม "สำรองข้อมูล > คัดลอกข้อความ" เก็บไว้เป็นระยะ
- เสียงหน้าเริ่มระบบ: ปกติจะดังเองตอนเปิดแอป ถ้า Android บล็อกเสียงอัตโนมัติ จะขึ้นข้อความ "แตะหน้าจอเพื่อเปิดเสียง" ให้แตะ 1 ครั้ง
  ปิดเสียงได้ที่ ตั้งค่า > เสียงแจ้งเตือนเบาๆ
- ถ้าอยากเปลี่ยนชื่อแพ็กเกจ ให้แก้ `appId` ใน `capacitor.config.json` ก่อนรันขั้นตอนที่ 3
