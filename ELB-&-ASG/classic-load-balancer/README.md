<h1 align="center">❌ <strong>AWS Classic Load Balancer (CLB)</strong></h1>

<h3 align="center">Hands-On: Classic Load Balancer with Auto Scaling Group</h3>

<p align="center">
  <img src="https://img.shields.io/badge/AWS-Classic_Load_Balancer-red?logo=amazon-aws" />
  <img src="https://img.shields.io/badge/Service-Legacy-important" />
  <img src="https://img.shields.io/badge/Auto_Scaling-Enabled-blue" />
  <img src="https://img.shields.io/badge/Hands--On-Lab-success" />
  <a href="https://github.com/Thiyagu-2003">
    <img src="https://img.shields.io/badge/Made%20By-Thiyagu%20S-brightgreen?logo=github" />
  </a>
</p>

---

## 📚 Index

1. [Overview](#-overview)
2. [Architecture Used](#-architecture-used)
3. [Prerequisites](#-prerequisites)
4. [Hands-On Implementation](#-hands-on-implementation)
   - [Step 1: Create EC2 Launch Template](#-step-1-create-ec2-launch-template)
   - [Step 2: Create Classic Load Balancer](#-step-2-create-classic-load-balancer-clb)
   - [Step 3: Create Auto Scaling Group](#-step-3-create-auto-scaling-group-asg)
   - [Step 4: Verify Load Balancing](#-step-4-verify-load-balancing)
5. [Testing Auto Scaling](#-testing-auto-scaling)
6. [Common Mistakes](#-common-mistakes-you-avoided-here)
7. [Interview Notes](#-interview-notes-classic-load-balancer)
8. [Conclusion](#-conclusion)
9. [Author](#-author)

---

## 📌 Overview

This hands-on demonstrates how to deploy a **Classic Load Balancer (CLB)** integrated with an **Auto Scaling Group (ASG)** to distribute traffic across multiple EC2 instances.

> ⚠️ **Important**
> - Classic Load Balancer is **legacy**
> - Do **NOT** use this for new production systems
> - Learn it only for **certifications, interviews, and legacy support**

---

## Diagram

![classic-load-balancer-1](https://github.com/user-attachments/assets/2c7a6786-0b52-4120-ad62-94969f7f9073)


---

## 🧩 Architecture Used

```text
User
 ↓
Classic Load Balancer
 ↓
EC2 Instances (Auto Scaling Group)
 ↓
Apache Web Server
````

---

## 🛠️ Prerequisites

* AWS account
* EC2 key pair
* Security Group allowing:

  * HTTP (80)
  * SSH (22)
* Basic Linux knowledge

---

## 🚀 Hands-On Implementation

---

### 🔹 Step 1: Create EC2 Launch Template

We use a **Launch Template**, not manual EC2 creation.
Auto Scaling Groups depend on this.

#### User Data Script (Dynamic – Correct Approach)

> ❌ Wrong approach: hardcoding “Server 1 / Server 2”
> ✅ Correct approach: use hostname for scalability

```bash
#!/bin/bash
yum install httpd -y
systemctl start httpd
systemctl enable httpd

echo "<h1>Server: $(hostname)</h1>" > /var/www/html/index.html
```

✔ Works for unlimited instances
✔ ASG-friendly
✔ No manual edits required

---

### 🔹 Step 2: Create Classic Load Balancer (CLB)

1. Navigate to **EC2 → Load Balancers**
2. Click **Create Load Balancer**
3. Select **Classic Load Balancer**
4. Configure:

   * Listener: `HTTP : 80`
   * VPC & Subnets
   * Health Check:

     * Ping Path: `/`
     * Port: `80`
5. Attach Security Group:

   * Allow HTTP (80)
   * Note: Wait for load balancer status check to be compete (2/2)
   * Then hit the load balancer link to the web
   * Example: the load balancer link looks like this : my-CLB-1079842188.us-east-1.elb.amazonaws.com

---

### 🔹 Step 3: Create Auto Scaling Group (ASG)

1. Go to **EC2 → Auto Scaling Groups**
2. Create ASG using the **Launch Template**
3. Set capacity:

   * Minimum: `2`
   * Desired: `2`
   * Maximum: `4`
4. Attach the **Classic Load Balancer**
5. Enable:

   * EC2 health checks
   * ELB health checks

✔ ASG will automatically:

* Launch instances
* Register them with CLB
* Replace unhealthy instances

---

### 🔹 Step 4: Verify Load Balancing

1. Copy the **CLB DNS name**
2. Open it in a browser
3. Refresh multiple times

Expected output:

```text
Server: ip-10-0-1-23
Server: ip-10-0-2-45
```

✔ Confirms traffic distribution
✔ Confirms ASG integration

---

## 🧪 Testing Auto Scaling

### Manual Test

* Manually terminate one EC2 instance
* ASG launches a replacement automatically
* CLB registers the new instance

✔ No downtime
✔ Self-healing confirmed

---

## ❌ Common Mistakes (You Avoided Here)

* ❌ Creating EC2 instances manually
* ❌ Hardcoding server names
* ❌ Skipping ELB health checks
* ❌ Using CLB for microservices (architecturally wrong)

---

## 🎤 Interview Notes (Classic Load Balancer)

* Operates at **Layer 4 & limited Layer 7**
* No path-based routing
* No host-based routing
* Legacy AWS service
* Replaced by **ALB & NLB**
* Still exists in older production systems

---

## 🔚 Conclusion

Classic Load Balancer with Auto Scaling helps understand **basic AWS load-balancing concepts**, but it is **not suitable for modern architectures**.

> Learn it → Understand it → **Move on to ALB & NLB**

---

## 👤 Author

```text
Name    : Thiyagu S
Role    : Cloud & DevOps Learner
Location: India 🇮🇳
GitHub  : Thiyagu-2003
```

---

<p align="center">
  <strong>Made with ❤️ by <a href="https://github.com/Thiyagu-2003">Thiyagu S</a></strong><br>
  Learn • Build • Scale
</p>

---




