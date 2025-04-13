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