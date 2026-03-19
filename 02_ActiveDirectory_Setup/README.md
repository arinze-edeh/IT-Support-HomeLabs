# Active Directory Lab: Domain Controller Deployment
### Lab 02 | Windows Server 2022 | Enterprise Identity Infrastructure

![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-Windows_Server_2022-0078D4?style=for-the-badge&logo=windows&logoColor=white)
![Service](https://img.shields.io/badge/Service-Active_Directory_DS-003366?style=for-the-badge)
![Lab Series](https://img.shields.io/badge/Lab_Series-02_of_N-orange?style=for-the-badge)

---

## Table of Contents

- [Executive Summary](#executive-summary)
- [Problem Statement](#problem-statement)
- [Architecture Overview](#architecture-overview)
- [Environment and Prerequisites](#environment-and-prerequisites)
- [Implementation Walkthrough](#implementation-walkthrough)
  - [Phase 1: OS Provisioning](#phase-1-os-provisioning)
  - [Phase 2: Network Hardening](#phase-2-network-hardening)
  - [Phase 3: AD DS Role Deployment](#phase-3-ad-ds-role-deployment)
  - [Phase 4: Domain Controller Promotion](#phase-4-domain-controller-promotion)
  - [Phase 5: Organizational Unit Structure](#phase-5-organizational-unit-structure)
- [Incidents and Resolutions](#incidents-and-resolutions)
- [Key Takeaways](#key-takeaways)
- [Outcome and Next Steps](#outcome-and-next-steps)

---

## Executive Summary

This lab documents the end-to-end provisioning of a **Windows Server 2022 Domain Controller** in a virtualized environment. The objective was to establish a production-grade Active Directory Domain Services (AD DS) foundation that supports centralized identity management, Group Policy enforcement, and scalable Organizational Unit (OU) hierarchies.

All configuration steps, encountered failure modes, and their corresponding resolutions are documented with full operational transparency.

---

## Problem Statement

> **Challenge:** A stand-alone Windows Server instance lacks centralized identity management, access control enforcement, and domain-level policy governance. Without a Domain Controller, enterprise-scale user and computer management is not operationally viable.

> **Resolution:** Deploy and configure an AD DS Domain Controller from scratch, establish a rooted domain namespace, implement a structured OU hierarchy, and validate DNS resolution readiness for downstream domain-joined clients.

---

## Architecture Overview

```
+--------------------------------------------------+
|              Virtualized Lab Environment         |
|                                                  |
|   +------------------+     +------------------+ |
|   | Windows Server   |     |   AD DS / DNS    | |
|   |     2022 DC      +---->|   Domain Root    | |
|   |  192.168.56.x    |     |  corp.local      | |
|   +------------------+     +------------------+ |
|            |                                     |
|   +--------+--------+                           |
|   |    OU Structure  |                           |
|   |  - Users         |                           |
|   |  - Computers     |                           |
|   |  - Admins        |                           |
|   +------------------+                           |
+--------------------------------------------------+
```

---

## Environment and Prerequisites

| Component | Specification |
|---|---|
| Hypervisor | VirtualBox / VMware Workstation |
| Guest OS | Windows Server 2022 (Desktop Experience) |
| vCPU | 2 cores (4 recommended during promotion) |
| RAM | 4 GB minimum |
| Storage | 50 GB dynamically allocated |
| Network Adapter 1 | NAT (internet access) |
| Network Adapter 2 | Host-Only (192.168.56.x/24) |
| Static IP Assigned | 192.168.56.10 |
| DNS (Self-Referencing) | 127.0.0.1 or 192.168.56.10 |
| Domain Name | corp.local *(or your chosen namespace)* |

**Prerequisites checklist:**

- [ ] Hypervisor installed and virtual machine created
- [ ] Windows Server 2022 ISO mounted and OS installed
- [ ] At least two network adapters configured (NAT + Host-Only)
- [ ] Administrative credentials available
- [ ] Pending Windows Updates resolved before role installation

---

## Implementation Walkthrough

---

### Phase 1: OS Provisioning

**Objective:** Install Windows Server 2022 (Desktop Experience) on the virtual machine to serve as the base operating system for the Domain Controller role.

**Steps:**

1. Boot the virtual machine from the Windows Server 2022 ISO.
2. Select **Windows Server 2022 Standard Evaluation (Desktop Experience)**.
3. Perform a **Custom: Install Windows only** installation.
4. Complete the out-of-box setup and set the local Administrator password.
5. Confirm OS boot into the Server Manager dashboard.

**Screenshot: Windows Server 2022 installation wizard and initial setup**

<img width="1031" height="899" alt="Screenshot40149 - Windows Server Installation" src="https://github.com/user-attachments/assets/38bc2990-fce6-4b6f-be8b-8829347083dc" />

> **Validation:** Server Manager loads successfully. OS version confirmed as Windows Server 2022.

---

### Phase 2: Network Hardening

**Objective:** Assign a static IP address to ensure the Domain Controller is consistently reachable and can serve as a stable DNS endpoint for domain clients.

**Problem Addressed:** DHCP-assigned addresses are unsuitable for Domain Controllers. IP address changes break DNS registration, Kerberos authentication, and client domain-join operations.

**Steps:**

1. Open **Network and Sharing Center** > **Change adapter settings**.
2. Right-click the Host-Only adapter > **Properties**.
3. Select **Internet Protocol Version 4 (TCP/IPv4)** > **Properties**.
4. Configure the following static values:

```
IP Address    : 192.168.56.10
Subnet Mask   : 255.255.255.0
Default Gateway: 192.168.56.1
Preferred DNS : 127.0.0.1  (self-referencing, updated post-promotion)
```

5. Click **OK** and close all dialogs.
6. Run `ipconfig /all` in PowerShell to confirm the static configuration.

**Screenshot: Network adapter IPv4 static IP configuration panel**

<img width="957" height="798" alt="Screenshot033139 - Static IP Configuration" src="https://github.com/user-attachments/assets/6b86faba-ef50-4260-a48a-986cb7576608" />

> **Validation:** `ipconfig /all` reflects the assigned static IP. Internet connectivity confirmed via NAT adapter.

---

### Phase 3: AD DS Role Deployment

**Objective:** Install the Active Directory Domain Services role and its dependencies via Server Manager to prepare the server for domain controller promotion.

**Problem Addressed:** Without the AD DS role, the server has no domain management capability. This phase installs the binaries required to run the promotion wizard.

**Steps:**

1. Open **Server Manager** > **Manage** > **Add Roles and Features**.
2. Select **Role-based or feature-based installation**.
3. Confirm the local server as the destination.
4. Check **Active Directory Domain Services** from the role list.
5. Accept the prompt to **Add Features** (includes Group Policy Management, etc.).
6. Proceed through the wizard and click **Install**.
7. Do NOT close Server Manager; wait for the installation to complete.

**Screenshot: AD DS role installation progress in Server Manager**

<img width="959" height="1074" alt="Screenshot035055 - AD DS Role Installation" src="https://github.com/user-attachments/assets/e79e3414-ba28-465c-8368-6245e02c50bc" />

> **Validation:** Server Manager displays a yellow flag notification: *"Promote this server to a domain controller."* This confirms role installation success.

---

### Phase 4: Domain Controller Promotion

**Objective:** Promote the server from a standalone Windows Server instance to an Active Directory Domain Controller, establishing the domain root and DNS infrastructure.

**Problem Addressed:** A server with AD DS installed is not yet a Domain Controller. Promotion configures the domain namespace, replication topology, Sysvol, NTDS database, and integrated DNS.

**Steps:**

1. In Server Manager, click the **yellow notification flag** > **Promote this server to a domain controller**.
2. Select **Add a new forest**.
3. Enter the root domain name (e.g., `corp.local`).
4. Set the **Forest Functional Level** and **Domain Functional Level** to **Windows Server 2016** or higher.
5. Ensure **DNS Server** and **Global Catalog (GC)** checkboxes are enabled.
6. Set the **Directory Services Restore Mode (DSRM)** password.
7. Proceed through the wizard (DNS delegation warning is expected in lab environments; dismiss it).
8. Review the prerequisites check. Address any warnings before proceeding.
9. Click **Install**. The server will automatically restart upon completion.
10. Log in using `DOMAIN\Administrator` credentials post-restart.

**Screenshot: New forest domain name entry during promotion wizard**

<img width="960" height="1079" alt="Screenshot035434 - Domain Name Configuration" src="https://github.com/user-attachments/assets/94112cb3-e047-4021-9a1b-04ae7331b19e" />

**Screenshot: Domain Controller options and DSRM password configuration**

<img width="1118" height="1079" alt="Screenshot042108 - DC Options Panel" src="https://github.com/user-attachments/assets/490310a6-9f65-4cc8-a642-d054b4748410" />

> **Validation:** Post-restart login prompt displays `CORP\Administrator`. Server Manager reflects the server as a Domain Controller. DNS role is visible and running.

---

### Phase 5: Organizational Unit Structure

**Objective:** Design and implement a structured OU hierarchy within Active Directory to logically segment domain objects (users, computers, and administrators) in preparation for scoped Group Policy Objects (GPOs) and Role-Based Access Control (RBAC).

**Problem Addressed:** A flat AD structure without OUs prevents granular GPO targeting, makes bulk user/computer management operationally complex, and creates audit and delegation challenges at scale.

**Steps:**

1. Open **Active Directory Administrative Center** (ADAC) or **Active Directory Users and Computers** (ADUC).
2. Right-click the domain root > **New** > **Organizational Unit**.
3. Create the following top-level OUs:

| OU Name | Purpose |
|---|---|
| `_USERS` | Standard domain user accounts |
| `_COMPUTERS` | Domain-joined workstations and servers |
| `_ADMINS` | Privileged administrator accounts |

4. Confirm each OU appears under the domain root in the left-hand tree view.
5. Optionally, create nested OUs (e.g., `_USERS > HR`, `_USERS > IT`) for departmental segmentation.

**Screenshot: Active Directory Users and Computers showing OU creation dialog**

<img width="1009" height="890" alt="Screenshot044240 - OU Creation" src="https://github.com/user-attachments/assets/77af2dfb-0a2c-4e12-b8b9-fc3042c93697" />

**Screenshot: OU structure displayed in ADUC tree view**

<img width="1015" height="890" alt="Screenshot044340 - OU Tree View" src="https://github.com/user-attachments/assets/21222c6a-de21-472d-a9db-ad8a05c716f2" />

**Screenshot: Completed OU hierarchy with nested objects**

<img width="1129" height="1079" alt="Screenshot045649 - Full OU Hierarchy" src="https://github.com/user-attachments/assets/de038666-4b8f-4485-b240-e6db72deebbd" />

> **Validation:** All OUs are visible in ADUC. New user and computer objects can be created inside each OU without errors.

---

## Incidents and Resolutions

All issues encountered during this lab are documented below as operational incident records. Each entry follows a **Problem / Root Cause / Resolution** format consistent with enterprise runbook standards.

---

### INC-001: Server Lost Internet Access After Static IP Assignment

**Severity:** Medium
**Phase:** Phase 2 (Network Hardening)

**Problem:**
After configuring a static IP on the Host-Only adapter, the server lost all internet connectivity, blocking Windows Update and any web-based validation.

**Root Cause:**
The default gateway and DNS fields were either left blank or incorrectly pointed to the Host-Only adapter instead of the NAT adapter gateway, disrupting outbound routing.

**Resolution:**
- Verified that the **NAT adapter** was still enabled and connected in the hypervisor network settings.
- Set the **Default Gateway** on the Host-Only adapter to the correct value (`192.168.56.1`).
- Confirmed that the NAT adapter retained its DHCP-assigned settings and was not modified.
- Re-ran `ping 8.8.8.8` to validate internet reachability post-fix.

---

### INC-002: AD DS Role Installation Failed on First Attempt

**Severity:** Medium
**Phase:** Phase 3 (AD DS Role Deployment)

**Problem:**
The AD DS role installation returned an error mid-progress and rolled back without completing.

**Root Cause:**
A pending Windows Update required a system restart before new roles could be installed. The role installer detected an inconsistent system state and aborted.

**Resolution:**
- Ran Windows Update, applied all pending updates, and performed a full system restart.
- Verified no further updates were pending via `sconfig` or Settings > Windows Update.
- Re-launched the Add Roles and Features wizard and completed the installation successfully.

---

### INC-003: Domain Controller Promotion Stalled at 50-60%

**Severity:** High
**Phase:** Phase 4 (Domain Controller Promotion)

**Problem:**
The promotion wizard froze at the "Promoting this server to a domain controller" progress stage for an extended period with no forward movement.

**Root Cause:**
Insufficient RAM allocated to the virtual machine caused the promotion process (which writes the NTDS database, configures DNS, and initializes Sysvol) to thrash memory and stall.

**Resolution:**
- Shut down the virtual machine.
- Increased RAM allocation from **2 GB to 4 GB** in the hypervisor settings.
- Closed resource-heavy applications on the host system to free additional memory.
- Restarted the VM and re-ran the promotion wizard successfully.

---

### INC-004: "Access Denied" When Creating Organizational Units

**Severity:** High
**Phase:** Phase 5 (Organizational Unit Structure)

**Problem:**
Attempting to create new OUs in Active Directory Users and Computers returned an **"Access Denied"** error.

**Root Cause:**
The active session was authenticated as the **local Administrator** account rather than the **Domain Administrator** account. Local admin accounts have no authority over domain directory objects.

**Resolution:**
- Signed out of the local Administrator session.
- Signed back in using the domain credentials: `CORP\Administrator`.
- Confirmed the domain context was active by checking the username shown in Server Manager.
- Successfully created OUs without errors.

---

### INC-005: DNS Lookup Failures Preventing Clients from Joining the Domain

**Severity:** Critical
**Phase:** Post-Promotion Validation

**Problem:**
Downstream client machines could not join the domain. Name resolution for the domain (`corp.local`) failed, and the domain join wizard returned a "domain not found" error.

**Root Cause:**
The Domain Controller's own DNS server pointer was incorrectly configured. After promotion, the server's preferred DNS must point to itself (either `127.0.0.1` or its own static IP). Additionally, the DNS Server service was not confirmed as running before testing client joins.

**Resolution:**

1. Verified the DNS Server service was active:
   - Open **Server Manager** > **Tools** > **DNS**
   - Confirmed the server was listed and forward lookup zones for `corp.local` were present.

2. Updated the server's own IPv4 DNS setting:
   ```
   Preferred DNS: 127.0.0.1  (or 192.168.56.10)
   ```

3. Flushed the DNS cache on the Domain Controller:
   ```powershell
   ipconfig /flushdns
   ipconfig /registerdns
   Restart-Service DNS
   ```

4. Flushed the DNS cache on the client machine and reattempted the domain join:
   ```powershell
   ipconfig /flushdns
   ```

5. Confirmed client DNS pointed to the DC's static IP (`192.168.56.10`) and not to an external or DHCP-assigned DNS server.

> **Result:** DNS resolution restored. Clients successfully located the domain and completed the join process.

---

## Key Takeaways

**Active Directory Domain Services Architecture**
Understanding the distinction between the AD DS role (binaries only) and domain controller promotion (functional configuration) is critical. These are separate, sequential operations.

**DNS is Non-Negotiable in AD Environments**
Active Directory is entirely DNS-dependent. Every Kerberos ticket, LDAP lookup, and domain join relies on accurate DNS resolution. The Domain Controller must reference itself as its own DNS server post-promotion.

**Static IP is a Hard Requirement**
Domain Controllers must have static IP addresses. DHCP-assigned IPs break DNS SRV records, Kerberos, and replication. This is not a best practice recommendation; it is an architectural requirement.

**OU Design Drives Policy Governance**
Organizational Units are not cosmetic groupings. They are the targeting mechanism for Group Policy Objects and the delegation boundary for administrative roles. A flat OU structure undermines both.

**Credential Context Matters**
Domain-level operations require domain-level credentials. Confusing local administrator and domain administrator accounts is a common source of access errors, particularly early in lab environments.

---

## Outcome and Next Steps

**Outcome**

A fully operational Active Directory domain has been deployed on Windows Server 2022. The Domain Controller is authoritative for the `corp.local` namespace, DNS is integrated and resolving, and the OU hierarchy is in place for policy and user management.

| Deliverable | Status |
|---|---|
| Windows Server 2022 provisioned | Complete |
| Static IP configured | Complete |
| AD DS role installed | Complete |
| Server promoted to Domain Controller | Complete |
| DNS integrated and validated | Complete |
| OU hierarchy created | Complete |
| All incidents resolved | Complete |

---

*Lab documented as part of an enterprise IT series. All configurations performed in an isolated virtualized environment.*
