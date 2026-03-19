# 04 - Networking Lab: DHCP, DNS & Traffic Analysis

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Tools and Technologies](#tools-and-technologies)
4. [Lab Configuration Steps](#lab-configuration-steps)
   - [1. Configure DHCP Scope](#1-configure-dhcp-scope)
   - [2. Configure DNS Zones](#2-configure-dns-zones)
   - [3. Perform Connectivity Tests](#3-perform-connectivity-tests)
   - [4. Capture Packets with Wireshark](#4-capture-packets-with-wireshark)
   - [5. Troubleshooting Scenarios](#5-troubleshooting-scenarios)
5. [Key Learnings](#key-learnings)
6. [Issues & Fixes](#issues--fixes)
7. [Outcome Summary](#outcome-summary)

---

## Overview

This lab focused on building and troubleshooting core networking services within a Windows domain environment. The objective was to configure DHCP and DNS, verify client connectivity, analyze network traffic, and simulate real-world troubleshooting scenarios. Through hands-on testing, common networking issues were identified and resolved using industry-standard tools and techniques.

---

## Prerequisites

Before starting this lab, ensure the following are in place:

- A working **Windows Server** instance with Active Directory Domain Services (AD DS) installed
- A **Windows 10 client machine** joined to or reachable on the same network as the server
- **VirtualBox** (or equivalent hypervisor) configured with at least two VMs on an internal/host-only network
- DHCP and DNS server roles installed on Windows Server
- Basic familiarity with Windows Server Manager and command-line tools
- **Wireshark** installed on the client or server machine for packet capture

---

## Tools and Technologies

| Category | Tool / Technology |
|---|---|
| Server Roles | DHCP Server, DNS Server |
| Server Management | DHCP Management Console, DNS Manager |
| Client Diagnostics | `ipconfig`, `ping`, `tracert`, `nbtstat` |
| Packet Analysis | Wireshark |
| Virtualization | VirtualBox |
| Operating Systems | Windows Server, Windows 10 |

---

## Lab Configuration Steps

### 1. Configure DHCP Scope

Created and activated a DHCP scope on Windows Server, then verified that the client machine successfully obtained a dynamic IP lease.

- Opened **DHCP Management Console** and created a new IPv4 scope
- Defined the IP address range, subnet mask, default gateway, and DNS server options
- Activated the scope and confirmed lease assignment on the client using `ipconfig /all`

📸 *Screenshot: DHCP scope and lease*

<img width="1026" height="781" alt="Screenshot 20165136" src="https://github.com/user-attachments/assets/2ecc8ddc-7283-4383-b9e3-aa7689feefcd" />
<img width="1043" height="778" alt="Screenshot 2165724" src="https://github.com/user-attachments/assets/66d0db7d-2f0e-476c-97e4-ff437f0156b9" />
<img width="1083" height="974" alt="Screenshot 270120" src="https://github.com/user-attachments/assets/6b2bb328-cd90-4770-9896-6ad840f248cc" />
<img width="1087" height="1079" alt="Screenshot 20270641" src="https://github.com/user-attachments/assets/ad4c7128-8571-4604-a36a-6ffe9815e3ec" />
<img width="959" height="1009" alt="Screenshot 20224308" src="https://github.com/user-attachments/assets/9fa44cb8-824c-4709-b162-944271a35e08" />
<img width="961" height="961" alt="Screenshot 202024343" src="https://github.com/user-attachments/assets/1a5bc5ed-4d28-4b6c-8e30-de92a4f91032" />

---

### 2. Configure DNS Zones

Created a forward lookup zone in DNS Manager and verified that the client machine could resolve domain names correctly.

- Opened **DNS Manager** and created a new primary forward lookup zone
- Added host (A) records pointing to the domain controller
- Confirmed name resolution from the client using `ping <hostname>` and `nslookup`

📸 *Screenshot: DNS zone creation*

<img width="960" height="1079" alt="Screenshot 202031311" src="https://github.com/user-attachments/assets/ae987da2-337e-4240-b004-9a16ba67177c" />
<img width="959" height="1079" alt="Screenshot 202031723" src="https://github.com/user-attachments/assets/843d44f6-3ada-453d-a2ad-cc19ee5838a9" />

---

### 3. Perform Connectivity Tests

Used command-line tools to validate end-to-end communication between the client and the domain controller.

- Ran `ping <domain controller IP>` to confirm basic reachability
- Ran `tracert <hostname>` to verify the routing path between machines
- Confirmed all hops resolved within the expected virtual network

📸 *Screenshot: ping/tracert output*

<img width="959" height="779" alt="Screenshot 20232955" src="https://github.com/user-attachments/assets/23242697-13e5-4dc0-867c-d502cf32ce2d" />
<img width="962" height="1079" alt="Screenshot 20233434" src="https://github.com/user-attachments/assets/7046605d-2d13-4a20-94ea-b44d5e773dd1" />

---

### 4. Capture Packets with Wireshark

Captured and analyzed live network traffic to observe ICMP and DNS packet behavior during connectivity and name resolution tests.

- Started a Wireshark capture on the active network interface
- Applied display filters (`icmp`, `dns`) to isolate relevant traffic
- Observed DNS query/response pairs and ICMP echo request/reply sequences
- Analyzed packet details including source/destination IPs, TTL values, and response times

📸 *Screenshot: Wireshark capture*

<img width="1027" height="895" alt="Screenshot 2025-12-31 044449" src="https://github.com/user-attachments/assets/0fcd2968-08db-4366-8519-c6f729a09634" />
<img width="788" height="591" alt="Screenshot 2025-12-31 050413" src="https://github.com/user-attachments/assets/012c1bab-eaca-4305-9ce7-aebb47e44e61" />
<img width="788" height="591" alt="Screenshot 2025-12-31 050413" src="https://github.com/user-attachments/assets/2c7901e7-f4f4-4be7-9d0f-4ac6e8197234" />
<img width="1037" height="783" alt="Screenshot 2025-12-31 054228" src="https://github.com/user-attachments/assets/44bcd675-fd01-46e2-bac6-6e0ec9f6b7f8" />
<img width="1048" height="784" alt="Screenshot 2025-12-31 054550" src="https://github.com/user-attachments/assets/0916554e-b67d-4073-9f33-c998bf776b86" />
<img width="1035" height="779" alt="Screenshot 2025-12-31 054616" src="https://github.com/user-attachments/assets/381ce843-af93-4c19-b9ab-e7cf0fdb347f" />

---

### 5. Troubleshooting Scenarios

Simulated a DNS misconfiguration by isolating the client from the domain DNS server. With the domain-connected adapter disabled, name resolution failed as expected. The root cause was identified using `ipconfig /all` and resolved by restoring connectivity to the domain controller.

- Pointed the client to an external DNS server to simulate misconfiguration
- Disabled the domain-connected network adapter to fully isolate DNS resolution
- Flushed the DNS cache with `ipconfig /flushdns` and retested
- Re-enabled the adapter and confirmed DNS resolution was fully restored

📸 *Screenshot: before & after fix*

<img width="1028" height="778" alt="Screenshot 2025-12-31 164817" src="https://github.com/user-attachments/assets/ea3c3493-d192-4c3f-91b7-a5f5082b907e" />
<img width="1051" height="812" alt="Screenshot 2025-12-31 172342" src="https://github.com/user-attachments/assets/95d06832-2ffc-43a8-829f-082062f24c43" />
<img width="1155" height="1079" alt="Screenshot 2025-12-31 172620" src="https://github.com/user-attachments/assets/a0dc4100-3447-48fb-b7d3-e59af2ba4dc0" />
<img width="1154" height="1079" alt="Screenshot 2025-12-31 173448" src="https://github.com/user-attachments/assets/327860d3-a8a2-4fad-8e21-50693f875fb4" />

---

## Key Learnings

**DNS vs DHCP**
- DHCP dynamically assigns IP addresses, subnet masks, default gateways, and DNS server addresses to clients, eliminating the need for manual configuration.
- DNS resolves human-readable domain names to IP addresses, enabling network communication by name rather than by address.
- Both services must work in tandem for a domain environment to function correctly. A working IP address means nothing if the client cannot resolve hostnames.

**Subnetting Fundamentals**
- Devices must reside on compatible subnets to communicate without routing. Subnet masks define where one network ends and another begins.
- `ipconfig` is a reliable first check when diagnosing subnet or addressing mismatches.

**Network Troubleshooting Methodology**
- `ping`: tests basic reachability and validates DNS resolution when used with hostnames.
- `tracert`: reveals the routing path between two hosts, useful for isolating where connectivity breaks down.
- `ipconfig /all`: exposes full adapter configuration including IP, subnet, gateway, DHCP status, and DNS servers.
- Multiple active network adapters can cause unexpected behavior; always verify which adapter is being used for a given connection.

**Packet-Level Analysis with Wireshark**
- Filtering by protocol (`icmp`, `dns`) reduces noise and surfaces relevant traffic quickly.
- Observing DNS query/response pairs confirms whether resolution is happening locally, via the domain controller, or externally.
- Packet-level visibility is invaluable when higher-level tools give ambiguous results.

---

## Issues & Fixes

**DNS Resolution Did Not Fail as Expected**

| | Detail |
|---|---|
| **Issue** | DNS name resolution continued to work after pointing the client to external DNS servers. |
| **Root Cause** | The client had multiple active network adapters; one was still configured to use the domain controller as its DNS server. |
| **Fix** | Disabled the domain-connected network adapter and flushed the DNS cache with `ipconfig /flushdns`. |
| **Result** | Domain name resolution failed as expected. DNS functionality was fully restored after re-enabling the adapter. |

**DHCP Lease Release Command Failed**

| | Detail |
|---|---|
| **Issue** | `ipconfig /release` returned an error: no adapter in a permissible state. |
| **Root Cause** | The active network adapter was not eligible for a DHCP release at that point in the session. |
| **Fix** | Verified DHCP was enabled on the adapter and confirmed an active lease was present. |
| **Result** | Client successfully obtained a valid IP address and full network configuration via DHCP. |

**Intermittent Connectivity and Name Resolution**

| | Detail |
|---|---|
| **Issue** | Inconsistent connectivity and DNS behavior during testing. |
| **Root Cause** | Incorrect adapter priority combined with multiple active network paths. |
| **Fix** | Reviewed all adapter configurations and verified correct DNS server and subnet assignments using `ipconfig /all`. |
| **Result** | Stable connectivity and consistent name resolution were restored. |

---

## Outcome Summary

This lab delivered a fully functional Windows domain networking environment with the following verified results:

- **DHCP**: Scope configured and activated; client machines received valid dynamic IP leases.
- **DNS**: Forward lookup zone created; domain name resolution confirmed from the client.
- **Connectivity**: End-to-end communication validated using `ping` and `tracert`.
- **Traffic Analysis**: DNS and ICMP traffic captured and analyzed in Wireshark; packet flow observed for both name resolution and connectivity tests.
- **Troubleshooting**: DNS misconfiguration scenario simulated, root cause accurately identified (multiple adapters), and service restored successfully.

The lab reinforced practical skills in enterprise-style networking, command-line diagnostics, and packet-level analysis within a Windows domain environment.
