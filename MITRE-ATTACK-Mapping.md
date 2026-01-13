# 🧠 MITRE ATT&CK Mapping — Authentication Attack Analysis

This document maps observed behaviors during the investigation to the MITRE ATT&CK framework to contextualize attacker techniques that were assessed or ruled out.

---

## 🎯 ATT&CK Mapping Summary

| Tactic | Technique | ID | Observed | Notes |
|------|----------|----|---------|------|
| Credential Access | Brute Force | T1110 | 🟡 Assessed | High volume of failed logons observed |
| Initial Access | Valid Accounts | T1078 | 🟡 Assessed | Successful authentication occurred |
| Execution | Command-Line Interface | T1059 | 🟡 Assessed | PowerShell execution observed |
| Execution | PowerShell | T1059.001 | 🟡 Assessed | ExecutionPolicy Bypass used |
| Command and Control | Application Layer Protocol | T1071 | ❌ Not Observed | No suspicious outbound communication |
| Lateral Movement | Remote Services | T1021 | ❌ Not Observed | No lateral authentication detected |
| Persistence | Account Manipulation | T1098 | ❌ Not Observed | No account changes identified |

---

## 🔍 Technique Analysis

### T1110 — Brute Force
- Multiple failed authentication attempts observed
- Pattern consistent with password guessing against an exposed endpoint
- No evidence of automated distributed tooling identified

---

### T1078 — Valid Accounts
- Successful authentication observed for existing account (**josh**)
- Access followed a series of failed attempts
- Determined to be legitimate based on post-authentication behavior

---

### T1059.001 — PowerShell
- PowerShell executed interactively under authenticated user context
- Execution policy bypass observed
- No malicious payloads or obfuscated commands detected

---

### T1071 — Command and Control (Not Observed)
- PowerShell network traffic reviewed
- All destinations associated with Microsoft / Azure infrastructure
- No beaconing, suspicious domains, or anomalous protocols detected

---

## 🧠 Analytical Conclusion

Although several ATT&CK techniques were **assessed**, no techniques progressed into confirmed malicious execution or attacker lifecycle advancement.

This mapping demonstrates **defensive evaluation**, not just detection of compromise, and reflects realistic SOC analysis where many investigations result in *no breach* but still inform risk posture.

---
