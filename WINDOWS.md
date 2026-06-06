# คู่มือการติดตั้งบน Windows (Native — ไม่ต้องใช้ WSL)

> คู่มือนี้ใช้ **winget** (Windows Package Manager) ติดตั้งทุกอย่างใน PowerShell โดยตรง
> ไม่จำเป็นต้องติดตั้ง WSL

---

## ข้อกำหนดเบื้องต้น

- Windows 10 (1809+) หรือ Windows 11
- PowerShell 5+ (มาพร้อม Windows แล้ว) หรือ Windows Terminal (แนะนำ)
- Google Account (สำหรับ Antigravity)
- GitHub Account

---

## ขั้นตอนการติดตั้ง

### 1. ตรวจสอบ winget

```powershell
winget --version
```

ถ้าไม่มี ให้ดาวน์โหลด [App Installer จาก Microsoft Store](https://apps.microsoft.com/store/detail/app-installer/9NBLGGH4NNS1)

---

### 2. Git

```powershell
winget install --id Git.Git --exact
```

หลังติดตั้ง ปิดแล้วเปิด PowerShell ใหม่ จากนั้นตั้งค่า:

```powershell
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

---

### 3. GitHub CLI

```powershell
winget install --id GitHub.cli --exact
```

Login:

```powershell
gh auth login
```

---

### 4. Node.js (Global — ไม่ใช้ NVM)

> ต้องติดตั้งแบบ global เท่านั้น เพื่อให้ Claude Code Plugins ทำงานได้

```powershell
winget install --id OpenJS.NodeJS.LTS --exact
```

ตรวจสอบ:

```powershell
node --version
npm --version
```

---

### 5. Bun

```powershell
powershell -c "irm bun.sh/install.ps1 | iex"
```

ตรวจสอบ:

```powershell
bun --version
```

---

### 6. Python tools (pipx + uv)

```powershell
winget install --id Python.Python.3.12 --exact
```

ปิดแล้วเปิด PowerShell ใหม่:

```powershell
pip install pipx
pipx ensurepath
pipx install uv
```

ตรวจสอบ:

```powershell
uv --version
```

---

### 7. Antigravity (Google AI Coding Agent)

Antigravity มี 3 ส่วน — ติดตั้งทีละส่วน:

#### 7a. Antigravity CLI (`agy`)

```powershell
winget install --id Google.Antigravity --exact
```

Binary ชื่อ `agy` (ไม่ใช่ `antigravity`) ติดตั้งที่ `%LOCALAPPDATA%\Antigravity\`

ตรวจสอบ:

```powershell
agy --version
```

Login ด้วย Google Account:

```powershell
agy auth login
```

#### 7b. Antigravity 2.0 Desktop App

ดาวน์โหลดจาก [antigravity.google](https://antigravity.google) หรือ:

```powershell
winget install --id Google.Antigravity.Desktop --exact
```

#### 7c. Antigravity IDE Extension (VS Code)

ถ้าใช้ VS Code:

```powershell
code --install-extension google.antigravity-ide
```

ถ้าใช้ **Zed**: Antigravity IDE ยังไม่ integrate กับ Zed โดยตรง — ใช้ Zed AI แทน (built-in)

---

### 8. Zed Editor (Student Plan — $10/month)

ดาวน์โหลดจาก [zed.dev](https://zed.dev) หรือ:

```powershell
winget install --id Zed.Zed --exact
```

> สำหรับ Student Plan: สมัครผ่าน [zed.dev/pricing](https://zed.dev/pricing) ด้วย email มหาวิทยาลัย

---

### 9. Claude Code CLI (Optional — สำหรับดู instructor demo)

```powershell
npm install -g @anthropic-ai/claude-code
```

ตรวจสอบ:

```powershell
claude --version
```

---

## ตรวจสอบการติดตั้งทั้งหมด

```powershell
git --version
gh --version
node --version
bun --version
uv --version
agy --version
```

ถ้าทุกคำสั่งแสดงเวอร์ชัน = พร้อมสำหรับ Workshop

---

## ปัญหาที่พบบ่อย

| ปัญหา | วิธีแก้ |
|:---|:---|
| `agy` ไม่รู้จัก | ปิด PowerShell แล้วเปิดใหม่ หรือเพิ่ม `%LOCALAPPDATA%\Antigravity\` ใน PATH |
| `bun` ไม่รู้จัก | ปิด PowerShell แล้วเปิดใหม่ (installer แก้ไข PATH อัตโนมัติ) |
| winget ไม่มี | ติดตั้ง App Installer จาก Microsoft Store |
| `agy auth login` เปิด browser ไม่ได้ | ใช้ `agy auth login --no-browser` แล้ววาง URL ใน browser เอง |

---

## หมายเหตุ: ทำไมไม่ใช้ WSL?

Antigravity 2.0 มีปัญหากับ WSL:
- `agy` launcher เสียหลังอัปเดต 2.0
- OAuth login ล้มเหลวใน WSL boundary
- Skills ติดตั้งไปที่ Windows filesystem แต่ Antigravity ใน WSL อ่านไม่ได้
- Browser agent ไม่สามารถสื่อสารกับ Chrome บน Windows host

ใช้ native Windows ตรงๆ ดีกว่า WSL สำหรับ Antigravity ทุกกรณี
