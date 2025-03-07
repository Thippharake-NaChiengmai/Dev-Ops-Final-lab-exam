# Dev-Ops-Final-lab-exam

### **1. สร้างเครื่องเสมือน (Virtual Machine)**
- สร้าง instance บน AWS
- สร้างหรือเปิดใช้งาน Security groups
- สร้างหรือเปิดใช้งาน yourKey.pem

### **2. ติดตั้ง Nginx และแสดงหน้าโฮมเพจเริ่มต้น**
```bash
sudo apt update
sudo apt install nginx -y
sudo systemctl status nginx
```
- เปิดเบราว์เซอร์ไปที่ `http://<IP-ADDRESS>` เพื่อดูหน้าเว็บของ Nginx

### **3. แก้ไขไฟล์ HTML เพื่อเปลี่ยนหน้าเว็บ**
```bash
cd /var/www/html
echo "<h1>Hello, Nginx! This is Thippharake page.</h1>" | sudo tee index.html
sudo systemctl restart nginx
```
- เสร็จแล้ว cd ไปหน้า Home
```bash
cd
```
### **4. ติดตั้ง Docker และรัน "hello-world"**
```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
```bash
sudo docker run hello-world
```
### **5. รัน Docker image ของ Nginx และแสดงหน้าเว็บ**

```bash
sudo docker pull nginx
```
```bash
sudo docker run --name mynginx -d -p 8085:80 nginx
```
- ตรวจสอบ Docker
```bash
sudo docker ps
```
- เปิดVM `http://<IP-ADDRESS>:8085`

### **6. สร้าง Docker image ใหม่ที่แสดงไฟล์ HTML**
- เปิดไฟล์ `.html`
- สร้าง `Dockerfile`:
```Dockerfile
FROM nginx
COPY index.html /usr/share/nginx/html/index.html
```
- สร้างและรัน Docker image:
```bash
docker build -t thipppharake/exam1:v1 .
```

### **7. อัปโหลด Image ไปยัง Docker Hub**

```bash
docker push thipppharake/exam1:v1
```
- ไปเปิด window terminal
```bash
sudo docker pull thipppharake/exam1:v1
```
```bash
sudo docker run -d -p 8081:80 thipppharake/exam1:v1
```
- เปิด VM `http://<IP-ADDRESS>:8081`
### **8. ใช้ Volume เพื่อแสดงหน้าเว็บจากข้อ 3**
```bash
sudo docker run --name nginx-volume -d -p 8082:80 -v /var/www/html/index.html:/usr/share/nginx/html/index.html nginx
```
- เปิด VM `http://<IP-ADDRESS>:8082`

### **9. รัน `docker-compose.yml` ที่ได้รับจากผู้คุมสอบ**
- รับไฟล์ `docker-compose.yml`
```bash
docker-compose up -d
```
***เปิด VM***
```bash
docker build -t thipppharake/exam2:kv1 .
```
```bash
docker push thipppharake/exam2:v1
```
```bash
docker push thipppharake/exam2:v1
```
- ไปเปิด window terminal
```bash
sudo docker pull thipppharake/exam2:v1
```
```bash
sudo docker run -d -p 8080:80 thipppharake/exam2:v1
```
- เปิด VM `http://<IP-ADDRESS>:8080`
  
### **10. สร้าง HTML ใหม่และรันด้วย `docker-compose.yml`**
- สร้าง floder `./html`
- สร้างไฟล์ `.html` ข้างใน floder `./html`
- เขียน `docker-compose.yml`:
```yaml
version: '3.8'
services:
  web:
    image: nginx
    ports:
      - "8084:80"
    volumes:
      - ./html:/usr/share/nginx/html
```
```Dockerfile
FROM nginx:latest
COPY ./html /usr/share/nginx/html
```
- รัน:
```bash
docker-compose up -d
```
- เริ่ม build
```bash
docker build -t thipppharake/exam3:v1 .
```
```bash
docker push thipppharake/exam3:v1
```
```bash
docker push thipppharake/exam3:v1
```
- ไปเปิด window terminal
```bash
sudo docker pull thipppharake/exam3:v1
```
```bash
sudo docker run -d -p 8084:80 thipppharake/exam3:v1
```
- เปิด VM `http://<IP-ADDRESS>:8084`
