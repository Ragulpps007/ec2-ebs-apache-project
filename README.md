# EC2 + EBS + Apache Web Hosting Project

This project demonstrates how to host a website on an Amazon EC2 instance using a mounted EBS volume as the Apache DocumentRoot. The goal is to show end-to-end cloud setup, server configuration, storage management, and website deployment.

---

## 🚀 Project Overview

In this project, we:

1. Launched an EC2 instance (Amazon Linux 2).
2. Created and attached an EBS volume.
3. Mounted the EBS volume to `/mydata`.
4. Installed and configured Apache.
5. Updated Apache DocumentRoot to point to the EBS volume.
6. Deployed a simple HTML webpage.
7. Hosted the website publicly using EC2 Security Groups.
8. Tested the website using the EC2 public IP.

---

## 🏗 Architecture
User → EC2 Instance → Apache HTTP Server → EBS Volume (/mydata)



---

## 🖥 Technologies Used

- **AWS EC2**
- **AWS EBS**
- **Apache HTTP Server (httpd)**
- **Linux Commands**
- **Vim / nano editors**
- **Security Groups**
- **File Systems & Mounts**

---

## 📌 Step-by-Step Implementation

1️⃣ Launch EC2 Instance
- AMI: Amazon Linux 2
- Instance type: t2.micro (Free Tier)
- Security group: allow **HTTP (80)** & **SSH (22)**

---

2️⃣ Create and Attach EBS Volume
- Size: 1–5 GB
- Type: gp2/gp3
- Same Availability Zone as EC2
- Attach to `/dev/xvdf`

---

3️⃣ Format and Mount the EBS Volume

```bash
sudo mkfs.ext4 /dev/xvdf
sudo mkdir /mydata
sudo mount /dev/xvdf /mydata

Make the mount permanent:
sudo blkid
sudo nano /etc/fstab
/dev/xvdf   /mydata   ext4   defaults,nofail   0   2


4️⃣ Install Apache Web Server

sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd

5️⃣ Change Apache DocumentRoot to EBS Volume

Edit config:
  sudo nano /etc/httpd/conf/httpd.conf
Change:
  DocumentRoot "/var/www/html"
to:
  DocumentRoot "/mydata"
 
Also update the <Directory> block:
<Directory "/mydata">
    AllowOverride None
    Require all granted
</Directory>

Restart Apache:
 sudo systemctl restart httpd


 6️⃣ Deploy Webpage on EBS Volume

    echo "<h1>Welcome to my website hosted on EBS Volume!</h1>" | sudo tee /mydata/index.html

  7️⃣ Test the Website  
     http://<your-ec2-public-ip>
     


