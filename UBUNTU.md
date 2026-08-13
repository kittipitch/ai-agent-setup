# คู่มือการติดตั้งบน Ubuntu/WSL (Ubuntu Installation Guide)

คู่มือนี้สำหรับผู้ใช้ **Ubuntu** (หรือ **WSL** บน Windows) เพื่อติดตั้งเครื่องมือที่จำเป็นสำหรับการทำ Workshop **AI-Accelerated Software Development**

> [!NOTE]
> หากคุณใช้ **Windows** และยังไม่ได้ติดตั้ง WSL ให้ดู [WSL.md](WSL.md) ก่อน

---

## 1. อัปเดตและอัปเกรดระบบ

```bash
sudo apt update; sudo apt upgrade -y
```

---

## 2. ติดตั้งเครื่องมือพื้นฐาน (Git, gh, pipx, uv)

> [!IMPORTANT]
> **Git และ GitHub CLI (gh)** จำเป็นต้องใช้สำหรับการจัดการโปรเจกต์และรัน GSD
>
> เครื่องมือที่จะติดตั้งในส่วนนี้:
> - **Git** - Version control
> - **gh** - GitHub CLI
> - **python3-pip, python3-venv** - Python packages พื้นฐาน
> - **pipx** - ติดตั้ง Python CLI tools แบบ isolated
> - **uv** - Python package installer ที่เร็ว (รวม `uvx` command สำหรับ run packages โดยไม่ต้อง install)

```bash
# ติดตั้ง Git และ GitHub CLI
sudo apt update && sudo apt install -y git gh

# ติดตั้ง Packages พื้นฐาน (Python)
sudo apt install -y python3-pip python3-venv

# ติดตั้ง pipx
sudo apt install -y pipx
pipx ensurepath

# ติดตั้ง uv (รวม uvx command สำหรับ run Python packages โดยไม่ต้อง install)
curl -LsSf https://astral.sh/uv/install.sh | sh
source $HOME/.local/bin/env
```

---

## 3. การยืนยันตัวตน GitHub (gh auth login)

หลังจากติดตั้ง `gh` เสร็จแล้ว ต้องทำการ Login เพื่อให้ Claude Code สามารถสั่งงาน GitHub ได้:

```bash
gh auth login
```

**ขั้นตอนการตั้งค่า (ทำตามหน้าจอ):**
1. **What account?** → เลือก `GitHub.com`
2. **Preferred protocol?** → เลือก `HTTPS`
3. **Authenticate Git with your credentials?** → เลือก `Yes`
4. **How would you like to authenticate?** → เลือก `Login with a web browser`
5. คัดลอก **One-time code** และกด **Enter** เพื่อเปิดbrowser
6. วางโค้ดและยืนยันการขอสิทธิ์

---

## 4. ติดตั้ง Runtime หลัก (Bun)

เราใช้ **Bun** เป็น runtime หลักในการรันโปรเจกต์และ AI Agent

ติดตั้ง `unzip` ก่อน (Bun ต้องการ):
```bash
sudo apt update && sudo apt install -y unzip
```

ติดตั้ง Bun:
```bash
curl -fsSL https://bun.com/install | bash
source ~/.bashrc
```

**ตรวจสอบเวอร์ชัน:**
```bash
bun --version  # ควรได้ 1.x.x ขึ้นไป
```

---

## 5. ติดตั้ง Node.js + npm (Global Installation)

> [!IMPORTANT]
> **ต้องติดตั้ง Node.js แบบ Global** ไม่ใช่ผ่าน NVM หรือ FNM
>
> Plugin บางตัวของ Claude Code (เช่น **claude-mem**, **MemPalace**) ใช้ Node.js ใน hooks ที่ทำงานใน clean environment หากติดตั้ง Node ผ่าน NVM/FNM plugin จะหา `node` ไม่เจอและเกิด error ได้
>
> **แนะนำ**: Node.js **24** หรือเวอร์ชัน **20 ขึ้นไป** | npm จะติดตั้งมาพร้อมกับ Node.js อัตโนมัติ

```bash
# ติดตั้ง Node.js 24 ผ่าน NodeSource repository (เวอร์ชันล่าสุด)
curl -fsSL https://deb.nodesource.com/setup_24.x | sudo -E bash -
sudo apt-get install -y nodejs

# ตรวจสอบเวอร์ชัน
node --version
npm --version
```

> [!WARNING]
> **อย่าใช้ NVM หรือ FNM** สำหรับ Node.js ที่ใช้กับ Claude Code Plugins
>
> Node version manager เหล่านี้จะติดตั้ง Node ในโฟลเดอร์ผู้ใช้ (`~/.nvm/` หรือ `~/.fnm/`) ซึ่งจะ **ไม่พร้อมใช้งาน** ใน hooks ของ plugin ที่ทำงานใน clean environment ส่งผลให้เกิด error ได้

> [!TIP]
> **สำหรับผู้ใช้ WSL:** เพื่อป้องกันปัญหาคำสั่ง `npm`/`npx` ของ Windows ขัดแย้งกับของ Ubuntu ให้รันคำสั่งนี้เพื่อเพิ่มการตั้งค่าลงในทั้ง `~/.bashrc` และ `~/.zshrc`
> ```bash
> tee -a ~/.bashrc ~/.zshrc > /dev/null << 'EOF'
> 
> # Remove Windows npm/npx from PATH to avoid conflicts
> if [[ -e /proc/sys/fs/binfmt_misc/WSLInterop ]]; then
>   export PATH=$(echo "$PATH" | tr ':' '\n' | grep -v '/mnt/c/Program Files/nodejs' | tr '\n' ':' | sed 's/:$//')
> fi
> EOF
> source ~/.bashrc 2>/dev/null; source ~/.zshrc 2>/dev/null
> ```

---

## 6. ติดตั้ง Antigravity Ecosystem

Antigravity มี 3 ส่วน — ติดตั้งทีละส่วน:

### 6a. Antigravity CLI (`agy`)

> ตั้งแต่วันที่ 18 มิถุนายน 2026 Gemini CLI หยุดให้บริการสำหรับ free/student accounts — `agy` คือเครื่องมือทดแทนอย่างเป็นทางการ

ติดตั้งด้วย official install script:

```bash
curl -fsSL https://antigravity.google/cli/install.sh | bash
```

ปิดแล้วเปิด Terminal ใหม่ จากนั้นตรวจสอบ:

```bash
agy --version
```

Login: เปิด `agy` ครั้งแรก — login อัตโนมัติด้วย Google Account ผ่าน browser + keyring ของ OS (ไม่มีคำสั่ง `agy auth login`):

```bash
agy
```

> ถ้า browser เปิดไม่ได้ (SSH session หรือ WSL): agy จะแสดง auth URL ใน terminal — เปิด URL นั้นใน browser บนเครื่อง local แล้วนำ code กลับมาวางใน terminal

### 6b. Antigravity 2.0 Desktop App

ดาวน์โหลดจาก [antigravity.google](https://antigravity.google)

> Linux: ดาวน์โหลด `.deb` หรือ `.AppImage` จากหน้าดาวน์โหลดบนเว็บไซต์

### 6c. Antigravity IDE Extension (VS Code)

ถ้าใช้ VS Code:

```bash
code --install-extension google.antigravity-ide
```

<!-- Zed dropped (not using): ถ้าใช้ **Zed**: Antigravity CLI integrate ผ่าน ACP (Agent Client Protocol) อัตโนมัติ — ไม่ต้องติดตั้ง extension แยก -->

---

## 7. ติดตั้ง Code Editor (VS Code)

1. ติดตั้ง [VS Code](https://code.visualstudio.com/)
2. ติดตั้ง Antigravity IDE extension: `code --install-extension google.antigravity-ide`
3. **สำหรับ WSL**: ติดตั้ง Extension ชื่อ **"WSL"**

<!-- Zed dropped (not using):
ดาวน์โหลดจาก [zed.dev](https://zed.dev) หรือ:

```bash
curl https://zed.dev/install.sh | sh
```

> สำหรับ Student Plan: สมัครผ่าน [zed.dev/pricing](https://zed.dev/pricing) ด้วย email มหาวิทยาลัย
-->


---

## 8. Claude Code CLI (Optional — สำหรับดู instructor demo)

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

ตรวจสอบ:

```bash
claude --version
```

> **rtk** และ **GSD** ติดตั้งในคาบเรียน (S2/S3):
> - rtk: `rtk init -g --gemini` + เพิ่มกฎ "prefix shell commands with `rtk`" ใน GEMINI.md
> - GSD: `npx @opengsd/gsd-core@latest --antigravity --global --config-dir ~/.gemini/config --profile=full` แล้วรีสตาร์ท `agy` (คำสั่ง `/gsd-*`)

---

## เสร็จสิ้น!

เมื่อติดตั้งทุกอย่างเสร็จแล้ว คุณพร้อมที่จะเริ่ม Workshop **AI-Accelerated Software Development** แล้ว!

### ตรวจสอบการติดตั้ง

รันคำสั่งเหล่านี้เพื่อตรวจสอบว่าทุกอย่างติดตั้งถูกต้อง:

```bash
# ตรวจสอบเวอร์ชันของเครื่องมือต่างๆ
git --version      # Git version control
gh --version       # GitHub CLI
python3 --version  # Python
pipx --version     # Python package installer for CLI tools
uv --version       # Modern Python package manager
uvx --version      # Run Python packages without installing
bun --version      # JavaScript runtime
node --version     # Node.js (v24+)
npm --version      # Node package manager
agy --version      # Antigravity CLI
claude --version   # Claude Code CLI (Optional)
```

หากทุกอย่างแสดงเวอร์ชันถูกต้อง แสดงว่าคุณพร้อมใช้งานแล้ว! 🎉
