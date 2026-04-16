# คู่มือการติดตั้งสภาพแวดล้อม (Universal Setup Guide)

คู่มือนี้สำหรับผู้ใช้ทุกคน (Windows/macOS/Linux) เพื่อติดตั้งระบบที่รองรับการทำ Workshop **AI-Accelerated Software Development**

---

## 1. การเตรียมตัว (Windows/WSL Only)

หากคุณใช้ Windows เราจะใช้ **WSL2 (Windows Subsystem for Linux)** เพื่อให้ได้ประสบการณ์เดียวกับ Linux/macOS

### 1.1 ติดตั้ง WSL และ Ubuntu
เปิด PowerShell (Administrator) แล้วรัน:
```powershell
wsl --update
wsl --install -d Ubuntu-24.04
```
*เมื่อเสร็จแล้ว ให้ตั้ง Username และ Password ใน Ubuntu Terminal*

---

## 2. ติดตั้ง Runtime หลัก (Shared - ทุกระบบ)

เราใช้ **Bun** เป็นเครื่องยนต์หลักในการรันโปรเจกต์และ AI Agent

**สำหรับ WSL/Ubuntu:** ติดตั้ง `unzip` ก่อน (Bun ต้องการ):
```bash
sudo apt update && sudo apt install -y unzip
```

```bash
# ติดตั้ง Bun
curl -fsSL https://bun.com/install | bash
source ~/.bashrc
```

**ตรวจสอบเวอร์ชัน:**
```bash
bun --version  # ควรได้ 1.x.x ขึ้นไป
```

**ติดตั้ง Node.js + npm (จำเป็นสำหรับ Claude Code Plugins):**
```bash
# ติดตั้ง fnm (Fast Node Manager)
curl -fsSL https://fnm.vercel.app/install | bash
source ~/.bashrc

# ติดตั้ง Node.js (npm จะมากับ Node.js อัตโนมัติ)
fnm install 24
fnm use 24
```

---

## 3. ติดตั้ง Claude Code CLI (ทุกระบบ)

ติดตั้งเวอร์ชันล่าสุด (2.1.112) ด้วยคำสั่งเดียว - รองรับทุกระบบ (macOS/Linux/WSL):

```bash
curl -fsSL https://claude.ai/install.sh | sh
```

### 🚀 เริ่มต้นใช้งาน (ทุกระบบ)
```bash
claude
# ทำตามขั้นตอน Login บนหน้าจอ
```

---

## 4. ติดตั้ง Editor และ Extensions (Shared)

1. ติดตั้ง [VS Code](https://code.visualstudio.com/)
2. **สำหรับ Windows**: ติดตั้ง Extension ชื่อ **"WSL"**
3. **Claude Extension**: ค้นหา "Claude" ใน Marketplace และติดตั้งเวอร์ชันทางการจาก **Anthropic** (หรือ Claude Dev)

---

## ⚔️ คลังแสงวิศวกร (The Surgical Arsenal)

เครื่องมือเสริมประสิทธิภาพที่จะช่วยให้ AI Agent ของคุณทำงานได้เหมือนมืออาชีพ (ติดตั้งตาม Session ใน Workshop)

### ⚡ เพิ่มประสิทธิภาพ (Session 1-2)
- **RTK (Context Filter)**: กรองไฟล์ที่ไม่จำเป็นออกอัตโนมัติ
  ```bash
  curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/refs/heads/master/install.sh | sh
  rtk init -g
  ```
- **Caveman (Token Saver)**: ลด Token usage ลง 65% สำหรับงานง่ายๆ
  ```bash
  npx pi-caveman
  ```

### 🛡️ ความปลอดภัยและวิชาการ (Session 4-6)
- **Official Plugins**: ติดตั้งภายใน Claude Code
  ```bash
  /plugin install security-guidance
  /plugin install code-review
  ```
- **Claude-Mem (Memory Protocol)**: ระบบความจำระยะยาว
  ```bash
  /plugin install thedotmack/claude-mem
  ```
- **CPR (Context Preservation)**: กู้คืนบริบทงานได้อย่างรวดเร็ว
  ```bash
  git clone https://github.com/EliaAlberti/cpr-compress-preserve-resume ~/.claude/commands/cpr
  ```
- **code-review-graph (AST Analysis)**: วิเคราะห์โครงสร้างโค้ดเชิงลึก
  ```bash
  sudo apt install -y python3-pip  # สำหรับ WSL/Ubuntu
  pip install code-review-graph --break-system-packages
  ```

### 🎨 สถาปัตยกรรม (Architecture)
- **Hookify**: สร้าง Custom Workflow ของคุณเอง
  ```bash
  /plugin install hookify@claude-plugins-official
  ```
- **Claude Wizard**: ระบบแนะนำการทำงานแบบ 8-Phase
  ```bash
  curl -sL https://raw.githubusercontent.com/vlad-ko/claude-wizard/main/install.sh | bash
  ```

---

## ภาคผนวก: การออกจาก Editor
- **nano**: `Ctrl + X` → `Y` → `Enter`
- **vim**: `Esc` → `:q!` → `Enter`
