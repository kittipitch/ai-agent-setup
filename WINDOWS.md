# คู่มือการติดตั้งบน Windows (Native — ไม่ต้องใช้ WSL)

> คู่มือนี้ใช้ **winget** (Windows Package Manager) ติดตั้งทุกอย่างใน PowerShell โดยตรง
> ไม่จำเป็นต้องติดตั้ง WSL

---

## ข้อกำหนดเบื้องต้น

- Windows 10 (1809+) หรือ Windows 11
- PowerShell 7.4+ (จำเป็นสำหรับ Microsoft Coreutils — ดูขั้นตอนที่ 2)
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

### 2. PowerShell 7.4+ และ Microsoft Coreutils

> PowerShell 7.4+ จำเป็นสำหรับ Microsoft Coreutils (Coreutils ไม่ทำงานบน PowerShell 5)

ติดตั้ง PowerShell 7:

```powershell
winget install --id Microsoft.PowerShell --exact
```

ปิดแล้วเปิด PowerShell ใหม่ (ใช้ PowerShell 7 จากนี้ไป) จากนั้นติดตั้ง Microsoft Coreutils:

```powershell
winget install --id Microsoft.Coreutils --exact
```

Microsoft Coreutils นำ Unix commands มาสู่ Windows โดยไม่ต้องใช้ WSL: `ls`, `grep`, `find`, `cat`, `cp`, `mv` และอีก 70+ คำสั่ง

> **หมายเหตุ PATH**: Coreutils ต้องอยู่ก่อน `system32` ใน PATH เพื่อ override Windows built-ins บางคำสั่ง (เช่น `find`) ตรวจสอบหลังติดตั้ง:

```powershell
Get-Command find | Select-Object -ExpandProperty Source
```

ตรวจสอบ:

```powershell
grep --version
find --version
```

---

### 3. Git

```powershell
winget install --id Git.Git --exact
```

หลังติดตั้ง ปิดแล้วเปิด PowerShell ใหม่ จากนั้นตั้งค่า:

```powershell
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

---

### 4. GitHub CLI

```powershell
winget install --id GitHub.cli --exact
```

Login:

```powershell
gh auth login
```

---

### 5. Node.js (Global — ไม่ใช้ NVM)

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

### 6. Bun

```powershell
powershell -c "irm bun.sh/install.ps1 | iex"
```

ตรวจสอบ:

```powershell
bun --version
```

---

### 7. Python tools (pipx + uv)

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

### 8. Antigravity (Google AI Coding Agent)

Antigravity มี 3 ส่วน — ติดตั้งทีละส่วน:

#### 8a. Antigravity CLI (`agy`)

ติดตั้งด้วย PowerShell one-liner (official install method):

```powershell
irm https://antigravity.google/cli/install.ps1 | iex
```

ปิดแล้วเปิด PowerShell ใหม่ จากนั้นตรวจสอบ:

```powershell
agy --version
```

Login: เปิด `agy` ครั้งแรก — login อัตโนมัติผ่าน browser + keyring ของ OS

```powershell
agy
```

> ไม่มีคำสั่ง `agy auth login` — agy login อัตโนมัติเมื่อเปิดครั้งแรก (logout ใช้ `/logout` ใน session)

#### 8b. Antigravity 2.0 Desktop + Antigravity IDE

ไปที่ [antigravity.google/download](https://antigravity.google/download) แล้วดาวน์โหลด **ทั้งสองตัว**:

- **Antigravity 2.0** — Desktop app (multi-agent orchestration)
- **Antigravity IDE** — IDE / editor

> ถ้าใช้ **Zed**: ใช้ Zed AI (built-in) แทน Antigravity IDE

---

### 9. Zed Editor (Student Plan — $10/month)

ดาวน์โหลดจาก [zed.dev](https://zed.dev) หรือ:

```powershell
winget install --id Zed.Zed --exact
```

> สำหรับ Student Plan: สมัครผ่าน [zed.dev/pricing](https://zed.dev/pricing) ด้วย email มหาวิทยาลัย

---

### 10. Claude Code CLI (Optional — สำหรับดู instructor demo)

```powershell
npm install -g @anthropic-ai/claude-code
```

ตรวจสอบ:

```powershell
claude --version
```

---

### 11. agy Plugins (Skills)

ติดตั้ง plugins ที่ใช้ใน Workshop (ใช้ full GitHub URL — agy ยังไม่รองรับ shorthand `user/repo`):

```powershell
# caveman — terse output, ประหยัด token
agy plugins install https://github.com/JuliusBrussee/caveman

# superpower — surgical TDD (ใช้ใน S2)
agy plugins install https://github.com/obra/superpowers
```

ปิดแล้วเปิด `agy` ใหม่ — พิมพ์ `/caveman` เพื่อทดสอบ

> **rtk** และ **GSD** ติดตั้งในคาบเรียน (S2/S3):
> - rtk: `rtk init -g --gemini` + เพิ่มกฎ "prefix shell commands with `rtk`" ใน GEMINI.md
> - GSD: `ln -s ~/.gemini/antigravity-cli ~/.gemini/antigravity` แล้ว `npx @opengsd/gsd-core@latest`

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
| `agy` ไม่รู้จัก | ปิด PowerShell แล้วเปิดใหม่ (installer แก้ไข PATH อัตโนมัติ) |
| `bun` ไม่รู้จัก | ปิด PowerShell แล้วเปิดใหม่ (installer แก้ไข PATH อัตโนมัติ) |
| winget ไม่มี | ติดตั้ง App Installer จาก Microsoft Store |
| `agy auth login` เปิด browser ไม่ได้ | ใช้ `agy auth login --no-browser` แล้ววาง URL ใน browser เอง |
| Coreutils `find` ยังชี้ไปที่ Windows built-in | ตรวจสอบ PATH: Coreutils ต้องอยู่ก่อน `system32` ใน PATH |
| PowerShell 5 ไม่รองรับ Coreutils | อัปเกรดเป็น PowerShell 7 ก่อน: `winget install --id Microsoft.PowerShell --exact` |

