---

# 🔐 **AWS IAM – Complete Tutorial, Use Cases, Best Practices & Interview Questions**

<p align="center">
  <img src="https://img.shields.io/badge/AWS-IAM-orange?logo=amazon-aws&logoColor=white" />
  <img src="https://img.shields.io/badge/Security-Identity%20%26%20Access-blue" />
  <img src="https://img.shields.io/badge/Best%20Practices-Must%20Know-success" />
  <img src="https://img.shields.io/badge/Least%20Privilege-Yes-green" />
</p>

---

# 📑 **Table of Contents**

* [🔍 Overview](#-overview)
* [🧠 Real-World IAM Use Cases](#-real-world-iam-use-cases)
* [🏗️ 1. IAM Step-by-Step Tutorial](#️-1-iam-step-by-step-tutorial)

  * [1️⃣ Create IAM Users](#1️⃣-create-iam-users)
  * [2️⃣ Groups & Policies](#2️⃣-groups--policies)
  * [3️⃣ IAM Roles](#3️⃣-iam-roles)
  * [4️⃣ Security Best Practices](#4️⃣-security-best-practices)
* [🛡️ IAM Core Concepts](#️-iam-core-concepts)
* [🧩 Types of IAM Roles](#-types-of-iam-roles)
* [🧩 Types of IAM Policies](#-types-of-iam-policies)
* [⚠️ Common IAM Mistakes](#️-common-iam-mistakes)
* [🎯 Real-World IAM Patterns](#-real-world-iam-patterns)
* [💡 Important IAM Interview Questions](#-important-iam-interview-questions)
* [👤 Author](#-author)

---

# 🔍 **Overview**

IAM (**Identity & Access Management**) is the **security brain** of AWS.

It controls:

✔ Authentication
✔ Authorization
✔ Access boundaries
✔ Permissions
✔ Identity governance

Every AWS service depends on IAM for secure access.

---

# 🧠 **Real-World IAM Use Cases**

✔ Assign permissions to developers/admins
✔ Secure AWS services using **IAM roles**
✔ Limit access to specific S3 buckets, EC2 instances, RDS DBs
✔ Enforce MFA for high-security accounts
✔ Implement least-privilege across the organization
✔ Manage cross-account roles
✔ Centralize identity using SSO or Okta

---

# 🏗️ **1. IAM Step-by-Step Tutorial**

## 1️⃣ **Create IAM Users**

* Create users only for **real humans**
* **NEVER attach permissions directly to a user**
* Group users → assign policies to groups
* Disable console access unless needed
* Use programmatic access sparingly (keys become a risk)

**IAM Users = long-term human identities.**

---

## 2️⃣ **Groups & Policies**

Recommended groups:

| Group Name     | Purpose                                            |
| -------------- | -------------------------------------------------- |
| **Admin**      | Full access (restricted to trusted ops/admins)     |
| **DevOps**     | EC2, S3, Lambda, CloudWatch, IAM Read              |
| **Developers** | DynamoDB, S3 read, Lambda access (least privilege) |
| **ReadOnly**   | View-only access across account                    |

Policy options:

### ✔ **AWS-Managed Policies**

Safe, general-purpose defaults.

### ✔ **Customer-Managed Policies**

Your fine-grained least-privilege policies (best for production).

### ✔ **Inline Policies**

Attached to exactly ONE user/role/group.
Use only when absolutely required.

---

## 3️⃣ **IAM Roles**

Roles = **temporary credentials** for AWS services or external identities.

Used by:

✔ EC2 (example: access S3)
✔ Lambda (write logs to CloudWatch)
✔ ECS Tasks (read parameters from SSM)
✔ Cross-account access
✔ SSO / Federation (Google, AD, Okta)

Example:

```
EC2InstanceRole → AmazonS3ReadOnlyAccess
```

Roles help avoid hardcoded keys — essential for secure automation.

---

## 4️⃣ **Security Best Practices**

✔ Enable **MFA** for every human
✔ NEVER use the **Root User** (except billing)
✔ Rotate access keys or avoid them entirely
✔ Follow **strict least-privilege**
✔ Avoid `"*"` in both `Action` and `Resource`
✔ Prefer **roles** over static credentials
✔ Use **IAM Access Analyzer** for risk detection
✔ Enforce password policies
✔ Log all IAM activity via CloudTrail

---

# 🛡️ **IAM Core Concepts**

| Concept         | Meaning                                          |
| --------------- | ------------------------------------------------ |
| **User**        | Human identity                                   |
| **Group**       | Collection of users with shared permissions      |
| **Role**        | Temporary credentials for AWS services           |
| **Policy**      | JSON document defining allow/deny rules          |
| **MFA**         | Extra authentication factor                      |
| **Access Keys** | Programmatic credentials (avoid long-term usage) |

---

# 🧩 **Types of IAM Roles**

### ✅ **1. Service Roles**

Used by AWS services like EC2, Lambda, ECS.
Permissions defined by you.

---

### ✅ **2. Service-Linked Roles**

Created automatically by AWS (e.g., RDS, Organizations).
Permissions cannot be altered much.

---

### ✅ **3. Cross-Account Roles**

Used for accessing resources in another AWS account.
Example: Account A admin assumes a role in Account B.

---

### ✅ **4. Identity Federation Roles**

Used with SSO providers:

✔ Google Workspace
✔ Active Directory
✔ Okta
✔ AWS SSO

No IAM users needed.

---

# 🧩 **Types of IAM Policies**

### ✅ **1. AWS-Managed Policies**

Created and maintained by AWS.
Examples:

* `AmazonS3FullAccess`
* `AmazonEC2ReadOnlyAccess`

---

### ✅ **2. Customer-Managed Policies**

Your custom JSON policies.
Best for **least-privilege** production setups.

---

### ✅ **3. Inline Policies**

Attached to exactly one identity only.
Avoid unless the requirement is identity-specific.

---

### ✅ **4. Permissions Boundaries**

Caps the **maximum permissions** a user or role can get.

Even if a policy tries to grant more → boundary blocks it.

---

### ✅ **5. Organizations SCP (Service Control Policies)**

Top-level guardrails at **organization or OU level**.

Example: block S3 public access across all accounts.

---

# ⚠️ **Common IAM Mistakes**

🚫 Using Root user for daily work
🚫 Giving AdminAccess to everybody
🚫 Hardcoding Access Keys inside apps
🚫 Leaving unused keys active
🚫 Using `"Action": "*"`, `"Resource": "*"`
🚫 No MFA enabled
🚫 Creating IAM users for EC2/Lambda instead of roles

Most breaches come from **bad IAM hygiene**.

---

# 🎯 **Real-World IAM Patterns**

### ⭐ **EC2 Role → S3 Access**

Best way to give EC2 permission without keys.

### ⭐ **Group-Based Developer Access**

Developers → S3 read + DynamoDB access.

### ⭐ **Cross-Account AssumeRole**

DevOps team in Account A manages Account B.

### ⭐ **AWS SSO / Identity Federation**

Replace IAM users with centralized identity.

### ⭐ **Least-Privilege Custom Policies**

Each team gets only what their tasks require.

---

# 💡 **Important IAM Interview Questions**

1. Difference between **User, Group, Role**?
2. What is **Least Privilege**?
3. Why avoid root account?
4. What are **IAM Managed vs Inline Policies**?
5. How does **STS AssumeRole** work?
6. How to give EC2 secure access to S3?
7. What is MFA and why is it critical?
8. What are service-linked roles?
9. What are permission boundaries?
10. How do SCPs override IAM policies?

---

# 👤 **Author**

```
Name: Thiyagu S
Role: Cloud & DevOps Learner
Focus: AWS, IAM, EC2, S3, VPC, DevOps Tools
Country: India 🇮🇳
```

<p align="center">
  <strong>Made with ❤️ by Thiyagu S</strong><br>
  <sub>Learning. Building. Sharpening Skills.</sub>
</p>

---