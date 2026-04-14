# การติดตั้งสภาพแวดล้อมสำหรับ Windows (WSL2 Edition)

คู่มือนี้สำหรับผู้ใช้ Windows 10/11 เพื่อติดตั้งระบบที่รองรับการทำ Workshop ด้วย **Bun** และ **Claude Code** โดยใช้ **WSL2 (Windows Subsystem for Linux)** เป็นหัวใจหลัก

---

## 1. การติดตั้ง WSL2 และ Ubuntu 24.04

เราจะใช้ Ubuntu เป็นระบบปฏิบัติการหลักในการรัน AI Agent

### 1.1 เปิด PowerShell ด้วยสิทธิ์ Administrator
1. กดปุ่ม `Windows + R`
2. พิมพ์ `powershell`
3. กด `Ctrl + Shift + Enter` เพื่อรันเป็น Administrator

### 1.2 ติดตั้ง WSL
รันคำสั่งต่อไปนี้ใน PowerShell:

```powershell
# อัปเดต WSL
wsl --update

# ติดตั้ง Ubuntu (เวอร์ชันมาตรฐาน)
wsl --install -d Ubuntu-24.04
```

*เมื่อติดตั้งเสร็จ เครื่องอาจจะบังคับให้ Restart 1 รอบ*

### 1.3 ตั้งค่าคอนฟิกเริ่มต้น
เมื่อเปิด Ubuntu ขึ้นมาครั้งแรก ระบบจะให้ตั้ง **Username** และ **Password**
> **Note:** ตอนพิมพ์ Password ตัวอักษรจะไม่ปรากฏบนหน้าจอ (พิมพ์ไปได้เลยแล้วกด Enter)

อัปเดตระบบให้เป็นปัจจุบัน:
```bash
sudo apt update && sudo apt upgrade -y
```

---

## 2. ติดตั้ง Windows Terminal (แนะนำ)

เพื่อประสบการณ์การใช้งานที่ดีที่สุด ควรใช้ Windows Terminal แทน Command Prompt เดิม
- ดาวน์โหลดจาก Microsoft Store: [Windows Terminal](https://aka.ms/terminal)
- ตั้งค่า **Default Profile** เป็น "Ubuntu-24.04"

---

## 3. ติดตั้งเครื่องมือหลัก (Bun & Claude Code)

ใน Workshop นี้เราจะใช้ **Bun** แทน npm เพราะเร็วกว่าและรองรับการทำงานของ Agent ได้ดีกว่า

### 3.1 ติดตั้ง Bun
รันคำสั่งนี้ใน Ubuntu Terminal:
```bash
curl -fsSL https://bun.com/install | bash
source ~/.bashrc
```

**ตรวจสอบเวอร์ชัน:**
```bash
bun --version  # ควรได้ 1.x.x ขึ้นไป
```

### 3.2 ติดตั้ง Claude Code CLI (WSL Only — Paste Bug Fix)
เนื่องจากเวอร์ชันล่าสุดมี bug ที่ทำให้วางรหัสยืนยันไม่ได้ใน WSL ให้ติดตั้งเวอร์ชันที่เสถียรด้วย Bun ดังนี้:

```bash
# ลบเวอร์ชันเก่าออกก่อน (ถ้ามี)
npm uninstall -g @anthropic-ai/claude-code

# ติดตั้งเวอร์ชัน 2.1.104
bun install -g @anthropic-ai/claude-code@2.1.104
```

**เริ่มต้นใช้งาน:**
```bash
claude
```

**อัปเกรดเป็นเวอร์ชันล่าสุด (Optional - หลังจาก Login สำเร็จ):**
```bash
claude update
```

**ตรวจสอบความพร้อม:**
```bash
claude doctor
```

---

## 4. ติดตั้งเครื่องมือเสริมสำหรับ Workshop (Surgical Utilities)

เครื่องมือเหล่านี้จะช่วยเพิ่มประสิทธิภาพให้ AI Agent ของคุณ

### 4.1 Git (สำคัญมาก)
```bash
sudo apt install -y git
```

### 4.2 GSD Framework (Spec-Driven Development)
```bash
bun add -g get-shit-done-cc
```

### 4.3 เครื่องมือช่วยจำและวิเคราะห์ (Power-Ups)
```bash
# ติดตั้ง mempalace (Persistent memory)
bun add -g mempalace

# ติดตั้ง code-review-graph (AST analysis)
sudo apt install -y python3-pip
pip install code-review-graph --break-system-packages

# ติดตั้ง CPR (Session Management)
git clone https://github.com/EliaAlberti/cpr-compress-preserve-resume ~/.claude/commands/cpr
```

### 4.4 Agent Skills (ติดตั้งใน Claude Code)
เปิด `claude` แล้วรันคำสั่งเหล่านี้:
```bash
/plugin install thedotmack/claude-mem    # ระบบความจำเสริม
bunx skillsadd caveman                   # ลด Token usage 65%
```

---

## 5. การจัดการไฟล์ระหว่าง Windows และ WSL

คุณสามารถเข้าถึงไฟล์ใน Ubuntu ได้ง่ายๆ ผ่าน VS Code
1. ติดตั้ง [VS Code](https://code.visualstudio.com/) ใน Windows
2. ติดตั้ง Extension ชื่อ **"WSL"**
3. ใน Ubuntu Terminal พิมพ์ `code .` เพื่อเปิดโปรเจกต์

### 5.1 การติดตั้ง VS Code Extension (Claude)
เพิ่มพลัง AI เข้าไปใน Editor ของคุณ:
1. เปิด VS Code ใน Windows
2. กด `Ctrl + Shift + X` (Extension Marketplace)
3. ค้นหาคำว่า **"Claude"**
4. เลือกติดตั้ง Extension ของ **Anthropic** (หรือตัวที่ Instructor แนะนำ)
5. ทำการ Sign-in หรือระบุ API Key ตามคำแนะนำบนหน้าจอ

---

## 6. ภาคผนวก: การออกจาก Editor เมื่อติดค้าง

หากคุณหลงเข้าไปใน Text Editor ใน Terminal แล้วออกไม่ได้:
- **nano**: กด `Ctrl + X` → กด `Y` → กด `Enter`
- **vim**: กด `Esc` → พิมพ์ `:q!` → กด `Enter`
- **emacs**: กด `Ctrl + X` ตามด้วย `Ctrl + C`
