---

# <h1 align="center">🌐 <strong>AWS CloudFront Deployment Guide</strong></h1>

### <h3 align="center">🚀 With S3 / EC2 Website + ACM Certificate + Route 53 Domain</h3>

<p align="center">
  <img src="https://img.shields.io/badge/AWS-CloudFront-orange?logo=amazon-aws" />
  <img src="https://img.shields.io/badge/Deployment-Guide-blue" />
  <img src="https://img.shields.io/badge/CDN-Enabled-success" />
  <a href="https://github.com/Thiyagu-2003">
	<img src="https://img.shields.io/badge/Made%20By-Thiyagu%20S-green?logo=github" />
  </a>
</p>

---

# 📑 **Table of Contents**

1. [📌 Prerequisites](#-1-prerequisites)
2. [🪣 Create or Prepare S3 Bucket](#-2-create-or-prepare-s3-bucket-if-using-s3-hosting)
3. [🚀 Create CloudFront Distribution](#-3-create-cloudfront-distribution)
4. [🌍 Route-53 Domain Configuration](#-4-route-53-configuration-connect-domain--cloudfront)
5. [🧪 Test the Deployment](#-5-test-the-website)
6. [⚡ Optional Optimization](#-6-optional-improve-performance)
7. [🖥️ Architecture Diagram](#-7-cloudfront-architecture-diagram-high-level)
8. [🔀 Workflow Overview](#-8-full-workflow-overview)
9. [🎤 CloudFront Interview Questions](#-9-important-cloudfront-interview-questions)
10. [🔚 Conclusion](#-conclusion)
11. [👤 Author](#-author)
12. [❤️ Footer](#️-footer)

---

# 📌 **1. Prerequisites**

Before you start, make sure you have:

✔️ **A registered domain name** (GoDaddy, Hostinger, Namecheap, etc.)
✔️ **An SSL certificate** (AWS Certificate Manager – *must be in us-east-1*)
✔️ **A website build ready**

* Either hosted on **S3**
* Or already running on **EC2**
  ✔️ Basic AWS access (IAM user with admin or required permissions)

---

# 🪣 **2. Create or Prepare S3 Bucket (If using S3 hosting)**

> Skip this if your website is running on **EC2**.

### ✅ Step 1 — Create Bucket

* Open **S3 Console**
* Create a bucket with your domain name
  Example: **thiyagu.cloud**

### ✅ Step 2 — Enable Static Website Hosting

`Properties → Static website hosting → Enable`

### ✅ Step 3 — Add Public Bucket Policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::thiyagu.cloud/*"
    }
  ]
}
```

### ✅ Step 4 — Upload Your Build Files

Upload **exactly** the build output (no extra folders).

---

# 🚀 **3. Create CloudFront Distribution**

### 🔧 Step 1 — Create Distribution

Go to **CloudFront → Create Distribution**

### 🔧 Step 2 — Choose Origin

Choose based on your hosting:

| Website Type | CloudFront Origin                        |
| ------------ | ---------------------------------------- |
| S3 Website   | Select **S3 static website hosting URL** |
| EC2 Website  | Enter **EC2 Public DNS / Load Balancer** |

### 🔧 Step 3 — Configure Settings

* **Viewer protocol**: Redirect HTTP → HTTPS
* **Caching**: Use optimized settings
* **WAF**: Optional
* **Alternate domain names (CNAMEs):**

  * `thiyagu.cloud`
  * `www.thiyagu.cloud`

### 🔧 Step 4 — Add SSL Certificate

Choose the certificate you created earlier:

✔️ `*.thiyagu.cloud`
✔️ `thiyagu.cloud`
✔️ `www.thiyagu.cloud`

### 🔧 Step 5 — Create Distribution

The distribution takes **5–10 minutes** to deploy.

---

# 🌍 **4. Route 53 Configuration (Connect Domain → CloudFront)**

### Step 1 — Create Hosted Zone

* Go to **Route 53 → Hosted Zones → Create**
* Enter domain: `thiyagu.cloud`

### Step 2 — Update DNS at Domain Provider

Copy Route53 **NS records** → Paste into GoDaddy/Hostinger/Namecheap.

### Step 3 — Create Records in Route 53

#### ✅ **Record 1 — Main Domain (www)**

* Name: `www.thiyagu.cloud`
* Type: **A – Alias**
* Alias target: **CloudFront Distribution**

#### ✅ **Record 2 — Root Domain**

* Name: `thiyagu.cloud`
* Type: **A – Alias**
* Alias target: **CloudFront Distribution**

---

# 🧪 **5. Test the Website**

Visit:

🔗 **[https://thiyagu.cloud](https://thiyagu.cloud)**
🔗 **[https://www.thiyagu.cloud](https://www.thiyagu.cloud)**

If propagation is slow, wait 5–30 minutes.

---

# ⚡ **6. Optional – Improve Performance**

✔️ Enable **Compression**
✔️ Enable **Caching Policies**
✔️ Add **WAF protection**
✔️ Enable **HTTP/3**
✔️ Configure **Custom Error Pages**
✔️ Add **Geo Restrictions** (optional)

---

# 🖥️ **7. CloudFront Architecture Diagram (High-Level)**

```
               +-------------------+
               |   End User        |
               | (Browser/Client)  |
               +---------+---------+
                         |
                         v
               +-------------------+
               |   CloudFront      |
               |   Edge Network    |
               +---------+---------+
                         |
         +---------------+----------------+
         |                                |
         v                                v
+------------------+             +----------------------+
|     S3 Bucket    |             |  EC2 / Load Balancer |
| (Static Hosting) |             | (Dynamic Hosting)    |
+------------------+             +----------------------+

                         |
         +---------------+---------------+
         |                               |
         v                               v
+------------------+          +--------------------------+
| AWS Certificate  |          |       Route 53           |
|   Manager (ACM)  |          |   DNS + Domain Mapping   |
+------------------+          +--------------------------+
```

---

# 🔀 **8. Full Workflow Overview**

```
User → CloudFront → Cache? (Yes → Serve Cached Content)
                     |
                    No
                     ↓
Origin (S3 / EC2) → Response → Cached → Delivered securely via HTTPS
```

---

# 🎤 **9. Important CloudFront Interview Questions**

1. **What is CloudFront?**
   A global CDN that accelerates content delivery using edge locations.

2. **What are Edge Locations?**
   Geographically distributed caches (POPs) where CloudFront stores objects.

3. **What is an Origin?**
   The backend origin server (S3, EC2, ALB, API Gateway) that CloudFront fetches from.

4. **S3 Origin vs S3 Website Endpoint — difference?**

   * **S3 Origin:** API-based, supports origin access identity, no website redirects.
   * **S3 Website Endpoint:** supports static website features (index, error pages, redirects).

5. **What is a Distribution?**
   A CloudFront configuration grouping behaviors, origins, and settings.

6. **What is TTL in CloudFront?**
   Time To Live: duration cached objects remain at edge before revalidation.

7. **What is Cache Invalidation?**
   Process to remove objects from the cache before the TTL expires.

8. **Does CloudFront support HTTPS?**
   Yes — attach an ACM certificate (must be in us-east-1 for CloudFront).

9. **What are Signed URLs and Signed Cookies?**
   Methods to grant temporary access to private content via cryptographic signatures.

10. **What is AWS WAF and how does it integrate?**
    AWS Web Application Firewall protects CloudFront distributions from malicious traffic.

11. **What is Origin Shield?**
    A centralized caching layer to reduce the number of origin fetches.

12. **What is Geo-Restriction?**
    Restrict or allow content delivery by viewer country.

13. **Can CloudFront accelerate dynamic content?**
    Yes — it supports dynamic content acceleration and origin failover.

14. **What is Lambda@Edge?**
    Serverless compute executed at CloudFront edge locations for request/response modification.

15. **What is CloudFront Price Class?**
    Controls which edge locations are used (global vs regional) to manage cost vs coverage.

---

# 🔚 **Conclusion**

CloudFront is the fastest, most secure way to deploy global websites.
With just **S3 + CloudFront + ACM + Route 53**, you get:

✔️ Global CDN
✔️ HTTPS security
✔️ Custom domain
✔️ Low latency
✔️ Scalability without any configuration

---

# 👤 **Author**

```

Name: Thiyagu S
Role: Cloud & DevOps Learner
Location: India 🇮🇳
GitHub: Thiyagu-2003

```

---

# ❤️ **Footer**

<p align="center">
  <strong>Made with ❤️ by <a href="https://github.com/Thiyagu-2003">Thiyagu S</a></strong><br>
  Learning • Building • Improving
</p>

---
