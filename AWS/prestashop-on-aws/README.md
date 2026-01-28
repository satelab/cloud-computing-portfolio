# PrestaShop Deployment on AWS (Free Tier)

## Project Overview
This project demonstrates the deployment of a PrestaShop e-commerce application
on AWS using a two-tier architecture where the database and application run on
separate EC2 instances. The setup follows AWS best practices for security,
scalability, and performance while remaining within AWS Free Tier limits.

**Project Author:** Michael Isonguyo

## Requirements
- PrestaShop installed on AWS
- Database hosted on a separate server
- AWS Free Tier only
- Publicly accessible application URL
- Full documentation with screenshots

## Architecture
**Two-Tier Architecture**
- Database Tier (Private)
- Application Tier (Public)

### Server 1 – Database Server
- EC2 t2.micro (Ubuntu 22.04 LTS)
- MySQL 8.0
- Private access only

### Server 2 – Application Server
- EC2 t2.micro (Ubuntu 22.04 LTS)
- Apache 2.4
- PHP 8.1+
- PrestaShop 8.1.7
- Public HTTP access

![Architecture Diagram](architecture/architecture-diagram.png)

## AWS Services Used
- Amazon EC2
- Security Groups
- Elastic Block Store (EBS)

## Live Application
- Frontend: http://16.171.226.187
- Admin Panel: http://16.171.226.187/admin_secure

## Skills Demonstrated
- AWS EC2 provisioning
- Multi-tier architecture design
- Linux server administration
- MySQL remote configuration
- Apache & PHP configuration
- Cloud security best practices
- Troubleshooting production issues
