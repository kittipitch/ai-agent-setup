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

```bash
# ติดตั้ง Bun
curl -fsSL https://bun.com/install | bash
source ~/.bashrc
```

**ตรวจสอบเวอร์ชัน:**
```bash
bun --version  # ควรได้ 1.x.x ขึ้นไป
```

---

## 3. ติดตั้ง Claude Code CLI (สาขาแยกตามระบบ)

ขั้นตอนนี้สำคัญมากสำหรับผู้ใช้ Windows เพื่อหลีกเลี่ยง Bug ในการวางรหัสยืนยัน (Paste Bug)

### 🌿 สำหรับ Windows (WSL2) - RECOMMENDED
รันคำสั่งนี้เพื่อติดตั้งเวอร์ชันที่เสถียรสำหรับ WSL:
```bash
# ลบเวอร์ชันเดิม (ถ้ามี)
npm uninstall -g @anthropic-ai/claude-code

# ติดตั้งเวอร์ชัน 2.1.104
bun install -g @anthropic-ai/claude-code@2.1.104
```

### 🍎 สำหรับ macOS / Linux
รันคำสั่งติดตั้งมาตรฐาน:
```bash
curl -fsSL https://cdn.anthropic.com/claude-code/install.sh | sh
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
  bunx skillsadd caveman
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
