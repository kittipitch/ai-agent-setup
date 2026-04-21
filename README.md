# คู่มือการติดตั้งสภาพแวดล้อม (Universal Setup Guide)

คู่มือนี้สำหรับผู้ใช้ทุกคน (Windows/macOS/Linux) เพื่อติดตั้งระบบที่รองรับการทำ Workshop **AI-Accelerated Software Development**

---

## 🚀 เลือกแพลตฟอร์มของคุณ

คลิกที่คู่มือที่ตรงกับระบบปฏิบัติการของคุณ:

### 1. 🪟 Windows Users
ถ้าคุณใช้ **Windows** ให้เริ่มจากติดตั้ง WSL ก่อน:

#### 👉 [WSL.md](WSL.md) - คู่มือการติดตั้ง WSL

หลังจากติดตั้ง WSL เสร็จแล้ว ให้ดู:
#### 👉 [UBUNTU.md](UBUNTU.md) - คู่มือการติดตั้งบน Ubuntu/WSL

---

### 2. 🐧 Ubuntu/Linux Users
ถ้าคุณใช้ **Ubuntu** หรือ Linux distros อื่นๆ:

#### 👉 [UBUNTU.md](UBUNTU.md) - คู่มือการติดตั้งบน Ubuntu/WSL

---

### 3. 🍎 macOS Users
ถ้าคุณใช้ **macOS**:

#### 👉 [MACOS.md](MACOS.md) - คู่มือการติดตั้งบน macOS

---

## 📋 ภาพรวมเครื่องมือที่จะติดตั้ง

ทุกแพลตฟอร์มจะติดตั้งเครื่องมือชุดเดียวกัน:

| เครื่องมือ | ใช้สำหรับ |
|--------------|-------------|
| **Git** | จัดการ version control |
| **GitHub CLI (gh)** | ทำงานกับ GitHub ผ่าน command line |
| **Bun** | JavaScript runtime หลัก |
| **Node.js + npm** | จำเป็นสำหรับ Claude Code Plugins |
| **pipx** | ติดตั้ง Python CLI tools แบบ isolated |
| **uv** | Python package installer ที่เร็ว |
| **Claude Code CLI** | AI coding assistant |
| **VS Code** | Code editor |

> [!IMPORTANT]
> **Node.js ต้องติดตั้งแบบ Global** (ไม่ใช่ผ่าน NVM/FNM) เพื่อให้ Claude Code Plugins ทำงานได้

---

## 🎯 หลังจากติดตั้งเสร็จ

เมื่อติดตั้งทุกอย่างเสร็จแล้ว คุณจะมีสภาพแวดล้อมที่พร้อมสำหรับ:
- ✅ เขียนโค้ดด้วย AI assistance
- ✅ จัดการโปรเจกต์ด้วย Git/GitHub
- ✅ รัน JavaScript/TypeScript ด้วย Bun
- ✅ ใช้ Python tools ที่ modern (uv, pipx)
- ✅ ใช้ Claude Code CLI และ Plugins ได้อย่างเต็มประสิทธิภาพ

---

## ❓ ติดปัญหา?

หากพบปัญหาในการติดตั้ง:
1. ตรวจสอบว่าเลือกคู่มือที่ตรงกับแพลตฟอร์มของคุณ
2. ตรวจสอบเวอร์ชันของเครื่องมือด้วยคำสั่ง `--version` หรือ `--help`
3. อ่านข้อความ error ให้ละเอียด มักจะบอกสาเหตุและวิธีแก้ไข

---

## 📚 เอกสารเพิ่มเติม

- [CLAUDE.md](CLAUDE.md) - คู่มือสำหรับ Claude Code เมื่อทำงานใน repo นี้
- [downloads/](downloads/) - Skill templates สำหรับ Claude Code
