# 07 - Linux Server Lab

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Tools and Technologies](#tools-and-technologies)
4. [Lab Configuration Steps](#lab-configuration-steps)
5. [Key Learnings](#key-learnings)
6. [Issues and Fixes](#issues-and-fixes)
7. [Outcome Summary](#outcome-summary)

---

## Overview

This lab focused on automating recurring tasks on a Linux server. Cron was used to schedule commands to run at fixed time intervals, demonstrating how system administrators automate maintenance and monitoring tasks. The lab also covered foundational server setup tasks including SSH access, web server installation, and user and permission management.

---

## Prerequisites

- Basic familiarity with the Linux command line
- VirtualBox installed on the host machine
- Ubuntu Server 24.04 LTS ISO image
- Internet access for package installation

---

## Tools and Technologies

| Tool / Technology | Purpose |
|---|---|
| Ubuntu Server 24.04 LTS | Lab operating system |
| VirtualBox | Virtualized lab environment |
| Cron (crond service) | Task scheduling service |
| crontab | Command for managing cron jobs |
| Apache / Nginx | Web server for local testing |
| OpenSSH | Remote access via SSH |
| Nano | Terminal text editor |

---

## Lab Configuration Steps

### 1. Installed Ubuntu Server

Ubuntu Server 24.04 LTS was installed inside a VirtualBox virtual machine using the standard installation wizard.

Screenshot: installation menu

<img width="866" height="1079" alt="Screenshot 2026-01-01 065337" src="https://github.com/user-attachments/assets/e85ce5f3-31d4-415a-993e-8786239c37d4" />
<img width="1281" height="924" alt="Screenshot 2026-01-01 065315" src="https://github.com/user-attachments/assets/238827e9-127a-4a07-b2d6-a1a382f69445" />

---

### 2. Enabled SSH Access

The SSH service was enabled and verified to allow remote terminal access to the server.

Screenshot: SSH service status

<img width="1155" height="1079" alt="Screenshot 2026-01-01 070654" src="https://github.com/user-attachments/assets/de23f41b-fba8-4664-a021-ff1161ffd933" />

---

### 3. Installed Apache / Nginx

A web server (Apache or Nginx) was installed and tested locally using `curl` to confirm it was serving content correctly.

Screenshot: Apache service running (local test via curl)

```
curl http://localhost
```

<img width="1156" height="1079" alt="image" src="https://github.com/user-attachments/assets/918a549b-b90c-4525-868a-356d8fe84349" />

---

### 4. Created Cron Jobs

A scheduled cron job was created to run every 5 minutes. The job writes a status message to a log file at `/tmp/cronlog.txt`, verifying that automated task execution works correctly on Ubuntu Server.

Screenshot: crontab entry and log output

<img width="1152" height="924" alt="Screenshot 2026-01-01 080338" src="https://github.com/user-attachments/assets/f6267e84-7d1d-47a8-baec-2981c4d1c619" />
<img width="1154" height="1079" alt="image" src="https://github.com/user-attachments/assets/83b08fcf-61a2-47c3-8d96-9e5300634b40" />
<img width="1154" height="1079" alt="image" src="https://github.com/user-attachments/assets/652e7f13-e157-4201-8c81-bb283debde2f" />
<img width="1151" height="1079" alt="image" src="https://github.com/user-attachments/assets/f566dd6b-f90c-4a66-8fae-80ecdd193971" />

**Cron timing syntax used:**

```
*/5 * * * * echo "Cron running" >> /tmp/cronlog.txt
```

Fields in order: `minute | hour | day of month | month | day of week`

---

### 5. Managed Users and Permissions

User accounts and file permissions were created and configured to practice standard Linux access control.

Screenshot: commands and output

<img width="1155" height="1079" alt="image" src="https://github.com/user-attachments/assets/4bb07d84-0749-49c0-a856-073339baa2f9" />
<img width="1059" height="805" alt="image" src="https://github.com/user-attachments/assets/60894319-0773-4e05-bd9b-e461a405b8b6" />

---

## Key Learnings

- How cron functions as a background task scheduling service in Linux
- Understanding cron timing syntax and its five fields: minute, hour, day of month, month, and day of week
- How to create and edit cron jobs using `crontab -e`
- How to list all scheduled cron jobs using `crontab -l`
- How cron jobs execute in the background without any user interaction
- How to redirect and append cron job output to log files for verification
- Correct usage of the Nano editor, including the save and exit sequence: CTRL+X, then Y, then ENTER

---

## Issues and Fixes

**Issue:** CTRL+X did not appear to exit the Nano editor as expected.

**Fix:** Confirmed Nano was the active editor and followed the correct exit steps: press CTRL+X, then press Y to confirm saving, then press ENTER to confirm the filename.

---

**Issue:** Uncertainty about whether the cron job had been saved correctly.

**Fix:** Ran `crontab -l` to list all active cron jobs and confirmed the entry was present and correctly formatted.

---

## Outcome Summary

- A cron job was successfully created to run automatically every 5 minutes
- The job appends a timestamped status message to `/tmp/cronlog.txt`
- The cron job was verified as active using `crontab -l`
- The lab successfully reinforced automation concepts directly applicable to system administration and DevOps workflows
