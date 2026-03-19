# Active Directory Lab Part 2 -- Group Policy, Permissions, and Domain Authentication

![Active Directory](https://img.shields.io/badge/Platform-Windows_Server-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)
![Domain](https://img.shields.io/badge/Domain-arinzelab.local-blueviolet?style=for-the-badge)
![Tools](https://img.shields.io/badge/Tools-GPMC_%7C_PowerShell-orange?style=for-the-badge)

---

## Table of Contents

* [Overview](#overview)
* [Architecture](#architecture)
* [Prerequisites](#prerequisites)
* [Environment Setup](#environment-setup)
* [Implementation](#implementation)
  * [1. User Account Provisioning](#1-user-account-provisioning)
  * [2. Group Policy Object Creation](#2-group-policy-object-creation)
  * [3. GPO to OU Linking](#3-gpo-to-ou-linking)
  * [4. Login Script Deployment](#4-login-script-deployment)
  * [5. Client Policy Verification](#5-client-policy-verification)
* [Incident Register and Resolutions](#incident-register-and-resolutions)
* [Key Learnings](#key-learnings)
* [Outcome Summary](#outcome-summary)

---

## Overview

This lab documents the end-to-end configuration of **Group Policy Objects (GPOs), domain user accounts, login scripts, and client-side policy enforcement** within a Windows Server Active Directory environment.

The work is focused on solving real-world problems encountered during domain setup, such as DNS misconfiguration, DHCP failures, GPO non-application, and domain user authentication breakdowns. Every issue encountered is documented with its root cause and the precise fix applied.

---

## Architecture

```
arinzelab.local
     |
     +-- Domain Controller (Windows Server)
     |       |-- DHCP Service
     |       |-- DNS Service
     |       |-- GPMC (Group Policy Management)
     |       |-- Active Directory Users & Computers
     |
     +-- Client VM (Windows 10)
             |-- Domain-joined: arinzelab.local
             |-- Receives GPOs from DC
             |-- Authenticates domain users
```

---

## Prerequisites

| Requirement | Details |
|---|---|
| Domain Controller | Windows Server with AD DS role installed |
| Client Machine | Windows 10 VM on the same internal network |
| DNS | Client DNS must point to the Domain Controller |
| Network | Both machines on the same internal/host-only network adapter |
| Credentials | Domain Administrator account |

---

## Environment Setup

Before beginning, verify the following baseline conditions are met on the client machine.

**Confirm network connectivity to the domain controller:**

```powershell
ping arinzelab.local
```

**Verify DNS resolution is pointing to the Domain Controller:**

```powershell
ipconfig /all
```

**Flush stale DNS cache:**

```powershell
ipconfig /flushdns
```

**Confirm the client is domain-joined (not WORKGROUP):**

```powershell
systeminfo | findstr /B /C:"Domain"
```

---

## Implementation

### 1. User Account Provisioning

User accounts were created inside the appropriate Organizational Units (OUs) using **Active Directory Users and Computers (ADUC)**.

**Steps performed:**

1. Opened **Active Directory Users and Computers** on the Domain Controller.
2. Navigated to the target Organizational Unit.
3. Right-clicked the OU and selected **New > User**.
4. Populated the **First Name**, **Last Name**, **User Logon Name**, and initial password fields.
5. Configured password policy options (e.g., user must change password at next logon).
6. Confirmed account creation and verified the user appeared in the OU.

**Screenshot: User accounts created in ADUC**

<img width="958" height="792" alt="image" src="https://github.com/user-attachments/assets/6620b1af-ad4d-4dba-86c5-9412eb28f226" />

> *Active Directory Users and Computers showing newly provisioned domain user accounts.*

---

### 2. Group Policy Object Creation

Group Policy Objects were created using the **Group Policy Management Console (GPMC)** to enforce security and configuration settings across the domain.

**GPOs created:**

* `Disable-ControlPanel-GPO` -- Restricts access to Control Panel for standard users.
* `LoginScript-GPO` -- Executes a login script upon user authentication.

**Steps performed:**

1. Opened **Group Policy Management** (`gpmc.msc`) on the Domain Controller.
2. Right-clicked **Group Policy Objects** and selected **New**.
3. Named the GPO descriptively and confirmed creation.
4. Right-clicked the new GPO and selected **Edit** to configure the required settings.
5. For `Disable-ControlPanel-GPO`: Navigated to **User Configuration > Administrative Templates > Control Panel** and enabled **Prohibit access to Control Panel and PC settings**.
6. For `LoginScript-GPO`: Navigated to **User Configuration > Windows Settings > Scripts (Logon/Logoff)** and added the target script.

**Screenshot: GPO list in Group Policy Management Console**

<img width="990" height="789" alt="image" src="https://github.com/user-attachments/assets/ea4f1ef1-ec78-4c83-8c7c-de5f5234c710" />

> *GPMC showing all created Group Policy Objects ready for linking.*

---

### 3. GPO to OU Linking

GPOs were linked to the relevant Organizational Units to enforce policies on the correct scope of users and computers.

**Steps performed:**

1. In **GPMC**, right-clicked the target OU.
2. Selected **Link an Existing GPO**.
3. Chose the appropriate GPO from the list.
4. Confirmed the link appeared under the OU in the GPMC tree.
5. Verified link order and enforcement status.

**Screenshots: GPO linked to Organizational Unit**

<img width="997" height="894" alt="Screenshot054628" src="https://github.com/user-attachments/assets/1924411e-2d81-4968-bf0a-66fa8ea39154" />

<img width="988" height="775" alt="image" src="https://github.com/user-attachments/assets/d267af55-f263-478d-b69e-39669c10b319" />

> *GPMC showing the GPO successfully linked to the target OU.*


---

### 4. Login Script Deployment

A logon script was deployed via GPO to execute automatically when domain users authenticate.

**Script path convention used:**

```
\\arinzelab.local\NETLOGON\logon.bat
```

> Scripts placed in the **NETLOGON** share are replicated to all Domain Controllers and are accessible to all domain-joined clients during logon.

**Steps performed:**

1. Placed the logon script (`logon.bat`) in the `C:\Windows\SYSVOL\sysvol\arinzelab.local\scripts` directory.
2. Opened the `LoginScript-GPO` in the Group Policy Editor.
3. Navigated to **User Configuration > Windows Settings > Scripts > Logon**.
4. Clicked **Add** and browsed to the script path.
5. Applied and closed the editor.

**Screenshot: Script path configured in Group Policy Editor**

> ![Script Path GPO](screenshots/05-login-script-path.png)
> *Group Policy Editor showing the logon script configured under User Configuration.*

**Screenshot: NETLOGON share containing the script**

> ![NETLOGON Script](screenshots/06-netlogon-script-file.png)
> *NETLOGON share directory confirming the script file is accessible to clients.*

---

### 5. Client Policy Verification

After GPO linking and script deployment, policies were tested and verified on the Windows 10 client VM.

**Force a Group Policy refresh on the client:**

```powershell
gpupdate /force
```

**Verify applied policies and confirm GPO names:**

```powershell
gpresult /r
```

**Expected output excerpt (confirming applied GPOs):**

```
Applied Group Policy Objects
-----------------------------
    Disable-ControlPanel-GPO
    LoginScript-GPO
    Default Domain Policy
```

**Screenshot: gpupdate /force execution on client**

> ![gpupdate Force](screenshots/07-gpupdate-force.png)
> *PowerShell output confirming Group Policy refresh completed successfully.*

**Screenshot: gpresult /r output confirming applied GPOs**

> ![gpresult Output](screenshots/08-gpresult-applied-gpos.png)
> *gpresult /r confirming both Disable-ControlPanel-GPO and LoginScript-GPO applied.*

**Screenshot: Control Panel access blocked on client**

> ![Control Panel Blocked](screenshots/09-control-panel-blocked.png)
> *Windows 10 client showing the restriction message when attempting to access Control Panel.*

**Screenshot: Domain user desktop after login script execution**

> ![Login Script Executed](screenshots/10-login-script-result.png)
> *Client desktop confirming login script ran successfully at user logon.*

---

## Incident Register and Resolutions

This section documents every issue encountered during the lab, its root cause, and the exact resolution applied.

---

### INC-001: Client VM Not Joined to Domain

**Severity:** High
**Symptom:** Windows 10 client displayed `WORKGROUP` in system properties instead of `arinzelab.local`.

**Root Cause:**

The client machine had not been domain-joined. Without domain membership, no domain authentication, Group Policy, or centralized management is possible.

**Resolution Steps:**

1. On the client, opened **System Properties** (`sysdm.cpl`).
2. Clicked **Change** under Computer Name.
3. Selected **Domain** and entered `arinzelab.local`.
4. Provided Domain Administrator credentials when prompted.
5. Restarted the client to complete domain membership.

**Verification:**

```powershell
systeminfo | findstr /B /C:"Domain"
# Expected: Domain: arinzelab.local
```

**Screenshot: Client successfully joined to domain**

> ![Domain Join](screenshots/11-domain-join-success.png)

---

### INC-002: Domain Name Not Resolving

**Severity:** High
**Symptom:** `ping arinzelab.local` failed with the error `Ping request could not find host arinzelab.local`.

**Root Cause:**

The client's DNS server was not configured to point to the Domain Controller. Active Directory relies entirely on DNS to locate domain services including Kerberos, LDAP, and Group Policy.

**Resolution Steps:**

1. Opened **Network and Sharing Center** on the client.
2. Navigated to the active adapter's **IPv4 Properties**.
3. Set the **Preferred DNS Server** to the Domain Controller's IP address.
4. Flushed the DNS resolver cache:

```powershell
ipconfig /flushdns
```

5. Re-tested domain resolution:

```powershell
ping arinzelab.local
nslookup arinzelab.local
```

**Verification:**

```
Reply from 192.168.x.x: bytes=32 time<1ms TTL=128
```

**Screenshot: DNS configured on client NIC pointing to DC**

> ![DNS Fix](screenshots/12-dns-configured-to-dc.png)

---

### INC-003: DHCP Renewal Failure

**Severity:** Medium
**Symptom:** `ipconfig /renew` failed with a DHCP timeout error. The client could not obtain an IP address automatically.

**Root Cause:**

The client and Domain Controller were not on the same internal virtual network adapter. The DHCP service on the DC could not broadcast or respond to DHCP requests from the client because they were on isolated network segments.

**Resolution Steps:**

1. Reviewed the network adapter type on both VMs in the hypervisor settings.
2. Confirmed both were set to the same **Internal Network** or **Host-Only Adapter**.
3. Verified the DHCP Server service was running on the Domain Controller:

```powershell
Get-Service -Name DHCPServer
# If stopped:
Start-Service -Name DHCPServer
```

4. Ran `ipconfig /renew` on the client again after confirming network alignment.

**Verification:**

```powershell
ipconfig /all
# Expected: DHCP Enabled: Yes | IP Address assigned | DHCP Server shows DC IP
```

**Screenshot: DHCP lease successfully obtained**

> ![DHCP Lease](screenshots/13-dhcp-lease-obtained.png)

---

### INC-004: Domain User Login Failure After Password Reset

**Severity:** Medium
**Symptom:** A domain user was unable to log in after an administrator-initiated password reset. The logon screen rejected valid credentials.

**Root Cause:**

The password reset was configured with **"User must change password at next logon"** enabled. The user attempted to log in using the new password without completing the mandatory change prompt, or the prompt was not presented correctly in the VM console session.

**Resolution Steps:**

1. Opened **Active Directory Users and Computers** on the Domain Controller.
2. Located the affected user account.
3. Right-clicked and selected **Reset Password**.
4. Set a new password and **unchecked** "User must change password at next logon."
5. Ensured the account was not locked out by checking **Account is locked out** under the Account tab.
6. Attempted login on the client using the correct domain logon format:

```
arinzelab\jdoe
```

**Verification:**

Successful domain user desktop load with Group Policy applied.

**Screenshot: ADUC password reset options**

> ![Password Reset](screenshots/14-aduc-password-reset.png)

---

### INC-005: Group Policy Not Applying to Client

**Severity:** High
**Symptom:** After linking GPOs to the OU, policies did not appear in `gpresult /r` output on the client. Control Panel remained accessible despite the restriction GPO being linked.

**Root Cause:**

The client had not received the updated Group Policy since the GPOs were linked. Additionally, initial `gpresult /r` was run before the client user was authenticated under the correct OU scope, meaning the user-level GPOs were not being evaluated.

**Resolution Steps:**

1. Logged in to the client as the target domain user (from the correct OU).
2. Forced a Group Policy refresh:

```powershell
gpupdate /force
```

3. Ran `gpresult /r` to confirm policy receipt:

```powershell
gpresult /r
```

4. Confirmed both `Disable-ControlPanel-GPO` and `LoginScript-GPO` appeared under **Applied Group Policy Objects**.
5. Tested Control Panel access to confirm the restriction was enforced.

**Verification:**

```
Applied Group Policy Objects
-----------------------------
    Disable-ControlPanel-GPO
    LoginScript-GPO
```

**Screenshot: gpresult confirming GPO application post-fix**

> ![GPO Fix Confirmed](screenshots/15-gpresult-gpos-applied.png)

---

## Key Learnings

**Domain Join and DNS Dependency**

Active Directory domain operations, including authentication, Group Policy, and Kerberos ticketing, are entirely dependent on correct DNS configuration. The client DNS must point to the Domain Controller before any domain activity is attempted.

**Group Policy Scope and Targeting**

GPOs apply based on the location of the user or computer object within the AD hierarchy. A GPO linked to an OU only affects objects inside that OU. Verifying OU membership before testing policy application eliminates most GPO non-application issues.

**gpupdate and gpresult as First-Line Diagnostics**

`gpupdate /force` and `gpresult /r` are the primary tools for confirming Group Policy application. Running them in sequence after any GPO change is the standard procedure before escalating to deeper troubleshooting.

**Local vs. Domain Authentication**

Domain-level Group Policies override local Group Policy settings. This distinction is critical when troubleshooting whether a policy restriction is coming from a local GPO or a domain GPO.

**Virtualized Lab Network Configuration**

In virtualized lab environments, placing VMs on different network adapter types (NAT vs. Host-Only vs. Internal) is the most common cause of DHCP and DNS failures that appear to be service-level issues.

**Password Reset Best Practices**

Forcing a mandatory password change at next logon can create login failures in lab environments where console behavior differs from standard user sessions. Understanding when to use or bypass this setting prevents unnecessary lockout scenarios.

---

## Outcome Summary

| Objective | Status |
|---|---|
| Domain user accounts created in correct OUs | Passed |
| GPOs created and configured in GPMC | Passed |
| GPOs linked to correct Organizational Units | Passed |
| Login script deployed via NETLOGON share | Passed |
| Client VM joined to `arinzelab.local` | Passed |
| Domain user authentication verified on client | Passed |
| Group Policy application confirmed via gpresult | Passed |
| Control Panel restriction enforced on client | Passed |
| All 5 incidents diagnosed and resolved | Passed |

**Final Result:** All Group Policy Objects applied as expected. Domain user authentication succeeded. Login script executed on logon. All lab objectives completed.

---

*Lab documentation authored by Arinze. Part of an ongoing enterprise IT home lab series.*








# 03 – Active Directory Lab (Part 2)

## 📌 Overview
Configured **Group Policies, permissions, login scripts, and user accounts**.

---

## 🛠️ Tools Used
- GPMC (Group Policy Management Console)
- PowerShell

---

## 🧩 Steps Performed

### 1. Created User Accounts
📸 *Screenshot: Users created*



---

### 2. Created Group Policies
📸 *Screenshot: GPO list*


---

### 3. Linked GPOs to OUs
📸 *Screenshot: GPO link result*


---

### 4. Applied Login Scripts
📸 *Screenshot: Script path*
<img width="967" height="845" alt="Screenshot044122" src="https://github.com/user-attachments/assets/b2aec2a2-0823-4604-af9b-1c68d1e0d851" />

<img width="961" height="786" alt="Screenshot020444" src="https://github.com/user-attachments/assets/9db92d52-ef12-4d4c-bd3f-b67fcade7df5" />

---

### 5. Tested Policies with Client VM
📸 *Screenshot: Resulting policy applied*
<img width="1020" height="891" alt="Screenshot025235" src="https://github.com/user-attachments/assets/49882850-ccf0-4c71-8610-414a118249a5" />
<img width="1030" height="774" alt="Screenshot060858" src="https://github.com/user-attachments/assets/b7f0998d-c0fa-4b04-bbda-e0b097d276b6" />

<img width="988" height="766" alt="Screenshot055852" src="https://github.com/user-attachments/assets/ddb0d9dd-73c1-4716-8411-66dbef6f9f45" />

<img width="969" height="515" alt="Screenshot020315" src="https://github.com/user-attachments/assets/b634a310-1a79-480d-9f5e-ab3949be81fd" />


---

## 📘 What I Learned

- How to join a Windows client machine to an Active Directory domain.
- The importance of correct DNS configuration for domain authentication and Group Policy.
- How Active Directory uses DNS to resolve domain services.
- How to create and apply Group Policy Objects (GPOs) to Organizational Units.
- How to verify Group Policy application using `gpupdate /force` and `gpresult /r`.
- How to troubleshoot domain login issues related to password resets.
- The difference between local user authentication and domain user authentication.
- How to diagnose and fix client-side Group Policy issues.
- How domain-level Group Policies override local Group Policy settings.
- The importance of proper network configuration in virtualized lab environments.
  

---

## ❗ Issues & Fixes

### 1. Client VM Not Joined to Domain
**Issue:** Windows 10 client displayed `WORKGROUP` instead of the domain.  
**Fix:**  
- Joined the client to `arinzelab.local` using domain administrator credentials.  
- Restarted the system to complete domain membership.

---

### 2. Domain Name Not Resolving
**Issue:** `ping arinzelab.local` failed with “host not found.”  
**Fix:**  
- Configured the client’s DNS to point to the Domain Controller.  
- Cleared the DNS cache using `ipconfig /flushdns`.

---

### 3. DHCP Renewal Failure
**Issue:** `ipconfig /renew` failed with a DHCP timeout error.  
**Fix:**  
- Ensured both client and server were on the same internal network.  
- Verified the DHCP service was running on the Domain Controller.

---

### 4. Domain User Login Failure After Password Reset
**Issue:** User could not log in after a forced password change.  
**Fix:**  
- Reset the password without enforcing an immediate change.  
- Logged in using the correct format: `arinzelab\jdoe`.

---

### 5. Group Policy Not Applying
**Issue:** Group Policy Objects appeared not to apply to the client system.  
**Fix:**  
- Ran `gpresult /r` to confirm policy application.  
- Verified that **Disable Control Panel** and **LoginScript-GPO** were applied.


---

## ✅ Final Outcome
Successfully joined the domain, authenticated domain users correctly, and applied Group Policy Objects as expected.

