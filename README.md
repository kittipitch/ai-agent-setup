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

### 1.7 ติดตั้งเครื่องมือพื้นฐาน (Git, gh, pipx, uv)
> [!IMPORTANT]
> **Git และ GitHub CLI (gh)** จำเป็นต้องใช้สำหรับการจัดการโปรเจกต์และรัน GSD.

#### On WSL / Ubuntu (Linux)
```bash
# ติดตั้ง Git และ GitHub CLI
sudo apt update && sudo apt install -y git gh

# ติดตั้ง Packages พื้นฐาน (Python)
sudo apt install -y python3-pip python3-venv

# ติดตั้ง pipx
sudo apt install -y pipx
pipx ensurepath

# ติดตั้ง uv (โดยใช้ curl ตามมาตรฐาน)
curl -LsSf https://astral.sh/uv/install.sh | sh
source $HOME/.local/bin/env
```

#### On macOS
```bash
# ติดตั้ง Git และ GitHub CLI
brew install git gh

# ติดตั้ง pipx
brew install pipx
pipx ensurepath

# ติดตั้ง uv
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### 1.8 การยืนยันตัวตน GitHub (gh auth login)
หลังจากติดตั้ง `gh` เสร็จแล้ว ต้องทำการ Login เพื่อให้ Claude Code สามารถสั่งงาน GitHub ได้:

```bash
gh auth login
```

**ขั้นตอนการตั้งค่า (ทำตามหน้าจอ):**
1. **What account?** -> เลือก `GitHub.com`
2. **Preferred protocol?** -> เลือก `HTTPS`
3. **Authenticate Git with your credentials?** -> เลือก `Yes`
4. **How would you like to authenticate?** -> เลือก `Login with a web browser`
5. คัดลอก **One-time code** และกด **Enter** เพื่อเปิดเบราว์เซอร์
6. วางโค้ดและยืนยันการขอสิทธิ์

### 1.9 ตั้งค่า WSL เพิ่มเติม

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

> [!NOTE]
> แนะนำ **Node.js 24** หรือเวอร์ชัน **20 ขึ้นไป** npm จะติดตั้งมาพร้อมกับ Node.js อัตโนมัติ

#### On WSL / Ubuntu (Linux)
```bash
# ติดตั้ง Node.js 24 ผ่าน NodeSource repository (เวอร์ชันล่าสุด)
curl -fsSL https://deb.nodesource.com/setup_24.x | sudo -E bash -
sudo apt-get install -y nodejs

# ตรวจสอบเวอร์ชัน
node --version
npm --version
```

#### On macOS
```bash
# ติดตั้ง Node.js 24 ผ่าน Homebrew
brew install node@24

# เพิ่ม Node.js เข้า PATH (เพิ่มท้าย ~/.zshrc)
echo 'export PATH="/opt/homebrew/opt/node@24/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# ตรวจสอบเวอร์ชัน
node --version
npm --version
```

> [!TIP]
> **หากต้องการหลายเวอร์ชัน**: สามารถใช้ `fnm` (Fast Node Manager) แทนการติดตั้งแบบ global ได้ ดูวิธีติดตั้งได้ที่ https://fnm.vercel.app

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
