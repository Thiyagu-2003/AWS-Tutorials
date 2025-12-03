---

# 📘 **Amazon SNS (Simple Notification Service) — Complete Guide**

<p align="center">
  <img src="https://img.shields.io/badge/AWS-SNS-orange?logo=amazon-aws&logoColor=white" />
  <img src="https://img.shields.io/badge/Model-Pub/Sub-blue" />
  <img src="https://img.shields.io/badge/Status-Production%20Ready-brightgreen" />
</p>

<p align="center">
  <a href="https://github.com/Thiyagu-2003">
    <img src="https://img.shields.io/badge/Made%20By-Thiyagu%20S-green?logo=github" />
  </a>
</p>

---

# 📑 **Table of Contents**

* [🔎 Overview](#-overview)
* [📡 What SNS Actually Is](#-what-sns-actually-is)
* [⚔️ SNS vs SQS](#️-sns-vs-sqs)
* [📤 Message Delivery Targets](#-message-delivery-targets)
* [🧩 Core Components](#-core-components)
* [🏗️ Real-World Use Cases](#️-real-world-use-cases)
* [🔁 How SNS Works — Example](#-how-sns-works--example)
* [💵 Pricing](#-pricing)
* [🧠 Important Details Developers Forget](#-important-details-developers-forget)
* [🛠️ CLI Commands](#️-cli-commands)
* [🖥️ Console Steps (SNS Setup)](#️-console-steps-sns-setup)
* [🎤 Interview Questions (All 15 Included)](#-interview-questions-all-15-included)
* [👤 Author](#-author)
* [❤️ Footer](#️-footer)

---

# 🔎 **Overview**

Amazon SNS is AWS’s real-time **publish/subscribe** messaging system used for **fan-out notifications**, **event-driven communication**, and **broadcasting messages instantly** to multiple subscribers.

SNS delivers the message.
It does **NOT** store or queue it — that’s SQS.

---

# 📡 **What SNS Actually Is**

SNS = “**Publish once → deliver everywhere**”

It is specifically designed for:

* Real-time alerts
* Multi-service notifications
* Event broadcasts
* Cross-service communication

SNS is **not** built for workload processing or durable storage.

---

# ⚔️ **SNS vs SQS**

| Feature         | SNS                    | SQS             |
| --------------- | ---------------------- | --------------- |
| Model           | Pub/Sub                | Queue           |
| Delivery        | Push                   | Pull            |
| Stores Messages | ❌ No                   | ✔ Yes           |
| Ordering        | Not guaranteed         | FIFO available  |
| Best Use Case   | Fan-out, notifications | Task processing |

**Rule:**
If you need durable, retryable processing → SQS
If you need instant broadcast → SNS

---

# 📤 **Message Delivery Targets**

SNS supports pushing messages to:

* 📧 Email
* 📱 SMS
* 📲 Mobile Push (APNs, FCM)
* 🔁 AWS Lambda
* 📦 SQS
* 🌍 HTTPS endpoints
* 🔗 Other AWS services

---

# 🧩 **Core Components**

### **🔸 Topic**

The channel where messages are published.

### **🔹 Publisher**

Any service that sends the message (Lambda, EC2, API Gateway).

### **🔸 Subscriber**

Receives messages via Email, Lambda, SMS, SQS, etc.

---

# 🏗️ **Real-World Use Cases**

* 🔔 Application alerts
* 📡 Cross-service fan-out
* 🛒 E-commerce event broadcasting
* 📦 Logistics workflows
* 📈 Analytics triggers
* 📱 Mobile push notifications

---

# 🔁 **How SNS Works — Example**

### **Scenario: Order Placed**

**Publisher:** E-commerce API
**Topic:** `OrderEvents`
**Subscribers:**

* Lambda → Generate invoice
* SQS → Shipment service
* Email → Notify customer
* HTTPS → Dashboard update

One event → multiple automated systems triggered instantly.

---

# 💵 **Pricing**

* First **1M requests are FREE**
* Notifications = very cheap
* SMS cost varies

  * India: ~₹1–₹2 per SMS

---

# 🧠 **Important Details Developers Forget**

* ❌ Email has **no retry**
* ✔ Lambda / HTTPS / SQS get retries
* ✔ Message size max = **256 KB**
* ✔ KMS encryption supported
* ✔ SNS + SQS = the correct reliable fan-out pattern
* ❌ SNS does **not** store messages

---

# 🛠️ **CLI Commands**

### **Create Topic**

```bash
aws sns create-topic --name MyTopic
```

### **Subscribe Email**

```bash
aws sns subscribe \
  --topic-arn arn:aws:sns:region:111111111111:MyTopic \
  --protocol email \
  --notification-endpoint user@example.com
```

### **Publish Message**

```bash
aws sns publish \
  --topic-arn arn:aws:sns:region:111111111111:MyTopic \
  --message "Test message"
```

---

# 🖥️ **Console Steps — SNS Setup**

### **1️⃣ Open SNS**

AWS Console → SNS.

### **2️⃣ Create Topic**

Choose Standard → Name → Create.

### **3️⃣ Add Subscription**

Select protocol → enter endpoint → create.

### **4️⃣ Confirm Email**

If email was chosen, confirm link in inbox.

### **5️⃣ Publish Message**

Test notification.

---

# 🎤 **Interview Questions (All 15 Included)**

Here are **all 15 questions**, each with fresh emoji headers.

---

## **🟦 Q1: What is Amazon SNS and how does it work?**

SNS is a pub/sub service that **pushes** messages to multiple subscribers instantly.

---

## **🟨 Q2: SNS vs SQS — When do you use which?**

SNS → broadcast events
SQS → process tasks

Fan-out pattern:
SNS → multiple SQS → multiple consumers.

---

## **🟩 Q3: What are the two types of SNS Topics?**

* Standard
* FIFO

---

## **🟪 Q4: Does SNS store messages?**

No. It sends immediately.

---

## **🟫 Q5: How does SNS ensure message delivery?**

Retries work differently for each protocol:

* Email → no retry
* HTTPS → retries with backoff
* Lambda → retries
* SQS → guaranteed

---

## **🟥 Q6: What are SNS protocols?**

Email, SMS, HTTP/S, Lambda, SQS, Mobile Push.

---

## **🟧 Q7: What is message filtering in SNS?**

Subscribers can filter messages by attributes.

---

## **🟦 Q8: What happens if one subscriber fails?**

SNS still delivers to others.

---

## **🟩 Q9: What is the max message size in SNS?**

256 KB.

---

## **🟨 Q10: How does SNS work with SQS in fan-out?**

SNS broadcasts one event → multiple SQS queues receive copies → independent workers process.

---

## **🟪 Q11: Does SNS support encryption?**

Yes:

* KMS encryption at rest
* HTTPS in transit

---

## **🟫 Q12: Can SNS be used for email marketing campaigns?**

No.
Use **SES** for bulk emails.

---

## **🟥 Q13: SNS vs SES?**

SNS = notifications
SES = email sending/receiving system

---

## **🟧 Q14: What is a Topic in SNS?**

A communication channel for publishing and subscribing.

---

## **🟦 Q15: Does SNS have Dead-Letter Queues (DLQ)?**

No.
DLQs belong to SQS.
But SNS → SQS → SQS DLQ is common.

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