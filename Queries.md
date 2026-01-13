
# 🔐 Authentication Attack Analysis — Investigation Queries

This document contains the complete set of KQL queries used during an incident response investigation into abnormal authentication activity on an internet-exposed Windows virtual machine (**js-mde-test**).

Each query represents a deliberate investigative pivot following suspected brute-force authentication behavior and mirrors a real-world SOC / Incident Response workflow.

---

## ✅ QUERY 1 — Failed Authentication Attempts

**Purpose:**  
Identify the volume of failed logon attempts targeting the device to determine whether brute-force or password-guessing behavior occurred.

<details>
<summary>🔍 View KQL Query</summary>

```kql
DeviceLogonEvents
| where ActionType == "LogonFailed"
| where DeviceName contains "js-mde-test"
| summarize FailedLogons = count() by DeviceName
```

</details>


<img width="989" height="135" alt="image" src="https://github.com/user-attachments/assets/dda1fb76-6137-4c2f-b61e-936fe75480b1" />


**Why this matters:**  
A high number of failed logons against an exposed endpoint is a strong indicator of brute-force or credential-stuffing activity.

---

## ✅ QUERY 2 — Successful Authentication Attempts

**Purpose:**  
Determine whether any authentication attempts succeeded after the failed attempts, indicating potential credential compromise.

<details>
<summary>🔍 View KQL Query</summary>

```kql
DeviceLogonEvents
| where ActionType == "LogonSuccess"
| where DeviceName contains "js-mde-test"
| summarize SuccessfulLogons = count() by DeviceName
```

</details>

<img width="867" height="123" alt="image" src="https://github.com/user-attachments/assets/ef232790-4b34-4514-9567-487a7545e737" />




**Why this matters:**  
A success following repeated failures is a classic brute-force pattern and represents a critical pivot point in the investigation.

---

## ✅ QUERY 3 — Successful Logons by Account (Identity Pivot)

**Purpose:**  
Identify which user accounts successfully authenticated and establish the timeframe and frequency of access.

<details>
<summary>🔍 View KQL Query</summary>

```kql
DeviceLogonEvents
| where ActionType == "LogonSuccess"
| where DeviceName contains "js-mde-test"
| summarize
    SuccessCount = count(),
    FirstSeen = min(TimeGenerated),
    LastSeen  = max(TimeGenerated)
  by AccountName
| order by SuccessCount desc
```

</details>

<img width="901" height="198" alt="image" src="https://github.com/user-attachments/assets/f284dc26-95fa-43d3-b8e9-c75d1ae2e265" />



**Why this matters:**  
This query isolates the identity involved (for example, **josh**) and provides first/last seen timestamps used later to build an accurate investigation timeline.

---

## ✅ QUERY 4 — Suspicious Tool Execution (Post-Authentication)

**Purpose:**  
Identify execution of commonly abused native Windows binaries (LOLBins) after successful authentication under the specific user context.

<details>
<summary>🔍 View KQL Query</summary>

```kql
DeviceProcessEvents
| where DeviceName contains "js-mde-test"
| where Timestamp > datetime(2025-12-24T05:42:09.5813634Z)
| where AccountName == "josh"
| where FileName in~ (
  "powershell.exe","pwsh.exe","cmd.exe","wmic.exe","net.exe",
  "whoami.exe","nltest.exe","rundll32.exe","reg.exe","schtasks.exe",
  "sc.exe","bitsadmin.exe","certutil.exe","mshta.exe",
  "wscript.exe","cscript.exe"
)
| project
    Timestamp,
    AccountName,
    FileName,
    ProcessCommandLine,
    InitiatingProcessFileName
| order by Timestamp asc
```

</details>

**Why this matters:**  
Threat actors often rely on native tools to blend in with legitimate activity after gaining access.  
This query helps detect suspicious post-authentication behavior without relying on malware signatures.

---

## ✅ QUERY 5 — Post-Authentication Network Activity (PowerShell)

**Purpose:**  
Determine whether PowerShell activity resulted in outbound network connections to suspicious or attacker-controlled infrastructure.

<details>
<summary>🔍 View KQL Query</summary>

```kql
DeviceNetworkEvents
| where DeviceName contains "js-mde-test"
| where Timestamp > datetime(2025-12-24T05:42:09.5813634Z)
| where InitiatingProcessFileName == "powershell.exe"
| project
    Timestamp,
    InitiatingProcessAccountName,
    InitiatingProcessFileName,
    RemoteIP,
    RemotePort,
    Protocol,
    RemoteUrl
| order by Timestamp asc
```

</details>

**Why this matters:**  
Outbound connections following authentication can indicate command-and-control (C2), payload retrieval, or data exfiltration attempts.

---

## 🧠 Investigation Logic Summary

The investigation followed a structured, real-world SOC methodology:

1. **Confirm brute-force behavior** via failed authentication events  
2. **Validate successful access** following failures  
3. **Identify the affected identity** and access timeframe  
4. **Inspect post-authentication process execution** for suspicious tooling  
5. **Analyze outbound network activity** to assess C2 or exfiltration risk  

This approach ensures both technical accuracy and defensible investigative reasoning.

---
