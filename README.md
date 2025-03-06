# Dev-Ops-Final-lab-exam

## **โจทย์**

1. Create a virtual machine
2. Install Nginx (not using Docker) to the virtual machine and show the default home page.
3. Edit the HTML to show the new index.html in the Nginx
4. Install Docker in the virtual machine, and call the "hello-world" image to run
5. Run the Docker image Nginx and show the default page
6. The proctor will send you the HTML file, create a new image to show the HTML file you
received, and then run the container to show the output image in the VM
7. Push the image you have created to your Docker Hub
8. With your Docker image, map the volume of your container to show the website you
have done on question no. 3
9. The proctor will provide `docker-compose.yml` file, run the given docker-compose.yml and
show the output in the browser
10. Create a new HTML file in the VM, Write your `docker-compose.yml` to run your image,
and map the volume to your new HTML file, and show the output.


---

## **วิธีทำโดยละเอียด**

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
sudo docker run --name mynginx -d -p 8080:80 nginx
```
- ตรวจสอบ Docker
```bash
sudo docker ps
```
- เปิดเบราว์เซอร์ไปที่ `http://<IP-ADDRESS>`

### **6. สร้าง Docker image ใหม่ที่แสดงไฟล์ HTML**
- สร้าง `Dockerfile`:
```Dockerfile
FROM nginx
COPY index.html /usr/share/nginx/html/index.html
```
- สร้างและรัน Docker image:
```bash
docker build -t mycustomnginx .
docker run --name customnginx -d -p 8080:80 mycustomnginx
```
- เข้าไปที่ `http://<IP-ADDRESS>:8080`

### **7. อัปโหลด Image ไปยัง Docker Hub**
```bash
docker login
docker tag mycustomnginx <DOCKER_USERNAME>/mycustomnginx
docker push <DOCKER_USERNAME>/mycustomnginx
```

### **8. ใช้ Volume เพื่อแสดงหน้าเว็บจากข้อ 3**
```bash
docker run --name nginx-volume -d -p 8081:80 -v /var/www/html/index.html:/usr/share/nginx/html/index.html nginx
```
- เข้าไปที่ `http://<IP-ADDRESS>:8081`

### **9. รัน `docker-compose.yml` ที่ได้รับจากผู้คุมสอบ**
```bash
docker-compose up -d
```
- เปิดเบราว์เซอร์และดูผลลัพธ์

### **10. สร้าง HTML ใหม่และรันด้วย `docker-compose.yml`**
```bash
nano /home/user/new_index.html
```
- เขียน `docker-compose.yml`:
```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8082:80"
    volumes:
      - /home/user/new_index.html:/usr/share/nginx/html/index.html
```
- รัน:
```bash
docker-compose up -d
```
- เข้าไปที่ `http://<IP-ADDRESS>:8082` เพื่อดูผลลัพธ์

---
