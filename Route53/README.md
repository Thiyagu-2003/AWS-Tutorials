\# 🌐 \*\*Domain + Route 53 + EC2 Deployment Guide\*\*



<p align="center">

&nbsp; <img src="https://img.shields.io/badge/AWS-Route%2053-orange?logo=amazon-aws\&logoColor=white" />

&nbsp; <img src="https://img.shields.io/badge/AWS-EC2-blue?logo=amazon-aws\&logoColor=white" />

&nbsp; <img src="https://img.shields.io/badge/DNS-Management-green" />

&nbsp; <img src="https://img.shields.io/badge/Status-Production%20Ready-success" />

</p>



---



<div align="center">



\### ⚡ A complete, polished guide to map a custom domain to an EC2 instance using Route 53.



</div>



---



\# 📑 \*\*Table of Contents\*\*



\* \[🚀 Overview](#-overview)

\* \[🛒 Step-0: Buy a Domain](#-step-0-buy-a-domain)

\* \[🌐 Step-1: Create a Hosted Zone](#-step-1-create-a-hosted-zone)

\* \[📝 Step-2: Update Registrar Name Servers](#-step-2-update-registrar-name-servers)

\* \[🖥️ Step-3: Create EC2 With Web Server](#️-step-3-create-ec2-with-web-server)

\* \[🔗 Step-4: Map Domain to EC2](#-step-4-map-domain-to-ec2)

\* \[🧭 Step-5: Explore Route 53 Features](#-step-5-explore-route-53-features)

\* \[💡 DNS Interview Questions](#-dns-interview-questions)

\* \[📌 Record Types](#-record-types)

\* \[📡 Routing Policies](#-routing-policies)

\* \[❤️ Health Checks](#️-health-checks)

\* \[👤 Author](#-author)



---



\# 🚀 Overview



This guide explains exactly how to:



✔ Connect a purchased domain to AWS

✔ Use Route 53 as your authoritative DNS

✔ Host a website on EC2

✔ Configure A, CNAME, MX, NS, SOA records

✔ Understand routing policies and health checks



No shortcuts, no confusion — just the correct workflow.



---



\# 🛒 \*\*Step-0: Buy a Domain\*\*



Purchase a domain from:



\* GoDaddy

\* Namecheap

\* Hostinger

\* Google Domains (if available)



You only need \*\*DNS management access\*\*.



---



\# 🌐 \*\*Step-1: Create a Hosted Zone\*\*



1\. AWS → \*\*Route 53 → Hosted Zones → Create Hosted Zone\*\*

2\. Enter your domain name

3\. Choose \*\*Public Hosted Zone\*\*



AWS automatically generates:



\* \*\*NS Record\*\*

\* \*\*SOA Record\*\*



These define your domain’s authority.



---



\# 📝 \*\*Step-2: Update Registrar Name Servers\*\*



1\. Open your domain DNS settings

2\. Replace the registrar's NS values with the \*\*Route 53 NS records\*\*

3\. Remove the trailing “.” when pasting



If this step is wrong → your domain will NOT work.

Propagation usually takes \*\*5–30 minutes\*\*.



---



\# 🖥️ \*\*Step-3: Create EC2 With Web Server\*\*



Use a bootstrap script when launching EC2:



```bash

\#!/bin/bash

yum install httpd -y

systemctl start httpd

systemctl enable httpd

echo "Welcome to my website" > /var/www/html/index.html

```



Make sure:



\* Port \*\*80\*\* is open

\* Use \*\*Elastic IP\*\* (static)



---



\# 🔗 \*\*Step-4: Map Domain to EC2\*\*



Create an \*\*A Record\*\*:



| Setting | Value                        |

| ------- | ---------------------------- |

| Name    | `yourdomain.com`             |

| Type    | \*\*A\*\*                        |

| Value   | EC2 Public IPv4 / Elastic IP |

| TTL     | 300                          |

| Routing | Simple Routing               |



Once propagated → your website goes live.



---



\# 🧭 \*\*Step-5: Explore Route 53 Features\*\*



Route 53 offers much more than basic DNS:



\* Alias records

\* Failover routing

\* Latency-based routing

\* DNS-level load balancing

\* Traffic flow

\* Health checks

\* Record visual editor



Experiment with them — they’re core interview topics.



---



\# 💡 \*\*DNS Interview Questions\*\*



\## \*\*1. What is a Hosted Zone?\*\*



A DNS database inside Route 53 where all domain records are stored.

Types:



\* \*\*Public Hosted Zone\*\*

\* \*\*Private Hosted Zone\*\*



---



\# 📌 \*\*Record Types\*\*



\### 🟦 A Record



Maps domain → IPv4



\### 🟪 AAAA Record



Maps domain → IPv6



\### 🟧 CNAME Record



Domain alias → Another domain

Used for:



\* ALB

\* CloudFront

\* S3 static sites



\### 🟩 MX Record



Mail server routing



\### 🟨 NS Record



Authoritative name servers for DNS



\### 🟥 SOA Record



Domain metadata: serial, refresh, TTL



---



\# 📡 \*\*Routing Policies\*\*



| Policy                | Use Case                       |

| --------------------- | ------------------------------ |

| \*\*Simple\*\*            | Default / single server        |

| \*\*Weighted\*\*          | Split traffic %                |

| \*\*Latency-Based\*\*     | Lowest latency region          |

| \*\*Failover\*\*          | Primary → secondary            |

| \*\*Geolocation\*\*       | Country/continent-based        |

| \*\*Geoproximity\*\*      | Location + bias-based routing  |

| \*\*Multivalue Answer\*\* | Round-robin with health checks |



---



\# ❤️ \*\*Health Checks\*\*



Monitors:



\* Web servers

\* APIs

\* Load balancers



If the primary endpoint fails → Route 53 automatically sends traffic to backup.



---



\# 👤 \*\*Author\*\*



```

Name: Thiyagu S  

Role: Cloud \& DevOps Learner  

Focus: AWS, EC2, VPC, Route 53, Terraform, DevOps Tools  

Country: India 🇮🇳  

```



<p align="center">

&nbsp; <strong>Made with ❤️ by Thiyagu S</strong><br>

&nbsp; <sub>Learning. Building. Improving.</sub>

</p>



---



