![Alt text](Host_a_WordPress_Website_on_AWS.png)

# Host a WordPress Website on AWS

This project demonstrates how to host a WordPress website on AWS using a comprehensive DevOps architecture. The deployment leverages various AWS services to ensure scalability, fault tolerance, and secure access.

## Project Overview
The wordpress website is hosted on EC2 instances within a secure and scalable AWS infrastructure. The architecture includes public and private subnets, load balancing, auto scaling, and secure communication protocols.

## Architecture Summary
- **VPC Configuration**: Created a Virtual Private Cloud with public and private subnets across two Availability Zones.
- **Internet Gateway**: Enabled connectivity between VPC instances and the Internet.
- **Security Groups**: Configured as network firewalls.
- **Availability Zones**: Used two zones for high availability.
- **Public Subnets**: Hosted NAT Gateway and Application Load Balancer.
- **EC2 Instance Connect Endpoint**: Provided secure access to instances.
- **EFS and RDS**: Provided distributed storage to the EC2 instances for hosting application code.
- **Private Subnets**: Hosted EC2 web servers for enhanced security.
- **NAT Gateway**: Allowed private instances to access the Internet.
- **Application Load Balancer**: Distributed traffic to EC2 instances.
- **Auto Scaling Group**: Managed EC2 instances for scalability and fault tolerance.
- **GitHub**: Used for version control and collaboration.
- **Certificate Manager**: Secured communications.
- **SNS**: Configured for Auto Scaling Group notifications.
- **Route 53**: Registered domain and configured DNS records.

## Deployment Steps
1. Launch EC2 instances in private subnets.
2. Use the following user data script to install and configure the web server:

```bash
#!/bin/bash
sudo su
sudo yum update -y
sudo mkdir -p /var/www/html
EFS_DNS_NAME=fs-01f12df7d7a0dacd3.efs.us-east-1.amazonaws.com
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport "$EFS_DNS_NAME":/ /var/www/html
sudo yum install -y httpd
sudo systemctl enable httpd
sudo systemctl start httpd
sudo dnf install -y php php-cli php-cgi php-curl php-mbstring php-gd php-mysqlnd php-gettext php-json php-xml php-fpm php-intl php-zip php-bcmath php-ctype php-fileinfo php-openssl php-pdo php-tokenizer
sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm
sudo dnf install -y mysql80-community-release-el9-1.noarch.rpm
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023
sudo dnf repolist enabled | grep "mysql.*-community.*"
sudo dnf install -y mysql-community-server
sudo systemctl start mysqld
sudo systemctl enable mysqld
sudo usermod -a -G apache ec2-user
sudo chown -R ec2-user:apache /var/www
sudo chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \;
sudo find /var/www -type f -exec sudo chmod 0664 {} \;
chown apache:apache -R /var/www/html
wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz
sudo cp -r wordpress/* /var/www/html/
sudo cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php
sudo vi /var/www/html/wp-config.php
sudo service httpd restart
```

3. Configure the Application Load Balancer and target group.
4. Set up Auto Scaling policies.
5. Secure the application using AWS Certificate Manager.
6. Configure SNS for notifications.
7. Register domain and set DNS records using Route 53.

## Technologies Used
- AWS EC2
- VPC
- Subnets (Public & Private)
- Internet Gateway
- NAT Gateway
- Security Groups
- Application Load Balancer
- Auto Scaling Group
- EFS
- RDS
- EC2 Instance Connect Endpoint
- GitHub
- Apache HTTP Server
- AWS Certificate Manager
- Amazon SNS
- Amazon Route 53

## Repository
The deployment scripts and architecture diagram are available in the [GitHub repository](https://github.com/Dolphincare79/host-a-static-website-on-aws).

