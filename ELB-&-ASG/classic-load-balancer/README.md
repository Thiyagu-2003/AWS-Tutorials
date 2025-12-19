<h1 align="center">❌ <strong>AWS Classic Load Balancer (CLB)</strong></h1>

<h3 align="center">Hands-On: Classic Load Balancer with Auto Scaling Group</h3>

<p align="center">
  <img src="https://img.shields.io/badge/AWS-Classic_Load_Balancer-red?logo=amazon-aws" />
  <img src="https://img.shields.io/badge/Service-Legacy-important" />
  <img src="https://img.shields.io/badge/Auto_Scaling-Enabled-blue" />
  <img src="https://img.shields.io/badge/Hands--On-Lab-success" />
</p>

---

## 📌 Overview

This hands-on demonstrates how to deploy a **Classic Load Balancer (CLB)** integrated with an **Auto Scaling Group (ASG)** to distribute traffic across multiple EC2 instances.

> ⚠️ **Important**
> - Classic Load Balancer is **legacy**
> - Do **NOT** use this for new production systems
> - Learn it only for **certifications, interviews, and legacy support**

---

## 🧩 Architecture Used

```

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

- AWS account
- EC2 key pair
- Security Group allowing:
  - HTTP (80)
  - SSH (22)
- Basic Linux knowledge

---

## 🚀 Hands-On Implementation

---

### 🔹 Step 1: Create EC2 Launch Template

We use a **Launch Template**, not manual EC2 creation.  
ASG depends on this.

#### User Data Script (Dynamic – Correct Way)

> ❌ Your original mistake: hardcoding “Server 1 / Server 2”  
> ✅ Correct: use instance hostname so ASG scales properly

```bash
#!/bin/bash
yum install httpd -y
systemctl start httpd
systemctl enable httpd

echo "<h1>Server: $(hostname)</h1>" > /var/www/html/index.html
````

✔ This works for **any number of instances**
✔ No manual changes needed

---

### 🔹 Step 2: Create Classic Load Balancer (CLB)

1. Go to **EC2 → Load Balancers**

2. Click **Create Load Balancer**

3. Select **Classic Load Balancer**

4. Configure:

   * Listener: HTTP : 80
   * VPC & subnets
   * Health Check:

     * Ping Path: `/`
     * Port: 80

5. Attach **Security Group**

   * Allow HTTP (80)

---

### 🔹 Step 3: Create Auto Scaling Group (ASG)

1. Go to **EC2 → Auto Scaling Groups**
2. Create ASG using the **Launch Template**
3. Configure:

   * Min: `2`
   * Desired: `2`
   * Max: `4`
4. Attach the **Classic Load Balancer**
5. Enable:

   * ELB health checks
   * EC2 health checks

✔ ASG will now:

* Launch instances automatically
* Register them with CLB
* Replace unhealthy instances

---

### 🔹 Step 4: Verify Load Balancing

1. Copy **CLB DNS name**
2. Open in browser
3. Refresh multiple times

Expected output:

```
Server: ip-10-0-1-23
Server: ip-10-0-2-45
```

✔ Confirms traffic is distributed
✔ Confirms ASG integration

---

## 🧪 Testing Auto Scaling

### Manual Test (Simple)

* Terminate one EC2 instance manually
* ASG automatically launches a replacement
* CLB registers new instance

✔ Zero downtime

---

## ❌ Common Mistakes (You Avoided Here)

* ❌ Creating EC2 manually instead of Launch Template
* ❌ Hardcoding server names
* ❌ Not enabling ELB health checks
* ❌ Using CLB for microservices (wrong)

---

## 🎤 Interview Notes (Classic Load Balancer)

* CLB operates at **Layer 4 & basic Layer 7**
* No path-based routing
* No host-based routing
* Legacy service
* Replaced by **ALB & NLB**
* Still appears in **older architectures**

---

## 🔚 Conclusion

Classic Load Balancer with Auto Scaling demonstrates **basic AWS load balancing concepts**, but it is **not suitable for modern applications**.

> Learn it → Understand it → **Move on to ALB / NLB**

---

## 👤 Author

```
Name    : Thiyagu S
Role    : Cloud & DevOps Learner
Location: India 🇮🇳
GitHub  : Thiyagu-2003
```

---

## ❤️ Footer

<p align="center">
  <strong>Made with ❤️ by <a href="https://github.com/Thiyagu-2003">Thiyagu S</a></strong><br>
  Learn • Build • Scale
</p>


---
