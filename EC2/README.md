---

# 🖥️ **AWS EC2 – Complete Tutorial, Use Cases, Best Practices & Interview Questions**

<p align="center">
  <img src="https://img.shields.io/badge/AWS-EC2-orange?logo=amazon-aws&logoColor=white" />
  <img src="https://img.shields.io/badge/Compute-Virtual%20Server-blue" />
  <img src="https://img.shields.io/badge/Beginner%20to%20Pro-success" />
  <img src="https://img.shields.io/badge/Hands--On%20Guide-Yes-green" />
  <a href="https://github.com/Thiyagu-2003">
    <img src="https://img.shields.io/badge/Made%20By-Thiyagu%20S-green?logo=github" />
  </a>
</p>

---

# 📑 **Table of Contents**

* [🔍 Overview](#-overview)
* [🧠 EC2 Real-World Use Cases](#-ec2-real-world-use-cases)
* [🏗️ EC2 Launch Tutorial](#-1-ec2-launch-tutorial)

  * [1️⃣ Select AMI](#1️⃣-select-ami)
  * [2️⃣ Choose Instance Type](#2️⃣-choose-instance-type)
  * [3️⃣ Configure Network](#3️⃣-configure-network)
  * [4️⃣ Security Groups](#4️⃣-configure-security-groups)
  * [5️⃣ Key Pair](#5️⃣-key-pair)
  * [6️⃣ Storage (EBS)](#6️⃣-storage-ebs)
  * [7️⃣ Launch Instance](#7️⃣-launch-instance)
* [🧪 Connectivity Testing](#-connectivity-testing)
* [🌐 Host a Web Server](#-host-a-simple-web-server)
* [🛑 Stop vs Terminate](#-stop-vs-terminate)
* [💸 Cost-Saving Best Practices](#-cost-saving-best-practices)
* [🧹 Safe Cleanup](#-delete-ec2-safely)
* [🔑 SSH Key Notes](#-ssh-key-notes)
* [⚠️ Common Mistakes](#️-common-ec2-mistakes)
* [🎯 Architecture Patterns](#-real-world-architecture-patterns)
* [💡 Interview Questions](#-important-ec2-interview-questions)
* [👤 Author](#-author)

---

# 🔍 **Overview**

EC2 (**Elastic Compute Cloud**) is AWS’s virtual machine offering.
You control:

**CPU • RAM • Storage • OS • Networking • Security**

It’s simply a flexible cloud-based VM — nothing more, nothing less.

---

# 🧠 **EC2 Real-World Use Cases**

### ✔ Web Apps & APIs

Node, Python, PHP, Java — everything runs fine.

### ✔ Microservices & Background Workers

Schedulers, API services, automation tasks.

### ✔ CI/CD Runners

Jenkins, GitLab runners, DevOps agents.

### ✔ Self-Managed Databases

MySQL, PostgreSQL, MongoDB (not ideal for production).

### ✔ Bastion / VPN Hosts

For secure private subnet access.

### ✔ High-Performance Computing

ML training, GPU workloads, simulation, rendering.

### ✔ Custom OS-Level Tasks

Monitoring agents, proxies, deep OS control.

---

# 🏗️ **1. EC2 Launch Tutorial**

## 1️⃣ **Select AMI**

Choose your OS:

* **Amazon Linux 2023** (recommended default)
* Ubuntu 22.04
* Windows Server
* Custom AMIs (golden images)

Pick based on your application requirements — no drama here.

---

## 2️⃣ **Choose Instance Type**

| Instance Type            | Best For                   |
| ------------------------ | -------------------------- |
| **t2.micro / t3.micro**  | Free tier, small workloads |
| **t3.small / t3.medium** | Lightweight apps           |
| **m5 series**            | General workloads          |
| **c5 series**            | Compute-intensive tasks    |
| **g4/g5**                | GPU / ML workloads         |

Match CPU/RAM to your need, not your ego.

---

## 3️⃣ **Configure Network**

Assign:

* VPC
* Subnet (public/private)
* Auto-assign Public IP (enable **only** for public instances)

**Public EC2:**
✔ Public Subnet
✔ Public IP
✔ IGW Route

**Private EC2:**
✔ Private Subnet
❌ No Public IP
✔ NAT Gateway Route (for updates)

---

## 4️⃣ **Configure Security Groups**

### 🔓 Public EC2 (Web Server SG)

```
SSH (22)   → Your IP only
HTTP (80)  → 0.0.0.0/0
HTTPS (443) → 0.0.0.0/0
```

Opening SSH to the world = asking for trouble.

### 🔐 Private EC2

```
Allow inbound only from Public SG ID
No direct internet exposure
```

This is the correct production-grade setup.

---

## 5️⃣ **Key Pair**

* Create/download `.pem`
* Store it securely
* AWS won’t let you download again
* Don’t upload to GitHub or share

---

## 6️⃣ **Storage (EBS)**

Defaults work fine:

* gp3
* 8–20 GB typical

More storage = more cost. Keep it realistic.

---

## 7️⃣ **Launch Instance**

Hit launch.
EC2 boots in **~20–30 seconds**.

---

# 🧪 **Connectivity Testing**

### ➤ SSH (Amazon Linux)

```
ssh -i your-key.pem ec2-user@<public-ip>
```

### ➤ SSH (Ubuntu)

```
ssh -i your-key.pem ubuntu@<public-ip>
```

### ➤ Update server

Amazon Linux:

```
sudo yum update -y
```

Ubuntu:

```
sudo apt update -y
```

---

# 🌐 **Host a Simple Web Server**

```
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
echo "EC2 is working" | sudo tee /var/www/html/index.html
```

Visit:

```
http://<public-ip>
```

If you see nothing → your **Security Group is misconfigured**.

---

# 🛑 **Stop vs Terminate**

### 🟡 Stop

* Keeps EBS
* Stops compute billing
* Restart anytime

### 🔴 Terminate

* EC2 destroyed
* EBS deleted (unless unchecked)

Don’t mix these up if you value your data.

---

# 💸 **Cost-Saving Best Practices**

✔ Stop unused instances
✔ Choose **t-series** instead of m-series when possible
✔ Delete unused EBS volumes
✔ Release unused Elastic IPs
✔ Use Spot Instances for dev/testing
✔ Prefer managed services (RDS, ECS, Lambda)

Bad EC2 hygiene drains your wallet fast.

---

# 🧹 **Delete EC2 Safely**

1. Terminate the instance
2. Delete leftover EBS volumes
3. Release Elastic IP
4. Remove unused Security Groups
5. Remove NAT if created only for this EC2

Elastic IPs silently drain money when idle.

---

# 🔑 **SSH Key Notes**

Avoid:

❌ Uploading `.pem` online
❌ Incorrect file permissions
❌ Keeping keys on shared systems
❌ Bad conversions to `.ppk`

If using PuTTY → convert `.pem → .ppk` using PuTTYgen.

---

# ⚠️ **Common EC2 Mistakes**

🚫 Wrong subnet → no internet
🚫 SSH (22) open to the world
🚫 No public IP assigned
🚫 Private EC2 without NAT → no updates
🚫 Forgotten EBS volumes
🚫 Unreleased Elastic IPs

90% of EC2 issues = your network configuration is wrong.

---

# 🎯 **Real-World Architecture Patterns**

### ⭐ Public EC2 (Web Server) → Private RDS

Standard production design.

### ⭐ Bastion Host for SSH

Correct way to access private EC2.

### ⭐ Auto Scaling + ALB

For scalable web applications.

### ⭐ Private EC2 via NAT

Secure + outbound updates.

### ⭐ EC2 as DevOps Runner

Jenkins, GitLab, or custom automation nodes.

---

# 💡 **Important EC2 Interview Questions**

1. Difference between Stop vs Terminate?
2. What components make up an AMI?
3. Instance Store vs EBS — explain differences.
4. Why avoid exposing EC2 directly to the internet?
5. What if a key pair is deleted?
6. How to SSH into private EC2?
7. SG vs NACL — what’s the difference?
8. What is a Placement Group?
9. When should Spot Instances be used?
10. What is EC2 Hibernate and when use it?

---

# 👤 **Author**

```
Name: Thiyagu S
Role: Cloud & DevOps Learner
Country: India 🇮🇳
```

---

# ❤️ **Footer**

<p align="center">
  <strong>Made with ❤️ by <a href="https://github.com/Thiyagu-2003">Thiyagu S</a></strong><br>
  Learning • Building • Improving
</p>

---
