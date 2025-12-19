---

# 🚀 AWS Elastic Load Balancing (ELB) & Auto Scaling Group (ASG)

Build **highly available**, **fault-tolerant**, and **auto-scaling** architectures on AWS using ELB and ASG.

---

## 📌 Overview

**Elastic Load Balancing (ELB)** and **Auto Scaling Group (ASG)** are foundational AWS services used to handle **traffic spikes**, **system failures**, and **dynamic workloads** without manual intervention.

### 🔹 What They Do
- **ELB**  
  Distributes incoming traffic across multiple healthy targets (EC2, containers, IPs).
- **ASG**  
  Automatically **scales out** (add instances) or **scales in** (remove instances) based on demand and health checks.

> ⚠️ Using ELB without ASG limits scalability.  
> ⚠️ Using ASG without ELB breaks traffic distribution.  
> ✅ Together, they form a production-ready architecture.

---

## 🧩 AWS Services Covered

- **Elastic Load Balancing (ELB)**
  - Classic Load Balancer
  - Application Load Balancer
  - Network Load Balancer
- **Auto Scaling Group (ASG)**

---

## 🌍 Real-World Scenario

### 🛒 Amazon Shopping Website – Prime Day

- Sudden surge in users
- Thousands of concurrent requests
- Manual scaling becomes impossible

**Solution:**
- ELB distributes traffic across multiple servers
- ASG automatically launches or terminates EC2 instances
- Users experience **zero downtime**

---

## ⚖️ Types of Load Balancers in AWS

---

### 1️⃣ Classic Load Balancer (CLB) ❌ *(Legacy)*

**Status:** Deprecated for modern architectures  

**Characteristics:**
- Operates at **Layer 4 & Layer 7** (very limited)
- Uses **basic round-robin traffic distribution**
- No path-based or host-based routing

❌ **Why avoid it?**
- Limited features
- No native microservices support
- AWS recommends ALB or NLB instead

> 📉 Use CLB only when maintaining **old legacy systems**.

---

### 2️⃣ Application Load Balancer (ALB) ✅ *(Default Choice)*

**Layer:** 7 (HTTP / HTTPS)

**Key Features:**
- Path-based routing
- Host-based routing
- Native support for microservices & containers

**Example Routing:**
```text
www.amazon.com/cart      → Cart Service
www.amazon.com/products  → Product Service
````

✔ Best suited for:

* Web applications
* REST APIs
* Microservices architectures

✔ Single ALB can intelligently route traffic to multiple services
✔ No need to create separate load balancers

---

### 3️⃣ Network Load Balancer (NLB) ⚡ *(High Performance)*

**Layer:** 4 (TCP / UDP)

**Strengths:**

* Extremely **low latency**
* Handles **millions of requests per second**
* Supports **static IP addresses**
* Highly resilient and scalable

✔ Ideal for:

* High-performance backends
* Real-time applications
* Gaming, streaming, IoT
* Container platforms (EKS)

> ⚠️ Important:
> NLB is **not better than ALB** — it solves **performance problems**, not routing logic.

---

### 4️⃣ Gateway Load Balancer (GWLB) 🛡️ *(Security Focused)*

**Purpose:** Deploy and scale **third-party network appliances**

**Common Use Cases:**

* Firewalls
* IDS / IPS
* Deep packet inspection
* Traffic monitoring

**Key Benefit:**

* Transparent traffic routing
* Centralized security enforcement

✔ Used mainly in **enterprise and security-heavy architectures**

---

## 🔍 Choosing the Right Load Balancer

| Use Case                        | Recommended Load Balancer   |
| ------------------------------- | --------------------------- |
| Legacy applications             | Classic (avoid if possible) |
| Web apps & REST APIs            | Application Load Balancer   |
| Ultra-low latency TCP/UDP apps  | Network Load Balancer       |
| Firewalls & security appliances | Gateway Load Balancer       |

---

## 🔄 ELB vs ASG — Clear Difference

| Service | Responsibility                     |
| ------- | ---------------------------------- |
| **ELB** | Distributes incoming traffic       |
| **ASG** | Scales EC2 instances automatically |

> 🚨 Alone, each service is incomplete.
> ✅ Together, they deliver **scalability, resilience, and availability**.

---

