
![Alt text](/Host_a_Static_Website_on_AWS.png)

# Host a Static Website on AWS

This project demonstrates how to host a static HTML web application on AWS using a comprehensive DevOps architecture. The deployment leverages various AWS services to ensure scalability, fault tolerance, and secure access.

## Project Overview
The static website is hosted on EC2 instances within a secure and scalable AWS infrastructure. The architecture includes public and private subnets, load balancing, auto scaling, and secure communication protocols.

## Architecture Summary
- **VPC Configuration**: Created a Virtual Private Cloud with public and private subnets across two Availability Zones.
- **Internet Gateway**: Enabled connectivity between VPC instances and the Internet.
- **Security Groups**: Configured as network firewalls.
- **Availability Zones**: Used two zones for high availability.
- **Public Subnets**: Hosted NAT Gateway and Application Load Balancer.
- **EC2 Instance Connect Endpoint**: Provided secure access to instances.
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

# Switch to the root user to gain full administrative privileges
sudo su

# Update all installed packages to their latest versions
yum update -y

# Install Apache HTTP Server
yum install -y httpd

# Change the current working directory to the Apache web root
cd /var/www/html

# Install Git
yum install git -y

# Clone the project GitHub repository to the current directory
git clone https://github.com/Dolphincare79/host-a-static-website-on-aws.git

# Copy all files, including hidden ones, from the cloned repository to the Apache web root
cp -R host-a-static-website-on-aws/. /var/www/html/

# Remove the cloned repository directory to clean up unnecessary files
rm -rf host-a-static-website-on-aws

# Enable the Apache HTTP Server to start automatically at system boot
systemctl enable httpd 

# Start the Apache HTTP Server to serve web content
systemctl start httpd
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
- EC2 Instance Connect Endpoint
- GitHub
- Apache HTTP Server
- AWS Certificate Manager
- Amazon SNS
- Amazon Route 53

## Repository
The deployment scripts and architecture diagram are available in the [GitHub repository](https://github.com/Dolphincare79/host-a-static-website-on-aws).

