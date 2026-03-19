# 08 - PowerShell Automation Lab
 
## Table of Contents
 
1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Tools and Technologies](#tools-and-technologies)
4. [Lab Configuration Steps](#lab-configuration-steps)
5. [Key Learnings](#key-learnings)
6. [Outcome Summary](#outcome-summary)
 
---
 
## Overview
 
This lab focuses on automating Active Directory (AD) user management using PowerShell. The goal was to design, execute, and validate a script capable of provisioning AD users programmatically within a Windows Server domain environment. Emphasis was placed on security best practices, automation efficiency, and robust error handling to produce a script safe for repeated administrative use.
 
---
 
## Prerequisites
 
Before beginning this lab, ensure the following are in place:
 
- A Windows Server instance configured as a Domain Controller
- Administrator-level access to the domain
- Active Directory Domain Services (AD DS) role installed and running
- PowerShell remoting enabled (if running scripts remotely)
- The Active Directory Module for PowerShell available (`RSAT: Active Directory DS and LDS Tools`)
- Basic familiarity with PowerShell scripting and Active Directory concepts
 
---
 
## Tools and Technologies
 
| Tool / Technology | Purpose |
|---|---|
| Windows Server (Domain Controller) | Host environment for Active Directory |
| Windows PowerShell | Scripting and automation engine |
| Active Directory Module for PowerShell | AD cmdlets (`New-ADUser`, `Get-ADUser`, etc.) |
| Active Directory Users and Computers (ADUC) | GUI-based validation of provisioned users |
 
---
 
## Lab Configuration Steps
 
### Step 1: Created Script for User Creation
 
A dedicated Organizational Unit named `Automation_Users` was created using PowerShell. Its existence was then verified using `Get-ADOrganizationalUnit` to confirm proper directory structure before any user automation was executed. This ensures all provisioned accounts land in the correct location within the AD hierarchy.
 
> Screenshots: script code
 
<img width="1150" height="1079" alt="Screenshot 2026-01-01 153714" src="https://github.com/user-attachments/assets/50e9161a-04f8-4942-afc3-ebe5690cdd1c" />
<img width="1150" height="1079" alt="Screenshot 2026-01-01 154115" src="https://github.com/user-attachments/assets/2508954e-0446-41a4-ad5a-b88a51704722" />
<img width="1152" height="1079" alt="Screenshot 2026-01-01 161104" src="https://github.com/user-attachments/assets/4f25053e-a203-4a49-b923-20761f4a1e06" />
<img width="1155" height="1079" alt="image" src="https://github.com/user-attachments/assets/50cf7b59-3c69-449d-b168-c17209b5119f" />
<img width="1151" height="1079" alt="image" src="https://github.com/user-attachments/assets/a5bbedf5-97cf-46d5-80df-22881df1baa5" />
<img width="1150" height="1079" alt="image" src="https://github.com/user-attachments/assets/b4fd7e15-2cd6-411d-9ea6-074946e2120d" />
 
---
 
### Step 2: Automated OU Assignments
 
The user creation script was executed to automate provisioning of accounts directly into the `Automation_Users` OU. During this step, AD errors were encountered and resolved, including server processing failures and duplicate User Principal Name (UPN) conflicts. The lab concluded with successful automated user provisioning within the domain.
 
> Screenshots: script execution
 
<img width="1150" height="1079" alt="image" src="https://github.com/user-attachments/assets/5ac20a56-9b18-439e-b224-6f3acb94bf63" />
<img width="1155" height="1079" alt="image" src="https://github.com/user-attachments/assets/8286ecff-c798-46ae-a3e7-508e43eb680a" />
 
---
 
### Step 3: Generated Passwords Automatically
 
Automatic password generation was implemented within the provisioning script. Passwords are generated programmatically, converted to `SecureString` format using `ConvertTo-SecureString`, and applied at the point of user creation. This approach eliminates hard-coded credentials and aligns with security best practices for AD automation.
 
> Screenshots: output
 
<img width="1148" height="1079" alt="image" src="https://github.com/user-attachments/assets/b481fd86-2cf6-4816-a994-771a2a57ab07" />
<img width="1151" height="1079" alt="image" src="https://github.com/user-attachments/assets/7160ad97-171e-4e44-acf6-5765fb4c4fb1" />
 
---
 
### Step 4: Validated Users in AD
 
Successful user creation was confirmed using multiple verification methods to ensure completeness and accuracy. User objects were inspected in ADUC to confirm placement in the correct OU. PowerShell's `Get-ADUser` cmdlet was used to verify attributes, account status, and domain-wide uniqueness. Screenshots were captured for each validation method.
 
> Screenshots: AD user list
 
<img width="1149" height="1079" alt="image" src="https://github.com/user-attachments/assets/a4306632-cc54-4e74-ad94-d933b553da89" />
<img width="1152" height="1079" alt="image" src="https://github.com/user-attachments/assets/dc0909f8-f8f2-47c2-8351-f848516e5409" />
<img width="1148" height="1078" alt="image" src="https://github.com/user-attachments/assets/4c924515-3e35-450e-8f96-c11d535227c8" />
 
---
 
### Step 5: Enhanced Script with Error Handling
 
The script was refactored to improve robustness and support safe re-execution. The following enhancements were applied:
 
- **Duplicate account prevention:** `Get-ADUser` checks run before each creation attempt, skipping existing accounts
- **Secure password handling:** `ConvertTo-SecureString` used throughout, eliminating plain-text credentials
- **Conditional logic and feedback:** The script outputs clear console messages for three distinct states: successful creation, skipped (user exists), and failed (error condition)
 
These changes make the script idempotent and suitable for recurring administrative use without risk of breaking the AD environment.
 
> Screenshots: improved script with error handling
 
<img width="1157" height="1079" alt="image" src="https://github.com/user-attachments/assets/7976db4b-b176-4262-9144-3ebbbac32d2f" />
<img width="1157" height="1079" alt="image" src="https://github.com/user-attachments/assets/3fd86c09-c866-48e0-a223-37217c99107e" />
 
---
 
## Key Learnings
 
- How to import and use the Active Directory PowerShell module in a domain environment
- How to create and verify Organizational Units programmatically before executing user provisioning
- How to automate user creation scoped to a specific OU using `New-ADUser`
- How to generate and securely handle passwords using `ConvertTo-SecureString`, avoiding plain-text exposure
- How to implement idempotent scripts using pre-creation existence checks with `Get-ADUser`
- How to build error handling logic that provides actionable console feedback without halting script execution
- How to cross-validate AD changes using both ADUC and PowerShell queries
 
---
 
## Outcome Summary
 
| Objective | Result |
|---|---|
| Automated AD user creation via PowerShell | Completed |
| OU creation and scoped provisioning | Completed |
| Secure password generation | Completed |
| Multi-method user validation (ADUC + PowerShell) | Completed |
| Duplicate account prevention | Completed |
| Error-resilient, idempotent script | Completed |
 
The final script successfully provisions Active Directory users into a dedicated OU, generates secure credentials automatically, validates existence before each creation attempt, and provides clear output for all execution outcomes. The script is safe for repeated use in a live domain environment.
