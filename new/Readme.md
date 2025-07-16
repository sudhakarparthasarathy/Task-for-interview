# AWS Web Application Infrastructure (IaC)

![AWS Architecture Diagram](architecture-diagram.png) *(visual diagram goes here)*

## Overview

This project deploys a scalable, secure web application infrastructure on AWS using Infrastructure as Code (CloudFormation). The architecture includes:

- **Public-facing Application Load Balancer**
- **Auto-scaled EC2 instances** in private subnets running Nginx/Apache
- **Multi-AZ RDS PostgreSQL database**
- **Highly available networking** across two Availability Zones

## Architecture Components

### Networking
- Custom VPC with CIDR `10.0.0.0/16`
- Public and private subnets across 2 AZs
- Internet Gateway for public subnets
- NAT Gateways for private subnet internet access
- Route tables with appropriate routes

### Compute
- Auto Scaling Group with EC2 instances
- Launch Template with UserData to install web server
- Application Load Balancer in public subnets

### Database
- RDS PostgreSQL instance in private subnets
- Multi-AZ deployment for high availability
- Not publicly accessible

### Security
- Security groups with least-privilege rules
- IAM role for EC2 instances (SSM access only)
- Database credentials passed securely via parameters

## Prerequisites

1. **AWS Account** with sufficient permissions
2. **AWS CLI** configured with credentials
3. **GitHub Secrets** (if using CI/CD):
   - `AWS_ACCESS_KEY_ID`
   - `AWS_SECRET_ACCESS_KEY`
   - `AWS_REGION`
   - `DB_USERNAME`
   - `DB_PASSWORD`

## Deployment

### Manual Deployment
```bash
aws cloudformation deploy \
  --stack-name webapp-infra \
  --template-file webapp-infra.yaml \
  --parameter-overrides \
    EnvironmentName=WebApp \
    DBUsername=admin \
    DBPassword=YourSecurePassword123 \
  --capabilities CAPABILITY_IAM
