# EC2 + EBS + Apache Project — All Commands

## 1. Update EC2 instance
sudo yum update -y

## 2. Install Apache (httpd)
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd

## 3. Check attached EBS volume
lsblk

## 4. Create filesystem on EBS volume
sudo mkfs -t xfs /dev/xvdb

## 5. Create mount directory
sudo mkdir /mydata

## 6. Mount EBS volume
sudo mount /dev/xvdb /mydata

## 7. Verify mount
df -h

## 8. Add test webpage into /mydata
echo "<h1>Welcome to my website hosted on EBS Volume</h1>" | sudo tee /mydata/index.html

## 9. Change default Apache DocumentRoot
sudo nano /etc/httpd/conf/httpd.conf
# Change:
/var/www/html 
# To:
/mydata

## 10. Restart Apache
sudo systemctl restart httpd

## 11. Make mount permanent (Optional)
sudo blkid
sudo nano /etc/fstab

## 12. Allow inbound traffic in AWS Security Group
Port: 80 (HTTP)
Port: 22 (SSH)

## 13. View website
http://<EC2-Public-IP>
