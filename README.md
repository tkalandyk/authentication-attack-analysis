# 🚨 Authentication Attack Analysis  
<p align="center">
  <img src="PATH_TO_YOUR_IMAGE.png" width="700" />
</p>

<p align="center">
  <em>Incident Response Investigation · Endpoint & Identity Security</em>
</p>

---



  <em>Incident Response Investigation · Endpoint & Identity Security</em>
</p>

---


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
- Commands included **`ExecutionPolicy Bypass`**

⚠️ This behavior required deeper inspection but is **not inherently malicious**.

---

### 4️⃣ Network Activity Analysis

- Outbound connections initiated by PowerShell were reviewed
- Findings:
  - All destinations resolved to **Microsoft / Azure infrastructure**
  - Traffic occurred over **HTTPS (port 443)**
  - ❌ No suspicious or attacker-controlled endpoints
  - ❌ No beaconing or anomalous patterns

---

### 5️⃣ No Evidence of Malicious Follow-On Activity

No evidence was found of:

- ❌ Malware execution  
- ❌ Credential dumping  
- ❌ Account creation or modification  
- ❌ Persistence mechanisms  
- ❌ Lateral movement  

---

## 🧠 Analyst Assessment

> **Suspicious authentication behavior was confirmed against an exposed endpoint; however, no malicious post-authentication activity was identified.**

This activity did **not meet the threshold for a confirmed breach**, but it represents **elevated security risk**.

---

## 📉 Impact Assessment

- **Data Loss:** None identified  
- **Malware Infection:** None observed  
- **Spread to Other Systems:** None detected  

**Primary impact:** security exposure risk, not compromise.

---

## 🛡️ Recommendations

- 🔒 Restrict or remove direct internet exposure to RDP/authentication services
- 🔐 Enforce Multi-Factor Authentication (MFA)
- 🔑 Apply strong password & lockout policies
- 👤 Limit interactive logon permissions
- 📊 Continue monitoring authentication anomalies

---

## 📂 Investigation Artifacts

- 🕒 **Investigation Timeline:** `Investigation-Timeline.md`
- 🔍 **KQL Queries:** Located in the `Queries/` directory

---

## ✅ Conclusion

This investigation demonstrates how **authentication telemetry**, when correlated with **process execution and network activity**, enables analysts to distinguish between **attempted intrusion** and **legitimate behavior**.

While no compromise was confirmed, the findings highlight the importance of **attack surface reduction** and strong identity controls.

---

## 👤 Author

**Ty Kalandyk**  
Cybersecurity · SOC · Incident Response  
Focused on real-world detection, investigation, and defensive analysis


