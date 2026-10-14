# Microsoft Sysmon (System Monitor)

## Description
Sysmon (System Monitor) is a Microsoft Sysinternals utility that runs as a background Windows service and device driver to log detailed security events to the Windows Event Log. Unlike standard Windows audit policies, Sysmon provides high-fidelity tracking of process creation (with full command lines and binary hashes), network connections, file modifications, registry changes, and DNS queries, making it essential for endpoint detection, threat hunting, and blue-team defense.

- **Developer:** Microsoft Sysinternals (Mark Russinovich)
- **Configuration Reference:** [SwiftOnSecurity Sysmon Config](https://github.com/SwiftOnSecurity/sysmon-config)
- **Log Path:** `Applications and Services Logs \ Microsoft \ Windows \ Sysmon \ Operational`
- **Detailed Practical Notes:** See [Windows Tools Practical Notes: Sysmon](../Windows_Tools.md#3-tool-3-sysmon-system-monitor--endpoint-activity-logging)

---

## Quick-Start Steps
1. **Prepare Directory:** Open PowerShell as Administrator and create the working folder:
   ```powershell
   New-Item -ItemType Directory -Path C:\sys -Force
   Set-Location C:\sys
   ```
2. **Download Configuration:** Clone the SwiftOnSecurity configuration repository:
   ```powershell
   git clone https://github.com/SwiftOnSecurity/sysmon-config.git
   ```
3. **Apply Configuration:** Update the active Sysmon rules using the `-c` switch:
   ```powershell
   Sysmon64.exe -c .\sysmon-config\sysmonconfig-export.xml
   ```
   *(Note: If Sysmon is not yet installed, run `Sysmon64.exe -accepteula -i .\sysmon-config\sysmonconfig-export.xml` first).*
4. **Open Event Viewer:** Press `Win+R`, type `eventvwr.msc`, and press Enter.
5. **Inspect Events:** Navigate to:
   ```text
   Event Viewer -> Applications and Services Logs -> Microsoft -> Windows -> Sysmon -> Operational
   ```
   Review recorded events (Event ID 1 for Process Creation, ID 3 for Network Connections, ID 22 for DNS Queries).

---

## Lab Record Summary
- **Aim:** To configure Microsoft Sysmon (System Monitor) on a Windows system using the SwiftOnSecurity configuration template and monitor detailed endpoint security events via the Windows Event Viewer.
- **Observation:** Sysmon was configured using the SwiftOnSecurity configuration file. The configuration was validated and updated successfully. Windows Event Viewer was opened to inspect events under Sysmon → Operational. The available events were reviewed to observe recorded system activity, verifying process creation (Event ID 1) and system events.
- **Result:** Microsoft Sysmon was successfully configured and updated with customized XML telemetry rules, and endpoint monitoring was verified through real-time event log inspection in Windows Event Viewer.

---

> For complete step-by-step procedures, command syntax, event ID references, viva questions, and revision notes, refer to [../Windows_Tools.md](../Windows_Tools.md#3-tool-3-sysmon-system-monitor--endpoint-activity-logging).
