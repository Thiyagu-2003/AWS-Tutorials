---

# 🌐 **Domain + Route 53 + EC2 Deployment Guide**

<p align="center">
  <img src="https://img.shields.io/badge/AWS-Route%2053-orange?logo=amazon-aws&logoColor=white" />
  <img src="https://img.shields.io/badge/AWS-EC2-blue?logo=amazon-aws&logoColor=white" />
  <img src="https://img.shields.io/badge/DNS-Management-green" />
  <img src="https://img.shields.io/badge/Status-Production%20Ready-success" />
  <a href="https://github.com/Thiyagu-2003">
    <img src="https://img.shields.io/badge/Made%20By-Thiyagu%20S-green?logo=github" />
  </a>
</p>

---

<div align="center">

### ⚡ A complete, polished guide to map a custom domain to an EC2 instance using Route 53.

</div>

---

# 📑 **Table of Contents**

* [🚀 Overview](#-overview)
* [🛒 Step-0: Buy a Domain](#-step-0-buy-a-domain)
* [🌐 Step-1: Create a Hosted Zone](#-step-1-create-a-hosted-zone)
* [📝 Step-2: Update Registrar Name Servers](#-step-2-update-registrar-name-servers)
* [🖥️ Step-3: Create EC2 With Web Server](#️-step-3-create-ec2-with-web-server)
* [🔗 Step-4: Map Domain to EC2](#-step-4-map-domain-to-ec2)
* [🧭 Step-5: Explore Route 53 Features](#-step-5-explore-route-53-features)
* [💡 DNS Interview Questions](#-dns-interview-questions)
* [📌 Record Types](#-record-types)
* [📡 Routing Policies](#-routing-policies)
* [❤️ Health Checks](#️-health-checks)
* [👤 Author](#-author)
* [📄 License](#-license)

---

# 🚀 Overview

This guide walks through the exact steps to:

✔ Connect a purchased domain to AWS
✔ Use Route 53 as the authoritative DNS
✔ Host a website on EC2
✔ Configure A, CNAME, MX, NS, SOA records
✔ Understand routing policies and health checks

Everything is structured for clarity — no shortcuts or confusion.

---

# 🛒 **Step-0: Buy a Domain**

Buy your domain from:

* GoDaddy
* Namecheap
* Hostinger
* Google Domains (if available)

You only need **DNS management access**.

---

# 🌐 **Step-1: Create a Hosted Zone**

1. Go to **AWS → Route 53 → Hosted Zones → Create Hosted Zone**
2. Enter your domain
3. Choose **Public Hosted Zone**

AWS creates:

* **NS Record**
* **SOA Record**

These define DNS authority.

---

# 📝 **Step-2: Update Registrar Name Servers**

1. Open your domain registrar dashboard
2. Replace their NS values with **Route 53 NS records**
3. Remove trailing dots

Propagation time: **5–30 minutes**.

---

# 🖥️ **Step-3: Create EC2 With Web Server**

Launch EC2 with this User Data:

```bash
#!/bin/bash
yum install httpd -y
systemctl start httpd
systemctl enable httpd
echo "Welcome to my website" > /var/www/html/index.html
```

Ensure:

* Port **80** is open
* Use an **Elastic IP**

---

# 🔗 **Step-4: Map Domain to EC2**

Create an **A Record**:

| Setting | Value          |
| ------- | -------------- |
| Name    | yourdomain.com |
| Type    | A              |
| Value   | Elastic IP     |
| TTL     | 300            |
| Routing | Simple         |

After propagation → your website goes live.

---

# 🧭 **Step-5: Explore Route 53 Features**

Route 53 provides:

* Alias records
* Failover routing
* Latency-based routing
* DNS load balancing
* Traffic flow
* Record visual editor
* Health checks

These are core interview topics.

---

# 💡 **DNS Interview Questions**

## **1. What is a Hosted Zone?**

A container inside Route 53 holding DNS records for a domain.

Types:

* **Public Hosted Zone**
* **Private Hosted Zone**

---

# 📌 **Record Types**

### 🟦 A Record

Maps domain → IPv4

### 🟪 AAAA Record

Maps domain → IPv6

### 🟧 CNAME Record

Points one domain → Another domain
Used for ALB, CloudFront, S3 sites

### 🟩 MX Record

Mail server routing

### 🟨 NS Record

Authoritative name servers

### 🟥 SOA Record

Domain metadata (serial, TTL, refresh)

---

# 📡 **Routing Policies**

| Policy                | Use Case                    |
| --------------------- | --------------------------- |
| **Simple**            | Single server               |
| **Weighted**          | Percentage-based split      |
| **Latency-Based**     | Lowest latency region       |
| **Failover**          | Primary → secondary         |
| **Geolocation**       | Country/continent           |
| **Geoproximity**      | Location + bias             |
| **Multivalue Answer** | Round-robin + health checks |

---

# ❤️ **Health Checks**

Monitors:

* Web servers
* APIs
* Load balancers

If primary fails → Route 53 shifts traffic to backup.

---

# 👤 **Author**

```
Name: Thiyagu S
Role: Cloud & DevOps Learner
Country: India 🇮🇳
```

---

# 📄 **License**

This project is licensed under the **MIT License**.
The full license text is available here:

➡️ [`LICENSE`](./LICENSE)

---

# ❤️ **Footer**

<p align="center">
  <strong>Made with ❤️ by <a href="https://github.com/Thiyagu-2003">Thiyagu S</a></strong><br>
  Learning • Building • Improving
</p>

---
