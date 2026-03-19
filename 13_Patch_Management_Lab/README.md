# Lab 13 - Patch Management Lab

---

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Tools and Technologies](#tools-and-technologies)
4. [Lab Configuration Steps](#lab-configuration-steps)
5. [Key Learnings](#key-learnings)
6. [Outcome Summary](#outcome-summary)

---

## Overview

This lab simulates a real-world patch management workflow covering update deployment, post-patch validation, and rollback procedures across both Windows and Linux environments. The goal is to understand how organizations maintain system integrity and security through structured, repeatable patching processes.

---

## Prerequisites

- A Windows machine (physical or virtual) with access to Windows Update and Control Panel
- A Linux machine or VM with `apt` package manager available
- Basic familiarity with system administration on both platforms
- Administrator or sudo privileges on both systems

---

## Tools and Technologies

| Tool | Platform | Purpose |
|---|---|---|
| Windows Update | Windows | Check and deploy system updates |
| Windows Installed Updates | Windows | Review update history and perform rollbacks |
| WSUS (Concepts) | Windows | Centralized enterprise patch management |
| apt | Linux | Package manager for update deployment |

---

## Lab Configuration Steps

### Step 1 - Checked for System Updates

- Navigated to Windows Update settings
- Reviewed update availability and current system status
- Noted an end-of-support warning for the operating system

📸 *Screenshot: System checked for updates - End-of-support warning displayed*
<img width="1153" height="1079" alt="image" src="https://github.com/user-attachments/assets/b549e486-143b-4193-aa96-065f9e0ae4c3" />

📸 *Screenshot: Terminal showing available updates*
<img width="1154" height="1079" alt="Screenshot 2026-01-10 051019" src="https://github.com/user-attachments/assets/65174a04-ef76-4d2d-8b47-e64a36f28574" />

---

### Step 2 - Deployed Updates

- Reviewed previously installed Windows updates
- Confirmed updates were successfully applied to the system
- Ran `apt` commands on Linux to install available packages

📸 *Screenshot: Update history reviewed - no new updates available*
<img width="1154" height="1079" alt="image" src="https://github.com/user-attachments/assets/70615b76-c26d-4ed9-b2e7-e73bd4d31dea" />

📸 *Screenshot: Updates installing in terminal*
<img width="1153" height="1079" alt="image" src="https://github.com/user-attachments/assets/40bfa7ea-35e1-4d7f-b3d0-e4fc7b1a16ae" />
<img width="1157" height="1077" alt="image" src="https://github.com/user-attachments/assets/d5fb49a2-da22-4175-be7d-a7bf1d95ac56" />
<img width="1151" height="1079" alt="image" src="https://github.com/user-attachments/assets/3a4fd38b-d8dd-4a42-85b4-a235719a99a1" />

---

### Step 3 - Validated After Update

- Confirmed the system booted successfully following the update
- Verified core system functionality was intact after patch installation
- Ensured no critical services were disrupted

📸 *Screenshot: System operational after update check*
<img width="1154" height="1079" alt="image" src="https://github.com/user-attachments/assets/00cc0bad-0bb1-4e18-ad50-1f99c5b170fe" />
<img width="1153" height="1079" alt="image" src="https://github.com/user-attachments/assets/7b851ff8-e7e1-464a-85ca-ff5299d8fb4d" />
<img width="1151" height="1079" alt="image" src="https://github.com/user-attachments/assets/be52a2d1-156e-4816-b968-f9262ea0f579" />

---

### Step 4 - Simulated Bad Update and Rollback

- Accessed Control Panel and navigated to Installed Updates
- Verified the ability to uninstall a problematic update
- Confirmed the rollback mechanism was available and functional
- Noted that certain updates (such as servicing stacks) are non-removable by design

📸 *Screenshot: Installed Windows updates reviewed and rollback (uninstall) option verified*
<img width="1153" height="1079" alt="image" src="https://github.com/user-attachments/assets/6da80c48-2283-44b9-ac38-2c39bb8745a8" />

---

### Step 5 - Applied Best Practices

- Reviewed industry-standard patch management best practices
- Emphasized the value of pre-deployment testing, post-deployment validation, and maintaining rollback readiness
- Documented update management procedures for future reference

📸 *Screenshot: Patch management documentation*
<img width="1151" height="1079" alt="image" src="https://github.com/user-attachments/assets/8f69042c-1074-4520-bd6d-471ca2cd996c" />

---

## Key Learnings

- **Pre-deployment testing** prevents untested patches from destabilizing production systems
- **Rollback options** are essential for minimizing downtime when a patch causes issues
- **Post-patch validation** ensures system integrity is maintained after every update cycle
- **Lifecycle management** matters: end-of-support operating systems require documented exceptions and migration planning
- **WSUS and centralized patching** allow enterprises to control update rollout timing, scope, and approval across fleets of machines
- **Not all updates are reversible**: servicing stacks and certain cumulative updates cannot be uninstalled, making pre-patch snapshots or backups critical in enterprise environments

---

## Outcome Summary

Successfully simulated a full patch management lifecycle including update discovery, deployment, post-patch validation, and rollback verification on both Windows and Linux platforms. Applied and documented industry-standard best practices for patch testing, approval workflows, and rollback readiness. Identified real-world constraints such as non-removable servicing stack updates and end-of-support OS lifecycle implications
