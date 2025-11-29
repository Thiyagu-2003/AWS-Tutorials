---

# 🛡️ **VPC TUTORIAL – MANUAL SETUP GUIDE**

<p align="center">
  <img src="https://img.shields.io/badge/AWS-VPC-orange?logo=amazon-aws&logoColor=white" />
  <img src="https://img.shields.io/badge/Subnets-Public%20%2B%20Private-blue" />
  <img src="https://img.shields.io/badge/NAT%20Gateway-Enabled-green" />
  <img src="https://img.shields.io/badge/Level-Beginner%20to%20Pro-success" />
</p>

---

# 📑 **Table of Contents**

* [🔰 Overview](#-overview)
* [🏗️ 1. Manual VPC Creation](#️-1-manual-vpc-creation)
* [📦 2. Subnet Setup](#-2-subnet-setup)
* [🌍 3. Internet Gateway](#-3-internet-gateway)
* [🗺️ 4. Route Tables](#️-4-route-tables)
* [🛡️ 5. Security Groups](#️-5-security-groups)
* [🖥️ 6. EC2 Setup](#️-6-ec2-setup)
* [🌐 7. NAT Gateway](#-7-nat-gateway)
* [🧪 8. Connectivity Testing](#-8-connectivity-testing)
* [⚠️ Common Mistakes to Avoid](#️-common-mistakes-to-avoid)
* [🎯 Real-World Use Cases](#-real-world-use-cases)
* [💡 Important VPC Interview Questions](#-important-vpc-interview-questions)
* [🗑️ Delete VPC Safely](#️-delete-vpc-safely)
* [🔑 SSH Command](#-ssh-command)
* [🔁 Key Conversion (PEM ↔ PPK)](#-key-conversion-pem--ppk)
* [👤 Author](#-author)

---

# 🔰 **Overview**

A complete manual guide to building a **production-style VPC** from scratch:

✔ Custom VPC
✔ Public + Private Subnets
✔ IGW + NAT Gateway
✔ Route Tables
✔ EC2 in Public + EC2 in Private
✔ Private Instance → Internet via NAT
✔ SSH into private via bastion (public EC2)

Nothing unnecessary. No shortcuts.

---

# 🏗️ **1. Manual VPC Creation**

### **Step 1: Create VPC**

Choose your VPC CIDR:

```
10.0.0.0/16   (65,536 IPs)
```

---

# 📦 **2. Subnet Setup**

Create **two subnets**:

### **Public Subnet**

```
10.0.1.0/24
```

### **Private Subnet**

```
10.0.2.0/24
```

Requirements:

* Use **different AZs**
* Public subnet → Internet access
* Private subnet → No public access (internet via NAT)

---

# 🌍 **3. Internet Gateway**

1. Create **Internet Gateway**
2. Attach to the VPC

This enables outbound internet for the public subnet.

---

# 🗺️ **4. Route Tables**

### **Public Route Table**

* Associate with Public Subnet
* Add:

```
0.0.0.0/0 → Internet Gateway
```

### **Private Route Table**

* Associate with Private Subnet
* Later will add NAT route

---

# 🛡️ **5. Security Groups**

### **Public SG**

Allow:

```
SSH (22) → 0.0.0.0/0
HTTP (80) → 0.0.0.0/0
HTTPS (443) → 0.0.0.0/0
```

### **Private SG**

Allow:

```
All TCP → Source: Public SG-ID
```

Private instance is protected from the internet.

---

# 🖥️ **6. EC2 Setup**

### **Public EC2**

* Subnet: Public
* SG: Public SG
* Public IP: Enabled

### **Private EC2**

* Subnet: Private
* SG: Private SG
* Public IP: Disabled

Login to private EC2 → via SSH inside the public EC2.

---

# 🌐 **7. NAT Gateway**

1. Create NAT Gateway **inside Public Subnet**
2. Allocate Elastic IP
3. Update Private Route Table:

```
0.0.0.0/0 → NAT Gateway
```

Now private EC2 has secure outbound internet.

---

# 🧪 **8. Connectivity Testing**

### **Public EC2 Test**

```
ping google.com
sudo yum update -y
```

### **SSH into Private EC2**

From Public EC2:

```
ssh -i key.pem ec2-user@<private-ip>
```

### **Test Internet from Private EC2**

```
ping google.com
sudo yum update -y
```

If this works → network is correct.

---

# ⚠️ **Common Mistakes to Avoid**

These are the mistakes juniors repeat — avoid them:

### ❌ Using same AZ for both subnets

**Fix:** Keep subnets across different AZs for HA.

### ❌ Giving private EC2 a public IP

This defeats the concept of “private”.

### ❌ Not associating subnets with correct route tables

Most common reason for “no internet”.

### ❌ Creating NAT Gateway in private subnet

It **must** be inside public subnet.

### ❌ Forgetting to attach Internet Gateway

Public subnet becomes dead.

### ❌ Security Group allows 0.0.0.0/0 for everything

Sloppy and unsafe.

---

# 🎯 **Real-World Use Cases**

### 🏢 **1. 2-Tier Web App**

* Public Subnet → Web Server
* Private Subnet → DB Server

A classic architecture.

---

### 🛡️ **2. Bastion Host Setup**

Public EC2 used to SSH into private EC2s.

---

### 🛠️ **3. Backend Application Servers**

Private EC2s running APIs with no public exposure.

---

### ☁️ **4. Hybrid Cloud VPN Setup**

On-prem to AWS via VPN inside custom VPC.

---

### 💼 **5. Secure Microservices**

Different private subnets running isolated services.

---

# 💡 **Important VPC Interview Questions**

These are the real questions interviewers use — no fluff.

### **1. What is the difference between SG and NACL?**

### **2. Can a subnet span across multiple AZs?**

### **3. Why do we need a NAT Gateway?**

### **4. Difference between NAT Instance vs NAT Gateway?**

### **5. What happens if IGW is not attached?**

### **6. What is a Bastion Host? Why do we need it?**

### **7. How does traffic flow from private subnet to internet?**

### **8. Explain Route Table Propagation?**

### **9. Can a Private Subnet talk to the internet without NAT?**

### **10. Difference between Public, Private & VPN-only subnets?**

---

# 🗑️ **Delete VPC Safely**

Delete in this order:

1. NAT Gateway
2. Release Elastic IP
3. Detach + Delete IGW
4. Delete Subnets
5. Delete Route Tables
6. Delete SGs
7. Delete VPC

Avoid unexpected billing.

---

# 🔑 **SSH Command**

```
ssh -i <your-key.pem> ec2-user@<public-ip>
```

---

# 🔁 **Key Conversion (PEM ↔ PPK)**

### **Convert PEM → PPK (PuTTY)**

1. Open PuTTYgen
2. Load `.pem`
3. Save `.ppk`

### **Convert PPK → PEM**

1. Load `.ppk` in PuTTYgen
2. Conversions → Export OpenSSH key
3. Save `.pem`

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