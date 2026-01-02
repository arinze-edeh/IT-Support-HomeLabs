# 08 – PowerShell Automation Lab

## 📌 Overview

- This lab focuses on automating Active Directory user management using PowerShell
- The goal was to create, validate, and manage AD users programmatically
- Emphasis was placed on security, automation, and error handling

---

## 🛠️ Tools Used
- Windows Server (Domain Controller)
- Windows PowerShell
- Active Directory Module for PowerShell
- Active Directory Users and Computers (ADUC)

---

## 🧩 Steps Performed

### 1. Created Script for User Creation

Created a dedicated Organizational Unit (Automation_Users) using PowerShell.
Verified OU creation using Get-ADOrganizationalUnit to ensure proper directory structure before user automation.

📸 *Screenshot: script code*

<img width="1150" height="1079" alt="Screenshot 2026-01-01 153714" src="https://github.com/user-attachments/assets/50e9161a-04f8-4942-afc3-ebe5690cdd1c" />
<img width="1150" height="1079" alt="Screenshot 2026-01-01 154115" src="https://github.com/user-attachments/assets/2508954e-0446-41a4-ad5a-b88a51704722" />
<img width="1152" height="1079" alt="Screenshot 2026-01-01 161104" src="https://github.com/user-attachments/assets/4f25053e-a203-4a49-b923-20761f4a1e06" />
<img width="1155" height="1079" alt="image" src="https://github.com/user-attachments/assets/50cf7b59-3c69-449d-b168-c17209b5119f" />
<img width="1151" height="1079" alt="image" src="https://github.com/user-attachments/assets/a5bbedf5-97cf-46d5-80df-22881df1baa5" />
<img width="1150" height="1079" alt="image" src="https://github.com/user-attachments/assets/b4fd7e15-2cd6-411d-9ea6-074946e2120d" />

---

### 2. Automated OU Assignments

This focused on automating Active Directory user creation using PowerShell. Tasks included creating an Organizational Unit (OU), executing a user-creation script, and troubleshooting AD errors such as server processing failures and duplicate UPN conflicts. The lab concluded with successful automated user provisioning within the domain.

📸 *Screenshot: script execution*

<img width="1150" height="1079" alt="image" src="https://github.com/user-attachments/assets/5ac20a56-9b18-439e-b224-6f3acb94bf63" />
<img width="1155" height="1079" alt="image" src="https://github.com/user-attachments/assets/8286ecff-c798-46ae-a3e7-508e43eb680a" />

---

### 3. Generated Passwords Automatically

Implemented automatic password generation within the PowerShell user-provisioning script. 
Passwords are generated programmatically, converted to SecureString format, 
and applied during Active Directory user creation to improve security 
and eliminate hard-coded credentials.

📸 *Screenshot: output*

<img width="1148" height="1079" alt="image" src="https://github.com/user-attachments/assets/b481fd86-2cf6-4816-a994-771a2a57ab07" />
<img width="1151" height="1079" alt="image" src="https://github.com/user-attachments/assets/7160ad97-171e-4e44-acf6-5765fb4c4fb1" />



---

### 4. Validated Users in AD

Validated successful user creation using multiple verification methods. Confirmed user objects existed in the correct Organizational Unit (OU) via Active Directory Users and Computers (ADUC), verified attributes and status through PowerShell (`Get-ADUser`), and ensured uniqueness across the domain. Screenshots were captured for each validation method to demonstrate successful deployment and verification.

📸 *Screenshot: AD user list*

<img width="1149" height="1079" alt="image" src="https://github.com/user-attachments/assets/a4306632-cc54-4e74-ad94-d933b553da89" />
<img width="1152" height="1079" alt="image" src="https://github.com/user-attachments/assets/dc0909f8-f8f2-47c2-8351-f848516e5409" />
<img width="1148" height="1078" alt="image" src="https://github.com/user-attachments/assets/4c924515-3e35-450e-8f96-c11d535227c8" />

---

### 5. Enhanced Script with Error Handling

The PowerShell script was enhanced to improve robustness and prevent execution failures during Active Directory user creation. User existence validation was implemented using `Get-ADUser` to avoid duplicate accounts. Automated password generation was securely handled with `ConvertTo-SecureString`. Basic error handling logic was added to gracefully manage failures and provide clear console feedback for successful creation, existing users, and error conditions. These enhancements make the script safer, idempotent, and more suitable for repeated administrative use in Active Directory environments.

📸 *Screenshot: Improved script with error handling*

<img width="1157" height="1079" alt="image" src="https://github.com/user-attachments/assets/7976db4b-b176-4262-9144-3ebbbac32d2f" />
<img width="1157" height="1079" alt="image" src="https://github.com/user-attachments/assets/3fd86c09-c866-48e0-a223-37217c99107e" />

---

## 📘 What I Learned
- How to import and use the Active Directory PowerShell module
- How to automate user creation in a specific Organizational Unit (OU)
- How to generate secure passwords programmatically
- How to validate user existence before creation
- How to enhance scripts with basic error handling 

---

## ❗ Issues & Fixes
- **Issue: Duplicate user creation attempts**
   - Fix: Implemented Get-ADUser existence checks

 - **Issue: Plain-text password handling**
   - Fix: Converted passwords to SecureString format

 - **Issue: Script errors stopping execution**
   - Fix: Added conditional logic and error-handling output

 - **Issue: Validation uncertainty**
   - Fix: Verified users via ADUC, PowerShell queries, and screenshots

---

## ✅ Final Outcome
 - Successfully automated Active Directory user creation
- Users were validated across multiple methods
- Script safely handles re-runs without breaking
- Clear console output for success and failure cases
