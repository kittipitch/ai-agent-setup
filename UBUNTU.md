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
5. คัดลอก **One-time code** และกด **Enter** เพื่อเปิดเบราว์เซอร์
6. วางโค้ดและยืนยันการขอสิทธิ์

---

## 4. ติดตั้ง Runtime หลัก (Bun)

เราใช้ **Bun** เป็นเครื่องยนต์หลักในการรันโปรเจกต์และ AI Agent

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
> **ต้องติดตั้ง Node.js แบบ Global (ระดับระบบ)** ไม่ใช่ผ่าน NVM หรือ FNM
>
> ปลั๊กอินบางตัวของ Claude Code (เช่น **claude-mem**, **MemPalace**) ใช้ Node.js ใน hooks ที่ทำงานในสภาพแวดล้อมที่สะอาด (clean environment) หากติดตั้ง Node ผ่าน NVM/FNM ปลั๊กอินจะหา `node` ไม่เจอและเกิด error ได้
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
> เครื่องมือจัดการเวอร์ชัน Node เหล่านี้จะติดตั้ง Node ในโฟลเดอร์ผู้ใช้ (`~/.nvm/` หรือ `~/.fnm/`) ซึ่งจะ **ไม่พร้อมใช้งาน** ใน hooks ของปลั๊กอินที่ทำงานในสภาพแวดล้อมใหม่ ส่งผลให้เกิด error ได้

---

## 6. ติดตั้ง Claude Code CLI

ติดตั้งเวอร์ชันล่าสุดด้วยคำสั่งเดียว:
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

### 🚀 เริ่มต้นใช้งาน
```bash
claude
# ทำตามขั้นตอน Login บนหน้าจอ
```

---

## 7. ติดตั้ง Editor และ Extensions

1. ติดตั้ง [VS Code](https://code.visualstudio.com/)
2. **สำหรับ WSL**: ติดตั้ง Extension ชื่อ **"WSL"**
3. **Claude Extension**: ค้นหา "Claude" ใน Marketplace และติดตั้งเวอร์ชันทางการจาก **Anthropic** (หรือ Claude Dev)

---

## ✅ เสร็จสิ้น!

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
claude --version   # Claude Code CLI
```

หากทุกอย่างแสดงเวอร์ชันถูกต้อง แสดงว่าคุณพร้อมใช้งานแล้ว! 🎉
