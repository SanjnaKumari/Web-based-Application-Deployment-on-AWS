# Web-Based Bookstore Deployment on AWS

This repository contains the deployment scripts and configuration files for hosting a web-based bookstore system on Amazon Web Services (AWS). The system is built using React for the front end, Java (JSP/Servlet) for the backend, with Tomcat as the application server, and MySQL as the database.

## Basic Deployment
The basic deployment includes setting up two EC2 instances in a VPC:

- **Web Server:** Hosts the application server (Tomcat) and has internet access to serve the bookstore website to customers.
- **Database Server:** Hosts the MySQL database but is not accessible from outside networks.

## Advanced Deployment
The advanced deployment architecture includes:

- **Public Subnet:** Hosting the application server in the public subnet.
- **Private Subnet:** Hosting the database server in the private subnet to restrict external access.
- **Internet Gateway:** For internet access from the public subnet.
- **NAT Gateway:** Allows the private subnet to access the internet for updates and patches.

## Requirements
- Only HTTP/HTTPs, SSH, and MySQL traffic is allowed into the deployment.
- HTTP access from the network to the internet is allowed.
- The database is not accessible from outside networks.
- AWS free tier services are utilized as much as possible.

## AWS Services Used
- **EC2 Instances:** t2.micro running Amazon Linux 2023.
- **VPC:** Custom setup with public and private subnets.
- **Software Stack:** Tomcat (version 10), MySQL, Java (11 or later).

## Deployment Architecture
- **VPC:** Custom VPC with CIDR block `10.0.0.0/16`
- **Subnets:**
  - Public Subnet (`10.0.1.0/24`) for the web server
  - Private Subnet (`10.0.2.0/24`) for the database server
- **Instances:**
  - EC2 Instance for Web App (Public IP: `63.32.152.192`)
  - EC2 Instance for Database (Private IP: `10.0.2.196`)
- **Security Groups:**
  - `sg_appserver_group17` for the web app
  - `sg_dbserver_group17` for the database
- **Gateways:**
  - Internet Gateway for public access
  - NAT Gateway for secure internet access from the private subnet
- **Routing Tables:**
  - Public and Private routing tables configured for appropriate traffic flow

## Setup Instructions
1. **VPC and Subnets:**
   - Create a VPC with CIDR `10.0.0.0/16`
   - Create public subnet `10.0.1.0/24` and private subnet `10.0.2.0/24`

2. **Instances:**
   - Launch EC2 instances and assign them to the respective subnets

3. **Internet Gateway:**
   - Attach to the VPC for internet access

4. **NAT Gateway:**
   - Deploy in the public subnet for private subnet access

5. **Routing Tables:**
   - Configure routes for internet and NAT gateway traffic

6. **Security Groups:**
   - Allow HTTP/HTTPS, SSH, and MySQL as per requirements

7. **Application Deployment:**
   - Deploy the React frontend and Java backend on the web server
   - Install MySQL on the database server

## Commands for MySQL Installation
```bash
sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-5.noarch.rpm
sudo yum localinstall mysql80-community-release-el9-5.noarch.rpm  
sudo yum install mysql-community-server
sudo systemctl start mysqld.service
