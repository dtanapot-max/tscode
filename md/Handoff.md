# กฎเกณฑ์และข้อพึงระวังสำหรับ AI (Engineering Handoff Protocol)

## 1. กฎเหล็กเชิงสถาปัตยกรรม (Strict Core Rules)
1. **Zero Framework / Zero Dependency:** ห้ามติดตั้ง React, Vue, jQuery ทุกอย่างต้องรันด้วย Web Standard APIs บริสุทธิ์
2. **Self-Extracting Pattern:** ห้ามเขียนสตริงโค้ดซ้ำซ้อนในตัวแปร ปุ่ม 📦 Deploy จะต้องสกัดสดจาก Memory และ DOM เท่านั้น
3. **Mount-Once Lifecycle:** เมธอด `mount()` ต้องทำงานครั้งเดียวตลอดอายุแอป ห้ามล้าง DOM Shell ด้วย `innerHTML` ทั้งหน้าจอ

## 2. กฎการจัดการภาษาไทยและข้อความ
1. **Grapheme Clusters:** ห้ามใช้ `str.length` นับอักขระไทย ให้ใช้ `getVisualLength()` ผ่าน `Intl.Segmenter` เสมอ
2. **IME Composition Guard:** ใช้ธง `isComposing` ดัก `compositionstart` / `compositionend` ป้องกันสระและวรรณยุกต์กระโดดหลุด
3. **BOM UTF-8:** เมื่อ Export ไฟล์ข้อความ ต้องเติม `\uFEFF` นำหน้า Blob เสมอ ป้องกันภาษาต่างด้าวบน Windows

## 3. Storage & State Performance
1. **True 1-Doc-per-File:** เอกสารแต่ละฉบับจะถูกแยกเซฟเป็น `doc_<id>.md` จริงบน OPFS เพื่อรองรับไฟล์ระดับหลายสิบ MB ได้อย่างราบรื่น
2. **Cursor Protection:** ก่อน Render Stage Textarea ต้องเช็ก `dataset.activeId` เสมอ เพื่อป้องกัน Focus หลุดและ Cursor Jump
3. **Debounced Sync:** Event `TYPING_DOC` ต้องหน่วงเวลา Debounce 400ms ก่อนเขียนลงดิสก์