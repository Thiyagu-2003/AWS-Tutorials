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

# 📑 Table of contents

1. [📌 Prerequisites](#sec-1-prerequisites)  
2. [🪣 Create or prepare S3 bucket](#sec-2-create-or-prepare-s3-bucket-if-using-s3-hosting)  
3. [🚀 Create CloudFront distribution](#sec-3-create-cloudfront-distribution)  
4. [🌍 Route-53 configuration](#sec-4-route-53-configuration-connect-domain--cloudfront)  
5. [🧪 Test the deployment](#sec-5-test-the-website)  
6. [⚡ Optional optimization](#sec-6-optional-improve-performance)  
7. [🖥️ Architecture diagram](#sec-7-cloudfront-architecture-diagram-high-level)  
8. [🔀 Workflow overview](#sec-8-full-workflow-overview)  
9. [🎤 CloudFront interview questions](#sec-9-important-cloudfront-interview-questions)  
10. [🔚 Conclusion](#sec-conclusion)  
11. [👤 Author](#sec-author)  
12. [❤️ Footer](#sec-footer)

---

<a id="sec-1-prerequisites"></a>
# 📌 1. Prerequisites

Before you start, make sure you have:

- **Domain:** A registered domain name (GoDaddy, Hostinger, Namecheap, etc.)
- **Certificate:** An SSL certificate in AWS Certificate Manager (must be in us-east-1)
- **Website build:** Either hosted on S3 or already running on EC2
- **Access:** IAM user with admin or required permissions

---

<a id="sec-2-create-or-prepare-s3-bucket-if-using-s3-hosting"></a>
# 🪣 2. Create or prepare S3 bucket (if using S3 hosting)

> Skip this if your website is running on EC2.

### ✅ Step 1 — Create bucket
- Open S3 Console
- Create a bucket with your domain name (example: thiyagu.cloud)

### ✅ Step 2 — Enable static website hosting
Properties → Static website hosting → Enable

### ✅ Step 3 — Add public bucket policy

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

### ✅ Step 4 — Upload your build files
Upload exactly the build output (no extra folders).

---

<a id="sec-3-create-cloudfront-distribution"></a>
# 🚀 3. Create CloudFront distribution

### 🔧 Step 1 — Create distribution
CloudFront → Create Distribution

### 🔧 Step 2 — Choose origin

| Website Type | CloudFront Origin                        |
| ------------ | ---------------------------------------- |
| S3 Website   | Select S3 static website hosting URL     |
| EC2 Website  | Enter EC2 Public DNS / Load Balancer     |

### 🔧 Step 3 — Configure settings
- **Viewer protocol:** Redirect HTTP → HTTPS  
- **Caching:** Use optimized settings  
- **WAF:** Optional  
- **Alternate domain names (CNAMEs):**
  - thiyagu.cloud
  - www.thiyagu.cloud

### 🔧 Step 4 — Add SSL certificate
Select the certificate created in ACM:
- *.thiyagu.cloud
- thiyagu.cloud
- www.thiyagu.cloud

### 🔧 Step 5 — Create distribution
Distribution takes 5–10 minutes to deploy.

---

<a id="sec-4-route-53-configuration-connect-domain--cloudfront"></a>
# 🌍 4. Route-53 configuration (connect domain → CloudFront)

### Step 1 — Create hosted zone
- Route 53 → Hosted Zones → Create
- Domain: thiyagu.cloud

### Step 2 — Update DNS at domain provider
Copy Route53 NS records → Paste into GoDaddy/Hostinger/Namecheap.

### Step 3 — Create records in Route 53

#### ✅ Record 1 — Main domain (www)
- Name: www.thiyagu.cloud
- Type: A – Alias
- Alias target: CloudFront Distribution

#### ✅ Record 2 — Root domain
- Name: thiyagu.cloud
- Type: A – Alias
- Alias target: CloudFront Distribution

---

<a id="sec-5-test-the-website"></a>
# 🧪 5. Test the website

Visit:
- https://thiyagu.cloud
- https://www.thiyagu.cloud

If propagation is slow, wait 5–30 minutes.

---

<a id="sec-6-optional-improve-performance"></a>
# ⚡ 6. Optional – Improve performance

- **Compression:** Enable GZIP/Brotli
- **Caching policies:** Set TTLs; use Cache Policy + Origin Request Policy
- **WAF:** Attach AWS WAF for DDoS and common exploit protection
- **HTTP/3:** Enable for improved latency on modern clients
- **Custom error pages:** User-friendly 404/500 responses
- **Geo restrictions:** Allow/deny countries if needed
- **Origin Shield:** Reduce origin fetches and improve cache hit ratio
- **Price Class:** Use only needed edge regions to control cost
- **Signed URLs/Cookies:** Protect private content
- **Invalidations:** Use targeted paths to refresh cache on deploys

---

<a id="sec-7-cloudfront-architecture-diagram-high-level"></a>
# 🖥️ 7. CloudFront architecture diagram (high-level)

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

<a id="sec-8-full-workflow-overview"></a>
# 🔀 8. Full workflow overview

```
User → CloudFront → Cache? (Yes → Serve Cached Content)
                     |
                    No
                     ↓
Origin (S3 / EC2) → Response → Cached → Delivered securely via HTTPS
```

---

<a id="sec-9-important-cloudfront-interview-questions"></a>
# 🎤 9. Important CloudFront interview questions

1. **CloudFront:** Global CDN that accelerates content delivery using edge locations.
2. **Edge locations:** Geographically distributed caches (POPs) where CloudFront stores objects.
3. **Origin:** Backend (S3, EC2, ALB, API Gateway) CloudFront fetches from.
4. **S3 origin vs S3 website endpoint:**
   - **S3 Origin:** API-based, supports origin access identity, no website redirects.
   - **S3 Website Endpoint:** static website features (index, error pages, redirects).
5. **Distribution:** CloudFront configuration grouping behaviors, origins, and settings.
6. **TTL:** Time objects remain cached before revalidation.
7. **Cache invalidation:** Remove objects from cache before TTL expires.
8. **HTTPS:** Yes—attach an ACM certificate (must be in us-east-1 for CloudFront).
9. **Signed URLs/Cookies:** Temporary access to private content via signatures.
10. **AWS WAF:** Protects CloudFront distributions from malicious traffic.
11. **Origin Shield:** Centralized caching layer to reduce origin fetches.
12. **Geo-Restriction:** Restrict/allow content by viewer country.
13. **Dynamic content acceleration:** Supported with origin failover.
14. **Lambda@Edge:** Serverless compute at edge for request/response modification.
15. **Price Class:** Control edge coverage to balance cost vs performance.

---

<a id="sec-conclusion"></a>
# 🔚 Conclusion

CloudFront is a fast, secure way to deploy global websites. With S3 + CloudFront + ACM + Route 53, you get global CDN, HTTPS, custom domain, low latency, and scalable delivery with minimal configuration.

---

<a id="sec-author"></a>
# 👤 Author

```
Name: Thiyagu S
Role: Cloud & DevOps Learner
Location: India 🇮🇳
GitHub: Thiyagu-2003
```

---

<a id="sec-footer"></a>
# ❤️ Footer

<p align="center">
  <strong>Made with ❤️ by <a href="https://github.com/Thiyagu-2003">Thiyagu S</a></strong><br>
  Learning • Building • Improving
</p>

---
