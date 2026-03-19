# Lab 04 - Networking Services: DHCP and DNS Configuration in a Windows Domain Environment

![Lab Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Platform](https://img.shields.io/badge/Platform-Windows%20Server%20%7C%20Windows%2010-blue)
![Tools](https://img.shields.io/badge/Tools-DHCP%20%7C%20DNS%20%7C%20Wireshark%20%7C%20VirtualBox-orange)
![Category](https://img.shields.io/badge/Category-Enterprise%20Networking-purple)

---

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Tools and Technologies](#tools-and-technologies)
- [Lab Configuration Steps](#lab-configuration-steps)
  - [1. DHCP Scope Configuration](#1-dhcp-scope-configuration)
  - [2. DNS Zone Configuration](#2-dns-zone-configuration)
  - [3. Client Connectivity Validation](#3-client-connectivity-validation)
  - [4. Network Traffic Analysis](#4-network-traffic-analysis)
  - [5. Troubleshooting Simulation](#5-troubleshooting-simulation)
- [Incidents and Resolutions](#incidents-and-resolutions)
  - [INC-001: DNS Resolution Did Not Fail as Expected](#inc-001-dns-resolution-did-not-fail-as-expected)
  - [INC-002: DHCP Lease Release Command Failed](#inc-002-dhcp-lease-release-command-failed)
  - [INC-003: Intermittent Connectivity and Name Resolution](#inc-003-intermittent-connectivity-and-name-resolution)
- [Key Learnings](#key-learnings)
- [Outcome Summary](#outcome-summary)

---

## Overview

This lab simulates an enterprise-grade Windows domain networking environment. The objective was to provision and validate core infrastructure services, specifically **DHCP** for dynamic IP address management and **DNS** for domain name resolution, then perform real-world troubleshooting under simulated fault conditions.

The lab mirrors scenarios commonly encountered by Systems Administrators and Network Engineers in production environments, emphasizing a structured **problem identification, root cause analysis, and resolution** workflow.

---

## Architecture

```
+---------------------+          +---------------------------+
|   Windows 10 Client |          |   Windows Server (DC)     |
|                     |          |                           |
|  ipconfig           | <------> |  DHCP Server              |
|  ping               |          |  DNS Server               |
|  tracert            |          |  Active Directory         |
|  nbtstat            |          |  DNS Manager              |
|  Wireshark          |          |  DHCP Management Console  |
+---------------------+          +---------------------------+
         |                                    |
         +----------------+-------------------+
                          |
                   VirtualBox Network
                   (Internal / NAT)
```

---

## Prerequisites

| Requirement | Details |
|---|---|
| Hypervisor | Oracle VirtualBox (latest stable) |
| Server OS | Windows Server (2016 / 2019 / 2022) |
| Client OS | Windows 10 (any edition) |
| Server Roles | Active Directory Domain Services, DHCP, DNS |
| Network Mode | VirtualBox Internal Network or Host-Only Adapter |
| Privileges | Domain Administrator |
| Packet Analyzer | Wireshark (latest stable) |

---

## Tools and Technologies

| Tool | Role |
|---|---|
| Windows Server DHCP | Dynamic IP address assignment and lease management |
| Windows Server DNS | Forward lookup zone creation and domain name resolution |
| DNS Manager | GUI administration of DNS zones and records |
| DHCP Management Console | GUI administration of DHCP scopes and leases |
| `ipconfig` | IP configuration inspection and DHCP lease operations |
| `ping` | ICMP-based connectivity and DNS resolution testing |
| `tracert` | Hop-by-hop route path analysis |
| `nbtstat` | NetBIOS over TCP/IP name resolution diagnostics |
| Wireshark | Packet capture and protocol-level traffic analysis |
| VirtualBox | Isolated virtualized lab environment |

---

## Lab Configuration Steps

### 1. DHCP Scope Configuration

**Objective:** Provision a DHCP scope on Windows Server and verify that the Windows 10 client successfully obtains a dynamic IP address lease.

**Steps Performed:**

1. Opened the **DHCP Management Console** on Windows Server.
2. Created a new IPv4 scope with a defined IP address range, subnet mask, default gateway, and DNS server option.
3. Activated the scope.
4. On the Windows 10 client, ran `ipconfig /release` followed by `ipconfig /renew` to request a fresh lease.
5. Confirmed the client received an IP address within the defined scope range.
6. Verified the active lease appeared under **Address Leases** in the DHCP Management Console.

**Commands Used:**
```cmd
ipconfig /release
ipconfig /renew
ipconfig /all
```

**Screenshots:**

> ***Screenshot 1: DHCP scope creation wizard showing IP range and subnet mask configuration***

![DHCP Scope Creation](https://github.com/user-attachments/assets/2ecc8ddc-7283-4383-b9e3-aa7689feefcd)

> ***Screenshot 2: Activated DHCP scope visible in the DHCP Management Console***

![DHCP Scope Active](https://github.com/user-attachments/assets/66d0db7d-2f0e-476c-97e4-ff437f0156b9)

> ***Screenshot 3: Address Leases panel showing client lease assignment***

![DHCP Lease](https://github.com/user-attachments/assets/6b2bb328-cd90-4770-9896-6ad840f248cc)

> ***Screenshot 4: DHCP scope options configured including gateway and DNS server assignment***

![DHCP Scope Options](https://github.com/user-attachments/assets/ad4c7128-8571-4604-a36a-6ffe9815e3ec)

> ***Screenshot 5: Client ipconfig /all output confirming DHCP-assigned address and DNS server***

![DHCP Client ipconfig](https://github.com/user-attachments/assets/9fa44cb8-824c-4709-b162-944271a35e08)

> ***Screenshot 6: DHCP lease details confirming client hostname and assigned IP address***

![DHCP Lease Details](https://github.com/user-attachments/assets/1a5bc5ed-4d28-4b6c-8e30-de92a4f91032)

---

### 2. DNS Zone Configuration

**Objective:** Create a DNS forward lookup zone on Windows Server and verify that the Windows 10 client can resolve domain names using the domain controller.

**Steps Performed:**

1. Opened **DNS Manager** on Windows Server.
2. Created a new **Forward Lookup Zone** for the lab domain.
3. Verified the zone appeared and was active in DNS Manager.
4. On the Windows 10 client, configured the DNS server setting to point to the domain controller's IP address.
5. Used `ping` with the fully qualified domain name (FQDN) to confirm successful name resolution.

**Commands Used:**
```cmd
ping <domain-controller-hostname>
ipconfig /all
```

**Screenshots:**

> ***Screenshot 1: DNS Manager displaying the newly created forward lookup zone***

![DNS Zone Creation](https://github.com/user-attachments/assets/ae987da2-337e-4240-b004-9a16ba67177c)

> ***Screenshot 2: DNS zone properties confirming zone type and replication scope***

![DNS Zone Properties](https://github.com/user-attachments/assets/843d44f6-3ada-453d-a2ad-cc19ee5838a9)

---

### 3. Client Connectivity Validation

**Objective:** Confirm end-to-end network connectivity between the Windows 10 client and the domain controller using standard diagnostic utilities.

**Steps Performed:**

1. From the Windows 10 client, executed `ping <DC-hostname>` to verify ICMP connectivity and DNS resolution simultaneously.
2. Executed `tracert <DC-hostname>` to confirm the network path and identify the number of hops between client and server.
3. Reviewed output for packet loss, latency, and routing anomalies.

**Commands Used:**
```cmd
ping <domain-controller-hostname>
tracert <domain-controller-hostname>
```

**Screenshots:**

> ***Screenshot 1: ping output showing successful ICMP replies from the domain controller with resolved IP address***

![Ping Output](https://github.com/user-attachments/assets/23242697-13e5-4dc0-867c-d502cf32ce2d)

> ***Screenshot 2: tracert output confirming single-hop path between client and domain controller***

![Tracert Output](https://github.com/user-attachments/assets/7046605d-2d13-4a20-94ea-b44d5e773dd1)

---

### 4. Network Traffic Analysis

**Objective:** Capture and analyze live network traffic to observe ICMP and DNS packet behavior at the protocol level using Wireshark.

**Steps Performed:**

1. Launched **Wireshark** on the Windows 10 client and started a capture on the active network adapter.
2. Applied display filters to isolate **ICMP** traffic during ping operations.
3. Applied display filters to isolate **DNS** traffic during name resolution operations.
4. Observed the DNS query and response exchange (UDP port 53) between client and domain controller.
5. Observed ICMP Echo Request and Echo Reply packets confirming connectivity.
6. Stopped the capture and saved the file for review.

**Wireshark Display Filters Used:**
```
icmp
dns
dns.qry.name contains "<domain>"
```

**Screenshots:**

> ***Screenshot 1: Wireshark capture showing ICMP Echo Request and Echo Reply packets during ping***

![Wireshark ICMP](https://github.com/user-attachments/assets/0fcd2968-08db-4366-8519-c6f729a09634)

> ***Screenshot 2: Wireshark capture showing DNS query packet (UDP port 53) sent from client to domain controller***

![Wireshark DNS Query](https://github.com/user-attachments/assets/012c1bab-eaca-4305-9ce7-aebb47e44e61)

> ***Screenshot 3: Wireshark capture showing DNS response packet with resolved IP address***

![Wireshark DNS Response](https://github.com/user-attachments/assets/2c7901e7-f4f4-4be7-9d0f-4ac6e8197234)

> ***Screenshot 4: Packet detail pane showing full DNS query structure including QNAME and QTYPE fields***

![Wireshark DNS Detail](https://github.com/user-attachments/assets/44bcd675-fd01-46e2-bac6-6e0ec9f6b7f8)

> ***Screenshot 5: Wireshark filter applied isolating DNS traffic stream during name resolution test***

![Wireshark DNS Filter](https://github.com/user-attachments/assets/0916554e-b67d-4073-9f33-c998bf776b86)

> ***Screenshot 6: Full packet capture session overview showing protocol distribution across the capture window***

![Wireshark Session Overview](https://github.com/user-attachments/assets/381ce843-af93-4c19-b9ab-e7cf0fdb347f)

---

### 5. Troubleshooting Simulation

**Objective:** Simulate a real-world DNS misconfiguration scenario, identify the root cause using command-line diagnostics, and restore full functionality.

**Steps Performed:**

1. Redirected the client DNS setting away from the domain controller to an external DNS server (e.g., 8.8.8.8) to simulate a misconfiguration.
2. Attempted to resolve the internal domain name using `ping <domain-controller-hostname>` to confirm resolution failure.
3. Ran `ipconfig /all` to inspect the active DNS configuration on all adapters.
4. Identified that the client had a secondary network adapter still configured with the domain controller as DNS, preventing the expected failure.
5. Disabled the domain-connected adapter to fully isolate the client from internal DNS.
6. Flushed the DNS resolver cache using `ipconfig /flushdns`.
7. Confirmed DNS resolution now failed as expected.
8. Re-enabled the domain-connected adapter and restored the correct DNS server setting.
9. Verified full DNS resolution and connectivity were restored.

**Commands Used:**
```cmd
ipconfig /all
ping <domain-controller-hostname>
ipconfig /flushdns
ipconfig /renew
```

**Screenshots:**

> ***Screenshot 1: ipconfig /all output showing multiple active adapters with conflicting DNS server configurations***

![Troubleshoot Adapters](https://github.com/user-attachments/assets/ea3c3493-d192-4c3f-91b7-a5f5082b907e)

> ***Screenshot 2: ping output confirming DNS resolution failure after domain adapter was disabled and cache flushed***

![DNS Failure](https://github.com/user-attachments/assets/95d06832-2ffc-43a8-829f-082062f24c43)

> ***Screenshot 3: Network Adapter settings panel showing domain-connected adapter being disabled***

![Adapter Disabled](https://github.com/user-attachments/assets/a0dc4100-3447-48fb-b7d3-e59af2ba4dc0)

> ***Screenshot 4: ping output confirming successful DNS resolution after restoring correct configuration***

![DNS Restored](https://github.com/user-attachments/assets/327860d3-a8a2-4fad-8e21-50693f875fb4)

---

## Incidents and Resolutions

### INC-001: DNS Resolution Did Not Fail as Expected

| Field | Details |
|---|---|
| **Severity** | Medium |
| **Category** | DNS Misconfiguration |
| **Status** | Resolved |

**Problem Statement**

After reconfiguring the client DNS server setting to point to an external resolver, internal domain name resolution continued to succeed. The expected failure state was not reproduced.

**Root Cause Analysis**

The Windows 10 client had multiple active network adapters. One adapter retained its original configuration pointing to the domain controller as the DNS server. Windows resolved names using the first responding DNS server across all active adapters, bypassing the intentionally misconfigured interface.

**Resolution**

1. Ran `ipconfig /all` to enumerate all active network adapters and their DNS server assignments.
2. Identified the secondary adapter still configured with the domain controller DNS address.
3. Disabled the domain-connected adapter via **Network Adapter Settings**.
4. Flushed the DNS resolver cache using `ipconfig /flushdns` to eliminate cached responses.
5. Confirmed DNS resolution failure using `ping <FQDN>`.
6. Re-enabled the adapter and verified full resolution was restored.

**Outcome**

DNS resolution failed as expected after adapter isolation. Full functionality was restored after re-enabling the correct adapter and verifying DNS server assignments on all interfaces.

---

### INC-002: DHCP Lease Release Command Failed

| Field | Details |
|---|---|
| **Severity** | Low |
| **Category** | DHCP Client Operation |
| **Status** | Resolved |

**Problem Statement**

Running `ipconfig /release` on the Windows 10 client returned an error indicating no adapter was in a state permissible for DHCP release.

**Root Cause Analysis**

The active network adapter was not in a compatible state for a DHCP release operation at the time the command was executed. This is commonly caused by the adapter being in a disconnected, transitioning, or statically configured state at the time of the command.

**Resolution**

1. Verified DHCP was enabled on the target network adapter via **Adapter Properties**.
2. Confirmed the client was connected to the network and the DHCP server was reachable.
3. Re-executed `ipconfig /renew` directly to obtain a fresh lease assignment.
4. Verified the lease appeared in the DHCP Management Console under **Address Leases**.

**Outcome**

The client successfully obtained a valid IP address and full network configuration from the DHCP server. Lease was confirmed active in both `ipconfig /all` output and the DHCP Management Console.

---

### INC-003: Intermittent Connectivity and Name Resolution

| Field | Details |
|---|---|
| **Severity** | Medium |
| **Category** | Network Adapter Configuration |
| **Status** | Resolved |

**Problem Statement**

During lab testing, connectivity and DNS resolution were inconsistent. `ping` commands produced intermittent timeouts and DNS queries returned unpredictable results.

**Root Cause Analysis**

Multiple active network adapters introduced conflicting routing paths and inconsistent DNS server priority. Windows selected DNS servers and routing paths non-deterministically across adapters, producing unstable results.

**Resolution**

1. Ran `ipconfig /all` to audit the full IP, DNS, and gateway configuration for all adapters.
2. Identified conflicting DNS server entries and incorrect subnet assignments across interfaces.
3. Corrected DNS server assignments to ensure only the domain controller was used for internal resolution.
4. Verified subnet masks and default gateway settings were consistent across relevant adapters.
5. Re-ran `ping` and `tracert` to confirm stable connectivity and consistent name resolution.

**Outcome**

Stable, consistent connectivity and DNS resolution were restored. All subsequent tests produced expected results with no intermittent failures.

---

## Key Learnings

### DNS and DHCP Interdependency
DHCP assigns IP addresses, subnet masks, default gateways, and DNS server addresses dynamically. DNS translates domain names to IP addresses. Both services must be correctly configured and reachable for a Windows domain environment to function properly.

### Multi-Adapter Behavior in Windows
Windows 10 does not isolate DNS queries to a single adapter by default. When multiple adapters are active with different DNS server configurations, Windows may resolve names using any available adapter. This behavior can mask misconfigurations and must be accounted for during troubleshooting.

### Importance of DNS Cache Management
Cached DNS responses persist after configuration changes and can produce misleading results during troubleshooting. `ipconfig /flushdns` must be executed after any DNS configuration change to ensure tests reflect the current state of the resolver.

### Wireshark for Protocol Verification
Packet captures provide ground-truth validation of what is actually traversing the network. Observing DNS queries and responses at the packet level confirmed that name resolution was functioning correctly at the protocol layer independent of application-layer behavior.

### Structured Troubleshooting Methodology
Applying a systematic approach (observe, isolate, hypothesize, test, resolve, verify) to each issue prevented guesswork and produced consistent, repeatable resolutions. `ipconfig /all` served as the primary first-response diagnostic command across all three incidents.

---

## Outcome Summary

| Objective | Result |
|---|---|
| DHCP scope provisioned and verified | Completed |
| Client obtained valid DHCP lease | Completed |
| DNS forward lookup zone created | Completed |
| Internal domain name resolution confirmed | Completed |
| End-to-end connectivity validated via ping and tracert | Completed |
| ICMP and DNS traffic captured and analyzed in Wireshark | Completed |
| DNS misconfiguration scenario simulated and resolved | Completed |
| All three incidents documented with root cause and resolution | Completed |

---

*Lab environment: VirtualBox isolated internal network. All configurations are non-production and intended for educational purposes only.*
















# 04 – Networking Lab

## 📌 Overview

This lab focused on building and troubleshooting core networking services within a Windows domain environment. The objective was to configure DHCP and DNS, verify client connectivity, analyze network traffic, and simulate real-world troubleshooting scenarios. Through hands-on testing, common networking issues were identified and resolved using industry-standard tools and techniques.

---

## 🛠️ Tools Used

- **Windows Server**
  - DHCP Server
  - DNS Server
  - DNS Manager
  - DHCP Management Console

- **Client Machine (Windows 10)**
  - Command Prompt (`ipconfig`, `ping`, `tracert`, `nbtstat`)

- **Packet Analysis**
  - Wireshark

- **Virtualization**
  - VirtualBox

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

- Successfully configured and verified DHCP services, ensuring dynamic IP address assignment to client machines.
- Created and validated DNS zones, enabling proper domain name resolution within the network.
- Confirmed end-to-end network connectivity using `ping` and `tracert`.
- Captured and analyzed network traffic using Wireshark to observe DNS and ICMP packet behavior.
- Simulated a real-world DNS misconfiguration scenario and accurately identified the root cause.
- Resolved connectivity and name resolution issues by restoring correct DNS and network adapter configurations.
- Gained hands-on experience troubleshooting enterprise-style networking issues in a Windows domain environment.
