# 05 – Windows Troubleshooting Lab

## 📌 Overview
Simulated common Windows system issues and performed real troubleshooting.

---

## 🛠️ Tools Used
- Windows Event Viewer
- Task Manager
- Sysinternals
- Malware removal tools

---

## 🧩 Steps Performed

### 1. Simulated Common Errors
Two startup applications were found disabled in Task Manager, causing them to fail to launch during user login. The issue was identified by checking startup settings and resolved by re-enabling the applications.

📸 *Screenshot: Disabled/Enabled startup applications*

<img width="1153" height="1079" alt="Screenshot 2025-12-31 211318" src="https://github.com/user-attachments/assets/c330aab9-9d38-4733-ac79-a0e8cfe0bb1e" />

<img width="1030" height="896" alt="Screenshot 2025-12-31 213130" src="https://github.com/user-attachments/assets/ba8a407b-d18f-4998-b092-3c83a0161835" />


---

### 2. Checked Event Viewer Logs
Event Viewer was used to review application logs and identify warnings and system events. No critical startup errors were observed, confirming that the issue was configuration-related rather than system failure.

📸 *Screenshot: event logs*

<img width="1153" height="1079" alt="Screenshot 2025-12-31 220714" src="https://github.com/user-attachments/assets/7c79fad1-88da-4723-b49d-f78d4e42e712" />
<img width="1154" height="1079" alt="Screenshot 2025-12-31 220806" src="https://github.com/user-attachments/assets/dd6679c2-a8a4-47f3-a9c9-1fd8507585d9" />
<img width="1149" height="1079" alt="Screenshot 2025-12-31 220841" src="https://github.com/user-attachments/assets/715cf654-9735-4c16-879b-e73aefa4a9be" />
<img width="1153" height="1079" alt="Screenshot 2025-12-31 220931" src="https://github.com/user-attachments/assets/8fec11c6-384c-47a9-9391-5b4845998bf1" />


---

### 3. Performed Malware Cleanup
A malware scan was performed using Windows Security. No threats were detected, confirming system integrity.

📸 *Screenshot: scan results*

<img width="1151" height="1079" alt="Screenshot 2025-12-31 222708" src="https://github.com/user-attachments/assets/d89034a0-9923-4248-8dca-e95f898b5ea2" />
<img width="1150" height="1079" alt="Screenshot 2025-12-31 222836" src="https://github.com/user-attachments/assets/b6a1eb47-62f4-4d83-8601-0a9c716f7cb7" />
<img width="1151" height="1079" alt="Screenshot 2025-12-31 223029" src="https://github.com/user-attachments/assets/35b60814-77da-43ac-bc3f-4cc32107ce75" />

---

### 4. Printer Troubleshooting
Printer issues were investigated by checking printer discovery and print queues. The Print Spooler service was restarted to restore printer functionality.

📸 *Screenshot: printer queue fix*

<img width="1151" height="1079" alt="Screenshot 2025-12-31 225941" src="https://github.com/user-attachments/assets/91d971b4-bb23-40a3-bd11-c891d08a16b2" />
<img width="1146" height="1079" alt="Screenshot 2025-12-31 230031" src="https://github.com/user-attachments/assets/249797a4-5dde-45ec-82c4-4967f6cca5ee" />
<img width="1144" height="1079" alt="Screenshot 2025-12-31 230147" src="https://github.com/user-attachments/assets/ec019e72-dfa2-4821-b20d-e14b3abf1268" />
<img width="1152" height="1079" alt="Screenshot 2025-12-31 230321" src="https://github.com/user-attachments/assets/b076fe28-95ef-4780-8df3-2110d4c5e4b2" />

---

### 5. Restored System / Applied Fix
System integrity was verified using System File Checker (SFC). System integrity issues were detected during the System File Checker (SFC) scan. Windows Resource Protection successfully repaired the corrupted system files, restoring system stability.

📸 *Screenshot: final working state*

<img width="1150" height="1079" alt="Screenshot 2025-12-31 235945" src="https://github.com/user-attachments/assets/8d7e23bd-c566-4319-82db-fcaceb307062" />
<img width="1152" height="1078" alt="Screenshot 2026-01-01 000356" src="https://github.com/user-attachments/assets/167965a6-629a-4586-9c96-421a3505fc7b" />
<img width="1155" height="1076" alt="Screenshot 2026-01-01 000909" src="https://github.com/user-attachments/assets/09aa1844-172b-45a2-84a1-aaf19f90053e" />


---

## 📘 What I Learned
- How to diagnose Windows issues  
- How to repair OS without reinstall  
- How logs and tools help identify root cause  

---

## ❗ Issues & Fixes
[Add notes]

---

## ✅ Final Outcome
Able to systematically troubleshoot Windows problems end-to-end.
