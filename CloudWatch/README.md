---

# 📘 **Amazon CloudWatch & CloudWatch Agent – Complete Guide**

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

* 🔎 Overview
* 📦 Prerequisites
* ⚙️ Install CloudWatch Agent
* 📝 Create Config File
* 🚀 Apply & Start Agent
* 📊 Verify in CloudWatch
* 🛠 Troubleshooting
* 🧾 Quick Reference
* 👤 Author
* ❤️ Footer

---

# 🔎 **1. Overview**

CloudWatch Agent collects **CPU, RAM, disk, network metrics, and log files** from EC2 → sends them to **CloudWatch Metrics & CloudWatch Logs**.

Use this when you want:

* Real-time EC2 monitoring
* Custom metrics
* Log ingestion
* Alarm setup
* Dashboards for infra visibility

---

# 📦 **2. Prerequisites**

Attach these policies to the EC2's IAM Role:

### ✔️ Required Policies

* `CloudWatchAgentServerPolicy`
* `AmazonSSMManagedInstanceCore` (if using SSM)

### ✔️ Required Access

* Outbound internet access
* Any Linux EC2 instance (AL2, Ubuntu, RHEL, CentOS)

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

Agent installs into:

```
/opt/aws/amazon-cloudwatch-agent/bin/
```

---

# 📝 **4. Create the Configuration File**

### 🅰️ Option A — Wizard (recommended)

```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
```

Generated file:

```
/opt/aws/amazon-cloudwatch-agent/bin/config.json
```

---

### 🅱️ Option B — Your Own JSON

Place custom config here:

```
/opt/aws/amazon-cloudwatch-agent/bin/config.json
```

---

# 🚀 **5. Apply Config & Start the Agent**

Run main command:

```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
  -a fetch-config -m ec2 \
  -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json \
  -s
```

### Validate config:

```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
  -a validate \
  -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json
```

### systemd controls:

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

Check log group names match your config.

---

# 🛠 **7. Troubleshooting**

---

## ❗ A. ec2tagger / Missing Tags

Error example:

`UnauthorizedOperation: ec2:DescribeTags`

Add these permissions:

* `ec2:DescribeTags`
* `ec2:DescribeInstances`
* `ec2:DescribeVolumes`

Restart:

```bash
sudo systemctl restart amazon-cloudwatch-agent
```

---

## ❗ B. Logs Not Appearing

Check:

1. IAM role → `CloudWatchAgentServerPolicy`
2. Log file path exists
3. Agent logs:

```bash
sudo tail -f /opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log
```

Look for `piping log to cloudwatchlogs`.

---

## ❗ C. Invalid JSON / Config Errors

```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
  -a validate \
  -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json
```

Fix formatting issues.

---

# 🧾 **8. Useful Paths & Commands (Quick Reference)**

### 📄 Agent Logs:

```
/opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log
```

### 📄 Config File:

```
/opt/aws/amazon-cloudwatch-agent/bin/config.json
```

### 📄 Converted TOML File:

```
/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.toml
```

### 🔧 Common Commands:

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
