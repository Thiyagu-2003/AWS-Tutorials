---

# ☁👀 **Amazon CloudWatch & CloudWatch Agent – Complete Guide**

<p align="center">
  <img src="https://img.shields.io/badge/AWS-CloudWatch-orange?logo=amazon-aws&logoColor=white" />
  <img src="https://img.shields.io/badge/Monitoring-System%20Metrics-blue" />
  <img src="https://img.shields.io/badge/Logs-CloudWatch%20Logs-green" />
  <a href="https://github.com/Thiyagu-2003">
    <img src="https://img.shields.io/badge/Made%20By-Thiyagu%20S-brightgreen?logo=github" />
  </a>
</p>

---

# 📑 **Table of Contents**

* 🔎 [Overview](#1-overview)
* 📦 [Prerequisites](#2-prerequisites)
* ⚙️ [Install CloudWatch Agent](#3-install-cloudwatch-agent)
* 📝 [Create Config File](#4-create-the-configuration-file)
* 🚀 [Apply & Start Agent](#5-apply-config--start-the-agent)
* 📊 [Verify in CloudWatch](#6-verify-in-cloudwatch-console)
* 🛠 [Troubleshooting](#7-troubleshooting)
* 📚 [Important CloudWatch Interview Questions](#9-important-cloudwatch-interview-questions)
* 🧾 [Quick Reference](#8-useful-paths--commands-quick-reference)
* 👤 [Author](#author)
* ❤️ [Footer](#footer)

---

# 🔎 **1. Overview**

CloudWatch Agent collects **CPU, RAM, disk, network metrics, and log files** from EC2 → sends them to:

✔ CloudWatch Metrics
✔ CloudWatch Logs
✔ AWS Logs Insights

Use this when you want:

* Real-time EC2 monitoring
* Custom metrics
* Log ingestion
* Alarm notifications
* Dashboard visualization

---

# 📦 **2. Prerequisites**

Attach these policies to the EC2 IAM Role:

### ✔ Required IAM Policies

* `CloudWatchAgentServerPolicy`
* `AmazonSSMManagedInstanceCore` (if using SSM Session Manager)

### ✔ Required System Access

* EC2 must have Internet access
* Linux instance supported by the CloudWatch Agent

---

# ⚙️ **3. Install CloudWatch Agent**

### 🟠 Amazon Linux 2 / RHEL / CentOS

```bash
sudo yum install -y amazon-cloudwatch-agent
```

### 🔵 Ubuntu / Debian

```bash
wget https://s3.amazonaws.com/amazoncloudwatch-agent/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb
sudo dpkg -i amazon-cloudwatch-agent.deb
```

Location:

```
/opt/aws/amazon-cloudwatch-agent/bin/
```

---

# 📝 **4. Create the Configuration File**

### 🅰️ Option A — Wizard

```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
```

Generated file:

```
/opt/aws/amazon-cloudwatch-agent/bin/config.json
```

---

### 🅱️ Option B — Custom JSON

Place your config at:

```
/opt/aws/amazon-cloudwatch-agent/bin/config.json
```

---

# 🚀 **5. Apply Config & Start the Agent**

### Start & apply config:

```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
  -a fetch-config -m ec2 \
  -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json \
  -s
```

### Validate configuration:

```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
  -a validate \
  -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json
```

### Systemd controls:

```bash
sudo systemctl start amazon-cloudwatch-agent
sudo systemctl restart amazon-cloudwatch-agent
sudo systemctl status amazon-cloudwatch-agent
```

---

# 📊 **6. Verify in CloudWatch Console**

### 📈 Metrics

```
CloudWatch → Metrics → CWAgent
```

### 📜 Logs

```
CloudWatch → Logs → Log groups
```

Ensure log group names match your config.

---

# 🛠 **7. Troubleshooting**

---

## ❗ A. ec2tagger / Missing Tags Error

Error:

```
UnauthorizedOperation: ec2:DescribeTags
```

Fix: Add IAM permissions:

* `ec2:DescribeTags`
* `ec2:DescribeInstances`
* `ec2:DescribeVolumes`

Restart agent:

```bash
sudo systemctl restart amazon-cloudwatch-agent
```

---

## ❗ B. Logs Not Coming to CloudWatch

Check:

1. IAM role has `CloudWatchAgentServerPolicy`
2. Log file path exists
3. Agent logs:

```bash
sudo tail -f /opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log
```

Look for:
➡️ `piping log to cloudwatchlogs`

---

## ❗ C. JSON Config Errors

Validate:

```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
  -a validate \
  -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json
```

---

# 📚 **9. Important CloudWatch Interview Questions**

Here are **15 high-value CloudWatch interview questions**:

1. ⭐ **What is the difference between CloudWatch and CloudTrail?**
2. 🔥 **What are CloudWatch Logs, Log Streams, and Log Groups?**
3. 🛰 **How does the CloudWatch Agent differ from the Default EC2 Metrics?**
4. 🧰 **What metrics are collected by default without installing the agent?**
5. 🚨 **What is a CloudWatch Alarm and what states does it have?**
6. 📊 **How does CloudWatch metric retention work?**
7. ⚡ **What are High-Resolution Metrics (1-second metrics)?**
8. 🔐 **How do you securely send logs to CloudWatch?**
9. 📡 **What is Metric Filter in CloudWatch Logs?**
10. 🎯 **What are the use cases of CloudWatch Dashboards?**
11. 🎛 **What is CloudWatch Contributor Insights?**
12. 🧹 **How do you reduce CloudWatch Logs cost?**
13. 🔄 **How do CloudWatch Alarms integrate with SNS?**
14. 💾 **What is CloudWatch Logs Insights used for?**
15. 🧩 **Why does the CloudWatch Agent need EC2 DescribeTags permission?**

---

# 🧾 **8. Useful Paths & Commands (Quick Reference)**

### 📄 Logs

```
/opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log
```

### 📄 Config File

```
/opt/aws/amazon-cloudwatch-agent/bin/config.json
```

### 📄 Runtime Config

```
/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.toml
```

### 🔧 Commands

```bash
sudo systemctl restart amazon-cloudwatch-agent
sudo systemctl status amazon-cloudwatch-agent
sudo tail -f /opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log
```

---

# 👤 **Author**

```
Name: Thiyagu S
Role: Cloud & DevOps Learner
Country: India 🇮🇳
GitHub: Thiyagu-2003
```

---

# ❤️ **Footer**

<p align="center">
  <strong>Made with ❤️ by <a href="https://github.com/Thiyagu-2003">Thiyagu S</a></strong><br>
  Learning • Building • Improving
</p>

---


