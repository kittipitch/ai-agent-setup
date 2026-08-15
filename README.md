# คู่มือการติดตั้งสภาพแวดล้อม (Universal Setup Guide)

คู่มือนี้สำหรับผู้ใช้ทุกคน (Windows/macOS/Linux) เพื่อติดตั้งระบบที่รองรับการทำ Workshop **AI-Accelerated Software Development**

> ## ทำให้เสร็จ **ก่อนวันงาน** ทั้งหมด
>
> ในห้องเรียนจะไม่มีเวลาติดตั้ง และ Wi-Fi ของสถานที่จัดงานทำให้ทุกอย่างช้าลงมาก
> โดยเฉพาะ **`gh auth login` ต้องทำมาก่อน** - กิจกรรมทีมในวันที่สองสร้าง repo
> และ push งานจริง ถ้ายังไม่ได้ login จะติดตรงนั้นทั้งทีม
>
> ไม่เคยใช้ terminal มาก่อน? เริ่มที่ **[GH_AUTH.md](GH_AUTH.md)** - อธิบายตั้งแต่
> วิธีเปิด terminal ไปจนถึงตรวจว่า login สำเร็จแล้วจริง ใช้เวลา ~15 นาที
>
> **ของแถม (ไม่บังคับ):** ถ้าอยากลอง mini workshop ที่เรียก Gemini API
> ขอ API key มาก่อนได้ที่ **[AI_STUDIO_KEY.md](AI_STUDIO_KEY.md)** ~5 นาที
> ไม่มี key ก็เรียนได้ครบทุกช่วง

---

## เลือกแพลตฟอร์มของคุณ

คลิกที่คู่มือที่ตรงกับระบบปฏิบัติการของคุณ:

### 1. Windows Users
ถ้าคุณใช้ **Windows** ให้ใช้ **WSL2 + Ubuntu** เป็นสภาพแวดล้อมหลัก:

#### [WINDOWS.md](WINDOWS.md) - จุดเริ่มต้นสำหรับ Windows (ผ่าน WSL2 + Ubuntu)

อ่าน [WINDOWS.md](WINDOWS.md) ก่อน แล้วทำ 2 ขั้นตอนนี้ตามลำดับ:

1. [WSL.md](WSL.md) - ติดตั้ง WSL2 + Ubuntu 24.04
2. [UBUNTU.md](UBUNTU.md) - ติดตั้งเครื่องมือทั้งหมดใน Ubuntu

---

### 2. Ubuntu/Linux Users
ถ้าคุณใช้ **Ubuntu** หรือ Linux distros อื่นๆ:

#### [UBUNTU.md](UBUNTU.md) - คู่มือการติดตั้งบน Ubuntu/WSL

---

### 3. macOS Users
ถ้าคุณใช้ **macOS**:

#### [MACOS.md](MACOS.md) - คู่มือการติดตั้งบน macOS

---

## ภาพรวมเครื่องมือที่จะติดตั้ง

ทุกแพลตฟอร์มจะติดตั้งเครื่องมือชุดเดียวกัน:

| เครื่องมือ | ใช้สำหรับ |
|--------------|-------------|
| **Claude Code CLI** | AI coding agent หลักของ Workshop |
| **VS Code** | Code editor พร้อม Claude Code extension |
| **Git** | จัดการ version control |
| **GitHub CLI (gh)** | ทำงานกับ GitHub ผ่าน command line |
| **Bun** | JavaScript runtime หลักสำหรับโปรเจกต์และ labs |
| **Node.js + npm** | จำเป็นสำหรับ Claude Code plugin hooks |
| **pipx** | ติดตั้ง Python CLI tools แบบ isolated |
| **uv** | Python package installer ที่เร็ว |

> [!IMPORTANT]
> ต้องมี **paid Claude account**: Pro, Max, Team, Enterprise หรือ Console API; free Claude.ai plan ยังใช้ Claude Code ไม่ได้
>
> **Node.js ต้องติดตั้งแบบ Global** (ไม่ใช่ผ่าน NVM/FNM) เพื่อให้ Claude Code plugin hooks ทำงานได้

---

## หลังจากติดตั้งเสร็จ

**ตรวจก่อนปิดเครื่อง** - รัน `gh auth status` ต้องขึ้นว่า `Logged in to github.com account <username>`
ถ้าไม่ขึ้น ให้ดู [GH_AUTH.md](GH_AUTH.md) ขั้นที่ 3

เมื่อติดตั้งทุกอย่างเสร็จแล้ว คุณจะมีสภาพแวดล้อมที่พร้อมสำหรับ:
- เขียนโค้ดด้วย AI assistance
- จัดการโปรเจกต์ด้วย Git/GitHub
- รัน JavaScript/TypeScript ด้วย Bun
- ใช้ Python tools ที่ทันสมัย (uv, pipx)
- ใช้ Claude Code CLI และ plugin ได้อย่างเต็มประสิทธิภาพ

---

## ติดปัญหา?

หากพบปัญหาในการติดตั้ง:
1. ตรวจสอบว่าเลือกคู่มือที่ตรงกับแพลตฟอร์มของคุณ
2. ตรวจสอบเวอร์ชันของเครื่องมือด้วยคำสั่ง `--version` หรือ `--help`
3. อ่านข้อความ error ให้ละเอียด มักจะบอกสาเหตุและวิธีแก้ไข

---

## เอกสารเพิ่มเติม

- [AGENTS.md](AGENTS.md) - คู่มือสำหรับ AI agent เมื่อทำงานใน repo นี้
- [downloads/](downloads/) - Skill templates สำหรับ Claude Code
