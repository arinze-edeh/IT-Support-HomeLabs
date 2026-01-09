# 11 – Monitoring Lab

## 📌 Overview

In this lab, I installed and configured Zabbix monitoring server, onboarded Windows and Linux hosts for monitoring, configured  triggers and validated alert generation, and built a basic monitoring dashboard to visualize system health and performance.

---

## 🛠️ Tools Used

- Zabbix Server (Ubuntu 24.04)
- Zabbix Agent (Linux)
- Zabbix Agent (Windows 10)
- Apache / MariaDB
- Web Browser (Zabbix UI)

---

## 🧩 Steps Performed

### 1. Installed Monitoring Server

- Installed Zabbix server, frontend, and database
- Started and enabled required services
- Accessed Zabbix web interface successfully

📸 *Screenshot: dashboard*

Installed Zabbix Server Packages (mysql, frontend php, apache conf, agent)

<img width="1153" height="1079" alt="image" src="https://github.com/user-attachments/assets/d6ce0865-8fca-4e64-8c26-26fe9682a18e" />

<img width="1156" height="1079" alt="image" src="https://github.com/user-attachments/assets/96e4ed26-365c-439b-92c6-e780aaeb8f96" />

<img width="1136" height="1074" alt="image" src="https://github.com/user-attachments/assets/c398d360-099a-4962-b071-6e8f1e295ebb" />
<img width="1148" height="1076" alt="image" src="https://github.com/user-attachments/assets/9a3af7d2-70e2-4c89-9b3f-d10a0b988079" />
<img width="1151" height="1079" alt="image" src="https://github.com/user-attachments/assets/88fca7b0-48d3-41ea-8614-ccd3dce22491" />


---

### 2. Added Windows & Linux Agents

- Installed Zabbix Agent on Ubuntu server
- Installed Zabbix Agent on Windows 10 client
- Created hosts in Zabbix UI
- Linked appropriate OS templates
- Verified host availability (green status)
  
📸 *Screenshot: Linux and Windows hosts added*

<img width="1154" height="1079" alt="image" src="https://github.com/user-attachments/assets/912a752e-108f-4cb8-9680-e28cf44de28b" />
<img width="1154" height="1079" alt="image" src="https://github.com/user-attachments/assets/1e56503a-ec7f-4653-8947-ce73318d7b93" />
<img width="1151" height="1079" alt="image" src="https://github.com/user-attachments/assets/d840fa00-dddb-4b73-a3b0-8f880a94ac0e" />

<img width="1152" height="1079" alt="image" src="https://github.com/user-attachments/assets/7bc822a3-1bc9-4e76-a85e-9bd082ab97f7" />


---

### 3. Configured Alerts

- Used existing Zabbix triggers from linked templates
- Verified trigger thresholds (CPU, memory, agent status)
- Ensured triggers were enabled and active
  
📸 *Screenshot: alert setup*

<img width="1154" height="1072" alt="image" src="https://github.com/user-attachments/assets/0514dca1-06cd-4d30-aba9-24ebe6c0ad55" />
<img width="1153" height="1079" alt="image" src="https://github.com/user-attachments/assets/b0032655-4df6-42f8-9a3a-43fe89941020" />

---

### 4. Generated Test Alerts

- Observed trigger activation in Monitoring → Problems
- Confirmed alert state changes in real time
  
📸 *Screenshot: triggered problem showing active alert*

<img width="1152" height="1079" alt="image" src="https://github.com/user-attachments/assets/92d62038-c052-4ee6-a46e-640f1450925d" />

---

### 5. Created Monitoring Dashboard

- Created custom dashboard in Zabbix UI
- Added widgets for CPU, memory, and availability
- Displayed graphs for monitored hosts
  
📸 *Screenshot: Dashboard with graphs and widgets*

<img width="1153" height="1079" alt="image" src="https://github.com/user-attachments/assets/5b1ae4a8-d5f3-4527-b7d0-5a112ca0c1f8" />

<img width="1149" height="1079" alt="image" src="https://github.com/user-attachments/assets/54cff910-82a8-47ab-ab64-d771a00ddbd2" />



---

## 📘 What I Learned

- How to deploy and configure Zabbix server on Linux
- How to add and monitor Windows and Linux hosts
- How Zabbix templates and triggers work
- How to validate alerts using controlled failures
- How to visualize system metrics using dashboards  

---

## ❗ Issues & Fixes

- Issue: Hosts not appearing after creation
  - Fix: Added correct agent interface (IP + port 10050)

- Issue: Trigger not firing
  - Fix: Verified trigger severity and agent availability

- Issue: Windows agent not responding
  - Fix: Opened port 10050 and confirmed agent service running

- Issue: Dashboard graphs empty
  - Fix: Allowed time for data collection and refreshed widgets

---

## ✅ Final Outcome

- Zabbix monitoring server fully operational
- Linux and Windows systems monitored successfully
- Alerts triggered and validated
- Dashboard displaying real-time metrics
- Monitoring lab completed successfully
