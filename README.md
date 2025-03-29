# n8n Course Project

โปรเจคนี้เป็นการตั้งค่า n8n พร้อมกับ PostgreSQL และ Nginx Proxy Manager สำหรับการใช้งานจริง

## ระบบที่จำเป็น

- Docker
- Docker Compose
- Git

## การติดตั้ง

1. Clone repository:
```bash
git clone <repository-url>
cd n8n-course
```

2. สร้างไฟล์ .env จาก .env.example:
```bash
cp .env.example .env
```

3. แก้ไขไฟล์ .env ให้เหมาะสมกับสภาพแวดล้อมของคุณ:
```env
POSTGRES_USER=your_postgres_user
POSTGRES_PASSWORD=your_postgres_password
N8N_ENCRYPTION_KEY=your_encryption_key
N8N_USER_MANAGEMENT_JWT_SECRET=your_jwt_secret
```

4. สร้างโฟลเดอร์สำหรับเก็บข้อมูล:
```bash
mkdir -p data/{n8n,postgres,npm,letsencrypt}
```

5. รัน Docker Compose:
```bash
docker-compose up -d
```

## การเข้าถึง

- n8n: http://localhost:5678
- Nginx Proxy Manager Admin UI: http://localhost:81
  - Email: admin@example.com
  - Password: changeme (ควรเปลี่ยนหลังจาก login ครั้งแรก)

## โครงสร้างโปรเจค

```
.
├── data/                    # ข้อมูลของ Docker volumes
│   ├── n8n/                # ข้อมูลของ n8n
│   ├── postgres/           # ข้อมูลของ PostgreSQL
│   ├── npm/                # ข้อมูลของ Nginx Proxy Manager
│   └── letsencrypt/        # SSL certificates
├── docker-compose.yml      # Docker Compose configuration
├── .env                    # Environment variables
├── .env.example           # ตัวอย่าง Environment variables
└── .gitignore             # Git ignore rules
```

## การตั้งค่า Nginx Proxy Manager

1. เข้าสู่ Nginx Proxy Manager Admin UI
2. เปลี่ยนรหัสผ่านเริ่มต้น
3. สร้าง Proxy Host สำหรับ n8n:
   - Domain: your-domain.com
   - Scheme: http
   - Forward Hostname / IP: localhost
   - Forward Port: 5678

## การสำรองข้อมูล

ข้อมูลทั้งหมดจะถูกเก็บในโฟลเดอร์ `data/` ซึ่งประกอบด้วย:
- ข้อมูล n8n
- ฐานข้อมูล PostgreSQL
- ข้อมูล Nginx Proxy Manager
- SSL certificates

## การบำรุงรักษา

### การอัพเดท

```bash
docker-compose pull
docker-compose up -d
```

### การดู logs

```bash
# ดู logs ทั้งหมด
docker-compose logs -f

# ดู logs เฉพาะ service
docker-compose logs -f n8n
docker-compose logs -f postgres
docker-compose logs -f npm
```

### การหยุดการทำงาน

```bash
docker-compose down
```

## การแก้ไขปัญหาเบื้องต้น

1. **ไม่สามารถเข้าถึง n8n ได้**
   - ตรวจสอบว่า port 5678 ไม่ถูกใช้งาน
   - ตรวจสอบ logs ของ n8n service

2. **ไม่สามารถเข้าถึง Nginx Proxy Manager ได้**
   - ตรวจสอบว่า port 81 ไม่ถูกใช้งาน
   - ตรวจสอบ logs ของ npm service

3. **ฐานข้อมูลไม่ทำงาน**
   - ตรวจสอบ logs ของ postgres service
   - ตรวจสอบการเชื่อมต่อใน .env file

## License

MIT