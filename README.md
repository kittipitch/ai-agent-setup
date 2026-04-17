# คู่มือการติดตั้งสภาพแวดล้อม (Universal Setup Guide)

คู่มือนี้สำหรับผู้ใช้ทุกคน (Windows/macOS/Linux) เพื่อติดตั้งระบบที่รองรับการทำ Workshop **AI-Accelerated Software Development**

---

## 1. การเตรียมตัว (Windows/WSL Only)

หากคุณใช้ Windows เราจะใช้ **WSL2 (Windows Subsystem for Linux)** เพื่อให้ได้ประสบการณ์เดียวกับ Linux/macOS

### 1.1 เปิด PowerShell แบบ Administrator
1. กดปุ่ม **Win + R**
2. พิมพ์ "powershell"
3. กด **Ctrl + Shift + Enter**
4. เลือก "Yes" ที่หน้าต่างป๊อปอัป

![ยืนยันการเปิด PowerShell](https://github.com/kittipitch/cs111env/raw/main/images/windows/img14_win_popup.png)

### 1.2 เปิดใช้งานฟีเจอร์ WSL
เปิด PowerShell (Administrator) แล้วรัน:

```powershell
# เปิดใช้งาน Windows Subsystem for Linux
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

# เปิดใช้งาน Virtual Machine Platform
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

![เปิดฟีเจอร์ WSL](https://github.com/kittipitch/cs111env/raw/main/images/windows/img06_win_features_on.png)

### 1.3 ติดตั้ง WSL และ Ubuntu
หลังจากรีสตาร์ทเครื่อง ให้รัน:

```powershell
# อัปเดต WSL
wsl --update

# ติดตั้ง WSL
wsl --install --no-distribution

# ตั้งค่า WSL 2 เป็นค่าเริ่มต้น
wsl --set-default-version 2

# ติดตั้ง Ubuntu 24.04
wsl --install -d Ubuntu-24.04
```

*เมื่อเสร็จแล้ว ให้ตั้ง Username และ Password ใน Ubuntu Terminal*

### 1.4 ติดตั้ง Windows Terminal
ดาวน์โหลดจาก: https://aka.ms/terminal

ตั้งค่า Default Profile เป็น "Ubuntu-24.04"

![ตั้งค่า Windows Terminal](https://github.com/kittipitch/cs111env/raw/main/images/windows/img02_win_term_profile_4.png)

### 1.5 ตั้งค่าให้ Ubuntu เห็นไฟล์ Windows
เปิด WSL Terminal แล้วรัน:

```bash
# เชื่อมโฟลเดอร์ Windows กับ Ubuntu
HOME_DIR=$(wslpath -u $(cmd.exe /c echo %USERPROFILE% | tr -d '\r'))
[[ -e "$HOME_DIR/OneDrive/Desktop" ]] && \
ln -sf "$HOME_DIR/OneDrive/Desktop" ~/ || \
ln -sf "$HOME_DIR/Desktop" ~/
[[ -e "$HOME_DIR/OneDrive/Documents" ]] && \
ln -sf "$HOME_DIR/OneDrive/Documents" ~/ || \
ln -sf "$HOME_DIR/Documents" ~/
ln -sf "$HOME_DIR/Downloads" ~/
```

### 1.6 อัปเดตและอัปเกรด Ubuntu
```bash
sudo apt update; sudo apt upgrade -y
```

### 1.7 ตั้งค่า WSL ขั้นสูง (แนะนำ)

#### แก้ไข permissions และเปิดใช้งาน systemd
แก้ไขไฟล์ `/etc/wsl.conf`:

```bash
sudo nano /etc/wsl.conf
```

เพิ่มข้อมูลต่อไปนี้:

```ini
[boot]
systemd=true
[automount]
enabled=true
options="metadata,uid=1000,gid=1000,umask=077"
```

![ตั้งค่า wsl.conf](https://github.com/kittipitch/cs111env/raw/main/images/nix/img22_nix_wsl_conf.png)

กด **Ctrl + O** → **Enter** → **Ctrl + X** เพื่อบันทึกและออก

#### จำกัดทรัพยากร WSL (ป้องกันใช้ RAM/เครื่องมากเกินไป)
สร้างไฟล์ `.wslconfig` ในโฟลเดอร์ผู้ใช้ Windows:

```bash
HOME_DIR=$(wslpath -u $(cmd.exe /c echo %USERPROFILE% | tr -d '\r'))
nano $HOME_DIR/.wslconfig
```

เพิ่มข้อมูลต่อไปนี้ (ปรับค่าตามสเปคเครื่อง):

```ini
[wsl2]
memory=2GB
processors=1
```

> **หมายเหตุ**: `memory` คือ RAM สูงสุดที่ WSL ใช้ได้, `processors` คือจำนวน CPU cores หลังจากแก้ไขให้รีสตาร์ท WSL:
> ```powershell
> wsl --shutdown
> ```
> จากนั้นเปิด WSL ใหม่

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
curl -fsSL https://claude.ai/install.sh | bash
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
- **[RTK](https://github.com/rtk-ai/rtk) (Context Filter)**: กรองไฟล์ที่ไม่จำเป็นออกอัตโนมัติ
  ```bash
  curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/refs/heads/master/install.sh | sh
  rtk init -g
  ```
- **[Caveman](https://github.com/juliusbrussee/caveman) (Token Saver)**: ลด Token usage ลง 65% สำหรับงานง่ายๆ
  ```bash
  claude plugin marketplace add JuliusBrussee/caveman && claude plugin install caveman@caveman
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
