##Investigation Timeline

Project: Authentication Attack Analysis
Device: js-mde-test
Primary Account Observed: josh
Data Sources: Microsoft Defender for Endpoint, Microsoft Sentinel
Time Zone: UTC

Timeline of Events
1. Repeated Failed Authentication Attempts Observed

Date: 2025-12-24
Time: Prior to 05:42 UTC

Multiple failed logon attempts were observed on device js-mde-test.

Failed authentication activity was identified through endpoint logon telemetry.

The volume and frequency of failures indicated abnormal authentication behavior against the device.

2. Successful Interactive Logon Detected

Date: 2025-12-24
Time: 05:42:09 UTC

A successful logon was recorded for account josh on device js-mde-test.

Logon type indicated an interactive user session.

This marked the first successful authentication following the failed logon attempts.

3. Desktop Session Components Created

Date: 2025-12-24
Time: Immediately following successful logon

System session accounts (dwm-*, umfd-*) were created.

These components are associated with Windows graphical desktop sessions.

Their creation confirms that an interactive user session was established on the device.

4. Additional Successful Interactive Logons

Date Range: 2025-12-24 to 2025-12-25

A total of 19 successful logons were recorded for account josh.

Each successful logon resulted in the creation of desktop session components.

No additional user accounts were observed logging in successfully during this period.

5. PowerShell Execution Initiated by Logged-In User

Date: 2025-12-24
Time: Shortly after first successful logon

PowerShell (powershell.exe) was executed under the context of account josh.

Initial PowerShell execution was launched via explorer.exe, indicating direct user interaction.

Subsequent PowerShell executions were spawned by PowerShell itself, consistent with scripted or sequential command execution.

PowerShell was executed with parameters including -ExecutionPolicy Bypass.

6. PowerShell Network Activity Observed

Date: 2025-12-24
Time: Following PowerShell execution

Outbound network connections initiated by powershell.exe were observed.

Network traffic was directed to Microsoft and Azure-associated endpoints.

Communication occurred over standard HTTPS (port 443).

No connections to unknown, suspicious, or non-Microsoft infrastructure were identified.

No repetitive beaconing or anomalous network patterns were observed.

7. No Additional Suspicious Post-Authentication Activity Detected

No evidence of:

Malware execution

Credential dumping

Account modification

Persistence mechanisms

Lateral movement

No malicious binaries or obfuscated command execution was observed during the investigation window.

End of Timeline
Notes

This timeline represents a chronological reconstruction of observed events.

Interpretations, conclusions, and remediation recommendations are documented separately in the final incident report.
