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
# ติดตั้ง Git จาก Ubuntu apt
sudo apt update && sudo apt install -y git

# ติดตั้ง GitHub CLI จาก official apt repository
(type -p wget >/dev/null || (sudo apt update && sudo apt install wget -y)) && sudo mkdir -p -m 755 /etc/apt/keyrings && out=$(mktemp) && wget -nv -O$out https://cli.github.com/packages/githubcli-archive-keyring.gpg && cat $out | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null && sudo chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg && sudo mkdir -p -m 755 /etc/apt/sources.list.d && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null && sudo apt update && sudo apt install gh -y

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

> **ทำขั้นนี้ก่อนวันงาน** ในห้องเรียนไม่มีเวลาทำ และวันที่สองต้องใช้จริงตอนสร้าง repo ของทีม
> ถ้าไม่เคยใช้ terminal มาก่อน ดูฉบับละเอียดทีละขั้นที่ [GH_AUTH.md](GH_AUTH.md)

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

เราใช้ **Bun** เป็น runtime หลักในการรันโปรเจกต์และ labs ของ Workshop

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
> **แนะนำ**: Node.js **24** หรือเวอร์ชัน **22 ขึ้นไป** | npm จะติดตั้งมาพร้อมกับ Node.js อัตโนมัติ

```bash
# ติดตั้ง Node.js 24 ผ่าน NodeSource repository (เวอร์ชันล่าสุด)
curl -fsSL https://deb.nodesource.com/setup_24.x -o nodesource_setup.sh
sudo -E bash nodesource_setup.sh
sudo apt install -y nodejs

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

## 6. ติดตั้ง Claude Code CLI (จำเป็น)

Claude Code เป็น AI coding agent หลักของ Workshop นี้ และวางหลัง Node.js เพราะ plugin hooks ต้องเห็น global Node ใน clean environment

> [!IMPORTANT]
> ต้องใช้ **paid Claude account**: Pro, Max, Team, Enterprise หรือ Console API; free Claude.ai plan ยังใช้ Claude Code ไม่ได้

ติดตั้งด้วย official install script:

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

ตรวจสอบ:

```bash
claude --version
claude doctor
```

> **rtk** และ **GSD** ติดตั้งในคาบเรียน (S2/S3):
> - rtk: ตั้งค่าให้ใช้กับ Claude Code และเพิ่มกฎ "prefix shell commands with `rtk`" ใน `CLAUDE.md` ตามคำสั่งในคาบเรียน
> - GSD: ใช้ `npx @opengsd/gsd-core@latest` (gsd-core 1.10.0) แล้วเลือก Claude Code/runtime และ global/local แบบ interactive จากนั้นรีสตาร์ท `claude` (คำสั่ง `/gsd-*`)

---

## 7. ติดตั้ง Code Editor (VS Code)

1. ติดตั้ง [VS Code](https://code.visualstudio.com/)
2. ติดตั้ง Claude Code extension: `code --install-extension anthropic.claude-code`

> [!IMPORTANT]
> **ผู้ใช้ WSL**: ติดตั้ง VS Code เวอร์ชัน **Windows** บนเครื่อง Windows (ไม่ใช่ใน Ubuntu)
> ระหว่างติดตั้งให้เลือก **Add to PATH**
> จากนั้นติดตั้ง extension `ms-vscode-remote.remote-wsl` ใน VS Code ฝั่ง Windows
> แล้วเปิด project ด้วยคำสั่ง `code .` จาก Ubuntu shell

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
claude --version   # Claude Code CLI
claude doctor      # Claude Code diagnostics
```

หากทุกอย่างแสดงเวอร์ชันถูกต้อง แสดงว่าคุณพร้อมใช้งานแล้ว! 🎉
