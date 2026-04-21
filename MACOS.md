# คู่มือการติดตั้งบน macOS (macOS Installation Guide)

คู่มือนี้สำหรับผู้ใช้ **macOS** เพื่อติดตั้งเครื่องมือที่จำเป็นสำหรับการทำ Workshop **AI-Accelerated Software Development**

---

## ข้อกำหนดเบื้องต้น (Prerequisites)

คู่มือนี้ถือว่าคุณมี:
- **macOS** รุ่นที่รองรับ (macOS 10.15 Catalina ขึ้นไป)
- **Homebrew** ติดตั้งแล้ว

หากยังไม่มี Homebrew ให้ติดตั้งก่อน:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

---

## 1. ติดตั้งเครื่องมือพื้นฐาน (Git, gh, pipx, uv)

> [!IMPORTANT]
> **Git และ GitHub CLI (gh)** จำเป็นต้องใช้สำหรับการจัดการโปรเจกต์และรัน GSD
>
> เครื่องมือที่จะติดตั้งในส่วนนี้:
> - **Git** - Version control
> - **gh** - GitHub CLI
> - **pipx** - ติดตั้ง Python CLI tools แบบ isolated
> - **uv** - Python package installer ที่เร็ว (รวม `uvx` command สำหรับ run packages โดยไม่ต้อง install)
>
> **หมายเหตุ**: macOS มี Python มาให้ตั้งแต่รุ่น Catalina หากต้องการ Python เวอร์ชันอื่น สามารถติดตั้งผ่าน Homebrew: `brew install python@3.12`

```bash
# ติดตั้ง Git และ GitHub CLI
brew install git gh

# ติดตั้ง pipx
brew install pipx
pipx ensurepath

# ติดตั้ง uv (รวม uvx command สำหรับ run Python packages โดยไม่ต้อง install)
curl -LsSf https://astral.sh/uv/install.sh | sh
```

---

## 2. การยืนยันตัวตน GitHub (gh auth login)

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

## 3. ติดตั้ง Runtime หลัก (Bun)

เราใช้ **Bun** เป็นเครื่องยนต์หลักในการรันโปรเจกต์และ AI Agent

```bash
# ติดตั้ง Bun
curl -fsSL https://bun.com/install | bash
source ~/.zshrc
```

**ตรวจสอบเวอร์ชัน:**
```bash
bun --version  # ควรได้ 1.x.x ขึ้นไป
```

---

## 4. ติดตั้ง Node.js + npm (Global Installation)

> [!IMPORTANT]
> **ต้องติดตั้ง Node.js แบบ Global (ระดับระบบ)** ไม่ใช่ผ่าน NVM หรือ FNM
>
> ปลั๊กอินบางตัวของ Claude Code (เช่น **claude-mem**, **MemPalace**) ใช้ Node.js ใน hooks ที่ทำงานในสภาพแวดล้อมที่สะอาด (clean environment) หากติดตั้ง Node ผ่าน NVM/FNM ปลั๊กอินจะหา `node` ไม่เจอและเกิด error ได้
>
> **แนะนำ**: Node.js **24** หรือเวอร์ชัน **20 ขึ้นไป** | npm จะติดตั้งมาพร้อมกับ Node.js อัตโนมัติ

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

> [!WARNING]
> **อย่าใช้ NVM หรือ FNM** สำหรับ Node.js ที่ใช้กับ Claude Code Plugins
>
> เครื่องมือจัดการเวอร์ชัน Node เหล่านี้จะติดตั้ง Node ในโฟลเดอร์ผู้ใช้ (`~/.nvm/` หรือ `~/.fnm/`) ซึ่งจะ **ไม่พร้อมใช้งาน** ใน hooks ของปลั๊กอินที่ทำงานในสภาพแวดล้อมใหม่ ส่งผลให้เกิด error ได้

---

## 5. ติดตั้ง Claude Code CLI

ติดตั้งเวอร์ชันล่าสุดด้วยคำสั่งเดียว:
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

### เริ่มต้นใช้งาน
```bash
claude
# ทำตามขั้นตอน Login บนหน้าจอ
```

---

## 6. ติดตั้ง Editor และ Extensions

1. ติดตั้ง [VS Code](https://code.visualstudio.com/)
2. **Claude Extension**: ค้นหา "Claude" ใน Marketplace และติดตั้งเวอร์ชันทางการจาก **Anthropic** (หรือ Claude Dev)

---

## เสร็จสิ้น!

เมื่อติดตั้งทุกอย่างเสร็จแล้ว คุณพร้อมที่จะเริ่ม Workshop **AI-Accelerated Software Development** แล้ว!

### ตรวจสอบการติดตั้ง

รันคำสั่งเหล่านี้เพื่อตรวจสอบว่าทุกอย่างติดตั้งถูกต้อง:

```bash
# ตรวจสอบเวอร์ชันของเครื่องมือต่างๆ
git --version      # Git version control
gh --version       # GitHub CLI
python3 --version  # Python (built-in on macOS)
pipx --version     # Python package installer for CLI tools
uv --version       # Modern Python package manager
uvx --version      # Run Python packages without installing
bun --version      # JavaScript runtime
node --version     # Node.js (v24+)
npm --version      # Node package manager
claude --version   # Claude Code CLI
```

หากทุกอย่างแสดงเวอร์ชันถูกต้อง แสดงว่าคุณพร้อมใช้งานแล้ว! 🎉

---

## เคล็ดลับเพิ่มเติม (macOS Tips)

### ใช้ `.zshrc` แทน `.bashrc`
macOS Catalina ขึ้นไปใช้ **zsh** เป็น default shell การแก้ไขค่าตั้งค่าควรทำใน `~/.zshrc`

### ติดตั้งเครื่องมือเพิ่มเติมผ่าน Homebrew
```bash
# เครื่องมือที่มักใช้บน macOS
brew install tree jq fzf ripgrep
```

### ใช้ Terminal ที่ดีกว่า default
แนะนำให้ลองใช้:
- [iTerm2](https://iterm2.com/) - Terminal ที่ทรงพลังกว่า default
- [Warp](https://www.warp.dev/) - Terminal สมัยใหม่ที่ใช้ AI ช่วย
