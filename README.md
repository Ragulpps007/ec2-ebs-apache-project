# ✅ EC2 + EBS + Apache Web Hosting Project

This project demonstrates how to host a website on an Amazon EC2 instance using a mounted EBS volume as the Apache DocumentRoot. It highlights **end-to-end cloud setup, server configuration, storage management, and public website deployment**.

---

## 🚀 Project Overview

In this project, we:

1. Launched an EC2 instance (Amazon Linux 2)  
2. Created and attached an EBS volume  
3. Mounted the EBS volume to `/mydata`  
4. Installed and configured Apache HTTP Server  
5. Updated Apache DocumentRoot to point to the EBS volume  
6. Deployed a simple HTML webpage  
7. Hosted the website publicly using EC2 Security Groups  
8. Tested the website using the EC2 public IP

---

## 🏗 Architecture

```
User → EC2 Instance → Apache HTTP Server → EBS Volume (/mydata)
```

---

## 🖥 Technologies Used

- AWS EC2 (Linux)  
- AWS EBS (Persistent Storage)  
- Apache HTTP Server  
- Linux Commands & Shell  
- Security Groups (Inbound Ports: 22, 80)  
- File System Management & Mounting  

---

## 📌 Step-by-Step Implementation

### 1️⃣ Launch EC2 Instance
- AMI: Amazon Linux 2  
- Instance Type: t2.micro (Free Tier)  
- Security Group: Allow SSH (22) & HTTP (80)  

### 2️⃣ Create and Attach EBS Volume
- Size: 1–5 GB  
- Type: gp2/gp3  
- Same Availability Zone as EC2  
- Attach to `/dev/xvdf`  

### 3️⃣ Format and Mount EBS Volume

```bash
sudo mkfs.ext4 /dev/xvdf
sudo mkdir /mydata
sudo mount /dev/xvdf /mydata
```

Make the mount permanent:

```bash
sudo blkid
sudo nano /etc/fstab
# Add:
/dev/xvdf   /mydata   ext4   defaults,nofail   0   2
```

### 4️⃣ Install Apache Web Server

```bash
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```

### 5️⃣ Update Apache DocumentRoot

Edit config:

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Change:

```
DocumentRoot "/var/www/html"
```

to:

```
DocumentRoot "/mydata"
```

Update `<Directory>` block:

```apache
<Directory "/mydata">
    AllowOverride None
    Require all granted
</Directory>
```

Restart Apache:

```bash
sudo systemctl restart httpd
```

### 6️⃣ Deploy Webpage on EBS Volume

```bash
echo "<h1>Welcome to my website hosted on EBS Volume!</h1>" | sudo tee /mydata/index.html
```

### 7️⃣ Test the Website

- Open browser → `http://<your-ec2-public-ip>`  
- Verify content is served from the EBS volume  
- Reboot EC2 → check data persists (EBS persistence)

---

## 🛠 Troubleshooting

| Issue | Root Cause | Solution |
|-------|------------|---------|
| Website not loading | Port 80 blocked | Update Security Group inbound rule |
| Apache not starting | Service failure | `sudo systemctl restart httpd` |
| EBS volume not mounted | Wrong device name / fstab config | Check `/dev/xvdf` & fstab entry |

---

## 📊 Skills Demonstrated

✔ EC2 deployment & Linux administration  
✔ EBS volume setup & persistent storage  
✔ Apache Web Server configuration  
✔ Security Groups & network configuration  
✔ Troubleshooting & monitoring  
✔ Web hosting best practices  

---

## 🚀 Future Enhancements

- Enable HTTPS with SSL/ACM  
- Configure auto-scaling and Load Balancer  
- Automate deployment using CI/CD pipeline  

---

## 🖼 Outputs & Screenshots

- EC2 instance running in AWS Console  
- Mounted EBS folder showing website files  
- Browser showing hosted webpage  
