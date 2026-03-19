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

**Screenshot: Script path configured in Group Policy Editor**

<img width="967" height="845" alt="Screenshot044122" src="https://github.com/user-attachments/assets/b2aec2a2-0823-4604-af9b-1c68d1e0d851" />

<img width="961" height="786" alt="Screenshot020444" src="https://github.com/user-attachments/assets/9db92d52-ef12-4d4c-bd3f-b67fcade7df5" />

> *Group Policy Editor showing the logon script configured under User Configuration.*

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

**Screenshots: gpupdate /force execution on client**

<img width="1020" height="891" alt="Screenshot025235" src="https://github.com/user-attachments/assets/49882850-ccf0-4c71-8610-414a118249a5" />
<img width="1030" height="774" alt="Screenshot060858" src="https://github.com/user-attachments/assets/b7f0998d-c0fa-4b04-bbda-e0b097d276b6" />

<img width="988" height="766" alt="Screenshot055852" src="https://github.com/user-attachments/assets/ddb0d9dd-73c1-4716-8411-66dbef6f9f45" />

<img width="969" height="515" alt="Screenshot020315" src="https://github.com/user-attachments/assets/b634a310-1a79-480d-9f5e-ab3949be81fd" />

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
| Login script deployed | Passed |
| Client VM joined to `arinzelab.local` | Passed |
| Domain user authentication verified on client | Passed |
| Group Policy application confirmed via gpresult | Passed |
| Control Panel restriction enforced on client | Passed |

**Final Result:** All Group Policy Objects applied as expected. Domain user authentication succeeded. Login script executed on logon. All lab objectives completed.

---

*Lab documentation authored by Arinze. Part of an ongoing enterprise IT home lab series.*

