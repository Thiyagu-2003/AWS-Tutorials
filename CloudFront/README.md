---

# 🌐 <strong>AWS CloudFront Deployment Guide</strong>
### 🚀 With S3 / EC2 Website + ACM Certificate + Route 53 Domain

<p align="center">
  <img src="https://img.shields.io/badge/AWS-CloudFront-orange?logo=amazon-aws" />
  <img src="https://img.shields.io/badge/Deployment-Guide-blue" />
  <img src="https://img.shields.io/badge/CDN-Enabled-success" />
  <a href="https://github.com/Thiyagu-2003">
    <img src="https://img.shields.io/badge/Made%20By-Thiyagu%20S-green?logo=github" />
  </a>
</p>

---

# 📑 Table of Contents

1. [Prerequisites](#prerequisites)  
2. [Create or Prepare S3 Bucket](#create-or-prepare-s3-bucket-if-using-s3-hosting)  
3. [Create CloudFront Distribution](#create-cloudfront-distribution)  
4. [Route 53 Configuration](#route-53-configuration-connect-domain--cloudfront)  
5. [Test the Website](#test-the-website)  
6. [Optional: Improve Performance](#optional-improve-performance)  
7. [Architecture Diagram (High-Level)](#architecture-diagram-high-level)  
8. [Full Workflow Overview](#full-workflow-overview)  
9. [Important CloudFront Interview Questions](#important-cloudfront-interview-questions)  
10. [Conclusion](#conclusion)  
11. [Author](#author)  
12. [Footer](#footer)

---

<a id="prerequisites"></a>
# ✅ Prerequisites

Before starting, make sure you already have:

- **A Domain Name** (GoDaddy, Hostinger, Namecheap, AWS Domains, etc.)  
- **A Website Hosted on S3 or EC2**  
  - S3 → Static Hosting (React/HTML/CSS/JS build folder)  
  - EC2 → Running Nginx/Apache/Node/Python etc.  
- **SSL Certificate (ACM)** — **must be created in `us-east-1`** (N. Virginia) for CloudFront  
  - Add both: `example.com` and `www.example.com`  
  - Validate via Route 53 (Create Record)  
- **Route 53 Hosted Zone** — domain must point to Route 53 nameservers

---

<a id="create-or-prepare-s3-bucket-if-using-s3-hosting"></a>
# 🔥 2. Create or Prepare S3 Bucket (If using S3 Hosting)

### Steps
1. Open **S3 Console → Create Bucket**  
2. Bucket name: `example.cloud` (replace with your domain)  
3. Turn **OFF** *Block All Public Access*  
4. Enable **Static Website Hosting** (Index: `index.html`, Error: `index.html` for SPA)  
5. Add this **Bucket Policy** (replace bucket name):

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
````

6. Upload the **build** folder contents exactly as generated (no renames)

---

<a id="create-cloudfront-distribution"></a>

# 🚀 3. Create CloudFront Distribution

Go to **CloudFront → Create Distribution**

## STEP 1 — Origin Settings

* **If S3**: Use **S3 Website Endpoint** (not the default bucket origin).

  * Example: `http://example.cloud.s3-website-us-east-1.amazonaws.com`
* **If EC2**: Use **Custom Origin** — EC2 Public IPv4 / Public DNS

## STEP 2 — Default Behavior Settings

* Viewer Protocol Policy → **Redirect HTTP to HTTPS**
* Allowed HTTP Methods → **GET, HEAD**
* Cache Policy → **CachingOptimized**

## STEP 3 — Add Custom Domain & SSL

* Alternate Domain Names (CNAMEs):

  * `example.cloud`
  * `www.example.cloud`
* Attach the ACM certificate (created in **us-east-1**)

## STEP 4 — Create Distribution

* Click **Create Distribution**
* Wait **5–15 minutes** for deployment
* CloudFront domain example: `d123exampleabcd.cloudfront.net`

---

<a id="route-53-configuration-connect-domain--cloudfront"></a>

# 🌍 4. Route 53 Configuration (Connect Domain → CloudFront)

Go to **Route 53 → Hosted Zone → example.cloud**

### Record 1 — Root Domain

* **Type:** A
* **Name:** example.cloud
* **Alias:** YES
* **Target:** CloudFront Distribution

### Record 2 — WWW Domain

* **Type:** A
* **Name:** [www.example.cloud](http://www.example.cloud)
* **Alias:** YES
* **Target:** CloudFront Distribution

---

<a id="test-the-website"></a>

# 🧪 5. Test the Website

Open in browser:

* `https://example.cloud`
* `https://www.example.cloud`

Both should:

* Load via **HTTPS**
* Be served by **CloudFront**
* Reflect caching behavior

---

<a id="optional-improve-performance"></a>

# ⚡ 6. Optional: Improve Performance

| Feature            | Recommendation                                |
| ------------------ | --------------------------------------------- |
| IPv6               | Enable for global reach                       |
| AWS WAF            | Add protection from bots & OWASP threats      |
| Cache Invalidation | Invalidate `/index.html` or paths post-deploy |
| Logging            | Enable CloudFront logs to S3 for analytics    |
| HTTP/3             | Enable for lower latencies where available    |
| Geo-Restriction    | Use to block/allow countries if needed        |
| Origin Shield      | Use to reduce origin load on high traffic     |

---

<a id="architecture-diagram-high-level"></a>

# 🖥️ 7. Architecture Diagram (High-Level)

```
               +------------------------+
               |     End User Browser   |
               +-----------+------------+
                           |
                           v
                +------------------------+
                |      Amazon CloudFront |
                |   (Global CDN + SSL)   |
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

> **Note:** This diagram is intentionally simple — CloudFront sits between users and the origin (S3 or EC2), delivering cached content from edge locations and enforcing HTTPS via ACM.

---

<a id="full-workflow-overview"></a>

# 🔀 8. Full Workflow Overview

```
User → CloudFront (Edge) → SSL (ACM) → Route 53 (DNS) → S3 or EC2 (Origin) → Response
```

---

<a id="important-cloudfront-interview-questions"></a>

# 🎤 9. Important CloudFront Interview Questions (Top 15)

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

<a id="conclusion"></a>

# 🔚 Conclusion

You now have a production-ready CloudFront setup:

* HTTPS & SSL via ACM
* Domain routing with Route 53
* CDN caching across global edge locations
* Supports S3 static hosting and EC2-based origins
* Options for security (WAF), performance (HTTP/3, Origin Shield), and logging

---

<a id="author"></a>

# 👤 Author

```
Name: Thiyagu S
Role: Cloud & DevOps Learner
Location: India 🇮🇳
GitHub: https://github.com/Thiyagu-2003
```

---

<a id="footer"></a>

# ❤️ Footer

<p align="center">
  <strong>Made with ❤️ by <a href="https://github.com/Thiyagu-2003">Thiyagu S</a></strong><br>
  <em>Learning • Building • Improving</em>
</p>

---
