# Azure-DevOps-Monitoring-Project
Production-style monitoring setup in Azure with VM Insights, alert rules, and notification pipelines for proactive incident detection.

# 🚀 Azure VM Monitoring & Alerting Implementation Guide

## 📌 Overview

This project demonstrates how to set up **real-time CPU monitoring** and **automated alerting (Email + SMS)** for an Azure Linux Virtual Machine using Azure Monitor.

---

## 🧾 Prerequisites

* Active Azure Subscription
* Linux Virtual Machine (Ubuntu recommended)
* SSH access to VM
* `stress` tool installed:

```bash
sudo apt update
sudo apt install stress -y
```

---

## 🏗️ Step 1: Provision Virtual Machine

### 1.1 Create VM

1. Login to Azure Portal
2. Navigate to **Virtual Machines**
3. Click **Create → Azure Virtual Machine**

---

### 1.2 Configure Basics

* **Resource Group**: `devops-rg`
* **VM Name**: `devops-vm`
* **Region**: Same region for all resources
* **Image**: Ubuntu 22.04 LTS
* **Size**: `Standard_B1s` (Free tier)

---

### 1.3 Authentication

* Select **Password**
* Username: `azureuser`
* Set a strong password

---

### 1.4 Networking

* Allow inbound ports:

  * ✅ SSH (22)

---

### 1.5 Create VM

* Click **Review + Create → Create**
* Wait 1–2 minutes

---

## 📡 Step 2: Enable Monitoring (VM Insights)

### 2.1 Navigate to Insights

1. Open your VM (`devops-vm`)
2. Go to:

   ```
   Monitoring → Insights
   ```

---

### 2.2 Enable Monitoring

* Click **Enable**
* Select or create **Log Analytics Workspace**
* Click **Review + Enable**

---

### ⏳ Wait

* 2–5 minutes for onboarding

---

### ✅ Verification

* Go to:

  ```
  VM → Insights
  ```
* You should see:

  * CPU graph
  * Memory usage
  * Disk activity

---

## 🔔 Step 3: Create Action Group

### 3.1 Navigate

```
Azure Monitor → Alerts → Action Groups
```

---

### 3.2 Create Action Group

* **Name**: `devops-action-group`
* **Display Name**: `devopsAG`
* **Region**: Same as VM

---

### 3.3 Add Notifications

#### 📧 Email

* Type: Email
* Name: `gmail-alert`
* Enter Gmail address

---

#### 📱 SMS

* Type: SMS
* Name: `sms-alert`
* Country: India (+91)
* Enter phone number

---

### ⚠️ Important

* Enable **Common Alert Schema**
* Ensure both notifications are listed

---

### 3.4 Create

* Click **Review + Create → Create**

---

## ⚙️ Step 4: Create Alert Rule

### 4.1 Navigate

```
VM → Alerts → + Create → Alert Rule
```

---

### 4.2 Configure Condition

* Signal Name: **Percentage CPU**
* Threshold Type: **Static**
* Aggregation: **Average**
* Operator: **Greater than**
* Threshold: **70%**
* Frequency: **1 minute**

---

### 4.3 Attach Action Group

* Select:

  ```
  devops-action-group
  ```

---

### 4.4 Alert Details

* Name: `high-cpu-alert`
* Severity: 2 (Warning)

---

### 4.5 Create

* Click **Review + Create → Create**

---

## 🧪 Step 5: Simulate High CPU Load

### 5.1 Connect to VM

```bash
ssh azureuser@<public-ip>
```

---

### 5.2 Run Stress Test

```bash
stress --cpu 4 --timeout 600
```

---

### 💡 Why 600 seconds?

* Ensures Azure captures sustained CPU usage
* Prevents false negatives

---

## 📊 Step 6: Verify Alert

### 6.1 Check in Azure Portal

```
Azure Monitor → Alerts → Fired Alerts
```

---

### Expected:

* Alert state: **Fired**
* Resource: `devops-vm`

---

### 6.2 Check Notifications

* 📧 Email (Inbox / Spam)
* 📱 SMS on phone

---

## ❗ Troubleshooting (IMPORTANT)

### 🔸 No Alert Triggered

* Ensure CPU crossed 70%
* Use:

  ```bash
  top
  ```

---

### 🔸 No Email/SMS

* Check Action Group linked properly
* Verify email confirmation
* Wait 5 minutes (Azure delay)

---

### 🔸 Region Error

* Use same region for:

  * VM
  * Action Group

---

## 🎯 Architecture

```
VM → Azure Monitor → Alert Rule → Action Group → Email/SMS
```

---

## 🧠 Key Learnings

* Azure Monitor collects metrics
* Alert Rules define conditions
* Action Groups handle notifications
* Stress testing validates monitoring

---

## 🚀 Future Enhancements

* Log-based alerts (KQL)
* Dashboard creation
* Integration with Slack / Teams
* Auto-remediation using Azure Functions

---

## 📌 Author

@Harshvardhansinh S Jadeja
