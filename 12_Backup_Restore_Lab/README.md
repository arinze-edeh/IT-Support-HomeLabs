# Lab 12 - Backup and Restore Lab

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Tools and Technologies](#tools-and-technologies)
- [Lab Configuration Steps](#lab-configuration-steps)
- [Key Learnings](#key-learnings)
- [Issues and Fixes](#issues-and-fixes)
- [Outcome Summary](#outcome-summary)

---

## Overview

This lab covers full backup and recovery testing across OS, file, and virtual machine layers. System resilience was validated using restore points, file backups, and VM snapshots, with successful recovery confirmed after intentional data loss.

- Performed full backup and recovery testing across OS, files, and virtual machine layers
- Validated system resilience using restore points, file backups, and VM snapshots
- Confirmed successful recovery after intentional data loss

---

## Prerequisites

- Windows OS with System Protection enabled
- Oracle VirtualBox installed with a Windows 10 virtual machine configured
- A secondary drive or location available for File History backups
- Basic familiarity with Windows system settings and VirtualBox snapshot management

---

## Tools and Technologies

| Tool | Purpose |
|---|---|
| Windows Backup | Full system and file backup |
| Windows System Restore | OS configuration rollback via restore points |
| File History | Granular file-level backup and recovery |
| Oracle VirtualBox | Hypervisor-level VM snapshots and restoration |

---

## Lab Configuration Steps

### 1. Created System Restore Point

- Opened System Properties
- Enabled system protection for the OS drive
- Created a manual restore point before making system changes
- Verified restore point creation

Screenshot: Restore point created successfully

<img width="1146" height="1079" alt="image" src="https://github.com/user-attachments/assets/d466c055-f05d-45b6-84df-813f0a1a1d79" />

---

### 2. Backed Up Important Files

- Enabled File History
- Selected target backup drive/location
- Ran manual backup of user documents
- Confirmed files were successfully backed up

Screenshot: File backup results

<img width="1152" height="1079" alt="image" src="https://github.com/user-attachments/assets/909aabaf-bdcc-4e60-ae62-366477049943" />

---

### 3. Took VM Snapshot

- Powered off the Windows 10 virtual machine
- Created a snapshot named `Lab12_PreRestore_Snapshot`
- Snapshot captured OS state, disk state, and VM configuration
- Confirmed snapshot appeared in the snapshot tree

Screenshot: VM snapshot screen

<img width="1263" height="1079" alt="image" src="https://github.com/user-attachments/assets/795ed0fa-ee5d-4655-8e03-c22d267a517b" />

---

### 4. Performed a Restore

- Deleted a test file inside the virtual machine
- Powered off the VM
- Restored the VM to `Lab12_PreRestore_Snapshot`
- VirtualBox rollback completed successfully

Screenshots: Snapshot restore process

<img width="1279" height="1079" alt="image" src="https://github.com/user-attachments/assets/5f33549a-3333-4792-a47f-d80a10196a75" />
<img width="1146" height="1079" alt="image" src="https://github.com/user-attachments/assets/e256af73-df0f-453a-b09b-a59256fffacd" />
<img width="1153" height="1079" alt="image" src="https://github.com/user-attachments/assets/f6eb01ae-50eb-4bb6-a7e5-67c49ace6388" />
<img width="1151" height="1079" alt="image" src="https://github.com/user-attachments/assets/72b25079-dd35-48db-a4a5-9caa3b670dce" />

---

### 5. Validated Recovery

- Booted the restored virtual machine
- Verified the deleted file was recovered
- Confirmed Windows booted without errors
- System state matched the pre-restore condition

Screenshots: Final restored system

<img width="1151" height="1079" alt="image" src="https://github.com/user-attachments/assets/62c8a365-5a4c-4696-bb6a-0cefc36bb42f" />
<img width="1152" height="1079" alt="image" src="https://github.com/user-attachments/assets/4693d029-bc2a-4bc7-a9db-a676e4c63454" />

---

## Key Learnings

- Backups must be tested, not just created -- an untested backup is an unreliable one
- System Restore protects OS configuration but does not cover personal files
- File History enables granular file-level recovery independent of system state
- Hypervisor snapshots provide fast and reliable full-system rollback
- VirtualBox confirms restore success through the snapshot tree and recovered files, not pop-up notifications

---

## Issues and Fixes

**Restore button appeared disabled**
- Cause: The VM had not diverged from the snapshot state
- Fix: Booted the VM, made changes, powered off, then restored the snapshot

**No confirmation message after restore**
- Cause: VirtualBox does not display success notifications
- Fix: Verified restore success via the snapshot tree and confirmed recovery of the deleted file

---

## Outcome Summary

| Objective | Status |
|---|---|
| System restore point created | Completed |
| Important files backed up and recovered | Completed |
| Virtual machine snapshot created | Completed |
| VM snapshot restored successfully | Completed |
| Deleted file fully recovered | Completed |
| Windows OS booted successfully after restoration | Completed |
| All lab objectives validated | Completed |
