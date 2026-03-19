# 05 - Windows Troubleshooting Lab

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Tools and Technologies](#tools-and-technologies)
4. [Lab Configuration Steps](#lab-configuration-steps)
5. [Key Learnings](#key-learnings)
6. [Outcome Summary](#outcome-summary)

---

## Overview

This lab simulates real-world Windows operating system issues commonly encountered by IT Support technicians. The objective was to identify, diagnose, and resolve a series of system-level problems using built-in Windows diagnostic and security tools. Each scenario was approached with structured troubleshooting methodology, from initial identification through to verification of the fix.

Scenarios covered in this lab include:

- Startup application failures
- System event log investigation
- Malware scanning and threat verification
- Printer service recovery
- System file corruption detection and repair

---

## Prerequisites

- Windows 10 installed on a virtual machine (client VM)
- Administrator account access (required for running SFC and managing services)
- Basic familiarity with the Windows GUI and Command Prompt
- Virtualization software (e.g., VirtualBox or VMware) to host the client VM

---

## Tools and Technologies

| Tool | Purpose |
|---|---|
| Windows 10 (Client VM) | Primary operating environment for all troubleshooting scenarios |
| Task Manager | Identified and managed disabled startup applications |
| Windows Event Viewer | Reviewed application and system logs to trace errors and warnings |
| Windows Security (Defender) | Performed malware scan to verify system integrity |
| Services Console (`services.msc`) | Inspected and restarted the Print Spooler service |
| Command Prompt (Administrator) | Executed system-level commands with elevated privileges |
| System File Checker (`sfc /scannow`) | Detected and repaired corrupted Windows system files |

---

## Lab Configuration Steps

### Step 1 - Simulated Common Startup Errors

Two startup applications were found disabled in Task Manager, causing them to fail to launch during user login.

**Actions taken:**
- Opened Task Manager and navigated to the Startup tab
- Identified the two disabled application entries
- Right-clicked each entry and selected Enable
- Restarted the system to confirm both applications launched successfully on login

![Disabled startup applications](https://github.com/user-attachments/assets/c330aab9-9d38-4733-ac79-a0e8cfe0bb1e)
![Re-enabled startup applications](https://github.com/user-attachments/assets/ba8a407b-d18f-4998-b092-3c83a0161835)

---

### Step 2 - Checked Event Viewer Logs

Event Viewer was used to review application and system logs, identify warnings, and confirm the absence of critical startup errors.

**Actions taken:**
- Opened Event Viewer via the Start menu
- Navigated to Windows Logs > Application and Windows Logs > System
- Reviewed recent warning and error entries
- Confirmed no critical startup failures were present, establishing that the earlier startup issue was configuration-related rather than a system fault

![Event Viewer application logs](https://github.com/user-attachments/assets/7c79fad1-88da-4723-b49d-f78d4e42e712)
![Event Viewer system log](https://github.com/user-attachments/assets/dd6679c2-a8a4-47f3-a9c9-1fd8507585d9)
![Event log details](https://github.com/user-attachments/assets/715cf654-9735-4c16-879b-e73aefa4a9be)
![Event log warning entry](https://github.com/user-attachments/assets/8fec11c6-384c-47a9-9391-5b4845998bf1)

---

### Step 3 - Performed Malware Cleanup

A full malware scan was run using Windows Security (Windows Defender) to verify that no threats were present on the system.

**Actions taken:**
- Opened Windows Security from the Start menu
- Navigated to Virus and Threat Protection
- Initiated a full system scan
- Reviewed scan results confirming zero threats detected

![Windows Security scan initiated](https://github.com/user-attachments/assets/d89034a0-9923-4248-8dca-e95f898b5ea2)
![Scan in progress](https://github.com/user-attachments/assets/b6a1eb47-62f4-4d83-8601-0a9c716f7cb7)
![Scan results: no threats found](https://github.com/user-attachments/assets/35b60814-77da-43ac-bc3f-4cc32107ce75)

---

### Step 4 - Printer Troubleshooting

Printer discovery and print queue issues were investigated. The root cause was identified as a stalled Print Spooler service.

**Actions taken:**
- Checked the Printers and Scanners settings to confirm the printer was not responding
- Opened the Services Console (`services.msc`)
- Located the Print Spooler service
- Right-clicked and selected Restart to clear the stalled state
- Returned to printer settings and confirmed printer discovery and queue were restored

![Printer settings review](https://github.com/user-attachments/assets/91d971b4-bb23-40a3-bd11-c891d08a16b2)
![Services Console open](https://github.com/user-attachments/assets/249797a4-5dde-45ec-82c4-4967f6cca5ee)
![Print Spooler restarted](https://github.com/user-attachments/assets/ec019e72-dfa2-4821-b20d-e14b3abf1268)
![Printer restored](https://github.com/user-attachments/assets/b076fe28-95ef-4780-8df3-2110d4c5e4b2)

---

### Step 5 - Restored System via SFC Scan

System File Checker (SFC) was run to verify and repair Windows system file integrity.

**Actions taken:**
- Opened Command Prompt as Administrator
- Ran the command: `sfc /scannow`
- Observed that Windows Resource Protection detected corrupted system files
- Allowed the repair process to complete
- Confirmed successful repair from the scan output
- Restarted the system and verified stable operation

![SFC scan command executed](https://github.com/user-attachments/assets/8d7e23bd-c566-4319-82db-fcaceb307062)
![SFC scan in progress](https://github.com/user-attachments/assets/167965a6-629a-4586-9c96-421a3505fc7b)
![SFC repair complete](https://github.com/user-attachments/assets/09aa1844-172b-45a2-84a1-aaf19f90053e)

---

## Key Learnings

- **Startup failures can be silent.** Disabled startup entries do not always generate visible error messages, making Task Manager's Startup tab an essential first stop during login-related troubleshooting.
- **Event Viewer distinguishes configuration issues from system failures.** Reviewing log severity levels (Information, Warning, Error, Critical) allows a technician to quickly scope the nature of a problem without guesswork.
- **Built-in security tools are sufficient for baseline threat assessment.** Windows Defender provides a reliable first-pass scan that is immediately accessible without third-party software.
- **Service restarts resolve many peripheral device issues.** Printer problems are frequently caused by a stalled Print Spooler rather than hardware or driver faults. Restarting the service is a fast, non-destructive first response.
- **SFC should be run with administrator privileges and followed by a reboot.** Running `sfc /scannow` without elevated rights produces no actionable output. Rebooting after repair ensures repaired files are fully loaded by the OS.
- **Verifying the fix matters as much as applying it.** Confirming system stability post-repair prevents incomplete resolutions from being marked as closed.

---

## Outcome Summary

All five troubleshooting scenarios were successfully diagnosed and resolved within the lab environment:

| Scenario | Root Cause | Resolution |
|---|---|---|
| Startup apps not launching | Entries disabled in Task Manager | Re-enabled via Task Manager Startup tab |
| No visible startup error messages | Configuration issue, not a crash | Confirmed via Event Viewer log review |
| Potential malware concern | Unverified threat status | Full scan run; system confirmed clean |
| Printer not responding | Print Spooler service stalled | Service restarted via Services Console |
| System instability | Corrupted Windows system files | Repaired using `sfc /scannow` |

This lab demonstrated practical, hands-on proficiency with core Windows diagnostic tools and reinforced a structured approach to IT troubleshooting. The skills exercised here are directly applicable to helpdesk, desktop support, and junior system administrator roles.
