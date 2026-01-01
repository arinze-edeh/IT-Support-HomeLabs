# 07 – Linux Server Lab

## 📌 Overview
- This lab focused on automating recurring tasks on a Linux server.
- Cron was used to schedule commands to run at fixed time intervals.
- The lab demonstrated how system administrators automate maintenance and monitoring tasks.

---

## 🛠️ Tools Used
- Ubuntu Server 24.04 LTS
- Cron (crond service)
- crontab command
- Nano text editor
- VirtualBox (Lab Environment)

---

## 🧩 Steps Performed

### 1. Installed Ubuntu Server

📸 *Screenshot: installation menu*
<img width="866" height="1079" alt="Screenshot 2026-01-01 065337" src="https://github.com/user-attachments/assets/e85ce5f3-31d4-415a-993e-8786239c37d4" />
<img width="1281" height="924" alt="Screenshot 2026-01-01 065315" src="https://github.com/user-attachments/assets/238827e9-127a-4a07-b2d6-a1a382f69445" />

---

### 2. Enabled SSH Access

📸 *Screenshot: SSH service status*

<img width="1155" height="1079" alt="Screenshot 2026-01-01 070654" src="https://github.com/user-attachments/assets/de23f41b-fba8-4664-a021-ff1161ffd933" />

---

### 3. Installed Apache/Nginx

📸 *Screenshot: Apache service running (local test via curl)*

curl http://localhost output
<img width="1156" height="1079" alt="image" src="https://github.com/user-attachments/assets/918a549b-b90c-4525-868a-356d8fe84349" />


---

### 4. Created Cron Jobs
Created a scheduled cron job that runs every 5 minutes and writes status output to a log file, verifying automated task execution on Ubuntu Server.

📸 *Screenshot: crontab entry*

<img width="1152" height="924" alt="Screenshot 2026-01-01 080338" src="https://github.com/user-attachments/assets/f6267e84-7d1d-47a8-baec-2981c4d1c619" />

<img width="1154" height="1079" alt="image" src="https://github.com/user-attachments/assets/83b08fcf-61a2-47c3-8d96-9e5300634b40" />
<img width="1154" height="1079" alt="image" src="https://github.com/user-attachments/assets/652e7f13-e157-4201-8c81-bb283debde2f" />
<img width="1151" height="1079" alt="image" src="https://github.com/user-attachments/assets/f566dd6b-f90c-4a66-8fae-80ecdd193971" />

---

### 5. Managed Users & Permissions

📸 *Screenshot: commands and output*

<img width="1155" height="1079" alt="image" src="https://github.com/user-attachments/assets/4bb07d84-0749-49c0-a856-073339baa2f9" />
<img width="1059" height="805" alt="image" src="https://github.com/user-attachments/assets/60894319-0773-4e05-bd9b-e461a405b8b6" />


---

## 📘 What I Learned
- How cron works as a task scheduling service in Linux
- Understanding cron timing syntax:
  - minute
  - hour
  - day of month
  - month
  - day of week
- How to create and edit cron jobs using:
  - `crontab -e`
- How to list scheduled cron jobs using:
  - `crontab -l`
- How cron jobs run in the background without user interaction
- How to log cron job output to files for verification

---

## ❗ Issues & Fixes
- ---
- Issue:
  - CTRL + X did not exit the editor
- Fix:
  - Confirmed the editor in use and followed the correct nano exit steps:
    - CTRL + X
    - Press Y to save
    - Press ENTER to confirm filename

- ---
- Issue:
  - Unsure if the cron job was saved correctly
- Fix:
  - Used `crontab -l` to verify the cron job was installed successfully
      
---

## ✅ Final Outcome
- ---
- A cron job was successfully created to run every 5 minutes
- The job appends a message to a log file:
  - `/tmp/cronlog.txt`
- The cron job was verified using `crontab -l`
- Automation concepts relevant to system administration and DevOps were successfully practiced
