# 🚨 Authentication Attack Analysis  
### Incident Response Investigation · Endpoint & Identity Security

---

## 🧠 Executive Summary

An incident response investigation was conducted after **abnormal authentication activity** was detected against an **internet-exposed Windows virtual machine (`js-mde-test`)**.

Between **December 24 and December 25, 2025**, the device experienced:

- ❌ A high volume of failed logon attempts  
- ✅ Successful interactive authentication  
- ⚙️ Subsequent PowerShell execution  

---

### 🎯 Investigation Outcome

✅ **No evidence of malware execution**  
✅ **No persistence mechanisms identified**  
✅ **No lateral movement detected**  
✅ **No command-and-control (C2) communication observed**

⚠️ **However:** the activity revealed **significant exposure risk** due to the system’s configuration.

---

## 🎯 Investigation Objectives

- Identify the source and nature of abnormal authentication behavior
- Determine whether successful access resulted in compromise
- Analyze post-authentication process execution
- Review outbound network activity for signs of C2 or data exfiltration
- Assess impact and recommend remediation

---

## 🧭 Scope of Investigation

The investigation focused on the following telemetry:

- ✅ Endpoint authentication logs (failed & successful logons)
- ✅ Interactive Windows session creation
- ✅ Post-authentication process execution
- ✅ Outbound network connections initiated after authentication

---

## 🛠️ Tools & Technologies

- Microsoft Defender for Endpoint  
- Microsoft Sentinel  
- Kusto Query Language (KQL)  
- Azure-hosted Windows Virtual Machine  

---

## 🧪 Key Findings

---

### 1️⃣ Abnormal Authentication Activity

- A large number of **failed authentication attempts** were observed
- Activity was consistent with **brute-force or password-guessing behavior**
- The device was confirmed to be **internet-exposed**

---

### 2️⃣ Successful Interactive Access

- Account **`josh`** successfully authenticated after failed attempts
- Desktop session artifacts confirmed:
  - A **real interactive Windows session**
- Multiple successful logons were observed during the investigation window

---

### 3️⃣ Post-Authentication PowerShell Execution

- **PowerShell (`powershell.exe`)** executed under authenticated user context
- Execution flow observed:
  - Launched via `explorer.exe`
  - Followed by chained PowerShell processes
- Commands included **`ExecutionPolicy Bypas**

