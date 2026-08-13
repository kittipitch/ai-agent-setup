# คู่มือการติดตั้งบน Windows (ผ่าน WSL2 + Ubuntu)

คู่มือนี้เป็นจุดเริ่มต้นสำหรับผู้ใช้ **Windows** ใน Workshop **AI-Accelerated Software Development**

Workshop นี้ใช้ **WSL2 + Ubuntu 24.04** เป็นเส้นทางเดียวสำหรับ Windows

เราไม่ใช้ PowerShell เป็น development shell และไม่ติดตั้ง Microsoft Coreutils เพราะคำสั่ง scripts skills และ labs ของ Workshop เขียนขึ้นสำหรับ Linux shell

การทำงานใน Ubuntu จริงช่วยลดปัญหาเรื่อง path การ quote และพฤติกรรมของเครื่องมือที่ต่างกันระหว่าง Windows กับ Linux

PowerShell ยังมีหน้าที่เดียวที่ถูกต้องในคู่มือนี้: เปิด **Administrator PowerShell** เพื่อรันคำสั่งจัดการ WSL เอง เช่น `wsl --install`, `wsl --update` และ `wsl --shutdown` หลังจากติดตั้ง Ubuntu แล้ว งานทั้งหมดให้ทำใน **Ubuntu shell**

---

## ข้อกำหนดเบื้องต้น

- Windows 10 เวอร์ชัน 22H2 (build 19045) ขึ้นไป หรือ Windows 11
- เปิด virtualization ใน BIOS/UEFI แล้ว
- GitHub Account
- paid Claude account: Pro, Max, Team, Enterprise หรือ Console API (free Claude.ai plan ยังใช้ Claude Code ไม่ได้)

---

## ขั้นตอนการติดตั้ง

### 1. ติดตั้ง WSL2 + Ubuntu

#### [WSL.md](WSL.md)

ติดตั้ง WSL2, Ubuntu 24.04, Windows Terminal และตั้งค่า WSL พื้นฐานสำหรับ Workshop

---

### 2. ติดตั้งเครื่องมือทั้งหมดใน Ubuntu

#### [UBUNTU.md](UBUNTU.md)

หลังจากเข้า Ubuntu shell แล้ว ให้ติดตั้ง Claude Code, Git, GitHub CLI, Node.js, Bun, Python tools และเครื่องมืออื่นๆ ตามคู่มือ Ubuntu

---

## ทำไมไม่ใช้ Windows แบบ native

- เส้นทาง native เดิมใช้ `winget` + PowerShell + Microsoft Coreutils ซึ่งยังมีความต่างจาก Linux shell จริงในหลายจุด
- Labs ของ Workshop อ้างอิงคำสั่งและพฤติกรรมของ Ubuntu เป็นหลัก การใช้ WSL ทำให้ทุกคนอยู่บนสภาพแวดล้อมเดียวกัน
- ปัญหา path เช่น `C:\Users\...` เทียบกับ `/home/...` และการ quote คำสั่งซ้อนกันจะลดลงมากเมื่อทำใน Ubuntu
- เครื่องมือสาย Python/Node และ AI coding agents มักมีเอกสารและตัวอย่างสำหรับ Linux/macOS ก่อน Windows native
- คู่มือ native เดิมยังถูกเก็บไว้ใน git history (commit `2c4f76a`) สำหรับคนที่จำเป็นต้องดูย้อนหลัง แต่เส้นทางนั้น **ไม่รองรับสำหรับ Workshop นี้**

---

## ปัญหาที่พบบ่อย

| ปัญหา | วิธีแก้ |
|:---|:---|
| ติดตั้ง WSL ไม่ได้เพราะ virtualization ปิดอยู่ | เปิด VT-x หรือ AMD-V ใน BIOS/UEFI แล้วรีสตาร์ทเครื่อง |
| `wsl --install` ล้มเหลว หรือ WSL เป็นเวอร์ชันเก่า | เปิด Administrator PowerShell แล้วรัน `wsl --update` จากนั้นรีสตาร์ทเครื่อง |
| Windows Terminal เปิด PowerShell แทน Ubuntu | ตั้งค่า Default Profile เป็น `Ubuntu-24.04` ตามขั้นตอนใน [WSL.md](WSL.md) |
| WSL ใช้ RAM มากเกินไป | ตั้งค่า `.wslconfig` เพื่อจำกัด `memory` และ `processors` ตามหัวข้อจำกัดทรัพยากร WSL ใน [WSL.md](WSL.md) |
