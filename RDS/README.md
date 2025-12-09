---
# 📘 Amazon RDS + EC2 Connectivity – Best Practice Guide

<p align="center">
<img src="https://img.shields.io/badge/AWS-RDS-blue?logo=amazon-aws&logoColor=white"/>
<img src="https://img.shields.io/badge/Database-MySQL%2FPostgreSQL-lightblue" />
<img src="https://img.shields.io/badge/Security-Best%20Practice-brightblue" />
<a href="https://github.com/Thiyagu-2003">
<img src="https://img.shields.io/badge/Made%20By-Thiyagu%20S-green?logo=github" />
</a>
</p>

---

## 📑 Table of Contents

- 🔎 [Overview](#-overview)
- 🚀 [Create RDS Instance](#-create-rds-instance)
- 🔥 [Configure RDS Security Group](#-configure-rds-security-group)
- 🖥️ [Configure EC2 Instance](#️-configure-ec2-instance)
- 🔗 [Connect EC2 to RDS](#-connect-ec2-to-rds)
- 🛡️ [Best Practices](#️-best-practices)
- ❓ [Interview Questions](#-interview-questions)
- 🏗️ [Architecture Diagram](#️-architecture-diagram)
- 👤 [Author](#-author)
- ❤️ [Footer](#️-footer)

---

## 🔎 Overview

Amazon RDS allows **fully managed relational databases** in AWS.

This guide covers **secure EC2 → RDS connectivity** using **EC2 SG references**, which is AWS best practice.

**Use Cases:**

- Private database connections
- Application access from EC2
- Automation & monitoring pipelines
- Production-grade secure setups

---

## 🚀 Create RDS Instance

### Step 1: AWS RDS Console → Create Database

- **Creation Method:** Standard Create
- **Engine:** MySQL / PostgreSQL
- **Template:** Free Tier (optional)

### Step 2: Configure DB Settings

- **DB Identifier:** `mydb`
- **Master Username:** `admin`
- **Password:** Strong custom password

### Step 3: Instance Size

- **Free Tier:** `db.t3.micro`
- **Storage:** GP3 (20GB)

### Step 4: Connectivity Settings

- **VPC:** Same as EC2
- **Subnet Group:** Default or custom
- **Public Access:** ❌ NO
- **VPC Security Group:** Create new → `rds-sg`

### Step 5: Additional Configurations

- Backup retention: Optional
- Monitoring: Optional
- Click **Create Database**

---

## 🔥 Configure RDS Security Group

### Why?

Never open RDS to the public (`0.0.0.0/0`). Allow **only EC2 SG → RDS SG**.

### Steps

1. Identify EC2 Security Group → `ec2-sg`
2. Edit RDS SG (`rds-sg`)

| Type         | Port | Source                          |
| ------------ | ---- | ------------------------------- |
| MySQL/Aurora | 3306 | **EC2 Security Group (ec2-sg)** |
| PostgreSQL   | 5432 | **EC2 Security Group (ec2-sg)** |

> AWS internally resolves **SG → SG references**; no IP is needed.

---

## 🖥️ Configure EC2 Instance

### Step 1: Launch EC2

- OS: Amazon Linux 2 / Ubuntu
- Assign `ec2-sg` security group

### Step 2: Install DB Client

**MySQL:**

```bash
sudo yum install mariadb -y
```

**PostgreSQL:**

```bash
sudo yum install postgresql -y
```

---

## 🔗 Connect EC2 to RDS

### Step 1: Get RDS Endpoint

`RDS → Databases → mydb → Connectivity → Endpoint`

### Step 2: Connect

**MySQL:**

```bash
mysql -h your-rds-endpoint.amazonaws.com -u admin -p
```

**PostgreSQL:**

```bash
psql -h your-rds-endpoint.amazonaws.com -U admin -d postgres
```

> ✅ Connection successful → SG configuration is correct.

---

## 🛡️ Best Practices

- ❌ Never expose RDS publicly
- ✅ Use **EC2 SG → RDS SG** only
- Enable automated backups
- Enable storage encryption
- Rotate passwords regularly
- Use IAM roles, not hardcoded credentials
- Store credentials in **AWS Secrets Manager**, not `.env` files

---

## ❓ Interview Questions

1. What is Amazon RDS and when should you use it?
2. Why use RDS instead of running a DB on EC2?
3. Which engines does RDS support?
4. What is Multi-AZ and how does it work?
5. Difference between RDS Read Replicas vs Multi-AZ?
6. How do automated backups differ from snapshots?
7. What happens during Multi-AZ failover?
8. Can you SSH into an RDS instance? Why not?
9. What are parameter groups and option groups?
10. How do security groups control RDS access?
11. Why should RDS NOT be publicly accessible?
12. How do you connect EC2 to RDS in a private subnet?
13. What is an RDS endpoint?
14. How do you scale RDS vertically & horizontally?
15. How do you troubleshoot “timeout” when EC2 cannot connect?

---

## 🏗️ Architecture Diagram

```
         🌐 Local Machine
           ┌─────────────┐
           │  SSH / DB   │
           └─────┬───────┘
                 │ 22 (SSH)
                 ▼
        🖥️ EC2 Instance (SG: ec2-sg)
           ┌─────────────┐
           │ Application │
           └─────┬───────┘
                 │ 3306 / 5432
                 ▼
        🗄️ RDS Database (SG: rds-sg)
           ┌─────────────┐
           │  MySQL/PG   │
           └─────────────┘
```

> EC2 acts as a **secure jump host**, RDS is never exposed publicly.

---

## 👤 Author

**Name:** Thiyagu S  
**Role:** Cloud & DevOps Learner  
**Country:** India 🇮🇳  
**GitHub:** [Thiyagu-2003](https://github.com/Thiyagu-2003)

---

## ❤️ Footer

<p align="center">
  <strong>Made with ❤️ by <a href="https://github.com/Thiyagu-2003">Thiyagu S</a></strong><br>
  <em>Learning • Building • Improving</em>
</p>

---
