# ⏱ Investigation Timeline — Authentication Attack Analysis

---

## 📌 Investigation Overview

| Field | Value |
|------|------|
| **Project** | Authentication Attack Analysis |
| **Device** | js-mde-test |
| **Primary Account** | josh |
| **Data Sources** | Microsoft Defender for Endpoint, Microsoft Sentinel |
| **Time Zone** | UTC |

---

## 🧭 Timeline of Events

---

### 🔴 Event 1 — Repeated Failed Authentication Attempts

**Date:** 2025-12-24  
**Time:** Prior to 05:42 UTC  

Multiple failed logon attempts were observed targeting the device **js-mde-test**.

- Failed authentication activity identified via endpoint logon telemetry
- Volume and frequency of failures exceeded normal baseline behavior
- Pattern consistent with brute-force or password-guessing activity against an internet-exposed system

---

### 🟢 Event 2 — Successful Interactive Logon Detected

**Date:** 2025-12-24  
**Time:** 05:42:09 UTC  

A successful authentication was recorded for account **josh** on **js-mde-test**.

- Logon type indicated an **interactive user session**
- This event marked the **first successful logon** following the series of failed attempts
- This transition represented a critical investigative pivot point

---

### 🖥 Event 3 — Desktop Session Components Created

**Date:** 2025-12-24  
**Time:** Immediately following successful logon  

System session accounts were created:

- `dwm-*`
- `umfd-*`

These components are associated with Windows graphical desktop sessions.

**Interpretation:**  
Their creation confirms that a full interactive desktop session was established, not a background or service-based logon.

---

### 🔁 Event 4 — Additional Successful Interactive Logons

**Date Range:** 2025-12-24 → 2025-12-25  

Additional successful logons were observed for account **josh**.

- **Total successful logons:** 19
- Each successful logon resulted in desktop session creation
- No other user accounts successfully authenticated during the investigation window

**Interpretation:**  
Repeated interactive access indicates ongoing user activity rather than a single anomalous event.

---

### ⚙️ Event 5 — PowerShell Execution Initiated

**Date:** 2025-12-24  
**Time:** Shortly after first successful logon  

PowerShell (`powershell.exe`) was executed under the context of account **josh**.

- Initial execution launched via `explorer.exe`, indicating direct user interaction
- Subsequent PowerShell processes were spawned by PowerShell itself
- Execution parameters included **`-ExecutionPolicy Bypass`**

**Interpretation:**  
While PowerShell usage can be legitimate, execution policy bypass is commonly examined during post-authentication investigations.

---

### 🌐 Event 6 — PowerShell Network Activity Observed

**Date:** 2025-12-24  
**Time:** Following PowerShell execution  

Outbound network activity initiated by `powershell.exe` was observed.

- Connections directed to **Microsoft / Azure-associated endpoints**
- Traffic occurred over **HTTPS (TCP 443)**
- No connections to unknown, suspicious, or attacker-controlled infrastructure
- No repetitive beaconing or anomalous network patterns detected

**Interpretation:**  
Network activity was consistent with legitimate system or cloud-related communication.

---

### ✅ Event 7 — No Additional Suspicious Post-Authentication Activity

During the investigation window, **no evidence** was found of:

- Malware execution
- Credential dumping
- Account modification or creation
- Persistence mechanisms
- Lateral movement to other systems

No obfuscated commands or malicious binaries were identified.

---

## 🧠 Timeline Summary

- Authentication failures preceded successful interactive access
- Access resulted in legitimate desktop session creation
- PowerShell activity occurred but showed no malicious follow-on behavior
- Network traffic remained within trusted Microsoft infrastructure
- No indicators of compromise were identified beyond exposure risk

---

## 📝 Notes

- This timeline represents a chronological reconstruction of observed events
- Conclusions, risk assessment, and remediation recommendations are documented separately

---

