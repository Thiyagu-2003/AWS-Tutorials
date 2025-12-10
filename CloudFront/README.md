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

# ✅ **1. Prerequisites**

Before starting, make sure you already have:

### ✔️ **A Domain Name**

GoDaddy, Hostinger, Namecheap, AWS Domains, etc.

### ✔️ **A Website Hosted on S3 OR EC2**

Your site can be:

* **S3** → Static Hosting (React/HTML/CSS/JS build folder)
* **EC2** → Running Nginx, Apache, Node.js, React App, Django, Flask, etc.

### ✔️ **SSL Certificate (ACM)** → Must be in **us-east-1**

Add:

```
example.com
www.example.com
```

Validate via Route 53 → “Create Record”.

### ✔️ **Route 53 Hosted Zone**

Your domain **must** point to Route 53 nameservers.

---

# 🔥 **2. Create or Prepare S3 Bucket (If using S3 Hosting)**

### 📌 Steps

1. Open **S3 Console → Create Bucket**
2. Bucket name: `example.cloud`
3. Disable **Block All Public Access**
4. Enable **Static Website Hosting**
5. Add this **Bucket Policy**:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::example.cloud/*"
    }
  ]
}
```

6. Upload the **build folder** (exactly as generated — no edits)

---

# 🚀 **3. Create CloudFront Distribution**

Go to **CloudFront → Create Distribution**

---

## ⭐ **STEP 1 — Origin Settings**

### 🔹 If Using S3

Choose → **S3 Website Endpoint** (NOT default S3 bucket option)

Example:

```
http://example.cloud.s3-website-us-east-1.amazonaws.com
```

### 🔹 If Using EC2

Origin type → **Custom Origin**
Use EC2:

* **Public IPv4**
* OR **Public DNS**

---

## ⭐ **STEP 2 — Default Behavior Settings**

* Viewer Protocol Policy → **Redirect HTTP → HTTPS**
* Allowed HTTP Methods → **GET, HEAD**
* Cache Policy → **CachingOptimized**

---

## ⭐ **STEP 3 — Add Custom Domain & SSL**

Add:

```
example.cloud
www.example.cloud
```

Then attach the **ACM certificate** (from us-east-1 region).

---

## ⭐ **STEP 4 — Create Distribution**

Click **Create Distribution**
Wait **5–15 minutes**.

CloudFront domain example:

```
d123exampleabcd.cloudfront.net
```

---

# 🌐 **4. Route 53 Configuration (Connect Domain → CloudFront)**

Go to: **Route 53 → Hosted Zone → example.cloud**

---

### 🔹 **Record 1 — Root Domain**

| Field  | Value                   |
| ------ | ----------------------- |
| Type   | A                       |
| Name   | example.cloud           |
| Alias  | YES                     |
| Target | CloudFront Distribution |

---

### 🔹 **Record 2 — WWW Domain**

| Field  | Value                                         |
| ------ | --------------------------------------------- |
| Type   | A                                             |
| Name   | [www.example.cloud](http://www.example.cloud) |
| Alias  | YES                                           |
| Target | CloudFront Distribution                       |

---

# 🎉 **5. Test the Website**

Open:

* [https://example.cloud](https://example.cloud)
* [https://www.example.cloud](https://www.example.cloud)

Both should:

✔ Load via HTTPS
✔ Use CloudFront CDN
✔ Show global caching

---

# 🧊 **6. Optional: Improve Performance**

| Feature                 | Recommendation                             |
| ----------------------- | ------------------------------------------ |
| IPv6                    | Enable                                     |
| AWS WAF                 | Protect from bots/DDOS                     |
| Cache Invalidation      | Invalidate `/index.html` after deployments |
| Logging                 | Enable logging → S3 bucket                 |
| HTTP/3                  | Improve latency                            |
| Geographic Restrictions | Block unwanted countries                   |

---

# 🖥️ **7. CloudFront Architecture Diagram (High-Level)**

```
               +------------------------+
               |     End User Browser   |
               +-----------+------------+
                           |
                           v
                +------------------------+
                |      Amazon CloudFront |
                |   (Global CDN + SSL)  |
                +-----------+------------+
                           |
          +----------------+----------------+
          |                                 |
          v                                 v
+-------------------+           +------------------------+
|  S3 Static Site   |           |     EC2 Web Server     |
| (HTML / CSS / JS) |           |   (Custom Application) |
+-------------------+           +------------------------+
```

---

# 🧩 **8. Full Workflow Overview**

```
User → CloudFront → SSL (ACM) → Route 53 → S3 or EC2 → Response
```

---

# 🎤 **9. Important CloudFront Interview Questions**

### **1. What is CloudFront?**

A global CDN that accelerates content delivery using edge locations.

### **2. What are Edge Locations?**

Geographical endpoints where CloudFront caches content.

### **3. What is an Origin?**

The backend server (S3, EC2, ALB, API Gateway) serving the content.

### **4. Difference between S3 Origin vs S3 Website Endpoint?**

* S3 Origin → API-based, no redirects
* S3 Website Endpoint → supports 301/302 redirects & static hosting

### **5. What is a Distribution?**

CloudFront configuration + settings for caching and traffic routing.

### **6. What is TTL in CloudFront?**

Time To Live: how long content stays cached at edge locations.

### **7. What is Cache Invalidation?**

Manual removal of cached objects before TTL expiry.

### **8. Does CloudFront support HTTPS?**

Yes — using ACM certificates (must be in us-east-1).

### **9. What is Signed URL?**

A URL that grants temporary access to private content.

### **10. What is WAF in CloudFront?**

A security firewall that protects against attacks (SQLi, bots, DDOS).

### **11. What is Origin Shield?**

An extra caching layer reducing load on the origin.

### **12. What is Geo-Restriction?**

Blocking content based on the viewer's country.

### **13. Can CloudFront serve dynamic content?**

Yes — CloudFront can accelerate both static and dynamic content.

### **14. What is Lambda@Edge?**

Serverless functions executed at CloudFront edge locations.

### **15. What is CloudFront Price Class?**

Controls which edge locations CloudFront uses to reduce cost.

---

# 🔚 **Conclusion**

Your CloudFront deployment is now fully production-ready:

✔ HTTPS + SSL enabled
✔ Domain integrated via Route 53
✔ CDN caching across global edge locations
✔ Supports both **S3** and **EC2 origins**
✔ Automatic certificate renewal
✔ High performance + security options

---

# 👤 **Author**

**Name:** Thiyagu S
**Role:** Cloud & DevOps Learner
**Country:** India 🇮🇳
**GitHub:** [Thiyagu-2003](https://github.com/Thiyagu-2003)

---

# ❤️ **Footer**

<p align="center">
  <strong>Made with ❤️ by <a href="https://github.com/Thiyagu-2003">Thiyagu S</a></strong><br>
  <em>Learning • Building • Improving</em>
</p>

---
