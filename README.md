🚨 Authentication Attack Analysis
Incident Response Investigation | Endpoint & Identity Security
<p align="center"> <img src="https://raw.githubusercontent.com/github/explore/main/topics/security/security.png" width="700"> </p>
🔎 Executive Summary

An incident response investigation was conducted after abnormal authentication activity was detected against an internet-exposed Windows virtual machine (js-mde-test).

Between December 24 and December 25, 2025, the device experienced:

A high volume of failed logon attempts

Followed by successful interactive authentication

Subsequent PowerShell execution

The objective of this investigation was to determine whether the authentication activity resulted in a confirmed compromise and whether malicious post-authentication behavior occurred.

🔐 Outcome:
No evidence of malware execution, persistence, lateral movement, or command-and-control (C2) communication was identified. However, the activity revealed significant exposure risk due to the system’s configuration.

🎯 Investigation Objectives

Identify the source and nature of abnormal authentication behavior

Confirm whether successful access resulted in compromise

Analyze post-authentication process execution

Review outbound network activity for signs of C2 or data exfiltration

Determine impact and recommend remediation

🧭 Scope of Investigation

The investigation focused on the following telemetry:

✅ Endpoint authentication logs (failed & successful logons)

✅ Interactive Windows session creation

✅ Post-authentication process execution

✅ Outbound network connections initiated after authentication

🛠️ Tools & Technologies Used

Microsoft Defender for Endpoint

Microsoft Sentinel

Kusto Query Language (KQL)

Azure-hosted Windows Virtual Machine

🧪 Key Findings
1️⃣ Abnormal Authentication Activity

A large number of failed logon attempts were observed against js-mde-test

The frequency and volume were consistent with brute-force or password-guessing behavior

The device was confirmed to be internet-exposed

2️⃣ Successful Interactive Access

After the failed attempts, account josh successfully authenticated

Windows desktop session artifacts (dwm-*, umfd-*) confirmed:

A real interactive user session

Multiple successful logons occurred for the same account during the investigation window

3️⃣ Post-Authentication PowerShell Execution

PowerShell (powershell.exe) was executed under the authenticated user context

Execution flow:

Launched interactively via explorer.exe

Followed by chained PowerShell executions

Commands included ExecutionPolicy Bypass, commonly associated with:

Administrative scripting

Automation

Lab activity

⚠️ This behavior warranted deeper inspection but is not inherently malicious on its own

4️⃣ Network Activity Analysis

PowerShell-initiated outbound connections were reviewed

Observations:

All destinations resolved to Microsoft / Azure infrastructure

Communication occurred over HTTPS (port 443)

No connections to unknown or attacker-controlled endpoints

No beaconing or anomalous traffic patterns detected

5️⃣ No Evidence of Malicious Follow-On Activity

The investigation found no evidence of:

❌ Malware execution

❌ Credential dumping

❌ Account creation or modification

❌ Persistence mechanisms

❌ Lateral movement

🧠 Analyst Assessment

Suspicious authentication behavior was confirmed against an exposed endpoint; however, no evidence of malicious post-authentication activity was identified.

This activity did not meet the threshold for a confirmed breach, but it represents elevated security risk due to exposure and authentication patterns.

📉 Impact Assessment

Data Loss: None identified

Malware Infection: None observed

Spread to Other Systems: None detected

Primary impact: Security exposure risk, not compromise.

🛡️ Recommendations

To reduce the risk of future authentication-based attacks:

🔒 Restrict or remove direct internet exposure to RDP/authentication services

🔐 Enforce Multi-Factor Authentication (MFA)

🔑 Implement strong password & lockout policies

👤 Limit interactive logon permissions

📊 Continue monitoring authentication anomalies

📂 Investigation Artifacts

🕒 Investigation Timeline: Investigation-Timeline.md

🔍 KQL Queries: Located in the Queries/ directory

✅ Conclusion

This investigation highlights how authentication telemetry, when correlated with process execution and network behavior, can effectively distinguish between attempted intrusion and legitimate activity.

While no compromise was confirmed, the findings underscore the importance of reducing attack surface and maintaining strong identity controls on exposed systems.

👨‍💻 Author

Ty Kalandyk
Cybersecurity | SOC & Incident Response Projects
Focused on real-world detection, investigation, and defensive analysis

🚀 Why this version works

Clear visual flow

Scannable sections

Feels like a real incident response write-up

Makes people want to keep reading

If you want next, I can:

Match the timeline styling to this README

Add a Queries/ section with commentary

Help you write a 30-second interview explanation for this project
