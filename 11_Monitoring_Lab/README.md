# Lab 11 - Monitoring with Zabbix

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Tools and Technologies](#tools-and-technologies)
4. [Lab Configuration Steps](#lab-configuration-steps)
   - [Step 1: Install the Monitoring Server](#step-1-install-the-monitoring-server)
   - [Step 2: Add Windows and Linux Agents](#step-2-add-windows-and-linux-agents)
   - [Step 3: Configure Alerts and Triggers](#step-3-configure-alerts-and-triggers)
   - [Step 4: Generate and Validate Test Alerts](#step-4-generate-and-validate-test-alerts)
   - [Step 5: Build a Monitoring Dashboard](#step-5-build-a-monitoring-dashboard)
5. [Key Learnings](#key-learnings)
6. [Issues and Fixes](#issues-and-fixes)
7. [Outcome Summary](#outcome-summary)

---

## Overview

This lab covers the end-to-end setup of a Zabbix monitoring environment. The work included installing and configuring a Zabbix server, onboarding both Windows and Linux hosts, defining triggers for alert generation, validating those alerts through controlled failures, and building a custom dashboard to visualize system health and performance metrics in real time.

---

## Prerequisites

Before starting this lab, the following should be in place:

- A working Ubuntu 24.04 virtual machine to serve as the Zabbix server
- A Windows 10 virtual machine to act as a monitored client
- Network connectivity between the server and both client machines
- Sudo or root access on the Linux host
- Administrator access on the Windows host
- Basic familiarity with Linux package management and Windows firewall settings
- Port 10050 (Zabbix Agent) open on all monitored hosts

---

## Tools and Technologies

| Tool / Technology | Role |
|---|---|
| Zabbix Server | Central monitoring server and alert engine |
| Zabbix Agent (Linux) | Agent installed on Ubuntu host for data collection |
| Zabbix Agent (Windows) | Agent installed on Windows 10 host for data collection |
| Apache | Web server for hosting the Zabbix frontend |
| MariaDB | Database backend for storing Zabbix data |
| Zabbix Web UI | Browser-based interface for configuration and dashboards |
| Ubuntu 24.04 | Operating system for the Zabbix server |
| Windows 10 | Client OS for Windows agent testing |

---

## Lab Configuration Steps

### Step 1: Install the Monitoring Server

Installed the Zabbix server, frontend, and agent packages on Ubuntu 24.04. This involved adding the official Zabbix repository, installing the MySQL/MariaDB backend, configuring the Apache frontend, and enabling the PHP configuration file for Zabbix. All required services (zabbix-server, zabbix-agent, apache2, mariadb) were started and enabled to run on boot. The Zabbix web interface was then accessed and confirmed reachable through the browser.

**Packages installed:** `zabbix-server-mysql`, `zabbix-frontend-php`, `zabbix-apache-conf`, `zabbix-agent`

> Screenshot: Zabbix server packages installed and services running

![Zabbix Install 1](https://github.com/user-attachments/assets/d6ce0865-8fca-4e64-8c26-26fe9682a18e)
![Zabbix Install 2](https://github.com/user-attachments/assets/96e4ed26-365c-439b-92c6-e780aaeb8f96)
![Zabbix Install 3](https://github.com/user-attachments/assets/c398d360-099a-4962-b071-6e8f1e295ebb)
![Zabbix Install 4](https://github.com/user-attachments/assets/9a3af7d2-70e2-4c89-9b3f-d10a0b988079)
![Zabbix Install 5](https://github.com/user-attachments/assets/88fca7b0-48d3-41ea-8614-ccd3dce22491)

---

### Step 2: Add Windows and Linux Agents

Installed and configured the Zabbix Agent on both the Ubuntu server and the Windows 10 client. Each host was then registered inside the Zabbix web interface with the correct IP address and port (10050). OS-appropriate templates were linked to each host to enable automatic item and trigger discovery. Host availability was confirmed by a green status indicator in the Zabbix UI.

> Screenshot: Linux and Windows hosts added and showing green status

![Hosts 1](https://github.com/user-attachments/assets/912a752e-108f-4cb8-9680-e28cf44de28b)
![Hosts 2](https://github.com/user-attachments/assets/1e56503a-ec7f-4653-8947-ce73318d7b93)
![Hosts 3](https://github.com/user-attachments/assets/d840fa00-dddb-4b73-a3b0-8f880a94ac0e)
![Hosts 4](https://github.com/user-attachments/assets/7bc822a3-1bc9-4e76-a85e-9bd082ab97f7)

---

### Step 3: Configure Alerts and Triggers

Leveraged built-in triggers provided by the linked Zabbix templates rather than creating triggers from scratch. Reviewed and confirmed trigger thresholds for key metrics including CPU utilization, memory usage, and agent availability. All triggers were verified to be enabled and active within the template configuration view.

> Screenshot: Triggers active and configured per host

![Triggers 1](https://github.com/user-attachments/assets/0514dca1-06cd-4d30-aba9-24ebe6c0ad55)
![Triggers 2](https://github.com/user-attachments/assets/b0032655-4df6-42f8-9a3a-43fe89941020)

---

### Step 4: Generate and Validate Test Alerts

Observed trigger activation by navigating to Monitoring > Problems in the Zabbix UI. Alert state changes were monitored in real time to confirm that the system correctly detected and reported threshold breaches. This step validated that the full pipeline from agent data collection through to alert surfacing was functioning correctly.

> Screenshot: Triggered problem showing active alert

![Alert](https://github.com/user-attachments/assets/92d62038-c052-4ee6-a46e-640f1450925d)

---

### Step 5: Build a Monitoring Dashboard

Created a custom dashboard within the Zabbix web interface. Widgets were added to display CPU usage, memory consumption, and host availability status. Graphs for each monitored host were embedded into the dashboard to provide a consolidated, real-time view of system health.

> Screenshot: Custom dashboard with graphs and availability widgets

![Dashboard 1](https://github.com/user-attachments/assets/5b1ae4a8-d5f3-4527-b7d0-5a112ca0c1f8)
![Dashboard 2](https://github.com/user-attachments/assets/54cff910-82a8-47ab-ab64-d771a00ddbd2)

---

## Key Learnings

- **Zabbix server deployment:** Understood the relationship between the server, database, web frontend, and agent components and how each must be configured to work together.
- **Agent-based monitoring:** Learned how agents communicate with the server over port 10050 and how host interface configuration directly affects availability status.
- **Templates and triggers:** Gained hands-on experience with how Zabbix templates bundle items, triggers, and graphs, and how linking them to a host activates monitoring instantly.
- **Alert validation:** Understood the importance of testing triggers through controlled failures to confirm that the monitoring pipeline is reliable before relying on it in production.
- **Dashboard design:** Practiced building meaningful dashboards by selecting relevant widgets and giving data time to populate before evaluating results.
- **Firewall and connectivity troubleshooting:** Reinforced how open ports and running services are prerequisites for successful agent communication.

---

## Issues and Fixes

| Issue | Fix Applied |
|---|---|
| Hosts not appearing after creation | Added the correct agent interface with the right IP address and port 10050 |
| Trigger not firing as expected | Verified trigger severity settings and confirmed agent availability was green |
| Windows agent not responding | Opened port 10050 in Windows Firewall and confirmed the Zabbix Agent service was running |
| Dashboard graphs showing empty | Waited for sufficient data collection cycles to complete, then refreshed the dashboard widgets |

---

## Outcome Summary

This lab was completed successfully with all core objectives met:

- Zabbix monitoring server deployed and fully operational on Ubuntu 24.04
- Linux (Ubuntu) and Windows 10 hosts onboarded and monitored with green availability status
- Template-based triggers active and validated through real-time alert observation
- Custom dashboard built and displaying live CPU, memory, and availability metrics
- Troubleshooting experience gained across agent configuration, firewall rules, and dashboard data population
