# 🖥️ EC2 + EBS + Apache Web Server on AWS

![AWS](https://img.shields.io/badge/AWS-EC2%20%7C%20EBS%20%7C%20CloudWatch-orange?logo=amazon-aws)
![Linux](https://img.shields.io/badge/Linux-Ubuntu-blue?logo=linux)
![Apache](https://img.shields.io/badge/Web%20Server-Apache2-red?logo=apache)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

## 📌 Project Overview

This project demonstrates how to host an Apache web server on an AWS EC2 instance with an EBS volume as the document root. It includes CloudWatch monitoring for real-time health tracking.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────┐
│                  AWS Cloud                  │
│                                             │
│   ┌──────────┐      ┌──────────────────┐   │
│   │  EC2     │─────▶│  EBS Volume      │   │
│   │ Instance │      │  (DocumentRoot)  │   │
│   └──────────┘      └──────────────────┘   │
│        │                                    │
│        ▼                                    │
│   ┌──────────────┐   ┌──────────────────┐  │
│   │    Apache    │   │   CloudWatch     │  │
│   │  Web Server  │   │   Monitoring     │  │
│   └──────────────┘   └──────────────────┘  │
└─────────────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| AWS EC2 | Virtual machine to host the web server |
| AWS EBS | Persistent block storage for web content |
| Apache2 | HTTP web server |
| AWS CloudWatch | CPU, disk, and memory monitoring |
| Ubuntu Linux | Operating system |

---

## ⚙️ What Was Done

- ✅ Launched EC2 instance on AWS (Ubuntu)
- ✅ Created and attached an EBS volume to the instance
- ✅ Mounted EBS volume and configured Apache `DocumentRoot` to point to it
- ✅ Deployed and tested a sample web page from the EBS volume
- ✅ Configured CloudWatch alarms for CPU utilization monitoring
- ✅ Verified web server availability via public IP

---

## 📋 Step-by-Step Setup

### 1. Launch EC2 Instance
```bash
# AMI: Ubuntu 22.04 LTS
# Instance type: t2.micro (Free Tier)
# Security Group: Allow HTTP (80), HTTPS (443), SSH (22)
```

### 2. Create & Attach EBS Volume
```bash
# Create volume in same AZ as EC2
# Attach as /dev/xvdf
```

### 3. Mount EBS Volume
```bash
sudo mkfs.ext4 /dev/xvdf
sudo mkdir /mnt/webdata
sudo mount /dev/xvdf /mnt/webdata

# Make persistent on reboot
echo '/dev/xvdf /mnt/webdata ext4 defaults,nofail 0 2' | sudo tee -a /etc/fstab
```

### 4. Install Apache & Configure DocumentRoot
```bash
sudo apt update && sudo apt install apache2 -y

# Edit Apache config
sudo nano /etc/apache2/sites-available/000-default.conf
# Change DocumentRoot to /mnt/webdata

sudo systemctl restart apache2
```

### 5. Deploy Web Content
```bash
sudo chown -R www-data:www-data /mnt/webdata
echo "<h1>Hello from EBS-backed Apache Server!</h1>" | sudo tee /mnt/webdata/index.html
```

### 6. CloudWatch Monitoring
```bash
# Installed CloudWatch agent
# Configured CPU alarm: alert if CPU > 70% for 5 minutes
```

---

## 📸 Key Learnings

- How EBS volumes differ from instance store (persistent vs ephemeral)
- Apache DocumentRoot configuration and virtual hosts
- CloudWatch alarm setup for proactive monitoring
- Linux disk mounting and fstab persistence

---

## 🔗 Related Projects

- [aws-devops-projects](../aws-devops-projects) — More AWS infrastructure projects
- [aws-cloud-support-practice](../aws-cloud-support-practice) — AWS support scenarios

---

## 👤 Author

**Ragul P** — AWS Certified Cloud Practitioner  
[LinkedIn](https://linkedin.com/in/ragul-p-16b757289) | [GitHub](https://github.com/Ragulpps007)
