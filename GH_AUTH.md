# GitHub CLI (gh): ติดตั้งและ Login ก่อนวันงาน

เอกสารนี้สำหรับคนที่**ไม่เคยใช้ terminal มาก่อน** ใช้เวลา ~15 นาที ทำครั้งเดียว
ที่บ้าน **ก่อนวันงาน** — ในงานจะไม่มีเวลาทำขั้นตอนนี้ และ Wi-Fi ของสถานที่จัดงาน
อาจทำให้ขั้นตอนนี้ยากขึ้นมาก

จบแล้วคุณจะได้: เครื่องที่สั่งงาน GitHub ได้จาก terminal — Claude Code และ
กิจกรรมทีมวันที่สอง (สร้าง repo, push งานของทีม) ใช้สิ่งนี้

## ก่อนเริ่ม: เปิด terminal อย่างไร

- **macOS**: กด `Cmd + Space` พิมพ์ `Terminal` กด Enter
- **Windows**: เปิด **Windows Terminal** แล้วเลือกแท็บ **Ubuntu**
  (ต้องติดตั้ง WSL2 + Ubuntu ตาม [WSL.md](WSL.md) ก่อน) —
  **คำสั่งทั้งหมดในเอกสารนี้พิมพ์ในแท็บ Ubuntu เท่านั้น**
- **Ubuntu/Linux**: กด `Ctrl + Alt + T`

พิมพ์คำสั่งทีละบรรทัด กด Enter แล้วรอให้ขึ้นบรรทัดใหม่ก่อนพิมพ์คำสั่งถัดไป

## ขั้นที่ 1 — สร้าง GitHub account (ทำใน browser)

ข้ามขั้นนี้ได้ถ้ามี account อยู่แล้วและจำ password ได้

1. เปิด browser ไปที่ `https://github.com/signup`
2. กรอก **email** ที่ใช้จริง → ตั้ง **password** → ตั้ง **username**
   (username จะโผล่ใน URL ของ repo ทีม เลือกที่พิมพ์ง่าย)
3. GitHub ส่งรหัสตัวเลขไปที่ email — เปิด email แล้วกรอกรหัสยืนยัน
4. คำถามแบบสอบถามหลังสมัคร (ใช้ทำอะไร ทีมกี่คน) ตอบอะไรก็ได้หรือกด skip
5. ทดสอบ: logout แล้ว login ใหม่หนึ่งครั้ง ให้แน่ใจว่าจำ password ได้จริง

## ขั้นที่ 2 — ติดตั้ง gh

### macOS

ต้องมี Homebrew ก่อน (ดู [MACOS.md](MACOS.md) หัวข้อ Prerequisites) แล้ว:

```bash
brew install gh
```

### Windows (ใน Ubuntu shell ของ WSL2) และ Ubuntu/Linux

คำสั่งเดียวกัน — copy ทั้งก้อนนี้ไปวางใน terminal แล้วกด Enter
(เป็นคำสั่งติดตั้งจาก official apt repository ของ GitHub ตาม
[UBUNTU.md](UBUNTU.md) ข้อ 2):

```bash
(type -p wget >/dev/null || (sudo apt update && sudo apt install wget -y)) && sudo mkdir -p -m 755 /etc/apt/keyrings && out=$(mktemp) && wget -nv -O$out https://cli.github.com/packages/githubcli-archive-keyring.gpg && cat $out | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null && sudo chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg && sudo mkdir -p -m 755 /etc/apt/sources.list.d && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null && sudo apt update && sudo apt install gh -y
```

ถ้าถาม password ให้พิมพ์ password ของเครื่อง (ตอนพิมพ์**จอจะไม่แสดงอะไรเลย** —
ปกติ พิมพ์ให้จบแล้วกด Enter)

### ตรวจว่าติดตั้งสำเร็จ (ทุกแพลตฟอร์ม)

```bash
gh --version
```

ต้องขึ้นประมาณ `gh version 2.x.x (...)` ถ้าขึ้น `command not found`
ให้ปิด terminal เปิดใหม่แล้วลองอีกครั้ง ยังไม่ได้ → ดูตาราง "ถ้าพัง" ท้ายไฟล์

## ขั้นที่ 3 — Login (`gh auth login`)

ขั้นนี้เชื่อม terminal กับ account จากขั้นที่ 1 ทำครั้งเดียว เครื่องจะจำไว้

พิมพ์:

```bash
gh auth login
```

`gh` จะถามทีละคำถาม เลื่อนลูกศรขึ้นลงแล้วกด Enter เพื่อเลือก
ตอบตามนี้ทุกข้อ:

| ลำดับ | คำถามบนจอ (โดยประมาณ) | ตอบ |
|---|---|---|
| 1 | What account do you want to log into? | `GitHub.com` |
| 2 | What is your preferred protocol for Git operations? | `HTTPS` |
| 3 | Authenticate Git with your GitHub credentials? | `Yes` |
| 4 | How would you like to authenticate GitHub CLI? | `Login with a web browser` |

จากนั้น:

5. จอแสดงรหัสใช้ครั้งเดียวรูปแบบ `XXXX-XXXX` พร้อมข้อความ
   `First copy your one-time code` — **copy หรือจดรหัสนี้ไว้** แล้วกด Enter
6. Browser จะเปิดหน้า `https://github.com/login/device` เอง
   (ถ้าไม่เปิด ให้เปิด browser แล้วพิมพ์ URL นี้เอง — รหัสยังอยู่บนจอ terminal)
7. ถ้า browser ให้ login GitHub ก่อน → login ด้วย account จากขั้นที่ 1
8. วางรหัส `XXXX-XXXX` → กด **Continue** → กด **Authorize github**
9. กลับมาดู terminal — ต้องขึ้นข้อความประมาณ `Logged in as <username>`

## ขั้นที่ 4 — ตรวจว่าสำเร็จ

```bash
gh auth status
```

**สำเร็จ** หน้าตาแบบนี้ (มีเครื่องหมายถูกหน้าบรรทัดแรก):

```text
github.com
  - Logged in to github.com account <username> (keyring)
  - Active account: true
  - Git operations protocol: https
  - Token: gho_************
```

เห็นแบบนี้ = เสร็จสมบูรณ์ ปิดเครื่องได้ วันงานไม่ต้องทำซ้ำ

**ไม่สำเร็จ** จะขึ้นข้อความทำนอง `You are not logged into any GitHub hosts` —
กลับไปทำขั้นที่ 3 ใหม่ ถ้าซ้ำสองรอบยังไม่ผ่าน ดูตารางถัดไป
แล้ว**มางานตามปกติ** — กิจกรรมทีมต้องการคนที่ login สำเร็จแค่ 1 คนจาก 5-6 คน
และในงานมีผู้สอนช่วยดูให้ได้

## ถ้าพัง

| อาการที่เห็น | สาเหตุที่พบบ่อย | ทำอย่างไร |
|---|---|---|
| `gh: command not found` หลังติดตั้ง | terminal เก่ายังไม่รู้จักคำสั่งใหม่ | ปิด terminal เปิดใหม่ ลอง `gh --version` อีกครั้ง |
| `command not found` บน Windows | ไม่ได้พิมพ์ในแท็บ Ubuntu | เปิด Windows Terminal เลือกแท็บ **Ubuntu** แล้วทำขั้นที่ 2 ใหม่ในนั้น |
| กด Enter แล้ว browser ไม่เปิด (พบบ่อยใน WSL) | terminal เรียก browser ของ Windows ไม่ได้ | ไม่เป็นปัญหา — เปิด browser เองไปที่ `https://github.com/login/device` แล้ววางรหัสจากจอ terminal |
| หน้า device ขึ้นว่า code หมดอายุ | รอนานเกินก่อนวางรหัส | รัน `gh auth login` ใหม่ ได้รหัสใหม่ |
| Browser login ไม่ผ่าน ติด 2FA/verify | GitHub บังคับยืนยันเพิ่มเติม | ทำตามที่หน้าเว็บบอกให้จบใน browser ก่อน (ผูก authenticator หรือ email code) แล้วค่อยกลับมา `gh auth login` |
| เครื่อง/network ที่ทำงานบล็อกการ login | proxy หรือ policy ขององค์กร | ลองใหม่จาก network บ้านหรือ hotspot มือถือ ถ้ายังไม่ได้ ให้มางานพร้อมข้อจำกัดนี้ — ไม่ต้องเป็นคน push ของทีม |

## คำถามที่จะมีคนถาม

- **ปลอดภัยไหม?** `gh auth login` ใช้ระบบ device authorization ของ GitHub เอง
  — password ไม่ผ่าน terminal เลย สิ่งที่เก็บในเครื่องคือ token ที่เพิกถอนได้
  ทุกเมื่อที่ `github.com/settings/apps` → Authorized OAuth Apps
- **ต้องเสียเงินไหม?** ไม่ — free account ใช้ได้ครบทุกอย่างใน workshop นี้
  รวมถึง private repo
- **มี account องค์กรอยู่แล้ว ใช้ได้ไหม?** ได้ ถ้า login ได้ปกติใน browser
  แต่ถ้าองค์กรมี policy เข้ม แนะนำสมัคร account ส่วนตัวใหม่ตามขั้นที่ 1
  จะเดาปัญหาได้น้อยกว่า
