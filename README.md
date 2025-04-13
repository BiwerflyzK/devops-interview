# devops-interview

## คำตอบข้อ 1

### 1. เตรียมความพร้อม

#### อุปกรณ์ที่ต้องใช้
- เครื่อง Server (On-premise)
- USB Flash Drive (8GB+)
- ไฟล์ ISO: [Ubuntu Server 22.04 LTS](https://ubuntu.com/download/server)
- โปรแกรมทำ USB Boot: Rufus หรือ Balena Etcher
- ข้อมูล Network (IP Address, Gateway, DNS ฯลฯ)

---

### 2. ติดตั้ง Ubuntu Server

#### ขั้นตอน:
1. สร้าง USB Boot ด้วยโปรแกรมเช่น Rufus หรือ Etcher
2. เสียบ USB เข้ากับเครื่อง Server และเข้า BIOS → ตั้งค่า Boot จาก USB
3. ทำตามขั้นตอนของ Ubuntu Installer:
   - เลือกภาษา / Keyboard
   - ตั้งค่า Network (Static IP หรือ DHCP)
   - เลือก Disk สำหรับติดตั้งระบบ
   - สร้าง User / Password
   - เปิดใช้งาน OpenSSH (เพื่อใช้ SSH Remote)
4. รอการติดตั้งให้เสร็จ แล้วรีสตาร์ทเครื่อง

---

### 3. ตั้งค่า Network (Static IP)

#### ตรวจสอบหรือกำหนด IP:
```bash
ip a
```

#### แก้ไข Netplan:
```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

#### Apply การตั้งค่า:
```bash
sudo netplan apply
```

---

### 4. ติดตั้ง Environment

#### อัปเดตระบบ:
```bash
sudo apt update && sudo apt upgrade -y
```

#### ติดตั้ง Docker + Docker Compose:
```bash
sudo apt install docker.io docker-compose -y
sudo systemctl enable docker
sudo usermod -aG docker $USER
```

#### ติดตั้ง Nginx:
```bash
sudo apt install nginx -y
sudo systemctl enable nginx
```

#### ติดตั้ง MySQL:
```bash
sudo apt install mysql-server -y
sudo mysql_secure_installation
```

## คำตอบข้อ 2

### 1. วางแผนและเตรียมการก่อนติดตั้ง (Pre-Setup)

- กำหนดวัตถุประสงค์การใช้งาน Server (Web Server, Database Server, File Server)
- เลือก Hardware ที่เหมาะสม (CPU, RAM, Storage, Network Interface)
- เตรียม Software ที่จำเป็น (ระบบปฏิบัติการ)

---

### 2. การ Set-up Hardware

- ประกอบหรือติดตั้ง Server บน Rack
- เชื่อมต่อสายไฟ, สาย LAN และอุปกรณ์ต่อพ่วง
- ตรวจสอบ POST และอัปเดต BIOS/UEFI Firmware

---

### 3. ติดตั้งระบบปฏิบัติการ (Operating System Installation)

- Boot จาก USB เพื่อติดตั้ง OS
- แบ่งพาร์ติชันและเลือก File System (เช่น EXT4, NTFS)
- กำหนดค่า Hostname, Region, Timezone
- ตั้งค่า User/Password สำหรับผู้ดูแลระบบ

---

### 4. ตั้งค่าระบบ Network

- กำหนด IP Address แบบ Static
- ตั้งค่า Gateway และ DNS
- ทดสอบการเชื่อมต่ออินเทอร์เน็ตหรือระบบเครือข่ายภายใน

---

### 5. ตั้งค่าความปลอดภัยเบื้องต้น

- เปิด/ปิด Firewall ตามความจำเป็น
- ปิด SSH root login (สำหรับ Linux)
- เปลี่ยนพอร์ต SSH (ถ้าต้องการเพิ่มความปลอดภัย)
- สร้าง Users และกำหนดสิทธิ์ (Permissions)

---

### 6. ติดตั้ง Service/Application ที่ต้องใช้งาน

- ติดตั้ง Web Server เช่น Nginx
- ติดตั้งระบบฐานข้อมูล เช่น MySQL, PostgreSQL, MSSQL
- ติดตั้งบริการแชร์ไฟล์ เช่น NFS
- ติดตั้ง Application ตามความต้องการของระบบ

---

### 7. ติดตั้งและตั้งค่า Monitoring และ Logging

- ติดตั้งระบบ Monitoring เช่น Prometheus, Grafana
- ตั้งค่าระบบ Logging เช่น ELK Stack
- ตั้งค่าการแจ้งเตือนผ่าน Email, Discord, Line Notify

---

### 8. ตั้งค่า Backup และ Recovery

- วางแผนการ Backup: รายวัน/รายสัปดาห์
- ติดตั้งและตั้งค่าโปรแกรม Backup เช่น Veeam, rsync, Bacula
- ทดสอบการ Restore เพื่อยืนยันความถูกต้อง

---

### 9. จัดทำเอกสารและ SOP (Standard Operating Procedures)

- บันทึกข้อมูลสำคัญ เช่น IP Address, User, Password, Config ต่าง ๆ
- เขียนคู่มือการดูแลและการอัปเดตระบบ
- สรุปรายชื่อผู้ดูแลระบบและช่องทางติดต่อกรณีฉุกเฉิน

---

### 10. ทดสอบการใช้งานจริง (Go-Live Test)

- ทดสอบการเข้าถึง Service จาก Client ภายนอก/ภายใน
- ตรวจสอบ Log และทรัพยากรระบบ (Resource Usage)
- ยืนยันว่า Server พร้อมใช้งานจริงตามวัตถุประสงค์

## คำตอบข้อ 4

### ส่วนประกอบพื้นฐานของ CI/CD

| องค์ประกอบ | รายละเอียด |
|------------|-------------|
| **Source Code Repository** | เช่น GitHub, GitLab, Bitbucket |
| **CI/CD Tools** | เช่น Jenkins, GitLab CI/CD, GitHub Actions, CircleCI |
| **Artifact Repository** | เช่น Docker Hub, JFrog Artifactory, Nexus |
| **Target Environment** | เช่น Dev, UAT, Production (VM, Container, Cloud) |

---

### Continuous Integration (CI)

> รวมโค้ดและทดสอบอัตโนมัติทุกครั้งที่มีการเปลี่ยนแปลงโค้ดใน repository

#### ขั้นตอน:
1. **Developer พัฒนาโค้ด** บนเครื่อง local และ `git push` ขึ้น remote
2. **Trigger CI Pipeline** อัตโนมัติจาก Git event (push, merge)
3. **Checkout Source Code** จาก branch ที่เกี่ยวข้อง
4. **Build Project** (ติดตั้ง dependencies, compile)
5. **Run Automated Tests**
   - Unit Tests
   - Integration Tests
   - Static Code Analysis (เช่น SonarQube)
6. **Generate Artifacts**
   - เช่น JAR/WAR, Docker Image, Zip File
7. **Upload Artifacts** ไปยัง Artifact Repository
   - เพื่อใช้ในขั้นตอน Deploy ต่อไป

---

### Continuous Delivery (CD)

> ทำให้ซอฟต์แวร์พร้อม deploy ได้ทุกเมื่อ โดยอัตโนมัติหลัง CI เสร็จ

#### ขั้นตอน:
1. **Trigger CD Pipeline** ต่อจาก CI เมื่อ build สำเร็จ
2. **เลือก Environment** ที่จะ deploy เช่น Dev, UAT
3. **Deploy Artifacts** ไปยังเซิร์ฟเวอร์หรือ container
4. **Post-Deployment Tasks**
   - Migrate database
   - Clear cache หรือ warm-up
5. **Notification**
   - ส่งแจ้งเตือนผ่าน Email, Slack, Discord เป็นต้น

> ต้องมีขั้นตอน Approval ก่อน deploy ไป Production

---

### Continuous Deployment (CD แบบเต็ม)

> Deploy โค้ดไป Production อัตโนมัติ **โดยไม่ต้องรอการอนุมัติ** (เมื่อผ่าน test ครบถ้วน)

#### เหมาะสำหรับ:
- ระบบที่มี automated tests ครบถ้วน
- มีระบบ rollback, health check หรือ monitoring

---