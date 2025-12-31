# 04 – Networking Lab

## 📌 Overview
Practiced DNS, DHCP, subnetting, IP troubleshooting, and packet capture.

---

## 🛠️ Tools Used
- DNS Manager
- DHCP Server
- Wireshark
- Command-line tools (ping, tracert, ipconfig)

---

## 🧩 Steps Performed

### 1. Configured DHCP Scope
- Created and activated a DHCP scope.
- Verified successful lease assignment on the client machine.
  
📸 *Screenshot: DHCP scope and lease*

<img width="1026" height="781" alt="Screenshot 20165136" src="https://github.com/user-attachments/assets/2ecc8ddc-7283-4383-b9e3-aa7689feefcd" />
<img width="1043" height="778" alt="Screenshot 2165724" src="https://github.com/user-attachments/assets/66d0db7d-2f0e-476c-97e4-ff437f0156b9" />
<img width="1083" height="974" alt="Screenshot 270120" src="https://github.com/user-attachments/assets/6b2bb328-cd90-4770-9896-6ad840f248cc" />
<img width="1087" height="1079" alt="Screenshot 20270641" src="https://github.com/user-attachments/assets/ad4c7128-8571-4604-a36a-6ffe9815e3ec" />
<img width="959" height="1009" alt="Screenshot 20224308" src="https://github.com/user-attachments/assets/9fa44cb8-824c-4709-b162-944271a35e08" />
<img width="961" height="961" alt="Screenshot 202024343" src="https://github.com/user-attachments/assets/1a5bc5ed-4d28-4b6c-8e30-de92a4f91032" />

---

### 2. Configured DNS Zones
- Created and verified DNS forward lookup zones.
- Confirmed domain name resolution using client tests.
  
📸 *Screenshot: DNS zone creation*

<img width="960" height="1079" alt="Screenshot 202031311" src="https://github.com/user-attachments/assets/ae987da2-337e-4240-b004-9a16ba67177c" />
<img width="959" height="1079" alt="Screenshot 202031723" src="https://github.com/user-attachments/assets/843d44f6-3ada-453d-a2ad-cc19ee5838a9" />

---

### 3. Performed Connectivity Tests
- Used ping and tracert to validate network connectivity.
- Confirmed communication between client and domain controller.
  
📸 *Screenshot: ping/tracert output*

<img width="959" height="779" alt="Screenshot 20232955" src="https://github.com/user-attachments/assets/23242697-13e5-4dc0-867c-d502cf32ce2d" />
<img width="962" height="1079" alt="Screenshot 20233434" src="https://github.com/user-attachments/assets/7046605d-2d13-4a20-94ea-b44d5e773dd1" />

---

### 4. Captured Packets with Wireshark
- Captured and analyzed ICMP and DNS traffic.
- Observed packet flow during ping and name resolution requests.
  
📸 *Screenshot: Wireshark capture*

<img width="1027" height="895" alt="Screenshot 2025-12-31 044449" src="https://github.com/user-attachments/assets/0fcd2968-08db-4366-8519-c6f729a09634" />
<img width="788" height="591" alt="Screenshot 2025-12-31 050413" src="https://github.com/user-attachments/assets/012c1bab-eaca-4305-9ce7-aebb47e44e61" />
<img width="788" height="591" alt="Screenshot 2025-12-31 050413" src="https://github.com/user-attachments/assets/2c7901e7-f4f4-4be7-9d0f-4ac6e8197234" />
<img width="1037" height="783" alt="Screenshot 2025-12-31 054228" src="https://github.com/user-attachments/assets/44bcd675-fd01-46e2-bac6-6e0ec9f6b7f8" />
<img width="1048" height="784" alt="Screenshot 2025-12-31 054550" src="https://github.com/user-attachments/assets/0916554e-b67d-4073-9f33-c998bf776b86" />
<img width="1035" height="779" alt="Screenshot 2025-12-31 054616" src="https://github.com/user-attachments/assets/381ce843-af93-4c19-b9ab-e7cf0fdb347f" />

---

### 5. Troubleshooting Scenarios

A DNS misconfiguration scenario was simulated by isolating the client from the domain DNS server. With the domain-connected adapter disabled, name resolution failed as expected. The issue was identified using ipconfig /all and resolved by restoring connectivity to the domain controller, after which DNS resolution was successfully restored.

- Simulated DNS misconfiguration.
- Identified the issue using command-line tools.
- Restored functionality and verified successful resolution.

📸 *Screenshot: before & after fix*
<img width="1028" height="778" alt="Screenshot 2025-12-31 164817" src="https://github.com/user-attachments/assets/ea3c3493-d192-4c3f-91b7-a5f5082b907e" />
<img width="1051" height="812" alt="Screenshot 2025-12-31 172342" src="https://github.com/user-attachments/assets/95d06832-2ffc-43a8-829f-082062f24c43" />
<img width="1155" height="1079" alt="Screenshot 2025-12-31 172620" src="https://github.com/user-attachments/assets/a0dc4100-3447-48fb-b7d3-e59af2ba4dc0" />
<img width="1154" height="1079" alt="Screenshot 2025-12-31 173448" src="https://github.com/user-attachments/assets/327860d3-a8a2-4fad-8e21-50693f875fb4" />

---

## 📘 What I Learned

- **DNS vs DHCP**
  - DHCP dynamically assigns IP addresses, subnet masks, gateways, and DNS servers to clients.
  - DNS resolves domain names to IP addresses for network communication.
  - Both services must work together for proper domain functionality.

- **Subnetting Fundamentals**
  - Devices must be on the correct subnet to communicate effectively.
  - Subnet masks define network boundaries.
  - Verified subnet configuration using `ipconfig`.

- **Network Troubleshooting Techniques**
  - Used `ping` to test connectivity and DNS resolution.
  - Used `tracert` to analyze routing paths and network hops.
  - Used `ipconfig /all` to inspect IP configuration, DNS servers, and DHCP status.
  - Learned how multiple network adapters can impact name resolution behavior.

- **Packet Analysis Basics**
  - Captured ICMP and DNS traffic using Wireshark.
  - Observed packet flow during connectivity and name resolution tests.
  - Gained hands-on experience interpreting packet-level network activity. 

---

## ❗ Issues & Fixes

- **DNS Resolution Did Not Fail as Expected**
  - Issue:
    - DNS name resolution continued to work even after pointing the client to external DNS servers.
  - Root Cause:
    - The client machine had multiple active network adapters.
    - One adapter was still configured to use the domain controller as its DNS server.
  - Fix:
    - Disabled the domain-connected network adapter on the client machine.
    - Flushed the DNS cache using `ipconfig /flushdns`.
  - Result:
    - Domain name resolution failed as expected.
    - DNS functionality was successfully restored after re-enabling the adapter.

- **DHCP Lease Release Command Failed**
  - Issue:
    - `ipconfig /release` returned an error indicating no adapter was in a permissible state.
  - Root Cause:
    - The active network adapter was not eligible for DHCP release at that moment.
  - Fix:
    - Verified DHCP was enabled on the network adapter.
    - Confirmed the client successfully obtained a valid lease from the DHCP server.
  - Result:
    - Client received a valid IP address and network configuration via DHCP.

- **Intermittent Connectivity and Name Resolution**
  - Issue:
    - Inconsistent connectivity and DNS behavior during testing.
  - Root Cause:
    - Incorrect adapter priority and multiple active network paths.
  - Fix:
    - Reviewed network adapter configurations.
    - Verified correct DNS server and subnet assignments using `ipconfig /all`.
  - Result:
    - Stable connectivity and consistent name resolution were restored.

---

## ✅ Final Outcome
Stronger practical networking skills and troubleshooting experience.
