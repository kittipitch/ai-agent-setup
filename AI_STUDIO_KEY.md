# Google AI Studio API Key: ขอ key ก่อนวันงาน

เอกสารนี้ใช้เวลา ~5 นาที ทำครั้งเดียวที่บ้าน **ก่อนวันงาน**
ในห้องเรียนจะไม่มีเวลารอหน้าเว็บและการยืนยันตัวตน

จบแล้วคุณจะได้: API key ของ Google AI Studio (Gemini) เก็บไว้ในเครื่องตัวเอง
ใช้ใน mini workshop ที่เรียก Gemini API จาก Python

**ไม่มี key ก็ยังเรียนได้** — mini workshop ตัวนี้เป็นของแถมสำหรับทำต่อเองหลังคลาส
แต่ถ้ามี key มาแล้ว จะลองรันตามในห้องได้ทันที

---

## ขั้นที่ 1 — ขอ key

1. เปิด browser ไปที่ `https://aistudio.google.com/apikey`
2. login ด้วย Google account (ใช้บัญชีส่วนตัวได้ ไม่ต้องเป็นบัญชีองค์กร)
3. กดปุ่มสร้าง API key ใหม่ (**Create API key**)
4. เลือกโปรเจกต์ที่ระบบเสนอให้ หรือให้มันสร้างใหม่ก็ได้
5. **copy key ที่ได้เก็บไว้** — หน้าตาเป็นสตริงยาว ๆ ขึ้นต้นด้วย `AIza`

> key นี้จะดูย้อนหลังได้จากหน้าเดิม ถ้าทำหาย ให้สร้างใหม่แล้วลบอันเก่าทิ้ง

---

## ขั้นที่ 2 — เก็บ key ไว้ในเครื่อง

**ห้ามพิมพ์ key ลงในไฟล์ code และห้าม commit ขึ้น git เด็ดขาด**
key ที่หลุดขึ้น GitHub จะถูกเพิกถอนอัตโนมัติ และนั่นคือบทเรียน Hardcoded Secret
ที่ workshop สอนในวันที่สอง

เก็บเป็น environment variable แทน — เปิด terminal แล้วพิมพ์:

**macOS (zsh):**

```bash
echo 'export GEMINI_API_KEY="วางkeyตรงนี้"' >> ~/.zshrc
source ~/.zshrc
```

**Windows (Ubuntu shell ของ WSL2) และ Ubuntu/Linux (bash):**

```bash
echo 'export GEMINI_API_KEY="วางkeyตรงนี้"' >> ~/.bashrc
source ~/.bashrc
```

> แทนที่ `วางkeyตรงนี้` ด้วย key จริง และเก็บเครื่องหมาย `"` ไว้ทั้งสองข้าง

---

## ขั้นที่ 3 — ตรวจว่าสำเร็จ

```bash
echo $GEMINI_API_KEY
```

ต้องขึ้น key ของคุณ ถ้าไม่ขึ้นอะไรเลย ให้ปิด terminal เปิดใหม่แล้วลองอีกครั้ง

ติดตั้ง SDK ที่ mini workshop ใช้ (ทำก่อนวันงานได้เลย จะได้ไม่ต้องรอ Wi-Fi ในห้อง):

```bash
pip install google-genai
```

---

## ถ้าพัง

| อาการที่เห็น | สาเหตุที่พบบ่อย | ทำอย่างไร |
|---|---|---|
| `echo $GEMINI_API_KEY` ไม่ขึ้นอะไร | terminal เดิมยังไม่ได้โหลดค่าใหม่ | ปิด terminal เปิดใหม่ หรือรัน `source ~/.zshrc` (macOS) / `source ~/.bashrc` (Ubuntu) |
| หน้า AI Studio ไม่ให้สร้าง key | บัญชีองค์กรมี policy บล็อกอยู่ | ใช้ Google account ส่วนตัวแทน |
| `ModuleNotFoundError: No module named 'google.genai'` | ยังไม่ได้ติดตั้ง SDK | `pip install google-genai` (ชื่อ package มีขีด ชื่อ module มีจุด) |
| กังวลว่าจะเสียเงิน | free tier มีโควตาให้ใช้ฟรีอยู่แล้ว | mini workshop เรียกไม่กี่ครั้ง อยู่ในโควตาฟรี |

---

## คำถามที่จะมีคนถาม

- **ทำไมต้องใช้ provider อื่นด้วย ในเมื่อคอร์สนี้สอน Claude Code?**
  เพราะประเด็นของ workshop คือ *ชั้นที่ตัดสินผลลัพธ์เป็นของคุณ* ไม่ใช่ของ vendor ไหน
  mini workshop ตัวนี้รัน schema เดียวกันและ check ชุดเดียวกันกับฝั่ง Claude
  ต่างกันแค่ client ที่เรียก
- **key นี้ใช้กับ Claude Code ได้ไหม?** ไม่ได้ คนละระบบ Claude Code login แยกของตัวเอง
- **ลบทิ้งทีหลังได้ไหม?** ได้ กลับไปที่ `https://aistudio.google.com/apikey` แล้วลบ key นั้น
