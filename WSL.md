# คู่มือการติดตั้ง WSL สำหรับ Windows (WSL Setup Guide)

> [!NOTE]
> ถ้ายังไม่ได้อ่าน [WINDOWS.md](WINDOWS.md) ให้อ่านก่อน - ข้อกำหนดเบื้องต้นและเหตุผลที่ Workshop นี้ใช้ WSL อยู่ในนั้น

คู่มือนี้สำหรับผู้ใช้ **Windows** ทุกคน **WSL2 (Windows Subsystem for Linux)** คือสภาพแวดล้อมที่ Workshop นี้ใช้ เพื่อให้ได้สภาพแวดล้อมแบบ Linux สำหรับการทำ Workshop **AI-Accelerated Software Development**

---

## 1. เปิด PowerShell แบบ Administrator

1. กดปุ่ม **Win + R**
2. พิมพ์ "powershell"
3. กด **Ctrl + Shift + Enter**
4. เลือก "Yes" ที่หน้าต่างป๊อปอัป

![ยืนยันการเปิด PowerShell](https://github.com/kittipitch/cs111env/raw/main/images/windows/img14_win_popup.png)

---

## 2. เปิดใช้งานฟีเจอร์ WSL

บน Windows 10 22H2 หรือ Windows 11 คำสั่ง `wsl --install` ในขั้นตอนถัดไปจะเปิด WSL features ที่จำเป็นให้อัตโนมัติ

> [!NOTE]
> ถ้า `wsl --install` ล้มเหลวเพราะ WSL features ยังไม่ถูกเปิด ให้เปิด PowerShell (Administrator) แล้วรัน fallback นี้ จากนั้นรีสตาร์ทเครื่อง:
>
> ```powershell
> # เปิดใช้งาน Windows Subsystem for Linux
> dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
>
> # เปิดใช้งาน Virtual Machine Platform
> dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
> ```

![เปิดฟีเจอร์ WSL](https://github.com/kittipitch/cs111env/raw/main/images/windows/img06_win_features_on.png)

---

## 3. ติดตั้ง WSL และ Ubuntu

เปิด PowerShell (Administrator) แล้วรัน:

```powershell
# อัปเดต WSL
wsl --update

# ติดตั้ง WSL
wsl --install --no-distribution

# ตั้งค่า WSL 2 เป็น default version
wsl --set-default-version 2

# ติดตั้ง Ubuntu 24.04
wsl --install -d Ubuntu-24.04
```

*เมื่อเสร็จแล้ว ให้ตั้ง **Username** และ **Password** ใน Ubuntu Terminal*

---

## 4. ติดตั้ง Windows Terminal

ดาวน์โหลดจาก: https://aka.ms/terminal

ตั้งค่า **Default Profile** เป็น "Ubuntu-24.04"

![ตั้งค่า Windows Terminal](https://github.com/kittipitch/cs111env/raw/main/images/windows/img02_win_term_profile_4.png)

---

## 5. ตั้งค่าให้ Ubuntu เห็นไฟล์ Windows

เปิด WSL Terminal แล้วรัน:

```bash
# สร้าง symlink โฟลเดอร์ Windows มาที่ Ubuntu
HOME_DIR=$(wslpath -u "$(cmd.exe /c echo %USERPROFILE% 2>/dev/null | tr -d '\r')")
[[ -e "$HOME_DIR/OneDrive/Desktop" ]] && \
ln -sf "$HOME_DIR/OneDrive/Desktop" ~/ || \
ln -sf "$HOME_DIR/Desktop" ~/
[[ -e "$HOME_DIR/OneDrive/Documents" ]] && \
ln -sf "$HOME_DIR/OneDrive/Documents" ~/ || \
ln -sf "$HOME_DIR/Documents" ~/
ln -sf "$HOME_DIR/Downloads" ~/
```

---

## 6. ตั้งค่า WSL เพิ่มเติม

### แก้ไข permissions และเปิดใช้งาน systemd

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

### จำกัดทรัพยากร WSL (ป้องกันใช้ RAM/เครื่องมากเกินไป)

สร้างไฟล์ `.wslconfig` ในโฟลเดอร์ผู้ใช้ Windows:

```bash
HOME_DIR=$(wslpath -u "$(cmd.exe /c echo %USERPROFILE% 2>/dev/null | tr -d '\r')")
nano "$HOME_DIR/.wslconfig"
```

เพิ่มข้อมูลต่อไปนี้ (ปรับค่าตามสเปคเครื่อง):

**เครื่องสเปคดี (RAM 16GB ขึ้นไป):**

```ini
[wsl2]
memory=8GB
processors=4
```

**เครื่องสเปคต่ำ (RAM 8GB):**

```ini
[wsl2]
memory=4GB
processors=2
```

> **หมายเหตุ**: `memory` คือ RAM สูงสุดที่ WSL ใช้ได้, `processors` คือจำนวน CPU cores
> ขั้นต่ำที่แนะนำสำหรับ Workshop นี้คือ **RAM 4GB และ 2 cores** อย่าตั้งต่ำกว่านี้
> (ต่ำกว่านี้ `claude`, `bun install`, และ Node hooks จะช้าหรือค้าง) ถ้าเครื่อง RAM มากกว่านี้
> ตั้งให้สูงขึ้นได้ตามสัดส่วน แต่เว้น RAM ไว้ให้ Windows อย่างน้อย 4-5GB
>
> **ค่า default ของ WSL2 (ถ้าไม่สร้าง `.wslconfig`)**: RAM = 50% ของเครื่อง หรือ 8GB แล้วแต่ค่าไหนต่ำกว่า,
> CPU = ทุก core เครื่อง 8GB จึงได้ราว 4GB + ทุก core อยู่แล้ว การตั้งค่าข้างบนคือการ *กำหนดให้ชัด*
> และจำกัด core ไว้ที่ 2 เพื่อเว้นให้ Windows ทำงานลื่น

หลังจากแก้ไขให้รีสตาร์ท WSL:
```powershell
wsl --shutdown
```
จากนั้นเปิด WSL ใหม่

---

## ถัดไป: ติดตั้งเครื่องมือบน Ubuntu

เมื่อติดตั้ง WSL เสร็จแล้ว ให้ดูคู่มือต่อไปนี้เพื่อติดตั้งเครื่องมือที่จำเป็นบน Ubuntu:

### [UBUNTU.md](UBUNTU.md)

คู่มือนี้จะแนะนำการติดตั้ง:
- Claude Code CLI
- VS Code + Claude Code extension
- Git, GitHub CLI (gh)
- Python tools (pipx, uv)
- Bun Runtime
- Node.js + npm (Global installation)
